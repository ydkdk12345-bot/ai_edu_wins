# uv 명령어 레퍼런스

`uv`는 Rust로 만든 빠른 Python 패키지·프로젝트 관리 도구입니다. `pip`, `venv`, `pyenv`, `pipx`, `poetry`가 하던 일을 하나로 처리합니다.

> 정확한 옵션은 `uv help <명령>` 또는 `uv <명령> --help`로 확인하세요.

---

## 1. 설치 · 업데이트

| 명령 | 설명 |
|---|---|
| `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 \| iex"` | Windows에 설치 |
| `curl -LsSf https://astral.sh/uv/install.sh \| sh` | macOS / Linux에 설치 |
| `pip install uv` | pip로 설치 |
| `uv --version` | 설치된 버전 확인 |
| `uv self update` | uv를 최신 버전으로 업데이트 (설치 스크립트로 설치한 경우) |

---

## 2. 프로젝트 만들기

| 명령 | 설명 |
|---|---|
| `uv init` | 현재 폴더를 uv 프로젝트로 초기화 |
| `uv init 프로젝트명` | 새 폴더를 만들고 프로젝트 초기화 |
| `uv init --lib 이름` | 라이브러리용 구조(`src/`)로 생성 |
| `uv init --package 이름` | 패키지(배포 가능) 구조로 생성 |
| `uv init --python 3.12` | Python 버전을 지정해 초기화 |

`uv init`이 만드는 파일은 다음과 같습니다.

```
pyproject.toml     # 프로젝트 정보, 의존성 목록
.python-version    # 이 프로젝트에서 쓸 Python 버전
README.md
.gitignore
main.py (또는 src/)
```

---

## 3. 의존성(패키지) 관리

| 명령 | 설명 |
|---|---|
| `uv add 패키지` | 패키지를 설치하고 `pyproject.toml`에 추가 |
| `uv add 패키지==1.2.3` | 버전을 지정해 추가 |
| `uv add "패키지>=1.0"` | 버전 범위를 지정해 추가 |
| `uv add --dev pytest` | 개발용 의존성으로 추가 |
| `uv add -r requirements.txt` | requirements.txt의 패키지를 한 번에 추가 |
| `uv remove 패키지` | 패키지 제거 (`pyproject.toml`에서도 삭제) |
| `uv sync` | `pyproject.toml`·`uv.lock` 기준으로 `.venv`를 맞춤 (협업 시 첫 명령) |
| `uv lock` | `uv.lock` 잠금 파일만 갱신 |
| `uv lock --upgrade` | 모든 패키지를 허용 범위 안에서 최신으로 갱신 |
| `uv lock --upgrade-package 패키지` | 특정 패키지만 갱신 |
| `uv tree` | 의존성 트리 보기 |

> 다른 사람의 프로젝트를 받았다면 `uv sync` 한 번으로 가상환경과 패키지가 모두 준비됩니다.

---

## 4. 실행

| 명령 | 설명 |
|---|---|
| `uv run main.py` | 프로젝트 가상환경으로 스크립트 실행 (activate 불필요) |
| `uv run python` | 가상환경의 Python 대화형 셸 실행 |
| `uv run pytest` | 가상환경에 설치된 도구 실행 |
| `uv run --with 패키지 script.py` | 일회성으로 패키지를 추가해 실행 |
| `uv run --python 3.11 script.py` | 특정 Python 버전으로 실행 |

> `uv run`은 실행 전에 환경을 자동으로 동기화하므로, 가상환경을 activate하지 않아도 됩니다.

---

## 5. 가상환경

| 명령 | 설명 |
|---|---|
| `uv venv` | 현재 폴더에 `.venv` 생성 |
| `uv venv 이름` | 지정한 이름의 폴더로 가상환경 생성 |
| `uv venv --python 3.12` | Python 버전을 지정해 생성 |
| `uv venv --prompt temp` | 프롬프트에 표시될 이름 지정 → `(temp) PS ...` |

가상환경 활성화 / 비활성화:

```powershell
# Windows PowerShell
.venv\Scripts\activate

# Windows cmd
.venv\Scripts\activate.bat

# macOS / Linux
source .venv/bin/activate

# 끄기 (공통)
deactivate
```

> PowerShell에서 "스크립트를 실행할 수 없으므로" 오류가 나면 한 번만 실행:
> `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`

---

## 6. Python 버전 관리

| 명령 | 설명 |
|---|---|
| `uv python list` | 설치 가능한 버전과 설치된 버전 목록 |
| `uv python install 3.12` | Python 3.12 설치 |
| `uv python install 3.11 3.12` | 여러 버전 동시 설치 |
| `uv python uninstall 3.11` | 설치된 버전 삭제 |
| `uv python pin 3.12` | 현재 프로젝트 버전 고정 (`.python-version` 생성·수정) |
| `uv python find` | 현재 사용될 Python 경로 확인 |

---

## 7. pip 호환 명령 (`uv pip`)

기존 `pip` 사용법과 거의 같고 속도만 빠릅니다. `pyproject.toml`에는 기록되지 않습니다.

| 명령 | 설명 |
|---|---|
| `uv pip install 패키지` | 현재 가상환경에 설치 |
| `uv pip install -r requirements.txt` | requirements.txt로 설치 |
| `uv pip install -e .` | 현재 프로젝트를 편집 모드로 설치 |
| `uv pip uninstall 패키지` | 삭제 |
| `uv pip list` | 설치된 패키지 목록 |
| `uv pip show 패키지` | 패키지 정보 |
| `uv pip freeze` | 설치 목록을 requirements 형식으로 출력 |
| `uv pip freeze > requirements.txt` | requirements.txt 파일로 저장 |
| `uv pip compile requirements.in -o requirements.txt` | 버전이 고정된 requirements 생성 |
| `uv pip sync requirements.txt` | 파일과 정확히 같도록 환경 맞춤 (나머지는 삭제) |

> 프로젝트 방식(`uv add`)과 pip 방식(`uv pip install`)을 섞어 쓰면 `pyproject.toml`에 기록이 남지 않으니, 수업 프로젝트에서는 `uv add`를 권장합니다.

---

## 8. 도구(CLI 프로그램) 실행 · 설치

| 명령 | 설명 |
|---|---|
| `uvx 도구` | 설치 없이 도구를 임시 실행 (`uv tool run`의 줄임말) |
| `uvx ruff check .` | 예: ruff로 코드 검사 |
| `uv tool install 도구` | 도구를 전역으로 설치 (예: `ruff`, `black`) |
| `uv tool list` | 설치된 도구 목록 |
| `uv tool upgrade 도구` | 도구 업데이트 |
| `uv tool upgrade --all` | 모든 도구 업데이트 |
| `uv tool uninstall 도구` | 도구 삭제 |

---

## 9. 빌드 · 배포

| 명령 | 설명 |
|---|---|
| `uv build` | `dist/`에 wheel, sdist 생성 |
| `uv publish` | PyPI에 업로드 |
| `uv version` | 프로젝트 버전 확인 |
| `uv version --bump minor` | 버전 올리기 (`major` / `minor` / `patch`) |

---

## 10. 캐시 · 기타

| 명령 | 설명 |
|---|---|
| `uv cache dir` | 캐시 폴더 위치 확인 |
| `uv cache clean` | 캐시 전체 삭제 |
| `uv cache prune` | 사용하지 않는 캐시만 정리 |
| `uv export --format requirements-txt > requirements.txt` | `uv.lock`을 requirements.txt로 내보내기 |
| `uv help` | 전체 도움말 |

---

## 자주 쓰는 흐름

**새 프로젝트 시작**
```powershell
uv init my_project
cd my_project
uv add requests pandas
uv run main.py
```

**다른 사람 프로젝트 받아서 실행**
```powershell
git clone <저장소 주소>
cd <폴더>
uv sync
uv run main.py
```

**기존 requirements.txt 프로젝트를 uv로 옮기기**
```powershell
uv init
uv add -r requirements.txt
```

---

## pip / venv와 비교

| 하고 싶은 일 | 기존 방식 | uv 방식 |
|---|---|---|
| 가상환경 만들기 | `python -m venv .venv` | `uv venv` |
| 패키지 설치 | `pip install 패키지` | `uv add 패키지` |
| 패키지 삭제 | `pip uninstall 패키지` | `uv remove 패키지` |
| 의존성 저장 | `pip freeze > requirements.txt` | 자동 (`pyproject.toml`, `uv.lock`) |
| 의존성 복원 | `pip install -r requirements.txt` | `uv sync` |
| 스크립트 실행 | activate 후 `python main.py` | `uv run main.py` |
| Python 설치 | python.org에서 직접 설치 | `uv python install 3.12` |
