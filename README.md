# 리눅스 독학 커리큘럼

명령어에서 시작해 동작 원리, 서버 운영과 보안, 컨테이너, 커널, ARM 임베디드까지 이어지는 독학 로드맵.
PQC 커리큘럼(Stage 2~9)과 홈서버 구축을 함께 받쳐주도록 설계했다.

- 참고 자료: 리얼리눅스 강의 목차(기초 명령어, SW 기본, 시스템 핵심정리, 쉘 스크립트, 도커/K8s, 네트워크, 트러블슈팅, 커널 중급 A·B, ARM 임베디드 기초·아키텍처·중급·트러블슈팅), 리눅스마스터 2급, Level UP 챌린지 Q1~Q124
- 표기: `Q번호` = Level UP 챌린지 실습 문제, `(추가)` = 원 자료에 없어서 보충한 항목, `→ PQC` = PQC 커리큘럼과 연결되는 지점

---

## 실습 원칙

- 실습은 **내 컴퓨터, 내 가상머신, 내 홈서버**에서만 한다
- 포트 스캔(nmap)은 `localhost`, 내 서버, 연습용으로 허락된 `scanme.nmap.org`만 대상으로 한다
- 커널 설정, 방화벽, 디스크 포맷처럼 시스템을 망가뜨릴 수 있는 실습은 **가상머신에서** 하고, 시작 전에 스냅샷을 찍어둔다
- 문제 하나를 풀 때마다 아래 **기록 양식**으로 `notes/`에 남긴다

---

## Part 0. 실습 환경 만들기 (추가) · 1주

- WSL2 + Ubuntu 24.04 설치 (윈도우 안에서 바로 쓰는 리눅스)
- VirtualBox 또는 VMware에 Rocky Linux 가상머신 설치 (Ubuntu와 Rocky 두 계열 모두 연습)
- 가상머신 스냅샷 찍기와 되돌리기
- 윈도우에서 가상머신으로 SSH 접속
- 리눅스에 Git 설정 (`user.name`, noreply 이메일)

---

## Part 1. 기초: 명령어와 쉘 · 약 6주

### 1-1. 터미널과 쉘
- 리눅스는 어디에 쓰이나, 서버 관련 직종
- 쉘 vs 터미널 vs 콘솔, GUI보다 CLI가 효율적인 이유
- 필수 기초 명령어, 명령어 히스토리, 쉘 내장 명령어
- `man`, `--help`로 사용법 찾기
- 실습: `Q1` 서버 가동 시간, `Q2` 시간대 변경, `Q41` 커널 버전

### 1-2. 디렉토리와 파일
- 루트 파일시스템과 기본 폴더(`/etc`, `/var`, `/usr`, `/home`, `/proc`, `/dev`)
- 파일 타입(일반, 디렉토리, 링크, 소켓, 블록, 문자 장치)
- VFS(가상 파일시스템) 개념
- 파일 편집: vi/vim 기본 조작 (추가: vimtutor로 연습)
- 필터링, 정렬, 부분 추출: `grep`, `sort`, `cut`, `awk`, `sed`
- 파일 찾기: `find`, `locate`
- 압축: `zip`, `gzip`, `tar`
- 리다이렉트 `>`와 파이프 `|`
- 실습: `Q46`~`Q52` 압축·검색, `Q113` vi 스왑 파일 충돌

### 1-3. 사용자와 권한
- 계정 생성과 홈 디렉토리, 그룹
- 권한 읽기(`rwx`), `chmod`, `chown`, 특수 권한(SetUID, Sticky bit)
- (추가) `sudo`와 `/etc/sudoers`, `umask`, 최소 권한 원칙
- 실습: `Q53`~`Q60` 계정·그룹·Permission denied 해결

### 1-4. 패키지 관리
- 배포판의 개념(Ubuntu 계열 vs Rocky 계열)
- `apt`/`dpkg`, `dnf`/`yum`/`rpm`
- 소스 설치: `tar`, `make`
- 실습: `Q16`~`Q23`, `Q76`~`Q85` 설치·삭제·검색·업데이트·의존성

### 1-5. 프로세스와 메모리
- `ps`, `top`, 시그널(`SIGTERM`, `SIGKILL`, `SIGSTOP`)
- 우선순위와 `nice`
- CPU, 메모리 정보 확인
- 시스템 종료와 재부팅
- 실습: `Q6` CPU 100% 프로세스, `Q7` 우선순위, `Q42` 메모리, `Q45` 메모리 Top 5, `Q65`~`Q67` 시그널

### 1-6. 디스크와 서비스
- systemd와 서비스 관리(`systemctl`, `journalctl`)
- 디스크와 파일시스템, `df`, `du`
- 디스크 I/O 모니터링 도구
- 실습: `Q13` 부팅 로그, `Q43` 디스크 용량, `Q44` 큰 폴더 찾기

### 1-7. 네트워크 도구
- IP·DNS 확인, 인터넷 연결 확인, 경로 추적
- 포트 사용 프로세스 찾기(`ss`, `lsof`)
- 포트 스캔(`nmap`, 허락된 대상만)
- (추가) `tcpdump`로 터미널에서 패킷 캡처 → Wireshark로 열어보기
- 실습: `Q11`, `Q12`, `Q24`~`Q27`, `Q115`~`Q118`

### 1-8. 로그와 작업 스케줄링
- 로그 위치와 보는 법, 최신 N줄(`tail -f`)
- `cron`, `at`, 부팅 시 자동 실행
- `logrotate`
- (추가) systemd timer (cron의 최신 대안)
- 실습: `Q4` 404 로그, `Q14` SSH 접속 로그, `Q15`, `Q28`~`Q32`

### 1-9. Bash 쉘 스크립트
- 스크립트 파일과 실행 권한
- 변수, 숫자·문자열, 인자, 반복문, 조건문, 함수
- 작업 관리(foreground / background)
- 쉘 설정: `.bashrc`, alias, 프롬프트, PATH
- 실전 예제: 프로세스별 메모리 파악, 디스크·메모리 점검, gcore 응용
- 실습: `Q33`~`Q36`

### 1-10. 웹 서버와 DB
- Nginx, Apache httpd, Node.js, Spring Boot 예제
- 포트 충돌 해결과 부하 테스트
- DB 설치와 기본 SQL, Python 연동, ORM API
- 실습: `Q3` Hello world 웹서버, `Q5` 80번 포트 충돌, `Q112` Nginx 403, `Q114` MariaDB 설정 잔재

### 체크포인트: 리눅스마스터 2급
- 위 범위 + 파티션, LVM, RAID, 파일시스템 복구와 fstab, 디스크 쿼터
- IPv4 주소 클래스, 서브넷, 네트워크 장비, X 윈도, 응용 분야
- 기출문제 풀이(2021~2023) → 핵심 암기 포인트 정리

---

## Part 2. 원리: 리눅스는 어떻게 동작하나 · 약 8주

→ PQC Stage 2(운영체제·자료구조), Stage 3(C·메모리)와 동시에 진행

### 2-1. OS 구조
- 리눅스 OS의 역할과 구성, 커널 구조
- 리눅스 OS는 C 프로그램이다
- 유저 함수 / 라이브러리 함수 / 커널 함수 실행
- `top`으로 CPU·메모리·프로세스 상태 읽기

### 2-2. C 소스에서 바이너리까지
- 전처리 → 컴파일 → 어셈블 → 링킹
- GDB로 유저 함수, 라이브러리 함수, 커널 함수(시스템콜) 호출 추적
- 실습: `Q8` strace로 시스템콜 보기, `Q61` 공유 라이브러리(`ldd`), `Q62` 정적 빌드

### 2-3. 가상 메모리와 포인터
- `int a = 1;`의 동작 원리
- 페이지 테이블, RSS와 VSZ
- 포인터 변수, 이중 포인터로 링크드리스트 insert 구현

### 2-4. Stack / Heap과 CPU 캐시
- C 코드에서 page fault까지
- Stack / Heap 메모리 분석, 스택 공간 할당 세부 분석
- CPU 캐시와 캐시 라인
- 실습: `Q63` heap·stack 주소 범위, `Q64` 프로세스 메모리 사용량
- (추가) 버퍼 오버플로우와 use-after-free 직접 일으키기, `valgrind`로 찾기 → PQC Stage 3

### 2-5. 스케줄링, 시그널, Swap
- 스케줄링 period, 타임슬라이스, virtual runtime, 선점
- 페이지 회수, Page-In/Out, Swap-In/Out

### 2-6. 디스크와 파일 I/O
- 블록, 섹터, 포맷, 새 디스크 추가와 mount
- RAID0(스트라이프) vs RAID1(미러링) I/O 테스트
- 페이지 캐시, Buffered I/O vs Direct I/O(`fio`)
- 파일 open과 파일 디스크립터, read/write 추적, readahead
- 블록 디바이스 I/O 추적

### 2-7. 네트워크 스택과 네트워크 I/O
- 리눅스 네트워크 스택(L4 / L3 / L2)
- ICMP, ARP, DNS, TCP 추적
- 웹 브라우저에서 웹 서버까지의 패킷 구조
- Blocking vs Non-Blocking I/O, `select`
- (추가) OSI 7계층 / TCP/IP 4계층 지도 그리기 → PQC Stage 2 자가 점검

---

## Part 3. 서버 운영과 보안 · 약 6주

원 자료에서 가장 비어 있던 부분이라 대부분 (추가) 항목. → PQC Stage 4~5, 정보보안 진로

### 3-1. SSH
- 키 기반 인증, 비밀번호 없이 접속
- (추가) SSH 하드닝: root 로그인 금지, 비밀번호 인증 끄기, 포트 변경의 의미와 한계
- 실습: `Q9`, `Q14`

### 3-2. 방화벽
- iptables / nftables 기본 구조
- (추가) ufw(Ubuntu), firewalld(Rocky)
- 실습: `Q10` 8080 포트 차단, `Q121` 도커와 iptables 충돌

### 3-3. 침입 방어와 감사 (추가)
- fail2ban으로 무차별 대입 차단
- auditd로 파일·명령 실행 감사 로그 남기기
- SELinux(Rocky) / AppArmor(Ubuntu) 기초
- 불필요한 서비스 끄기, 열린 포트 점검 습관

### 3-4. 인증서와 TLS (추가)
- OpenSSL 명령어 기초: 키 생성, 인증서 확인, 해시
- 자체 루트 CA와 서버 인증서 발급 → Nginx에 HTTPS 적용 → PQC Stage 5
- Let's Encrypt 자동 발급과 갱신
- 최신 OpenSSL의 ML-KEM 지원 확인과 PQC TLS 서버 실험 → PQC Stage 6

### 3-5. 네트워크 심화
- 라우팅, IPv4 주소와 서브넷, NAT, CIDR
- MTU와 MSS, 소켓 버퍼 커널 설정
- 네트워크 도구와 추적·모니터링

### 3-6. 백업과 복구 (추가)
- `rsync`, `tar`, cron으로 자동 백업
- 복구 연습: 스냅샷에서 되돌리기, 백업에서 파일 되살리기

---

## Part 4. 컨테이너와 오케스트레이션 · 약 6주

### 4-1. 컨테이너 원리
- 리눅스와 컨테이너, 이미지 vs 컨테이너, 가상머신 vs 컨테이너
- namespace 추적, cgroup으로 CPU·코어·메모리 제한
- 실습: `Q75`

### 4-2. 도커 기본
- 컨테이너와 이미지 다루기, save/load, export/import
- 로그, 포트 공개, 볼륨, 정리
- 실습: `Q37`~`Q40`, `Q68`~`Q72`, `Q111`

### 4-3. 도커 구조와 빌드
- OCI와 runc, dockerd / containerd / runc
- 이미지 레이어, Dockerfile, Docker Compose, Docker Hub
- 도커 네트워크(docker0)
- 실습: `Q73`, `Q74`, `Q120`, `Q122`
- (추가) 컨테이너 보안: rootless 도커, 이미지 취약점 스캔(trivy)

### 4-4. Docker Swarm
- 오케스트레이션 개념, 서비스 배포, 복제본, 롤링 업데이트, 자동 복구
- 실습: `Q101`~`Q110`

### 4-5. 쿠버네티스
- 구조와 오브젝트, k3s 설치
- Pod, Service, ReplicaSet, Deployment, 장애 복구
- kubectl, 노드 지정 스케줄링, YAML, Rollout / Rollback
- Ingress, Volume, 웹 애플리케이션 배포
- 실습: `Q86`~`Q100`

### 4-6. 컨테이너 네트워크
- 쿠버네티스 네트워크(Service, Ingress, kube-proxy), CoreDNS
- BPF와 XDP, Cilium
- AWS 네트워크 사례 → 관심 자격증 AWS SCS
- 실습: `Q123`, `Q124`

---

## Part 5. 트러블슈팅 · 약 4주

서버와 임베디드의 트러블슈팅 자료를 하나로 묶음.

- 문제 해결 전략: 진단 → 분석 → 검증(수정)
- 패키지 이슈
- 시스템 진단, 메모리 이슈(`valgrind`, `sysctl`)
- 파일 I/O 이슈와 모니터링 도구
- 네트워크 이슈와 진단법
- 실습: `Q111`~`Q124`의 문제 재현과 원인 분석

---

## Part 6. 커널 심화 · 약 8주

### 6-1. 메모리와 파일 I/O (커널 중급 A)
- 시스템콜 호출 과정 추적, syscall / VFS / ext4 소스 리딩
- page fault 핸들링, CPU 컨트롤 레지스터, Tracepoint
- 페이지 테이블 1~4 Level, 페이지 테이블 dump, VMA
- mmap vs read, 유저·커널 가상 주소 분리
- 커널 메모리: 버디 시스템, 슬랩 할당자, vmalloc, 페이지 회수
- 파일 Write / Read / Open 과정, Writeback
- VFS / FS 자료구조, inode, 블록 I/O, btrace, BPF 추적

### 6-2. 네트워크와 스케줄러 (커널 중급 B)
- 소켓 통신 추적, TCP/IP 커널 코드
- 인터럽트(IRQ), softirq, workqueue
- MSS·MTU, 소켓 버퍼, 패킷 receive 과정, XDP
- CFS 스케줄러, period, 타임슬라이스, vruntime
- 동기화: race condition, 세마포어·뮤텍스·스핀락·RCU, 선점 방지, 데드락과 Lockdep
- 시그널 generate / deliver
- cgroup과 namespace, 도커로 cgroup 커널 동작 추적

### 6-3. 추적 도구 (추가)
- `perf`로 성능 프로파일링
- `bpftrace`로 커널 함수 한 줄 추적
- 커널 실험은 반드시 가상머신에서

---

## Part 7. ARM 임베디드 (선택 트랙) · 약 10주

→ PQC Stage 7~9(하드웨어 보안, TrustZone, Secure Boot, SoC, 암호화 가속)의 바탕.
준비물: 라즈베리파이4

### 7-1. ARM 임베디드 기초
- 라즈베리파이 환경에서 리눅스 기초 복습
- 프로그램 실행 원리, ELF 바이너리, ARM 레지스터
- ARM 메모리 액세스와 가상 메모리, Stack / Heap
- 파일·네트워크 I/O
- 디바이스 드라이버와 커널 모듈, Hello World 드라이버, GPIO / LED 드라이버

### 7-2. ARM 아키텍처
- 어셈블리와 주요 명령어, ABI, ARMv8 레지스터
- NEON SIMD와 부동소수점 → PQC Stage 9 암호화 가속
- 예외 모델, EL(Exception Level), 32/64-bit 실행
- ARMv8 메모리 구조, page fault(Abort), perf
- 캐시 구조, 메모리 오더링
- TrustZone, 가상화, 칩 내장 암호 명령어 → PQC Stage 8~9

### 7-3. ARM 임베디드 리눅스 중급
- 임베디드 C(최적화 옵션, `volatile`, `pragma`)
- 임베디드 시스템 아키텍처, 인터럽트와 I/O
- ARM 어셈블리 프로그래밍
- `/proc`, `/sys`, `/dev`, 데이터시트로 GPIO 레지스터 읽기
- I2C, SPI 드라이버
- 커널 컴파일, sysctl, 커널 소스 구조
- 툴체인, QEMU로 리눅스 포팅, u-boot 부트로더
- rootfs, `.deb` 패키지 빌드
- 부팅 이슈 트러블슈팅
- (추가) u-boot verified boot로 Secure Boot 체험 → PQC Stage 9 (부팅 단계마다 전자서명 검증)

---

## 종합 프로젝트: 홈서버 구축 (추가)

Part 1~5에서 배운 것을 실제 서버 하나에 모두 적용한다.

1. Ubuntu Server 설치, 사용자와 sudo 설정
2. SSH 키 인증 + 하드닝, 방화벽, fail2ban
3. Nginx + HTTPS(자체 CA 또는 Let's Encrypt)
4. Docker Compose로 서비스 여러 개 운영
5. cron / systemd timer로 자동 백업과 로그 관리
6. 리소스 모니터링과 장애 대응 기록
7. (도전) ML-KEM을 지원하는 TLS 설정으로 PQC 웹서버 → Wireshark로 핸드셰이크 확인
8. 설정 파일과 스크립트는 GitHub에 올리되, **비밀번호·키·개인정보는 제외**

---

## 자격증 경로

- 리눅스마스터 2급 (Part 1 완료 시점)
- (추가, 선택) LFCS 또는 RHCSA: 실기형 국제 자격증 (Part 3 이후)

---

## PQC 커리큘럼 연결표

| PQC 단계 | 리눅스 커리큘럼 |
|---|---|
| Stage 2 전산학 기초 | Part 2-3 가상 메모리, 2-5 스케줄링, 2-7 네트워크 스택 |
| Stage 3 C/C++ · 리눅스 | Part 0 환경, Part 2-2 GDB, 2-4 Stack/Heap·버퍼 오버플로우 |
| Stage 4 암호학 기초 | Part 3-4 OpenSSL, 1-7 tcpdump |
| Stage 5 PKI | Part 3-4 자체 CA, Nginx HTTPS |
| Stage 6 PQC | Part 3-4 PQC TLS 서버, 종합 프로젝트 7번 |
| Stage 8 TEE | Part 4-1 namespace·cgroup, Part 7-2 TrustZone |
| Stage 9 가속·RoT·SoC | Part 7-2 NEON·암호 명령어, 7-3 Secure Boot |

---

## 기록 양식

문제 하나를 풀 때마다 `notes/Q번호_주제.md`로 남긴다.

```markdown
# Q27. 외부에서 열려 있는 포트번호 조회하기

## 목표
무엇을 할 수 있게 되는가 (한 줄)

## 개념
이 문제를 풀기 위해 알아야 할 원리

## 명령어
사용한 명령어와 각 옵션의 뜻

## 결과 해석
출력에서 무엇을 읽어야 하는가

## 막힌 점과 해결
에러 메시지, 원인, 해결 과정

## 핵심 정리
다음에 다시 볼 때 필요한 세 줄
```