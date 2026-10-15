---
title: Trading Engine Explorer
allowTitleToDifferFromFilename: true
confluence_url:
confluence_status: draft
---
**TRADING ENGINE EXPLORER**

Tài liệu kiến trúc, triển khai & vận hành

**Phiên bản:** 1.0.0-a8

**Ngày:** 2026-09-28

**Phạm vi:** Kiến trúc, luồng dữ liệu, triển khai, vận hành

# 1. Tổng quan

**TE Explorer biến luồng event của Trading Engine (TE) thành dữ liệu tra cứu, giám sát và truy vết được.**

TE là core của sàn giao dịch coin tập trung, kiến trúc event sourcing trên Kafka + S3 — hoạt động như một "hộp đen" khó monitor. Explorer **đọc song song** các Kafka topic của TE bằng consumer group riêng, lưu vào ClickHouse và cung cấp Web Console / API / MCP cho đội vận hành.

```mermaid
flowchart LR
    ops(["Đội vận hành / AI agent"])

    subgraph te["Trading Engine · hệ thống giao dịch"]
        services["TE services"] <--> kafka[("Kafka topics")]
    end

    subgraph explorer["TE Explorer · chỉ đọc, bất đồng bộ"]
        direction LR
        subgraph colApp[" "]
            direction TB
            console["Web Console"]
            worker["Worker<br/>Projector"]
        end
        backend["Backend<br/>Ingester + Query API"]
        subgraph colStore[" "]
            direction TB
            ch[("ClickHouse")]
            redis[("Redis riêng")]
        end
        console --> backend
        backend --> ch
        worker -.-> ch
        backend -.-> redis
        worker -.-> redis
    end

    kafka -->|"consumer group riêng"| backend
    ops --> console
    ops -.->|MCP| backend

    classDef app fill:#d1fae5,stroke:#10b981,color:#064e3b
    classDef db fill:#ffedd5,stroke:#f97316,color:#7c2d12
    classDef actor fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    classDef plain fill:#ffffff,stroke:#94a3b8,color:#0f172a
    class console,backend,worker app
    class ch,redis db
    class ops actor
    class services,kafka plain

    style te fill:#f8fafc,stroke:#cbd5e1,color:#334155
    style explorer fill:#f8fafc,stroke:#cbd5e1,color:#334155
    style colApp fill:none,stroke:none
    style colStore fill:none,stroke:none
```

**Nguyên tắc cốt lõi:** Explorer không nằm trên đường xử lý lệnh, không ghi vào Kafka, không dùng chung group/offset/Redis keyspace với TE. Explorer lỗi hay chậm thì TE vẫn chạy bình thường.

# 2. Kiến trúc & cấu phần

## 2.1 Tổng thể

```mermaid
flowchart LR
    subgraph src["① Nguồn - Trading Engine"]
        direction TB
        eventRaw[/event-raw/]
        fundFlow[/fund-flow-orchestrator/]
        matching[/matching-engine<br>matching-engine-hot-1/]
        syncSvc[/sync-service/]
        schemaReg[Schema Registry]
    end

    subgraph ingest["② Ingest"]
        ingester["Ingester<br/>5 consumer · group riêng"]
    end

    subgraph mid[" "]
        direction TB
        subgraph store["③ Lưu trữ - ClickHouse"]
            direction LR
            subgraph storeIn[" "]
                direction TB
                lifecycle[("Lifecycle<br/>facts → state (MV)")]
                checkpoint[("Checkpoint<br/>consumer · projector")]
            end
            subgraph storeOut[" "]
                direction TB
                raw[("Raw<br/>engine_events")]
                tracing[("Tracing<br/>business_action_events")]
                serving[("Serving<br/>samples · health · 1m rollup")]
            end
        end
        subgraph agg["④ Tổng hợp"]
            worker["Worker / Projector<br/>leader duy nhất"]
        end
    end

    subgraph serve["⑤ Phục vụ"]
        direction TB
        mcp["MCP endpoint"]
        subgraph apiRow[" "]
            direction LR
            api["Query API · /api/v1"]
            sql["SQL console (read-only)"]
        end
        web["Web Console<br/>React + Nginx"]
        redis[("Redis Explorer<br/>cache · leader lock · MCP token")]
    end

    eventRaw --> ingester
    fundFlow --> ingester
    matching --> ingester
    syncSvc --> ingester
    schemaReg -->|schema| ingester

    ingester --> raw
    ingester --> tracing
    ingester --> lifecycle
    ingester --> checkpoint

    lifecycle --> worker
    checkpoint --> worker
    worker --> serving

    raw --> api
    tracing --> api
    serving --> api
    api --> mcp
    api --> sql
    web --> api

    worker -.-> redis
    api -.-> redis

    classDef app fill:#d1fae5,stroke:#10b981,color:#064e3b
    classDef db fill:#ffedd5,stroke:#f97316,color:#7c2d12
    classDef plain fill:#ffffff,stroke:#94a3b8,color:#0f172a

    class ingester,worker,mcp,api,sql,web app
    class raw,tracing,lifecycle,checkpoint,serving,redis db
    class eventRaw,fundFlow,matching,matchingHot,syncSvc,schemaReg plain

    style src fill:#f8fafc,stroke:#cbd5e1,color:#334155
    style ingest fill:#f8fafc,stroke:#cbd5e1,color:#334155
    style store fill:#f8fafc,stroke:#cbd5e1,color:#334155
    style agg fill:#f8fafc,stroke:#cbd5e1,color:#334155
    style serve fill:#f8fafc,stroke:#cbd5e1,color:#334155
    style mid fill:none,stroke:none
    style storeIn fill:none,stroke:none
    style storeOut fill:none,stroke:none
    style apiRow fill:none,stroke:none
```

Dữ liệu đi **một chiều**: Kafka → Ingester → ClickHouse → Worker → bảng serving → API. Không cấu phần nào ghi ngược vào TE. S3 của TE hiện **không** được Explorer sử dụng (chỉ có khai báo config, chưa có luồng đọc).

## 2.2 Cấu phần

| **Cấu phần**           | **Trách nhiệm**                                                                                               | **Phụ thuộc**                      | **Source**                         |
| ---------------------- | ------------------------------------------------------------------------------------------------------------- | ---------------------------------- | ---------------------------------- |
| **Ingester**           | Consume Kafka, decode protobuf qua Schema Registry, lọc event, phân loại lifecycle, ghi batch, lưu checkpoint | Kafka, Schema Registry, ClickHouse | `internal/kafka`                   |
| **Worker (Projector)** | Từ lifecycle state dựng completed samples, in-flight health, rollup 1 phút; ghi pipeline status               | ClickHouse, Redis (leader lock)    | `internal/reconcile`               |
| **Query API**          | Auth, tra cứu event/order/deposit/withdrawal, timeline, metrics, dashboard                                    | ClickHouse, Redis (cache, token)   | `internal/query, internal/routers` |
| **SQL console**        | Chạy SQL tùy ý ở chế độ `readonly=1`, giới hạn rows/bytes/thời gian                                           | ClickHouse                         | `internal/sqlconsole`              |
| **MCP**                | Cho AI agent gọi các query API + SQL qua token riêng                                                          | Redis (token)                      | `internal/routers/mcp.go`          |
| **Web Console**        | Dashboard, tìm kiếm, timeline, payload, SQL, quản lý MCP token                                                | Query API                          | `te-explore-fe/src`                |
| **Migration job**      | Áp schema ClickHouse trước khi rollout (Helm hook)                                                            | ClickHouse                         | `migrations/`                      |

### Vai trò runtime

Backend là **một binary**; vai trò bật bằng ENV. Không đặt cờ nào ⇒ bật cả ba (dùng cho local).

```mermaid
flowchart LR
    be["te-explore-be<br/>(1 image)"]

    be -->|INGESTER=true| kafka["Kafka consumers"]
    be -->|QUERIER=true| query["HTTP API · MCP · SQL"]
    be -->|WORKER=true| worker["Projector loops"]
    be -->|luôn có| health["/ping · /metrics"]

    style be fill:#dbeafe,stroke:#2563eb,color:#1e3a8a
    style kafka fill:#d1fae5,stroke:#10b981,color:#064e3b
    style query fill:#d1fae5,stroke:#10b981,color:#064e3b
    style worker fill:#d1fae5,stroke:#10b981,color:#064e3b
    style health fill:#d1fae5,stroke:#10b981,color:#064e3b
```

| **Deployment (Helm)** | **INGESTER** | **QUERIER** | **WORKER** | **Scale**                                                                |
| --------------------- | ------------ | ----------- | ---------- | ------------------------------------------------------------------------ |
| `te-explore-be`       | ✅           | ✅          | ❌         | Ingest song song tối đa = số partition; thêm replica cũng nhân tải query |
| `te-explore-worker`   | ❌           | ❌          | ✅         | Chỉ 1 leader làm việc; replica thêm chỉ để failover                      |
| `te-explore-fe`       | —            | —           | —          | Stateless                                                                |

## 2.3 Mô hình dữ liệu ClickHouse

```mermaid
flowchart LR
    subgraph ing["Ingester ghi"]
        direction TB
        ee[(engine_events)]
        bae[(business_action_events)]
        alf[(action_lifecycle_facts)]
        err[("event_decode_failures<br/>lifecycle_classification_anomalies")]
        cp[(consumer_checkpoints)]
    end

    subgraph mv["Materialized View (tự động)"]
        direction TB
        ams[(action_milestone_state)]
        als[(action_lifecycle_state)]
    end

    subgraph wk["Worker ghi"]
        direction TB
        acs[(action_completed_samples)]
        m1m[("action_metrics_1m<br/>action_flow_metrics_1m<br/>action_health_1m")]
        ahc[("action_health_current<br/>action_health_cohorts")]
        chk[("action_metrics_reconciler_checkpoints<br/>action_metrics_pipeline_status")]
    end


    alf --> ams
    alf --> als
    ams --> acs
    als --> acs
    als --> ahc
    acs --> m1m

    classDef raw fill:#ffedd5,stroke:#f97316,color:#9a3412
    classDef view fill:#fef3c7,stroke:#d97706,color:#92400e
    classDef serve fill:#d1fae5,stroke:#10b981,color:#065f46

    class ee,bae,alf,err,cp raw
    class ams,als view
    class acs,m1m,ahc,chk serve

    style ing fill:#eff6ff,stroke:#93c5fd,color:#1e3a8a
    style mv fill:#fffbeb,stroke:#fcd34d,color:#92400e
    style wk fill:#eff6ff,stroke:#93c5fd,color:#1e3a8a
```

| **Lớp**   | **Bảng**                                                                                          | **Engine / khóa**                                                                 | **Ý nghĩa**                                                        |
| --------- | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| Raw       | `engine_events`                                                                                   | `ReplacingMergeTree`, key (`topic`, `partition`, `offset`)                        | 1 dòng / Kafka record: payload, user/entity ID, vị trí Kafka       |
| Tracing   | `business_action_events`                                                                          | `ReplacingMergeTree`, key theo `action_type`, `entity_type`, `action_id`, `role…` | Gom event của cùng 1 nghiệp vụ (order, deposit, withdrawal, trade) |
| Lifecycle | `action_lifecycle_facts` → `action_lifecycle_state`, `action_milestone_state`                     | Facts: `ReplacingMergeTree`; State: `AggregatingMergeTree` qua MV                 | Mốc bắt đầu / phase / kết thúc của từng action                     |
| Serving   | `action_completed_samples`, `action_health_*`, `action_*_*1m`                                     | Worker ghi qua staging + `REPLACE PARTITION`                                      | Dữ liệu đã tổng hợp cho dashboard, histogram, in-flight            |
| Tiến độ   | `consumer_checkpoints`, `action_metrics_reconciler_checkpoints`, `action_metrics_pipeline_status` | `ReplacingMergeTree`                                                              | Resume ingest, cursor worker, freshness                            |
| Lỗi       | `event_decode_failures`, `lifecycle_classification_anomalies`                                     | `ReplacingMergeTree`                                                              | Dead-letter decode, event không khớp rule lifecycle                |

- **Idempotent khi replay:** key theo vị trí Kafka ⇒ đọc lại cùng offset chỉ tạo bản trùng tạm thời, được gộp khi merge (dùng `FINAL`/khử trùng khi cần số chính xác).
- **Retention:** TTL theo `expires_at` — mặc định **30 ngày**, một số event log hệ thống **72 giờ**; `pipeline_status` 30 ngày; completed samples 365 ngày. Explorer **không** phải kho lưu trữ vĩnh viễn của TE.
- **Thời gian:** lưu UTC; Web Console đổi timezone khi hiển thị.

## 2.4 Redis

| **Mục đích**       | **Key (prefix recent-events-storer:)** | **TTL**         | **Mất Redis thì**                           |
| ------------------ | -------------------------------------- | --------------- | ------------------------------------------- |
| Leader lock Worker | `action-metrics-projector`             | 30 s            | Worker dừng tổng hợp; ingest vẫn chạy       |
| Fencing generation | `action-metrics-generation` (INCR)     | —               | Leader cũ bị chặn ghi khi generation đổi    |
| Cache dashboard    | `dashboard:utc1:*`                     | 60 s (cấu hình) | API dashboard trả lỗi                       |
| MCP token          | theo user + token ID                   | 1–30 ngày       | MCP bị từ chối; login console vẫn dùng được |

Session đăng nhập console là **bearer token ký HMAC, không lưu Redis**. Key là cố định ⇒ mỗi môi trường Explorer cần Redis DB riêng.

## 2.5 Cô lập với TE

```mermaid
flowchart TB
    subgraph kafka["Kafka"]
        topics[/Topics TE/]
    end

    topics -->|"group TE"| te["TE consumers<br/>offset của TE"]
    topics -->|"group recent-events-storer-*"| ing["Explorer Ingester<br/>offset riêng · chỉ đọc"]

    subgraph redis["Redis"]
        direction LR
        teDb[("DB của TE")]
        exDb[("DB riêng Explorer<br/>(ưu tiên: instance riêng)")]
    end

    te -.-> teDb
    ing -.-> exDb

    classDef teNode fill:#eef2f6,stroke:#94a3b8,color:#334155
    classDef exNode fill:#d1fae5,stroke:#34d399,color:#065f46

    class te,teDb teNode
    class ing,exDb exNode

    style kafka fill:#f8fafc,stroke:#cbd5e1,color:#334155
    style topics fill:#f8fafc,stroke:#64748b,color:#1e293b
    style redis fill:#f8fafc,stroke:#cbd5e1,color:#475569
```

| **Tài nguyên** | **Yêu cầu bắt buộc**                                                                                             | **Lưu ý**                                                                                               |
| -------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Kafka          | Group ID riêng (`recent-events-storer-*`), credential riêng, ACL **chỉ READ** topic + quản lý group của Explorer | Code chỉ chặn trùng group trong cấu hình Explorer, không biết group của TE — kiểm tra khi review config |
| Redis          | Instance riêng; nếu dùng chung thì **DB index khác** TE (`REDIS_DB`)                                             | Khác DB chỉ tách keyspace, vẫn chung CPU/RAM/eviction. Redis là dependency bắt buộc khi khởi động       |
| ClickHouse     | Database riêng của Explorer                                                                                      | Query API có giới hạn range (168 h), limit, timeout; SQL console `readonly=1`                           |
| Mạng/IO        | Giới hạn tốc độ khi replay `earliest` lượng lớn                                                                  | Đọc Kafka vẫn cạnh tranh IO broker với TE                                                               |

# 3. Luồng xử lý

## 3.1 Ingest: Kafka → ClickHouse

```mermaid
sequenceDiagram
    autonumber
    participant Kafka
    participant Ingester
    participant SR as Schema Registry
    participant CH as ClickHouse

    Kafka->>Ingester: Poll batch (≤ 50k record, 8 partition song song)
    Ingester->>SR: Lấy schema khi cần
    Ingester->>Ingester: Decode · lọc allowlist/ignore · map action + lifecycle
    Ingester->>CH: INSERT events · actions · facts · anomalies · decode failures

    alt [Ghi thành công]
        CH-->>Ingester: OK
        Ingester->>Ingester: Mark offset + ghi nhớ next_offset
        Ingester->>Kafka: Auto-commit offset đã mark (mỗi 1 s)
    else [ClickHouse lỗi]
        CH-->>Ingester: Error
        Ingester->>Ingester: Retry backoff lũy tiến, vô hạn — KHÔNG mark offset
    end

    Note over Ingester,CH: Mỗi 90 s: flush consumer_checkpoints (start_offset, next_offset)
```

| **Quy tắc**                                               | **Hệ quả**                                                                                                          |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Ghi ClickHouse **trước**, mark offset **sau**             | At-least-once; không mất event, có thể ghi trùng khi restart (được khử trùng theo (`topic`, `partition`, `offset`)) |
| Tuần tự trong partition, song song giữa partition         | Không có thứ tự toàn cục giữa các topic — tracing dựa vào `event_time` + action ID                                  |
| Event decode lỗi vẫn được ghi vào `event_decode_failures` | Batch không bị kẹt vì 1 record hỏng; cần điều tra và xử lý riêng                                                    |
| Event bị lọc (ngoài allowlist hoặc `ignores`)             | Không lưu, nhưng offset vẫn tiến                                                                                    |
| ClickHouse down                                           | Ingester đứng chờ, lag tăng; TE không bị ảnh hưởng                                                                  |

### Consumer đang cấu hình

| **Consumer**            | **Topic**                | **Group ID**                                 | **Lọc**                                      |
| ----------------------- | ------------------------ | -------------------------------------------- | -------------------------------------------- |
| `ingress`               | `event-raw`              | `recent-events-storer-event-raw`             | Mọi event (`*`) trừ `ignores`                |
| `fund-flow`             | `fund-flow-orchestrator` | `recent-events-storer-fund-flow`             | Mốc điều phối deposit/withdrawal/order/trade |
| `matching-engine`       | `matching-engine`        | `recent-events-storer-matching-engine`       | `*_order.fund_locked`, `cancel_requested`    |
| `matching-engine-hot-1` | `matching-engine-hot-1`  | `recent-events-storer-matching-engine-hot-1` | Như trên                                     |
| `terminal`              | `sync-service`           | `recent-events-storer-sync-service`          | Mốc settle / fail / fill progress            |

Danh sách đầy đủ: config/config.yaml → kafka.consumers. TE thêm topic hoặc event type mới ⇒ phải cập nhật allowlist.

## 3.2 Resume sau restart / rebalance

```mermaid
flowchart TD
    assign(["Partition được assign"]) --> check{"Có checkpoint<br/>trong ClickHouse?"}

    check -->|Không| earliest["Lấy offset earliest / latest<br/>(KAFKA_NO_CHECKPOINT_START_POSITION)"]
    earliest --> save["Lưu làm start_offset =<br/>next_offset"]

    check -->|Có| resumeMin["resume =<br/>min(checkpoint.next_offset,<br/>Kafka committed offset)"]
    resumeMin --> resumeMax["resume = max(resume,<br/>start_offset)"]

    save --> read(["Bắt đầu đọc"])
    resumeMax --> read

    classDef startEnd fill:#f8fafc,stroke:#94a3b8,color:#334155
    classDef decision fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    classDef step fill:#d1fae5,stroke:#10b981,color:#064e3b
    class assign,read startEnd
    class check decision
    class earliest,save,resumeMin,resumeMax step
```

- Luôn chọn offset **bảo thủ** (nhỏ hơn) ⇒ có thể đọc lại một đoạn, không bỏ sót.
- `start_offset` là mốc sàn: reset offset Kafka về trước mốc này **không có tác dụng**.
- Log để xác nhận: `Selected conservative Kafka resume offset` (kèm `source: clickhouse_checkpoint` / `kafka_committed_offset` / `*_no_checkpoint`).

## 3.3 Tổng hợp: Worker / Projector

```mermaid
flowchart LR
    subgraph redis["Redis"]
        direction TB
        lock["Leader lock 30 s"]
        gen["Generation (INCR)"]
    end

    cp[("consumer_checkpoints<br/>biên dữ liệu đã ghi")]
    life[("lifecycle / milestone state")]

    subgraph leader["Worker leader"]
        direction TB
        reconcile["Reconcile · 60 s<br/>completed samples"]
        rollup["Rollup · 600 s<br/>metrics 1 phút"]
        health["Health · 300 s<br/>in-flight · cohort"]
    end

    samples[("action_completed_samples")]
    m1m[("action_*_1m")]
    ahc[("action_health_current /<br/>cohorts")]
    status[("pipeline_status<br/>reconciler_checkpoints")]

    lock -.->|chỉ leader chạy| leader
    gen -.->|chặn leader cũ ghi| leader

    cp --> reconcile
    life --> reconcile
    reconcile --> samples
    samples --> rollup
    rollup --> m1m
    rollup --> status
    reconcile --> status

    life --> health
    health --> ahc
    ahc --> status
    health --> status
    classDef redisNode fill:#fee2e2,stroke:#ef4444,color:#7f1d1d
    classDef job fill:#d1fae5,stroke:#34d399,color:#065f46
    classDef table fill:#ffedd5,stroke:#fb923c,color:#7c2d12

    class lock,gen redisNode
    class reconcile,rollup,health job
    class cp,life,samples,m1m,ahc,status table

    style redis fill:#f8fafc,stroke:#fecaca,color:#7f1d1d
    style leader fill:#f1f5f9,stroke:#cbd5e1,color:#334155
```

- Worker **không đọc Kafka**; chỉ đọc ClickHouse và chỉ xử lý tới offset đã có trong `consumer_checkpoints`.
- Chỉ **1 leader** chạy; mỗi lần lên leader lấy generation mới, mọi lần ghi kiểm tra generation ⇒ tránh 2 worker ghi đè nhau.
- Ghi serving qua bảng `*_staging `rồi `REPLACE PARTITION` ⇒ người đọc không thấy dữ liệu nửa vời.
- Chu kỳ là **tần suất chạy**, không phải SLA độ trễ. Dashboard là eventual consistency — luôn xem `data_freshness_at` / `pipeline-status`.

## 3.4 Truy vấn: Console / MCP → API

```mermaid
sequenceDiagram
    autonumber
    actor Ops as Vận hành / AI agent
    participant FE as Nginx (FE)
    participant API as Query API
    participant Redis
    participant CH as ClickHouse

    Ops->>FE: /api/v1/...
    FE->>API: proxy
    API->>API: Xác thực bearer (HMAC)

    opt [Token MCP]
        API->>API: Token còn hiệu lực?
    end

    alt [Dashboard]
        API->>Redis: Đọc cache
        alt [Miss]
            API->>CH: Query (singleflight, gộp request trùng)
            API->>Redis: Ghi cache TTL 60 s
        end
    else [Search / timeline / metrics / SQL]
        API->>CH: Query có giới hạn range · limit · timeout
    end

    API-->>Ops: JSON
```

| **Kênh**          | **Xác thực**                                                               | **Giới hạn chính**                        |
| ----------------- | -------------------------------------------------------------------------- | ----------------------------------------- |
| Web Console       | `POST /api/v1/auth/login` (Basic) → bearer token ký HMAC                   | Range ≤ 168 h, limit ≤ 500, timeout 15 s  |
| MCP (/api/v1/mcp) | Bearer token MCP, tạo trong console, lưu Redis TTL 1–30 ngày, thu hồi được | Như query API + SQL                       |
| SQL console       | Bearer console                                                             | `readonly=1`, ≤ 1000 rows, ≤ 8 MB, ≤ 15 s |

Danh sách endpoint: `GET /api/v1/query-apis` hoặc `query_catalog.go`.

## 3.5 Tracing một giao dịch

```mermaid
flowchart LR
    s1["① Kiểm tra freshness<br/>pipeline-status"]
    s2["② Tìm action<br/>user / order / action ID"]
    s3["③ Xem timeline<br/>các mốc theo thứ tự"]
    s4["④ Mở raw event<br/>topic · partition · offset"]
    s5["⑤ Kết luận<br/>chặng thiếu / chậm"]

    s1 --> s2 --> s3 --> s4 --> s5

```

Ví dụ vòng đời limit order qua các topic:

```mermaid
flowchart LR
    created["limit_order.created<br/>fund-flow"] --> locked["limit_order.fund_locked<br/>matching-engine"]
    locked --> placed["limit_order.placed<br/>sync-service"]
    placed --> fill["fill_progress_updated<br/>sync-service"]
    fill --> settled["limit_order.fund_settled<br/>sync-service"]

    locked -.->|lỗi| failed["fund_lock_failed"]
    placed -.->|hủy| cancel["cancel_requested →<br/>cancel_fund_settled"]

    classDef ok fill:#d1fae5,stroke:#34d399,color:#065f46
    classDef fail fill:#fee2e2,stroke:#ef4444,color:#991b1b
    classDef muted fill:#f1f5f9,stroke:#94a3b8,color:#334155

    class created,locked,placed,fill,settled ok
    class failed fail
    class cancel muted
```

Thiếu mốc? Kiểm tra theo thứ tự trước khi kết luận TE lỗi: **lag Kafka → allowlist/ignore → `event_decode_failures` → retention TTL → khoảng thời gian / timezone**.

# 4. Triển khai

## 4.1 Topology Kubernetes

```mermaid
flowchart TB
    client(["Trình duyệt / MCP client"])

    subgraph row[" "]
        direction LR

        subgraph ns["Namespace te-explore · Helm release"]
            direction TB
            ingress["Ingress · HTTPS<br/>te-explore.&lt;domain&gt;"]
            fe["te-explore-fe<br/>Nginx :8080 · /healthz"]
            api["/api/* → svc recent-events-storer"]
            secret["Secret te-explore-secret"]
            job["Migration Job<br/>pre-install / pre-upgrade"]
            worker["te-explore-worker<br/>WORKER · :8080 · /ping"]
            be["te-explore-be<br/>INGESTER + QUERIER · :8080 · /ping"]
        end

        subgraph ext["Hạ tầng cấp sẵn (ngoài chart)"]
            direction TB
            kafka[("Kafka + Schema Registry<br/>của TE")]
            subgraph stores[" "]
                direction LR
                ch[("ClickHouse<br/>DB Explorer")]
                redis[("Redis<br/>instance / DB riêng")]
            end
        end
    end

    client --> ingress
    ingress --> fe
    fe --> api
    api --> be

    secret -.-> job
    secret -.-> worker
    secret -.-> be

    kafka --> be
    job --> ch
    worker --> ch
    be --> ch
    worker --> redis
    be --> redis

    classDef actor fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    classDef edge fill:#dbeafe,stroke:#60a5fa,color:#1e3a8a
    classDef app fill:#d1fae5,stroke:#34d399,color:#064e3b
    classDef hook fill:#fef3c7,stroke:#d97706,color:#78350f
    classDef sec fill:#fffbeb,stroke:#d97706,color:#78350f,stroke-dasharray: 4 3
    classDef route fill:#fff7ed,stroke:#fdba74,color:#9a3412
    classDef db fill:#ffedd5,stroke:#f97316,color:#7c2d12

    class client actor
    class ingress edge
    class fe,worker,be app
    class job hook
    class secret sec
    class api route
    class kafka,ch,redis db

    style row fill:none,stroke:none
    style stores fill:none,stroke:none
    style ns fill:#f8fafc,stroke:#cbd5e1,color:#334155
    style ext fill:#f8fafc,stroke:#cbd5e1,color:#334155
```

| **Workload**        | **Image**                    | **Tài nguyên mặc định**   | **Probe**  |
| ------------------- | ---------------------------- | ------------------------- | ---------- |
| `te-explore-be`     | `te-explore-be`              | 2 CPU / 8 GiB             | `/ping`    |
| `te-explore-worker` | `te-explore-be` (cùng image) | 2 CPU / 8 GiB             | `/ping`    |
| `te-explore-fe`     | `te-explore-fe`              | 0.1–0.5 CPU / 128–256 MiB | `/healthz` |

`/ping` chỉ chứng minh process sống — **không** chứng minh ingest/worker đang chạy tiến. Xem §5 Vận hành.

## 4.2 Chuẩn bị hạ tầng

| **Hạng mục** | **Checklist**                                                                                                                                       |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Kafka        | ☐ User/credential riêng · ☐ ACL READ các topic ở §3.1 · ☐ ACL cho group `recent-events-storer-*` · ☐ Broker advertised address truy cập được từ pod |
| ClickHouse   | ☐ Database riêng (DSN `http(s)://…/?database=…`) · ☐ Quyền DDL cho migration, read/write cho app · ☐ Dung lượng đủ cho retention                    |
| Redis        | ☐ Instance riêng **hoặc** DB index khác TE · ☐ Mỗi môi trường Explorer 1 DB riêng                                                                   |
| K8s          | ☐ Namespace · ☐ Image pull secret · ☐ Secret te-explore-secret · ☐ Hostname + TLS ingress                                                           |

## 4.3 Cấu hình chính

Thứ tự ưu tiên: **ENV có prefix ENV_PREFIX_KEY** → ENV thường → `config.yaml`. Mẫu đầy đủ: `.env.sample`.

| **Nhóm**     | **Biến**                                                              | **Ghi chú**                                                                                                   |
| ------------ | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Vai trò      | INGESTER, QUERIER, WORKER                                             | Đọc trực tiếp ENV; không đặt gì = bật cả 3                                                                    |
| Kafka        | KAFKA*BROKERS, KAFKA_SCHEMAREGISTRYADDR, KAFKA_SASL\_\*, KAFKA_TLS*\* | Topic/group/allowlist khai báo trong YAML kafka.consumers (không truyền qua ENV)                              |
| Điểm bắt đầu | KAFKA_NO_CHECKPOINT_START_POSITION                                    | earliest \| latest; chỉ áp dụng khi **chưa có checkpoint**. YAML mặc định latest, convox.js đặt earliest      |
| ClickHouse   | CLICKHOUSE_DSN, CLICKHOUSE_TLS_CA_FILE, CLICKHOUSE_RETENTION_DAYS     | Migration job đọc biến **không prefix**. Luôn đặt rõ retention: mặc định migration 180 ngày ≠ runtime 30 ngày |
| Redis        | REDIS*ADDR, REDIS_PASSWORD, REDIS_DB, REDIS_TLS*\*                    | Bắt buộc — backend không khởi động nếu không kết nối được                                                     |
| Console      | RECENT_CONSOLE_USERNAME, RECENT_CONSOLE_PASSWORD                      | Đổi khỏi giá trị mặc định                                                                                     |
| Worker       | ACTION*METRICS_ENABLED, ACTION_METRICS*\*\_SECONDS                    | Chu kỳ reconcile / health / rollup                                                                            |
| Quan sát     | OTEL\_\*, LOG\_\*                                                     | /metrics Prometheus luôn bật                                                                                  |

TLS: dùng CA PEM mount vào container (\*\_TLS_CA_FILE), giữ \*\_INSECURE_SKIP_VERIFY=false. Chart hiện **chưa** có volume mount CA — cần bổ sung khi dùng private CA. Chưa hỗ trợ mTLS client cert.

## 4.4 Release

```mermaid
flowchart LR
    s1["① Bump<br/>VERSION.txt"] --> s2["② bin/test<br/>bin/helm-check"]
    s2 --> s3["③ Build + push<br/>2 image cùng tag"]
    s3 --> s4["④ Helm hook<br/>migration ClickHouse"]
    s4 --> s5["⑤ Rollout<br/>be · worker · fe"]
    s5 --> s6["⑥ Smoke test<br/>checkpoint tiến · timeline<br/>OK"]
    s4 -.->|lỗi → dừng,<br/>không rollout| abort(["Abort"])

    classDef ok fill:#d1fae5,stroke:#34d399,color:#065f46
    classDef fail fill:#fee2e2,stroke:#ef4444,color:#991b1b

    class s1,s2,s3,s4,s5,s6 ok
    class abort fail
```

```
npm --prefix te-explore-fe ci

bin/test && bin/helm-check

DOCKER_REGISTRY_URL=\<registry> ENV=\<env> NAMESPACE=te-explore bin/deploy
```

- bin/deploy = bin/build-images + bin/publish-k8s (Helm --atomic). Image đã có sẵn ⇒ chỉ chạy bin/publish-k8s.
- Topology nguồn ở convox.js → sinh helm-chart/values.generated.yaml. Sửa nguồn, không sửa file generated.
- Migration chỉ tiến, bỏ qua file đã chạy. **Helm rollback không rollback schema/dữ liệu ClickHouse.** V20260724000400 có DROP + tạo lại bảng — backup trước khi nâng cấp DB cũ.

**Hoàn tất khi:** pod ready · login được · consumer_checkpoints tiến khi có event mới · pipeline-status bắt kịp · tra được timeline 1 giao dịch đã biết.

## 4.5 Chạy local

```
cp te-explore-be/.env.sample te-explore-be/.env # trỏ tới Kafka/ClickHouse/Redis local, KHÔNG dùng group/DB của TE

sh te-explore-be/migrations/migrate.sh # cần CLICKHOUSE_DSN

bin/dev be # backend :10031, bật cả 3 vai trò

bin/dev fe # Vite :4174, proxy /api → backend
```

| **Lệnh**                      | **Kiểm chứng**                                                                  |
| ----------------------------- | ------------------------------------------------------------------------------- |
| bin/test                      | Go unit test + integration ClickHouse/Redis thật (Docker) + frontend test/build |
| bin/test-timezone-integration | Migration, query, projection trên ClickHouse thật với nhiều timezone            |
| bin/test-mcp-integration      | Vòng đời MCP token trên Redis thật                                              |
| bin/helm-check                | Lint + render chart                                                             |

# 5. Vận hành

## 5.1 Ba mốc tươi của dữ liệu

```mermaid
flowchart LR
    kafka[("Kafka<br/>offset mới nhất")] -->|"① Kafka lag"| cp["consumer_checkpoints<br/>next_offset"]
    cp -->|"② Projection lag"| rc["reconciler_checkpoints<br/>pipeline_status"]
    rc -->|"③ Cache ≤ 60 s"| dash["Dashboard"]

    classDef source fill:#ffedd5,stroke:#fb923c,color:#9a3412
    classDef stage fill:#d1fae5,stroke:#6ee7b7,color:#065f46

    class kafka source
    class cp,rc,dash stage
```

Kafka lag thấp **chưa đủ** để kết luận dashboard đúng. Search/timeline đọc raw (chỉ phụ thuộc ①); dashboard/histogram/in-flight phụ thuộc cả ①②③.

## 5.2 Giám sát

| **Tầng**              | **Tín hiệu**                                                                                               | **Nguồn**                     |
| --------------------- | ---------------------------------------------------------------------------------------------------------- | ----------------------------- |
| Kafka → Ingester      | Lag theo group recent-events-storer-\*; log insert failed; Kafka offsets remain uncommitted; log rebalance | Kafka monitoring, log backend |
| Ingester → ClickHouse | consumer_checkpoints.updated_at tiến ~90 s/lần; số dòng event_decode_failures mới                          | ClickHouse, SQL console       |
| Worker                | GET /api/v1/metrics/pipeline-status: source_next_offset vs projected_next_offset, last_error               | API / Console                 |
| API                   | HTTP 5xx, latency, lỗi Redis                                                                               | /metrics, log, OTel           |
| Hạ tầng               | ClickHouse disk/parts/merge; Redis memory/latency; tải broker                                              | Monitoring hạ tầng            |
| **TE**                | Latency/lag của TE khi Explorer replay lớn hoặc dùng chung Redis                                           | Monitoring của TE             |

SQL kiểm tra nhanh (SQL console):

```sql
-- Tiến độ ingest theo partition

SELECT consumer_id, topic, partition, argMax(next_offset, updated_at) AS next_offset, max(updated_at) AS updated_at

FROM consumer_checkpoints GROUP BY consumer_id, topic, partition ORDER BY updated_at;

-- Decode lỗi 1 giờ gần nhất

SELECT topic, count() FROM event_decode_failures

WHERE kafka_timestamp > now() - INTERVAL 1 HOUR GROUP BY topic;
```

## 5.3 Chẩn đoán sự cố

```mermaid
flowchart TD
    start(["Console thiếu / chậm dữ liệu"])
    api{"API trả lời?"}
    kafka{"Kafka lag tăng?"}
    pipe{"pipeline-status<br/>bắt kịp?"}

    ingress["Ingress · pod be · auth<br/>Redis (dashboard, MCP)"]
    ch["Log insert ClickHouse<br/>ACL/credential · rebalance"]
    worker["Pod worker · leader lock<br/>Redis<br/>last_error · query chậm"]
    allow["Allowlist/ignore · decode<br/>failures<br/>TTL · range thời gian / timezone"]

    start --> api
    api -->|Không| ingress
    api -->|Có| kafka
    kafka -->|Có| ch
    kafka -->|Không| pipe
    pipe -->|Không| worker
    pipe -->|Có| allow

    classDef decision fill:#e0f2fe,stroke:#38bdf8,color:#0c4a6e
    classDef action fill:#d1fae5,stroke:#6ee7b7,color:#065f46
    classDef symptom fill:#f8fafc,stroke:#94a3b8,color:#334155

    class api,kafka,pipe decision
    class ingress,ch,worker,allow action
    class start symptom
```

| **Sự cố**                    | **Hành vi hệ thống**                                                          | **Xử lý**                                                                                                            |
| ---------------------------- | ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| ClickHouse không ghi được    | Ingester retry vô hạn, không commit offset; lag tăng                          | Khôi phục ClickHouse → ingest tự chạy tiếp, không mất dữ liệu                                                        |
| Kafka / Schema Registry lỗi  | Client tự reconnect; record decode lỗi vào event_decode_failures              | Sửa kết nối/schema; thống kê record lỗi                                                                              |
| Redis lỗi                    | Worker mất leader → dừng tổng hợp; dashboard/MCP lỗi; pod mới không khởi động | Khôi phục Redis đúng DB. **Không** FLUSHALL/FLUSHDB nếu dùng chung với TE                                            |
| Worker dừng                  | Ingest + search vẫn đúng; dashboard cũ dần                                    | Kiểm tra pod, ACTION_METRICS_ENABLED, log leader                                                                     |
| Explorer gây tải lên TE      | —                                                                             | **Scale te-explore-be về 0 trước** (dừng cả ingest và API), điều tra sau. Checkpoint giữ nguyên, bật lại sẽ đọc tiếp |
| Event đã quá retention Kafka | Không đọc lại được                                                            | Chưa có luồng nạp từ S3 — ghi nhận khoảng thiếu                                                                      |

## 5.4 Restart · replay · rollback

| **Thao tác**                      | **Cách làm**                                                                                               | **Lưu ý**                                                                              |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| **Restart / scale**               | kubectl rollout restart bình thường                                                                        | Đọc lại một đoạn nhỏ là bình thường (khử trùng theo offset)                            |
| **Đọc từ đầu cho môi trường mới** | DB ClickHouse mới + KAFKA_NO_CHECKPOINT_START_POSITION=earliest                                            | Chỉ có tác dụng khi chưa có checkpoint; giới hạn tài nguyên để không tranh IO với TE   |
| **Backfill / debug 1 partition**  | Chạy instance cô lập với KAFKA*EVENTSTORESCOPE*\* (đọc trực tiếp topic/partition/offset, không dùng group) | Dùng DB ClickHouse + Redis DB riêng. **Không** xóa checkpoint của môi trường đang chạy |
| **Rollback app**                  | Helm rollback về image trước                                                                               | Kiểm tra tương thích schema; Helm **không** rollback ClickHouse — cần backup riêng     |

## 5.5 Checklist sau deploy / sự cố

```shell
kubectl -n te-explore get pods,jobs

kubectl -n te-explore logs deploy/te-explore-be --tail=100 | grep -E "resume offset|insert failed"

kubectl -n te-explore logs deploy/te-explore-worker --tail=100
```

☐ Pod ready, migration job thành công

☐ Log có Selected conservative Kafka resume offset cho mọi partition

☐ consumer_checkpoints tiến; không có decode failure bất thường

☐ pipeline-status bắt kịp sau 1–2 chu kỳ worker

☐ Tra được timeline 1 giao dịch mới

☐ Chỉ số của TE (lag, latency, Redis) không bị ảnh hưởng
