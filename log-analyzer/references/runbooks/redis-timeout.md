# Runbook: Redis 타임아웃 대응

## 즉시 (5분 내)

1. 현재 redis 상태 확인: `active`/`pool size`, `wait_queue` 길이, timeout 발생률
2. **wait_queue가 쌓이고 pool active가 95%+이면 즉시 증설** (2026-05 장애의 검증된 해결책):
   - redis 커넥션 풀 `redis.pool.max_size=100` → 설정 리로드 (재시작 불필요)
3. api-gateway 504 비율 확인 — 로그인/세션 경로 전파 여부
4. 슬로우 명령 점검: `SLOWLOG GET 10` — KEYS/대용량 SCAN이 있으면 즉시 중단·분할

## 후속 (당일)

- 트래픽 피크와 풀 정원 여유 재산정 (피크 동시연결 대비 1.5배 권장)
- redis pool `wait_queue > 0` 경고 알림이 동작했는지 점검
- big key·슬로우 명령을 코드/배치에서 제거

## 에스컬레이션

- 증설 후에도 timeout이 계속되면 redis 노드 자체 병목(CPU·네트워크·persistence fork) 의심 — 인프라팀 호출
