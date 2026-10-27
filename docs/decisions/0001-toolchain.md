# ADR-0001: 개발 도구와 기술 기준

- 상태: 확정
- 결정자: 개인(프로젝트 사용자)
- 날짜: 2026-10-27
- 근거: Q1 A; 제안서 §4.2~§4.3, 사용자 A안 채택 답변. [초기 질문 기록](../open-questions.md)

## 배경
Python 3.11 가상환경과 라이브러리는 제안서에 있지만 관리·검사 도구는 미정이었다.

## 선택지
A: uv·pyproject.toml/uv.lock·Ruff·pytest / B: venv·pip/pip-tools·Ruff·pytest / C: venv·pip 최소 목록

## 선택한 이유
재현 가능한 잠금·환경 동기화와 일관된 검사 경로를 위해 A를 채택했다.

## 영향
Python 3.11·로컬 실행을 기준으로 uv·Ruff·pytest를 사용한다. 제안서의 Streamlit·PyMuPDF·LangChain·LangGraph·ChromaDB·OpenAI SDK·python-dotenv 계획을 유지한다. gpt-4o-mini·text-embedding-3-small은 우선 검토 모델이며 최종 확정 모델이 아니다. 버전·설치·잠금·검사 명령은 미구성이다.
