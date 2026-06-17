---
name: analyze-logs
description: 운영 로그를 팀 지식 기반으로 분석한다. 로그 분석·에러 패턴 진단·장애 징후 판독 요청 시 사용.
when_to_use: 사용자가 애플리케이션/시스템 로그를 분석해 달라고 할 때, 에러·지연·이상 트래픽의 원인을 물을 때.
---

# 로그 분석 (팀 지식 기반)

운영 로그를 분석할 때 반드시 아래 절차를 따른다. **이 파일은 '방법'이고,
우리 시스템의 '지식'은 `${CLAUDE_PLUGIN_ROOT}/references/`에 있다 — 분석 전에 반드시 참조한다.**

## 절차

1. **패턴 대조** — `${CLAUDE_PLUGIN_ROOT}/references/patterns/`의 알려진 위험 패턴 문서를 읽고,
   입력 로그가 어느 패턴에 해당하는지 먼저 판별한다.
2. **이력 조회** — 패턴이 일치하면 `${CLAUDE_PLUGIN_ROOT}/references/history/`에서 유사 장애
   이력을 찾아 당시 원인·해결책·소요 시간을 분석에 인용한다.
3. **대응 절차** — `${CLAUDE_PLUGIN_ROOT}/references/runbooks/`의 해당 runbook을 근거로
   즉시 취할 조치를 권고한다.
4. **심각도 판정** — 패턴 문서의 기준으로 severity(none/low/medium/high)를 매긴다.
   기준이 없는 영역은 "팀 지식 미등록 — 확인 필요"로 표시한다.

## 출력 형식

- 감지한 패턴 이름 (없으면 "알려진 패턴 없음")
- 유사 이력 (날짜·원인·해결)
- 현재 상태 수치 (로그에서 추출)
- runbook 권고 조치
- severity + 근거
