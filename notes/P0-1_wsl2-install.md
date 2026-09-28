# P0-1. WSL2 + Ubuntu 24.04 설치

## 목표
윈도우 안에서 바로 쓰는 리눅스(WSL2 + Ubuntu 24.04)를 설치하고, 리눅스 사용자 계정을 만든다.

## 개념
- WSL2 (Windows Subsystem for Linux 2) : 윈도우 안에서 진짜 리눅스 커널을 가벼운 가상머신으로 돌려주는 마이크로소프트 공식 기능
- 필요한 윈도우 기능 2가지
    - Linux용 Windows 하위 시스템 : WSL 자체
    - 가상 머신 플랫폼(VirtualMachinePlatform) : WSL2가 리눅스 커널을 돌리는 데 필요
- 배포판(distribution) : 리눅스 커널 + 기본 프로그램 묶음. Ubuntu, Rocky 등
- 리눅스 계정은 윈도우 계정과 별개. 비밀번호는 sudo(관리자 권한 명령)를 쓸 때 필요

## 명령어
| 명령어 | 뜻 |
|---|---|
| `wsl --status` / `wsl --version` | WSL 설치 상태와 버전 확인 |
| `wsl --install -d Ubuntu-24.04` | WSL 기능 켜기 + 배포판 설치. `-d` = 설치할 배포판 지정 (관리자 권한 필요) |
| `cd ~` | 리눅스 홈 폴더로 이동. `~` = 내 홈 폴더 |
| `pwd` | 지금 위치 출력 (print working directory) |
| `whoami` | 지금 사용자 이름 출력 |
| `cat /etc/os-release` | 배포판 정보 파일 출력 |
| `uname -r` | 커널 버전 출력 (`-r` = release) |

## 결과 해석
- `cat /etc/os-release` → `Ubuntu 24.04.5 LTS (Noble Numbat)`
- `uname -r` → `6.18.33.2-microsoft-standard-WSL2` → 끝에 WSL2가 있으면 WSL2 위에서 동작 중
- 프롬프트 `사용자@컴퓨터이름:위치$`
    - `/mnt/c/...` = 리눅스에서 본 윈도우 C 드라이브
    - `~` = `/home/taeeun_eom` (리눅스 홈)
    - `$` = 일반 사용자, `#` = root(관리자)

## 막힌 점과 해결
1. `wsl --status`, `wsl --version`이 아무것도 출력하지 않음
    - Windows 기능 켜기/끄기(`optionalfeatures`)에서 확인 → Linux용 Windows 하위 시스템이 꺼져 있었음
2. `wsl --install` 실행 시 `WSL 설치가 손상된 것 같습니다 (Wsl/CallMsi/Install/REGDB_E_CLASSNOTREG)`
    - 윈도우에 미리 깔린 WSL 구성 요소가 일부만 설치된 상태
    - "아무 키나 눌러 복구" → 첫 시도는 `클래스가 등록되지 않았습니다`로 실패
    - 다시 실행해서 복구 → WSL 2.7.14로 업데이트되며 해결
3. 설치 명령이 VirtualMachinePlatform 기능을 켜고 재시작 안내 → 설치 명령 재실행 후 Ubuntu 설치 완료
4. 오타 : `cat /etc/os-releas` → `No such file or directory`. 파일 이름은 한 글자도 틀리면 안 됨 → Tab 자동완성 쓰기

## 핵심 정리
- WSL2 = Linux용 Windows 하위 시스템 + 가상 머신 플랫폼, 설치는 관리자 권한으로 `wsl --install -d 배포판`
- 설치 확인 세트 : `whoami` · `pwd` · `cat /etc/os-release` · `uname -r`
- 리눅스 실습은 `/mnt/c`(윈도우)가 아니라 `~`(리눅스 홈)에서 한다