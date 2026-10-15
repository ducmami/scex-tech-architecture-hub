# Cấu trúc và Quy chuẩn Logging Ứng dụng SCEX

## Overview

Format log chuẩn JSON (flatten) cho tất cả microservices trong hệ thống SCEX.

**Design principles:**

- Flatten JSON, separator \_ (underscore)
- Timestamp luôn là ISO 8601 Zulu time (UTC), ví dụ: 2026-08-03T04:30:00.12

---

JSON Sample (Full Fields)

```json
{
  "timestamp": "2026-08-03T04:30:00.123Z",
  "observed_timestamp": "2026-08-03T04:30:00.125Z",
  "schema_version": "1.0",

  "log_type": "application",
  "log_level": "INFO",
  "log_level_number": 9,
  "log_logger": "com.scex.order.OrderService",
  "event_name": "order.created",
  "message": "Order created successfully",

  "trace_id": "4bf92f3577b34da6a3ce929d0e0e4736",
  "span_id": "00f067aa0ba902b7",
  "trace_flags": "01",

  "service_name": "scex-order-service",
  "service_version": "1.2.0",
  "service_environment": "production",
  "service_node_name": "scex-order-service-7d4b8c6f9-x2k4m",

  "process_pid": 1,
  "process_thread_name": "http-nio-8080-exec-1",

  "k8s_namespace": "scex-prod",
  "k8s_pod_name": "scex-order-service-7d4b8c6f9-x2k4m",
  "k8s_container_name": "order-service",
  "k8s_node_name": "ip-10-0-1-42.ec2.internal",

  "host_name": "ip-10-0-1-42",
  "host_ip": "10.0.1.42",

  "http_request_id": "req-a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "http_request_method": "POST",
  "http_request_path": "/api/v1/orders",
  "http_request_query": "type=instant",
  "http_response_status_code": 201,
  "http_route": "/api/v1/orders",
  "http_request_duration_ms": 142,
  "client_address": "10.0.5.23",
  "user_agent_original": "SCEX-SDK/2.1.0 Android/14",

  "error_type": "com.scex.exception.InsufficientBalanceException",
  "error_message": "Insufficient balance for transaction",
  "error_stack_trace": "com.scex.exception.InsufficientBalanceException: Insufficient balance...\n\tat com.scex.order.OrderService.create(OrderService.java:45)...",

  "scex_order_id": "TXN-20260803-ABC123",
  "scex_user_id": "usr_9f8e7d6c",
  "scex_correlation_id": "corr-550e8400-e29b-41d4-a716-446655440000"
}
```

## Field Reference

> ✅ Bắt buộc ⚡ Tùy chọn

### 1. Core Fields (bắt buộc — ứng dụng phải log mục này)

| Field              | Type              | Required | Mô tả                                                                           |
| ------------------ | ----------------- | -------- | ------------------------------------------------------------------------------- |
| timestamp          | string (ISO 8601) | ✅       | Thời điểm event xảy ra (UTC/Zulu). Default time field trên Kibana.              |
| observed_timestamp | string (ISO 8601) | ⚡       | Thời điểm collection system ghi nhận. Dùng detect clock skew / ingestion delay. |
| schema_version     | string            | ✅       | Version logging schema. Hiện tại: `"1.0"`.                                      |
| log_type           | string            | ⚡       | Loại log: `access`, `application`, `audit`, `system`.                           |
| log_level          | string            | ✅       | Mức độ: `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `FATAL`.                     |
| log_level_number   | integer           | ⚡       | Severity số theo OTel. Dùng filter/compare bằng số.                             |
| log_logger         | string            | ⚡       | Tên logger/class phát log. Ví dụ: `com.scex.order.OrderService`.                |
| event_name         | string            | ⚡       | Tên business event (dot notation). Ví dụ: `order.created`.                      |
| message            | string            | ✅       | Nội dung human-readable của log.                                                |

### 2. Trace Context Fields (bắt buộc — ứng dụng phải log mục này)

| Field       | Type                  | Required | Mô tả                                                               |
| ----------- | --------------------- | -------- | ------------------------------------------------------------------- |
| trace_id    | string (32 hex chars) | ✅       | W3C Trace ID. Liên kết logs/spans trong cùng 1 distributed request. |
| span_id     | string (16 hex chars) | ⚡       | W3C Span ID. Identify operation trong trace.                        |
| trace_flags | string (2 hex chars)  | ⚡       | W3C Trace Flags. `"01"` = SAMPLED.                                  |

### 3. Service Fields (bắt buộc — ứng dụng phải log mục này)

| Field               | Type   | Required | Mô tả                                                |
| ------------------- | ------ | -------- | ---------------------------------------------------- |
| service_name        | string | ✅       | Tên microservice. Ví dụ: `scex-order-service`.       |
| service_version     | string | ⚡       | Version đang chạy. Ví dụ: `1.2.0`.                   |
| service_environment | string | ⚡       | Environment: `development`, `staging`, `production`. |
| service_node_name   | string | ⚡       | Instance/pod name cụ thể.                            |

### 4. Process Fields (tùy chọn ứng dụng nên ghi)

| Field               | Type    | Required | Mô tả                                                            |
| ------------------- | ------- | -------- | ---------------------------------------------------------------- |
| process_pid         | integer | ⚡       | Process ID. Trong K8s thường là 1.                               |
| process_thread_name | string  | ⚡       | Tên thread đang xử lý. Optional với Go (goroutine không có tên). |

### 5. Kubernetes Fields (Agent K8s sẽ inject, ứng dụng chưa cần ghi)

| Field              | Type   | Required | Mô tả                                            |
| ------------------ | ------ | -------- | ------------------------------------------------ |
| k8s_namespace      | string | ⚡       | Kubernetes namespace. Ví dụ: `scex-prod`.        |
| k8s_pod_name       | string | ⚡       | Tên pod đang chạy.                               |
| k8s_container_name | string | ⚡       | Tên container trong pod. Hữu ích khi có sidecar. |
| k8s_node_name      | string | ⚡       | Tên K8s worker node.                             |

### 6. Host Fields (Agent K8s sẽ inject, ứng dụng chưa cần ghi)

| Field     | Type        | Required | Mô tả                                   |
| --------- | ----------- | -------- | --------------------------------------- |
| host_name | string      | ⚡       | Hostname của machine/node chạy service. |
| host_ip   | string (IP) | ⚡       | IP address của host/node.               |

### 7. HTTP Fields (conditional — chỉ có khi log từ HTTP request context)

| Field                     | Type          | Required | Mô tả                                                                     |
| ------------------------- | ------------- | -------- | ------------------------------------------------------------------------- |
| http_request_id           | string (UUID) | ⚡       | Unique request ID do server generate. Nên trả về header `X-Request-Id`.   |
| http_request_method       | string        | ⚡       | HTTP method: `GET`, `POST`, `PUT`, `DELETE`, `PATCH`. Unknown → `_OTHER`. |
| http_request_path         | string        | ⚡       | URI path. Ví dụ: `/api/v1/orders`.                                        |
| http_request_query        | string        | ⚡       | Query string (không gồm `?`). **Phải scrub** token/password.                  |
| http_response_status_code | integer       | ⚡       | HTTP status code. Ví dụ: `200`, `400`, `500`.                             |
| http_route                | string        | ⚡       | Route template (low cardinality). Ví dụ: `/api/v1/orders/{id}`.           |
| http_request_duration_ms  | long          | ⚡       | Thời gian xử lý request (ms).                                             |
| client_address            | string        | ⚡       | IP client (sau proxy, từ `X-Forwarded-For`).                              |
| user_agent_original       | string        | ⚡       | Giá trị header `User-Agent`.                                              |

### 8. Error Fields (conditional — chỉ có khi có exception/error)

| Field             | Type   | Required | Mô tả                                                 |
| ----------------- | ------ | -------- | ----------------------------------------------------- |
| error_type        | string | ⚡       | Fully qualified exception class name.                 |
| error_message     | string | ⚡       | Message của exception.                                |
| error_stack_trace | string | ⚡       | Full stack trace. Có thể rất dài — cân nhắc truncate. |

### 9. SCEX Business Fields (conditional — thêm vào từ application code khi có context)

| Field               | Type   | Required | Mô tả                                                                              |
| ------------------- | ------ | -------- | ---------------------------------------------------------------------------------- |
| scex_order_id       | string | ⚡       | Order Id của hệ thống                                                              |
| scex_user_id        | string | ⚡       | ID người dùng. **Không log PII** (tên, email, SĐT).                                    |
| scex_correlation_id | string | ⚡       | Correlation ID do client generate. Business-level ID, tồn tại qua retry/new trace. |
