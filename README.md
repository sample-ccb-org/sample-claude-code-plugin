# our-team-plugins 플러그인 마켓플레이스

팀의 운영 지식을 Claude Code 플러그인으로 공유하는 마켓플레이스입니다.

## 설치 (팀원은 이 두 줄)

```
/plugin marketplace add sample-ccb-org/sample-claude-code-plugin
/plugin install log-analyzer@our-team-plugins
```

> `@our-team-plugins`는 이 레포 `.claude-plugin/marketplace.json`의 `name` 필드(마켓플레이스
> 식별자)입니다 — 레포 이름과 달라도 됩니다.

## 플러그인

| 이름 | 설명 |
|---|---|
| `log-analyzer` | 운영 로그 분석 — 방법은 `skills/`, 우리 시스템의 지식은 `references/`에 |

## 기여하기

장애를 겪었거나 위험 패턴을 알게 됐다면 — 코드가 아니라 **마크다운 한 장**이면 됩니다.

1. `log-analyzer/references/` 아래에 패턴/이력/runbook 문서 추가
2. `log-analyzer/.claude-plugin/plugin.json`의 `version` 올리기 (예: 1.0.0 → 1.1.0)
3. PR → 리뷰 → merge
4. 팀원들은 `/plugin update log-analyzer@our-team-plugins` 한 번이면 다음 분석부터 반영
