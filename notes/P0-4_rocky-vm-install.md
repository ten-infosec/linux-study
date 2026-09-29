# P0-4. VirtualBox에 Rocky Linux 9.8 가상머신 설치

- 날짜: 2026-09-29
- 환경: Windows 11, VirtualBox 7.2.10, Rocky Linux 9.8 Minimal (x86_64)

## 왜 하는가

- WSL Ubuntu(Debian 계열)와 다른 **Red Hat 계열** 리눅스를 함께 연습하기 위해
- 기업 서버에서 많이 쓰는 환경이고, 망가져도 되는 실습용 서버가 필요하다

## 버전 선택

- Rocky 10.2가 최신이지만 최신 CPU 기능(x86-64-v3)을 요구한다
- WSL2(Hyper-V)와 VirtualBox를 함께 쓰면 CPU 기능이 가상머신에 전달되지 않을 수 있어 **9.8**을 선택 (지원 종료 2032-05-31)
- 화면(GUI) 없는 **Minimal ISO** 사용

## ISO 무결성 검증

```bash
ls -l "/mnt/c/Users/Taeeun Eom/Downloads/Rocky-9.8-x86_64-minimal.iso"
sha256sum "/mnt/c/Users/Taeeun Eom/Downloads/Rocky-9.8-x86_64-minimal.iso"
```

- 계산한 SHA256이 공식 `CHECKSUM` 파일의 값과 64자리 모두 일치
- `CHECKSUM`의 `#` 줄(크기 설명)은 실제 크기와 달랐다 → **판단 기준은 지문(해시) 값**

## 가상머신 설정

| 항목 | 값 | 이유 |
| --- | --- | --- |
| 이름 | `rocky9` | 짧고 명령어에서 쓰기 쉽게 |
| 메모리 | 2048 MB | Minimal은 GUI가 없어 충분 |
| CPU | 2 | 설치·업데이트 속도 |
| 디스크 | 30 GB, VDI, 동적 할당 | 실제로 쓴 만큼만 커짐 |
| EFI | 끔 | 공부용은 BIOS 방식이 단순 |
| 무인 설치 | 끔 | 설치 과정을 직접 보기 위해 |

## 설치 설정

- 언어: English (한국어는 텍스트 콘솔에서 글자가 깨짐)
- Software Selection: Minimal Install, 추가 패키지 없음 (필요한 것은 나중에 `dnf`로 직접 설치)
- Installation Destination: 30 GiB 가상 디스크, Automatic 파티션, 암호화 안 함
- root 계정 비활성화, 관리자 사용자 `taeeun` 생성 (`wheel` 그룹 → `sudo` 사용 가능)

## 확인

```bash
cat /etc/os-release
```

- `PRETTY_NAME="Rocky Linux 9.8 (Blue Onyx)"`
- `ID_LIKE="rhel centos fedora"` → Red Hat 계열 (WSL Ubuntu는 `debian`)
- `PLATFORM_ID="platform:el9"` → Enterprise Linux 9

## 막혔던 점

- 설치 화면에서 마우스 클릭 위치가 어긋남 → **Tab / 방향키 / Enter / Alt + D(Done)** 키보드로 진행
- 오른쪽 Ctrl = VirtualBox **호스트 키** (마우스 풀기, `+F` 전체 화면, `+Home` 메뉴)
- 부팅 후 로그인 안내가 커널 메시지에 묻혀 멈춘 것처럼 보임 → Enter를 누르면 다시 나온다
- 콘솔 글자가 흐림 → 비트맵 글꼴의 한계. `sudo setfont latarcyrheb-sun32`로 키울 수는 있지만, 깔끔하게 쓰려면 SSH 접속(P0-6)
- 콘솔 창에는 복사·붙여넣기가 안 된다 → 이것도 SSH로 해결 예정