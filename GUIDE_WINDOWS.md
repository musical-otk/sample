# Windows 시작 가이드

코딩을 한 번도 안 해봤어도 괜찮습니다. 순서대로 따라오세요.

---

## 1단계 — GitHub 계정 만들기

**GitHub**는 코드를 저장하고 인터넷에 공유할 수 있는 서비스입니다. 우리가 만드는 앱은 GitHub에 올려두면 누구나 링크로 접속할 수 있습니다. 네이버 블로그에 글을 올리는 것과 비슷하다고 생각하면 됩니다. 무료로 사용할 수 있어요.

가입할 때 정한 사용자명이 앱 주소가 됩니다. 예를 들어 사용자명이 `myname`이고 앱 이름을 `rebecca2026`으로 정하면 앱 주소는 `https://myname.github.io/rebecca2026/` 이 됩니다.

이미 계정이 있으면 건너뛰세요.

1. https://github.com 접속
2. **Sign up** 클릭
3. 이메일, 비밀번호, 사용자명 입력 후 가입

---

## 2단계 — 필요한 프로그램 설치

### Git 설치

파일을 GitHub에 올리는 데 필요한 도구입니다. **Git Bash**(터미널 대용)도 함께 설치됩니다.

1. https://git-scm.com/download/win 접속
2. 다운로드된 `.exe` 파일 실행
3. 설치 중 옵션은 모두 기본값으로 **Next** 클릭
4. 설치 완료 후 바탕화면 또는 시작 메뉴에서 **Git Bash** 실행

> 이후 이 가이드의 "터미널"은 모두 **Git Bash**를 의미합니다. Windows 기본 PowerShell이 아닙니다.

### Claude Code 설치

Claude AI와 대화하며 앱을 만드는 도구입니다. 두 가지 방법 중 하나를 선택하세요.

#### 방법 A — VS Code 확장 (추천)

VS Code는 코드를 편집하는 프로그램입니다. Claude Code가 내장 터미널에서 바로 실행됩니다.

1. https://code.visualstudio.com 에서 **Download for Windows** 클릭 후 설치
2. VS Code 실행
3. 왼쪽 아이콘 중 네모 4개 모양(Extensions) 클릭, 또는 `Ctrl+Shift+X`
4. 검색창에 `Claude Code` 입력
5. **Claude Code** (Anthropic 제작) 옆 **Install** 클릭
6. 설치 후 상단 메뉴 **Terminal → New Terminal** 클릭
7. 하단 터미널 창 오른쪽 `+` 옆 화살표 클릭 → **Git Bash** 선택
8. `claude` 입력 후 엔터

> VS Code 터미널에서 Git Bash를 기본으로 설정하려면: `Ctrl+Shift+P` → `Select Default Profile` → **Git Bash** 선택

#### 방법 B — Git Bash 직접 사용

1. https://claude.ai/code 에서 Windows용 다운로드 후 설치
2. Git Bash를 열고 `claude` 입력 후 엔터

### Claude Code 로그인

처음 `claude`를 실행하면 로그인 안내가 나옵니다.

1. Anthropic 계정이 없으면 https://claude.ai 에서 가입
2. 터미널에 나오는 안내에 따라 로그인

---

## 3단계 — 이 레포 받아오기

Git Bash(또는 VS Code 터미널)에서 아래 명령을 **한 줄씩** 입력하세요. 각 줄 입력 후 엔터를 누릅니다.

```
git clone https://github.com/musical-otk/sample.git my-musical-diary
```
```
cd my-musical-diary
```

> `cd`는 폴더로 이동하는 명령입니다. `my-musical-diary` 폴더 안으로 들어가는 거예요.

---

## 4단계 — 앱 만들기

터미널에서 Claude Code를 실행합니다.

```
claude
```

실행되면 아래 명령을 입력하세요.

```
/new-musical
```

> **처음이라 막막하다면**: `/new-musical` 대신 "나 코딩 처음인데 뮤지컬 앱 만들고 싶어" 라고 말해도 됩니다. Claude가 하나씩 안내해줍니다.

Claude가 단계별로 질문합니다. 처음 실행 시 GitHub 사용자명을 물어보는데, 1단계에서 만든 사용자명을 입력하면 됩니다. 이후엔 자동으로 기억합니다.

질문 예시:
```
뮤지컬 한글명이 무엇인가요?
→ 레베카  (입력 후 엔터)

앱 대표 이모지를 골라주세요.
→ 🌹

역할 수는 몇 개인가요?
→ 2
```

모든 질문에 답하면 앱 파일 생성과 GitHub 배포까지 자동으로 완료됩니다.

---

## 5단계 — GitHub Pages 활성화

gh CLI(GitHub 자동화 도구)가 없으면 아래 순서로 직접 활성화합니다.

1. https://github.com/{내 사용자명}/{레포명} 접속
2. 상단 **Settings** 탭 클릭
3. 왼쪽 메뉴에서 **Pages** 클릭
4. **Source** → **Deploy from a branch** 선택
5. **Branch** → **main** / 폴더 → **/ (root)** 선택 후 **Save**

1~3분 후 `https://{사용자명}.github.io/{레포명}/` 에서 앱을 확인할 수 있습니다.

---

## 막혔을 때

- 어떤 단계든 에러가 나거나 이해가 안 되면 Claude에게 그대로 붙여넣고 물어보세요.
- 예: "이런 에러가 났어요: [에러 메시지]"
