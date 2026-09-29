# P0-3. WSL Ubuntu에서 SSH로 GitHub 연결

- 날짜: 2026-09-29
- 환경: WSL2 Ubuntu 24.04 (사용자 `taeeun_eom`), git 2.43.0

## 왜 하는가

- HTTPS 방식은 push할 때 GitHub 로그인(토큰)이 필요하다. SSH 키를 등록해 두면 키로 인증한다.
- 키를 WSL 안(`~/.ssh`)에 만들었으므로, SSH·Git 명령도 WSL Ubuntu 터미널에서 실행해야 한다.

## 실행한 명령

```bash
ssh-keygen -t ed25519 -C "333761270+ten-infosec@users.noreply.github.com"
# GitHub → Settings → SSH and GPG keys → New SSH key 에 id_ed25519.pub 내용 등록
ssh -T git@github.com
cd "/mnt/c/Users/Taeeun Eom/Desktop/01_study/linux-study"
git remote -v
git remote set-url origin git@github.com:ten-infosec/linux-study.git
git fetch
git status
```

| 명령/파일 | 의미 |
| --- | --- |
| `id_ed25519` | 개인키. 절대 공개·업로드 금지 |
| `id_ed25519.pub` | 공개키. GitHub에 등록하는 것 |
| `ssh -T git@github.com` | SSH 인증 테스트 (`-T`: 셸 화면을 요청하지 않음) |
| `~/.ssh/known_hosts` | 처음 접속할 때 확인한 서버 지문(fingerprint)을 저장 |
| `git remote set-url` | 원격 저장소 주소를 HTTPS → SSH로 교체 |
| `git fetch` | 원격의 최신 기록만 받아옴 (내 파일은 안 바뀜) |

## 확인

- `ssh -T git@github.com` → `Hi ten-infosec! You've successfully authenticated...` 이면 인증 성공
- `git remote -v` → `git@github.com:ten-infosec/linux-study.git` 로 바뀐 것 확인
- `git fetch` 후 `git status` → `up to date with 'origin/main'`

## 막혔던 점

- `cat /.P0-2_git-config.md` → `/`로 시작하면 최상위(루트)부터 찾는다. 현재 위치 기준은 `./` 또는 `notes/...`
- passphrase 없이 만든 줄 알았는데 접속할 때 물어봄 → 실제로는 설정되어 있었다. 틀리면 다시 물어본다.
- 저장소가 Windows 폴더(`/mnt/c/...`)에 있어서 권한이 `rwxrwxrwx`로 보인다.
- 원격 주소를 SSH로 바꿨기 때문에, 이 폴더에서 Windows 쪽 Git으로 push하면 실패할 수 있다 (Windows에는 SSH 키가 없음).
