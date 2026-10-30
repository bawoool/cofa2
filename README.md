# cofa2

AI 해커톤 2차 예선(Cofa-Probe)용 개인 작업 하네스. 직접 작성한 지침과 점검 스크립트만 들어 있고, 과제 내용·답안·결과물 코드는 없다.

운영팀 안내에 따라 이 저장소는 **2차 예선 결과 통지 전까지 공개 상태로 유지**한다. 키·토큰 등 비밀값은 넣지 않는다.

## 구성
| 파일 | 용도 |
|---|---|
| `AGENTS.md` | Codex 작업 규칙 (토큰 절약·검증·서버·커밋·보안 리뷰·하지 않을 것, 짧은 명령) |
| `templates/NOTES.md` | 문제 정의·결정·AI 확인·수정 로그·한계·동작 설명·실행 방법·인계 틀 |
| `PROMPTS.md` | 사람이 복사해 Codex에 붙여넣는 단계별 지시문 12개 + 막혔을 때 표 |
| `RUNBOOK.md` | 사람용 당일 운영표·탭 구성·인터뷰 질문·서술형 답 틀 |
| `setup.sh` | 작업 폴더 준비: .gitignore(숨김 파일·`*.log` 제외), git 작성자, AGENTS.md·NOTES.md 배치, 첫 커밋 |
| `check.sh` | 점검 4가지: 커밋·미커밋 변경, 테스트 실제 실행, 비밀값, 3000/8000의 0.0.0.0 LISTEN |

## 사용 (과제 README가 있는 폴더에서)
```bash
git clone https://github.com/bawoool/cofa2.git ~/.harness && bash ~/.harness/setup.sh
```
- 플랫폼 설정(`~/.codex` 등)은 건드리지 않는다. 과제가 준 AGENTS.md가 있으면 덮어쓰지 않고 뒤에 덧붙인다.
- `.harness/`는 숨김 폴더라 `.gitignore`의 `.*` 규칙으로 커밋에서 빠진다.
