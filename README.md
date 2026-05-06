# 뮤지컬 관극 다이어리 Kit

Claude AI와 대화하듯 질문에 답하면 나만의 뮤지컬 관극 다이어리 앱이 자동으로 만들어집니다.

**만들어지는 앱 기능**
- 공연 일정 캘린더 (배우별 필터)
- 관람 기록 (좌석, 캐스팅, 할인권)
- 쿠폰/할인권 잔여 관리
- 재관람 스탬프 카드 (마일스톤 보상)
- 다크모드 / 홈 화면 추가(PWA)

---

## 시작하기 전에 설치할 것

### 1. Claude Code 설치

Claude Code는 Claude AI와 대화하며 코드를 만드는 도구입니다. **터미널 앱**과 **VS Code 확장** 두 가지 방법으로 사용할 수 있습니다.

#### 방법 A — VS Code 확장 (추천, 터미널이 낯설다면)

1. [VS Code](https://code.visualstudio.com) 설치
2. VS Code 실행 후 Extensions 패널 열기
   - Mac: `Cmd+Shift+X`
   - Windows: `Ctrl+Shift+X`
3. 검색창에 `Claude Code` 입력
4. **Claude Code** (Anthropic 공식) 설치
5. 설치 후 VS Code 하단 터미널 패널 열기
   - Mac: `` Ctrl+` ``
   - Windows: `` Ctrl+` ``
6. 터미널에 `claude` 입력하면 로그인 안내가 나옵니다

#### 방법 B — 터미널 앱

👉 https://claude.ai/code 에서 다운로드 후 설치

터미널 여는 법:
- Mac: `Launchpad → 기타 → 터미널`
- Windows: `시작 → Git Bash` (Git이 설치된 경우) 또는 `시작 → PowerShell`

설치 후 터미널에서 로그인:
```
claude
```
처음 실행하면 Anthropic 계정으로 로그인하라는 안내가 나옵니다.

### 2. GitHub CLI 설치 (선택)

레포 생성과 Pages 배포를 자동으로 해주는 도구입니다. 없어도 되지만 있으면 편합니다.

👉 https://cli.github.com 에서 설치

설치 후 로그인:
```
gh auth login
```

### 3. GitHub 계정

앱을 무료로 배포하려면 GitHub 계정이 필요합니다.

👉 https://github.com 에서 가입

---

## 사용 방법

### Step 1 — 이 레포 클론

터미널(또는 VS Code 터미널)에서 아래 명령을 입력합니다.

```bash
git clone https://github.com/musical-otk/sample.git my-musical-diary
cd my-musical-diary
```

> **git이 없다면**: [git-scm.com](https://git-scm.com) 에서 설치하세요. Windows는 Git Bash도 함께 설치됩니다.

### Step 2 — Claude Code 실행

```bash
claude
```

Claude Code가 실행되면 아래 명령을 입력합니다:

```
/new-musical
```

### Step 3 — 질문에 답하기

Claude가 단계별로 질문합니다. 처음 실행 시 GitHub 사용자명을 한 번 입력하면 이후엔 자동으로 기억합니다.

질문 예시:
```
뮤지컬 한글명이 무엇인가요?
> 레베카

앱 대표 이모지를 골라주세요.
> 🌹

역할 수는 몇 개인가요?
> 2
```

모든 정보를 입력하면 앱 파일을 자동 생성하고 GitHub에 배포까지 완료합니다.

### Step 4 — GitHub Pages 활성화 (gh CLI가 없는 경우)

gh CLI가 있으면 자동으로 처리됩니다. 없다면 아래 순서로 직접 활성화합니다:

1. https://github.com/{내 사용자명}/{레포명} 접속
2. 상단 **Settings** 탭 클릭
3. 왼쪽 메뉴에서 **Pages** 클릭
4. Source → **Deploy from a branch** 선택
5. Branch → **main**, 폴더 → **/ (root)** 선택 후 **Save**

몇 분 후 `https://{사용자명}.github.io/{레포명}/` 에서 앱을 확인할 수 있습니다.

---

## 자주 묻는 것

**Q. 앱을 업데이트했는데 폰에서 예전 버전이 보여요**

PWA 캐시 때문입니다. 브라우저에서 강제 새로고침하거나, 앱을 홈 화면에서 삭제 후 다시 추가하세요.

**Q. 앱 URL에 접속이 안 돼요**

GitHub Pages가 활성화된 후 반영까지 1~3분 걸립니다. 잠시 기다렸다가 다시 접속해보세요.

**Q. 데이터가 다른 기기에서 안 보여요**

이 앱은 데이터를 기기 로컬에 저장합니다. 기기 간 동기화는 지원하지 않습니다.

**Q. Claude Code 사용 비용이 드나요?**

Claude Code는 Anthropic 계정이 있으면 사용할 수 있습니다. 사용량에 따라 과금될 수 있으니 Anthropic 요금제를 확인하세요. 앱 생성은 보통 한 번에 완료됩니다.

---

## 템플릿 앱 미리보기

`template/` 폴더에 샘플 앱 코드가 있습니다. `/new-musical` 실행 시 이 코드를 기반으로 새 앱을 만듭니다.
