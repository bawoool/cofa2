# RUNBOOK (사람용) — 10:00 시작 기준

## 0. 시작 직후 (5분)
```bash
git clone https://github.com/bawoool/cofa2.git ~/.harness && bash ~/.harness/setup.sh
cat ~/.codex/AGENTS.md
codex
```
- 업데이트 창 → **Skip until next version** (Codex 업데이트 금지)
- `/model` → GPT-6.1-Sol (Astra 선택 금지)
- `/status` → Agents.md 줄에 두 파일 / F2로 warning 한 번 확인
- 에디터에서 `.harness/RUNBOOK.md`(이 파일), `.harness/PROMPTS.md`를 열어 둔다
- **PROMPTS.md**: 단계별 Codex 지시문 12개 + "막혔을 때" 표. 복사해서 `( )`만 채워 붙여넣는다.

**탭 구성**: 탭1 Codex, 탭2 서버(내가 띄움, 하나만)
```bash
# --user 설치 시 uvicorn이 PATH에 없을 수 있음 → python3 -m uvicorn
python3 -m uvicorn main:app --host 0.0.0.0 --port 8000 --reload 2>&1 | tee server.log   # FastAPI (uvicorn 없으면 설치)
node --watch server.js 2>&1 | tee server.log                                 # Node (코드에서 0.0.0.0·8000)
python3 -m http.server 8000 --bind 0.0.0.0                                   # 정적 파일만
```
**pip 설치 3단계** (막히면 다음 단계)
```bash
# --system-site-packages가 없으면 미리 설치된 fastapi·pandas가 venv에서 안 보인다
pip install uvicorn httpx                                    # 1차
python3 -m venv --system-site-packages .venv && . .venv/bin/activate && pip install uvicorn httpx   # 2차: 기존 fastapi·pandas 유지
rm -rf .venv                                                 # 2차 실패 시 남은 잔해 제거
pip install --user --break-system-packages uvicorn httpx     # 3차: venv 생성 실패 시
```
- FastAPI 버전에 따라 TestClient가 `httpx2`를 요구할 수 있다. 에러 메시지가 안내하는 패키지를 설치한다.
- 주의: venv면 서버 탭에서 `. .venv/bin/activate` 후 서버 실행. 테스트도 activate 후(또는 `.venv/bin/python -m pytest`).

**서버 재시작**
- 자동(--reload/--watch): 백엔드 코드 수정. 프론트(HTML·CSS·JS)는 미리보기 새로고침만.
- 수동: 패키지 설치 후, .env·설정 파일 변경, 시작할 때 읽는 데이터 변경, 서버가 죽었을 때, 동작이 이상할 때.
- 방법: 서버 탭에서 Ctrl+C → ↑ Enter. 헷갈리면 그냥 재시작.

**첫 25분 (분 단위)**
| 시각 | 할 일 | 어디서 |
|---|---|---|
| 10:00 | clone + setup.sh (위 명령) | 탭1 |
| 10:01 | `cat ~/.codex/AGENTS.md` 훑기 / 에디터에 README·RUNBOOK·PROMPTS 열기 | 탭1·에디터 |
| 10:02 | **README 직접 정독** (Codex보다 내가 먼저) | 에디터 |
| 10:06 | `codex` → 업데이트 Skip → `/model` Sol 6.1 → `/status` | 탭1 |
| 10:07 | **PROMPTS 1** 붙여넣기 → 응답 기다리는 동안 **실무자 인터뷰 시작** | Codex·메신저 |
| 10:15 | Codex가 뽑은 "실무자에게 물을 질문" 중 빠진 것 추가 질문 | 메신저 |
| 10:18 | 실무자 답변 붙여넣기 → **PROMPTS 2** | Codex |
| 10:20 | 합의 확인(인터뷰 9번) → **PROMPTS 3·4** | 메신저·Codex |
| 10:23 | 스택 결정. FastAPI면 탭2에서 바로 pip 설치 | 탭2 |
| 10:25 | 첫 완주 시작 (**PROMPTS 7**) | Codex |

- Codex 응답 대기 시간에는 인터뷰나 README 재확인을 한다. 놀지 않는다.
- 첫 25분은 코드를 쓰지 않는다 (PROMPTS 1~4 모두 코드 금지).

## 1. 시간표
| 시각 | 경과 | 할 일 | 끝 조건 |
|---|---|---|---|
| 10:00 | 0 | 준비 명령, README 정독, PROMPTS 1 | 무엇을 하는 과제인지 한 문장 |
| 10:05 | 5 | 실무자 인터뷰 (아래 질문) → PROMPTS 2 | NOTES 1절 채움 |
| 10:20 | 20 | 합의문 실무자 확인, PROMPTS 3·4 | 성공 기준 숫자 확정, 커밋 |
| 10:25 | 25 | **첫 완주**: 가장 작은 end-to-end | 하나가 실제로 돈다, 커밋 |
| ~11:00 | 60 | **체크①** + **보안 리뷰①**(PROMPTS 9, 기준=준비 커밋) → 핵심 기능 + 테스트, 단위마다 커밋 | 문제 목록 처리, 테스트 통과 개수 |
| ~12:10 | 130 | 실패 경로·경계값 + **보안 리뷰②** | 문제 목록 처리 |
| 12:25 | 145 | **기능 동결** + **체크②**. 이후 검증·문서만 | FAIL 0 |
| 12:30 | 150 | "제출 준비"(**체크③**) | check FAIL 0, NOTES 7·8절 |
| 12:40 | 160 | PROMPTS 11, NOTES 4·6·7절 소리 내어 읽기 | 서술형 대비 |
| 12:50 | 170 | **제출 시작** → 미리보기 → 확정 | 확정 후 수정 불가 |
| ~13:40 | — | 대기 5분 → 서술형 6문항 × 5분 | 14:00 전 종료 |

체크 = Codex에 "체크" (또는 `bash ~/.harness/check.sh`). 늦게 시작하면 14:00에서 거꾸로 계산해 압축한다.

## 2. 실무자 인터뷰 (얕게 → 깊게, 하나씩)
실무자는 무엇을 만들지 말해 주지 않는다. 문제만 말한다. "할 일 1·2·3으로 정리해 달라"는 막혀 있다. Codex는 메신저를 못 본다 → 답변은 내가 붙여넣는다.
1. 요즘 업무에서 가장 시간이 많이 드는 일은 뭔가요?
2. 그 일은 언제, 얼마나 자주 생기나요? (하루 몇 건)
3. 지금은 어떤 순서로 처리하세요? 어떤 도구를 쓰세요?
4. 그중 가장 답답한 단계는 어디인가요? 왜 그런가요?
5. 거기서 실수가 나면 무슨 일이 생기나요?
6. 데이터는 어디에 있고 어떤 형태인가요? 민감한 정보가 있나요?
7. 이게 해결되면 무엇이 얼마나 달라져야 성공인가요? (숫자로)
8. 이번에 꼭 필요하지 않은 건 뭔가요?
9. (마지막) 제가 이해한 문제는 ○○이고, 성공 기준은 ○○입니다. 맞나요?

## 3. 원칙
- 같은 문제에서 AI가 **2번 틀리면** 그 대화는 접고 직접 확인하거나 새 세션으로 간다.
- AI가 틀렸거나 내가 고친 건 **바로** NOTES 4절에 한 줄 (서술형 2번 유형 답 재료).
- "상태" / "리뷰" / "체크" / "인계" / "제출 준비" 한 단어로 부른다 (AGENTS.md 8절).
- 대화가 길어지면 "인계" → `/quit` → 새 `codex` → PROMPTS 12.

## 4. 서술형 답 틀 (AI·복붙 불가, 5분)
- 핵심 동작: **무엇을** 한다 → **어떻게 확인**했다 (명령·테스트·결과 숫자) → 남은 한계.
- AI 수정: AI가 ○○를 제안 → ○○ 때문에 의심 → ○○로 확인 → ○○로 고침 → 결과.
- 약점 질문: 내 코드의 약점 → 그 위험의 파급 → 검증·개선 계획.
- 질문의 전제가 실제 구현과 다르면 "실제로는 ○○로 구현했다"고 먼저 밝힌다.

## 5. 하지 않는 것
Claude Code·Astra·Codex 업데이트·승인 우회 옵션·플랫폼 파일 수정·1024 미만 포트·sudo·git push·비밀값 커밋.
