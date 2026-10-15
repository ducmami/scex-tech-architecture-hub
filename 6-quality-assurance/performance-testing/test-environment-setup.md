---
title: Cấu hình môi trường trước khi performance testing
allowTitleToDifferFromFilename: true
confluence_url: https://confluence.scex.com.vn/spaces/SDO/pages/10560248/C%E1%BA%A5u+h%C3%ACnh+m%C3%B4i+tr%C6%B0%E1%BB%9Dng+tr%C6%B0%E1%BB%9Bc+khi+performance+testing
confluence_id: "10560248"
confluence_space: SDO
confluence_status: publish
---

# Cấu hình môi trường trước khi performance testing

Tài liệu ghi các thay đổi bắt buộc trên môi trường trước khi chạy load test. Mục đích: cô lập hệ thống khỏi vendor thật (Incom, FPT eContract, FIS eID, Blitz), tắt OTP Slack, và bật chế độ loadtest trên SCEX Core

**Nguyên tắc:** backup giá trị ENV cũ trước khi đổi. Sau khi load test xong, restore đúng giá trị cũ.

Mock endpoint dùng chung:

```text
http://mock-server.lpex-be.svc.cluster.local:80
```

---

## 1. Tắt setting trong admin config

| Setting                                             | Giá trị load test |
| --------------------------------------------------- | ----------------- |
| `send_otp_via_slack_enabled`                        | `false`           |
| `trade_verification_configs.slippage_tolerance_bps` | `9900`            |

---

## 2. ENV `lpex-be` — trỏ sang mock server

Backup giá trị cũ trước khi đổi. Restore sau khi load test xong.

| Biến ENV                  | ENV mới (load test)                               | ENV cũ (backup / restore)       |
| ------------------------- | ------------------------------------------------- | ------------------------------- |
| `INCOM_API_URL`           | `http://mock-server.lpex-be.svc.cluster.local:80` | `https://omni.incom.vn/api`     |
| `FPT_E_CONTRACT_BASE_URL` | `http://mock-server.lpex-be.svc.cluster.local:80` | `https://econtract.fpt.com/app` |

---

## 3. ENV `lpex-verify-portal` — trỏ sang mock server

Backup giá trị cũ trước khi đổi. Restore sau khi load test xong.

| Biến ENV                  | ENV mới (load test)                               | ENV cũ (backup / restore)   |
| ------------------------- | ------------------------------------------------- | --------------------------- |
| `FIS_EID_ENDPOINT`        | `http://mock-server.lpex-be.svc.cluster.local:80` | `https://eid.fis.fpt.com`   |
| `BLITZ_API_BASE_URL`      | `http://mock-server.lpex-be.svc.cluster.local:80` | `https://192.168.2.201/api` |
| `BLITZ_KEYCLOAK_BASE_URL` | `http://mock-server.lpex-be.svc.cluster.local:80` | `https://192.168.2.201`     |

---

## 4. Bật chế độ loadtest cho `lpex-be`

| Biến ENV                   | ENV mới (load test) |
| -------------------------- | ------------------- |
| `SERVER_TEST_MODE`         | `loadtest`          |
| `ALLOW_SKIP_CAPTCHA`       | `true`              |
| `LOADTEST_WORKERS_ENABLED` | `true`              |

`SERVER_TEST_MODE=loadtest` bật chế độ load test trên `lpex-be`. `ALLOW_SKIP_CAPTCHA=true` bỏ captcha để kịch bản tự động không bị chặn.

---

## 5. Bật loadtest cho `lpex-trading-core`

| Biến ENV            | ENV mới (load test) | ENV cũ (backup / restore) |
| ------------------- | ------------------- | ------------------------- |
| `LOAD_TEST_ENABLED` | `true`              | `false`                   |

## 6. Restore sau khi load test

**Seeding data trước khi test**

- Chuẩn bị user
- Chuẩn bị số dư
- Chuẩn bị order book

## 7. Restore sau khi load test

Hoàn tất load test xong, trả môi trường về giá trị cũ:

1. Restore ENV `lpex-be`:
   - `INCOM_API_URL` → `https://omni.incom.vn/api`
   - `FPT_E_CONTRACT_BASE_URL` → `https://econtract.fpt.com/app`
2. Restore ENV `lpex-verify-portal`:
   - `FIS_EID_ENDPOINT` → `https://eid.fis.fpt.com`
   - `BLITZ_API_BASE_URL` → `https://192.168.2.201/api`
   - `BLITZ_KEYCLOAK_BASE_URL` → `https://192.168.2.201`
3. Tắt chế độ loadtest trên `lpex-be` (`SERVER_TEST_MODE`, `ALLOW_SKIP_CAPTCHA` về giá trị vận hành bình thường).
4. Bật lại `send_otp_via_slack_enabled` nếu môi trường đó cần OTP Slack.

Restart / rollout workload sau khi đổi ENV để pod nhận cấu hình mới.
