# P0-2. WSL Ubuntu에 Git 설정

- 날짜: 2026-09-29
- 환경: WSL2 Ubuntu 24.04 (사용자 `taeeun_eom`), git 2.43.0

## 왜 하는가

- WSL Ubuntu는 윈도우와 별개의 리눅스 환경이라, 윈도우에 해둔 Git 설정이 적용되지 않는다.
- 커밋마다 작성자(이름·이메일)가 기록되므로, 개인 이메일 대신 GitHub noreply 주소를 쓴다.

## 설정한 명령

```bash
git config --global user.name "ten-infosec"
git config --global user.email "333761270+ten-infosec@users.noreply.github.com"
git config --global init.defaultBranch main
git config --global core.autocrlf input
```

| 설정 | 의미 |
| --- | --- |
| `--global` | 이 사용자의 모든 레포에 적용 (설정 파일: `~/.gitconfig`) |
| `user.name` | 커밋 작성자 이름 |
| `user.email` | 커밋 작성자 이메일 (GitHub → Settings → Emails의 noreply 주소) |
| `init.defaultBranch main` | 새 레포의 기본 브랜치를 GitHub와 같은 `main`으로 |
| `core.autocrlf input` | 줄바꿈을 리눅스 방식(LF)으로 통일 (윈도우 CRLF와 섞이는 문제 방지) |

## 확인

```bash
git config --global --list
git config --global user.email
```

## 막혔던 점

- 안내문의 `숫자+ten-infosec@...`에서 "숫자"를 글자 그대로 입력해 `user.email=숫자+ten-infosec@...`로 저장됐다.
- GitHub Settings → Emails의 "Keep my email addresses private"에서 실제 번호를 확인해 같은 명령으로 다시 설정했다 (같은 키로 다시 설정하면 덮어써짐).
- 잘못된 이메일로 커밋하면 GitHub가 내 계정으로 인식하지 못해 기여 기록에 잡히지 않는다.

## 참고

- GitHub의 "Block command line pushes that expose my email"을 켜 두면, 개인 이메일이 들어간 커밋의 push를 막아준다.`