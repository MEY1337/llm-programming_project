# P1~P2 기존 저장소 업로드 준비

확인 날짜: 2026-10-27(Asia/Seoul). 개인의 최종 commit·push 승인은 아직 받지 않았다. 이 문서는 기존 저장소로의 문서 추가 계획이며 새 저장소 생성·앱 구현·P3~P5 작업을 포함하지 않는다.

## 확인 결과

- 기존 origin(fetch/push): https://github.com/MEY1337/llm-programming_project.git
- 로컬 브랜치: main, 추적 브랜치: origin/main.
- 로컬 HEAD·origin/main·GitHub에서 읽은 main: 모두 `1eb662ed915fbd67d87d890f8b86aee4d583eb09`.
- 로컬/저장된 원격 사이 ahead/behind: 0/0. GitHub의 실제 main도 `ls-remote --heads origin main`으로 동일함을 확인했다. 같은 커밋이므로 비교를 위해 fetch·merge·pull할 필요가 없었다.
- 기존 이력: `a887cb4` Initial commit → `de45e2a` 분석 보고서 추가 → `1eb662e` 제안서 추가. 이력을 유지하고 그 뒤에 문서 커밋 하나를 추가할 계획이다.
- 기존 추적 파일은 `AI PDF RAG Agent`, `LICENSE`, 확장자 없는 `PDF 기반 AI 학습 도우미 — RAG 기반 질의응답·요약·문서 비교 시스템` 세 개다. HEAD와 작업 파일 diff는 없으며 보존한다.
- 최초 확인 시 문서 12개가 미추적 상태였고 스테이징·충돌·추적 파일 수정·삭제는 없었다. 이 준비 기록을 추가한 최종 후보는 아래 13개다.
- PATH에는 Git이 없지만 Codex 번들 Git `2.53.0.windows.3`을 사용할 수 있다. 앱 실행·Ruff·pytest 구성 여부와 별개다.

## 업로드 후보와 변경 내용

원격 main에 아래 경로가 없으므로 전부 **추가(A)** 대상이다. 기존 원격 파일 수정·삭제 대상은 없다.

| 경로 | 내용 |
|---|---|
| AGENTS.md | 작업 규칙·수정 경계·문서 연결 |
| README.md | 소개·준비·미구성 명령 상태·문서 탐색 |
| docs/project-brief.md | 제안서 근거·범위·현재 상태·확정 결정 요약; 이번에 Git 확인 상태 보완 |
| docs/open-questions.md | 초기 질문 5개와 A안 채택 이력 |
| docs/workflow.md | 한 CLI의 작성자·검토자 순차 수행과 개인의 책임 |
| docs/testing.md | 향후 검증 전략과 P2 문서 검사 기록 |
| docs/decisions/README.md | 중요 결정 확인 기준과 ADR 목록 |
| docs/decisions/0001-toolchain.md | 도구·기술 기준 |
| docs/decisions/0002-sample-data.md | 비민감 자체 PDF 정책 |
| docs/decisions/0003-external-transfer.md | 외부 전송 허용 범위 |
| docs/decisions/0004-api-budget-keys.md | 0원 예산·키 연결 보류 |
| docs/decisions/0005-local-retention.md | 로컬 보관·삭제 정책 |
| docs/upload-plan.md | 이번 비교·업로드 대상·승인 후 명령 |

## 제외·민감정보 확인

업로드는 위 파일의 명시 목록으로 제한한다. `git add .`나 폴더 전체 추가는 사용하지 않는다. .env·키·비밀번호·임시 파일·실데이터는 대상에 포함하지 않는다.
후보 문서 내용을 검토하고 키/토큰·개인키·비밀번호 할당 패턴, 이메일·전화번호, 원문 작성자의 이름·학번 패턴을 검색했다. 해당 값은 발견되지 않았다. 문서의 ‘이름·학번’ 같은 항목 설명과 공개 저장소 식별자는 실제 개인 연락처·학번 값과 구분했다. 패턴 검사는 모든 비밀의 부재를 보증하지 않으므로 승인 직전 staged diff도 검토한다.
기존 원문에는 작성자 개인정보가 있다. 이번에는 해당 파일을 수정·삭제·재스테이징하지 않으며 기존 GitHub 파일과 이력도 그대로 둔다. 현재 추적 여부와 무관한 전체 비밀·이력 정리는 이번 범위가 아니다.

## 승인 이후 실행할 PowerShell 명령

**현재 실행하지 않은 명령이다.** 개인이 위 13개와 커밋·main push를 최종 승인한 뒤에만 실행한다. 각 단계 결과를 보고 다음 단계로 진행한다. 명령 실패나 예상 밖 파일·커밋·원격 변화가 있으면 즉시 중단하고 개인에게 보고한다. 임의 merge/rebase/pull, force push, reset, 원격 파일 덮어쓰기는 하지 않는다.

### 1. 상태·최신 원격 재확인

```powershell
Set-Location -LiteralPath 'C:\Users\pc\llm-programming_project'
$taskGit = 'C:\Users\pc\.cache\codex-runtimes\codex-primary-runtime\dependencies\native\git\cmd\git.exe'
& $taskGit status --short --branch --untracked-files=all
& $taskGit remote -v
& $taskGit branch --show-current
& $taskGit rev-parse HEAD
& $taskGit ls-remote --heads origin main
& $taskGit diff --cached --name-status
& $taskGit diff --name-only --diff-filter=U
```

main 브랜치·origin URL·HEAD가 위 기록과 같은지 확인한다. 원격 main이 달라졌거나 스테이징된 파일·충돌이 있으면 멈춘다. 새 원격 상태 비교·충돌 해결은 개인에게 먼저 알린다. 최신 상태 확인은 네트워크 권한이 필요할 수 있다.

### 2. 승인 대상만 스테이징·검토

```powershell
$taskFiles = @(
  'AGENTS.md'
  'README.md'
  'docs/project-brief.md'
  'docs/open-questions.md'
  'docs/workflow.md'
  'docs/testing.md'
  'docs/decisions/README.md'
  'docs/decisions/0001-toolchain.md'
  'docs/decisions/0002-sample-data.md'
  'docs/decisions/0003-external-transfer.md'
  'docs/decisions/0004-api-budget-keys.md'
  'docs/decisions/0005-local-retention.md'
  'docs/upload-plan.md'
)
& $taskGit add -- $taskFiles
& $taskGit -c core.quotePath=false diff --cached --name-status
& $taskGit diff --cached --check
& $taskGit diff --cached --stat
& $taskGit -c core.quotePath=false diff --cached
```

staged 목록이 정확히 위 13개의 A인지 확인한다. 기존 파일 수정·삭제, 민감정보, 검사 실패가 있으면 commit하지 않는다. 제외 정책은 명시 목록과 검토로 적용하며 .gitignore를 새로 구성한 것은 아니다.

### 3. 승인된 commit·일반 push

```powershell
& $taskGit commit -m 'docs: add P1-P2 project harness'
& $taskGit show --stat --oneline HEAD
& $taskGit status --short --branch
& $taskGit ls-remote --heads origin main
```

커밋 성공과 변경 파일을 확인한다. 원격 main이 사전 확인 커밋에서 바뀌었으면 push하지 않고 보고한다. 작성자 설정·인증이 없으면 임의 계정 설정 없이 개인에게 필요한 조치를 알린다.

```powershell
& $taskGit push origin main
& $taskGit ls-remote --heads origin main
& $taskGit status --short --branch
```

일반 push만 사용한다. 원격 경합·인증·보호 규칙으로 거부되면 우회·강제 재시도하지 않는다. 성공 후 로컬 HEAD와 원격 main의 일치, commit ID, 추가 파일을 보고한다. push 승인은 P3~P5나 기능 구현 승인이 아니다.

## 현재 실행 상태

로컬·원격 읽기와 문서 준비만 수행했다. add·commit·push·원격 설정 변경·이력 변경은 하지 않았다. 최종 승인을 기다린다.
