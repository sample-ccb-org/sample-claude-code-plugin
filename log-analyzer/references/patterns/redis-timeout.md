# 패턴: Redis 커넥션/명령 타임아웃 (redis-timeout)

## 판별 기준

다음이 5분 윈도 안에 함께 보이면 **Redis 타임아웃 진행 중**으로 판정한다:

- `redis command timeout` 또는 `redis connection timeout` 에러가 2회 이상 반복
- redis pool `active`가 정원의 90% 이상 (예: 48/50 이상) **또는** `wait_queue`(대기 큐)가 0보다 큼
- upstream(API gateway 등)에서 `504`/`502`가 cache-service 경유로 동반 발생

> 주의: DB 커넥션 풀 고갈(`db-connection-spike`)과 증상이 비슷해 보이지만 **자원이 다르다**.
> 로그 식별자가 `redis`/`cache-service`면 이 패턴, `db connection`/`pool_in_use`면 DB 패턴이다.
> 자원을 혼동하면 권고 조치(`db.pool.max_size` vs redis 타임아웃 상향)가 어긋난다.

## 심각도

| 조건 | severity |
|---|---|
| pool 90% 미만, timeout 0회 | none |
| pool 90~95%, timeout 1회, wait_queue 0 | medium |
| **pool 95%+ 또는 timeout 반복 + wait_queue > 0 + upstream 504** | **high — 즉시 Slack 알림** |

## 영향 범위

이 시스템에서 redis는 session 캐시와 rate-limit 카운터를 담당한다.
cache-service 타임아웃 시 api-gateway가 504로 폴백하며 **로그인/세션 조회가 광범위하게 실패**한다.
DB 풀 고갈이 결제로 전파되는 것과 달리, redis 타임아웃은 **인증/세션 경로로 전파**된다.

## 주의

- 단일 키의 큰 값(big key) 조회 한 건으로 인한 일시 지연은 오탐이기 쉬움 — timeout 반복·wait_queue 동반 여부로 구분.
- `redis command timeout`만 보이고 pool은 여유로우면 슬로우 명령(KEYS, 대용량 SCAN) 의심 — runbook 참고.
