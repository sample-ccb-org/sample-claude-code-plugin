# 패턴: DB 커넥션 풀 고갈 (db-connection-spike)

## 판별 기준

다음이 5분 윈도 안에 함께 보이면 **풀 고갈 진행 중**으로 판정한다:

- `db connection timeout` 에러가 2회 이상 반복
- `pool_in_use`가 정원의 80% 이상 (예: 16/20 이상)
- 여러 caller(서비스)에서 동시 발생 — 단일 요청 문제가 아니라 풀 자체의 문제

## 심각도

| 조건 | severity |
|---|---|
| pool 80% 미만, timeout 0회 | none |
| pool 80~90%, timeout 1회 | medium |
| **pool 90%+ 또는 timeout 반복 + 다중 caller** | **high — 즉시 Slack 알림** |

## 영향 범위

이 시스템에서 DB 풀은 order-service → payment-service 경로가 공유한다.
풀 고갈 시 주문 처리 지연이 결제 실패로 전파된다(downstream 지연).

## 주의

- 새벽 배치(02:50~03:10)의 일시적 사용률 상승은 오탐이기 쉬움 — timeout 동반 여부로 구분.
