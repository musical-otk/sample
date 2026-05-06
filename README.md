# 뮤지컬 관극 다이어리 Kit

Claude를 이용해 뮤지컬 관극 다이어리 PWA 앱을 뚝딱 만들 수 있는 템플릿입니다.

## 어떤 앱인가요?

공연 일정 캘린더, 관람 기록, 쿠폰 관리, 재관람 스탬프 카드 등을 한 곳에서 관리하는 모바일 웹 앱입니다.
단일 HTML 파일로 구성된 PWA라 서버 없이 GitHub Pages로 바로 배포할 수 있습니다.

## 시작하기

### 1. 필요한 것

- [Claude Code](https://claude.ai/code) (Claude CLI)
- GitHub 계정
- [GitHub CLI (`gh`)](https://cli.github.com/) — 레포 생성 및 Pages 배포에 사용

### 2. 이 레포 클론

```bash
git clone https://github.com/musical-otk/sample.git my-musical-diary
cd my-musical-diary
```

### 3. Claude Code 실행 후 명령 입력

```
/new-musical
```

Claude가 뮤지컬 정보를 단계별로 질문합니다. 답변하면 앱 파일을 자동 생성하고 GitHub Pages에 배포합니다.

처음 실행 시 GitHub 사용자명/조직명을 한 번만 입력하면 이후엔 자동으로 기억합니다.

## 템플릿 앱 미리보기

`template/` 폴더에 있는 샘플 앱(로저 2026 기반)을 참고하세요.

## 생성되는 앱 기능

- 공연 일정 캘린더 (배우별 필터)
- 관람 기록 (좌석, 캐스팅, 할인권)
- 쿠폰/할인권 잔여 관리
- 재관람 스탬프 카드 (마일스톤 보상)
- 다크모드
- PWA (홈 화면 추가 가능)
