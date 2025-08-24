# mago-ubuntu-macbook

## 개요

이 프로젝트는 오렌지플래닛 교육 프로그램의 코드 및 개발 환경을 포함합니다. Ubuntu 22.04 기반의 Docker 컨테이너에서 동작하며, 개발 및 실습을 위한 다양한 도구와 설정이 포함되어 있습니다.

---

## 디렉토리 구조

- `mago-ubuntu-macbook/`
  Ubuntu 22.04 기반 개발 환경 Dockerfile 및 docker-compose 설정, 컨테이너 내부 bash 프롬프트 커스터마이징, 접속 스크립트, 라이선스 및 설명 포함

---

## 주요 파일 설명

### docker/Dockerfile
- Ubuntu 22.04 기반 이미지
- 개발에 필요한 패키지(빌드 도구, git, sox, ffmpeg, python3.10, openssh-server 등) 설치
- 도커 그룹 및 sudo 권한 사용자 추가
- bash 프롬프트 커스터마이징(`add_bash.txt` 적용)
- SSH root 로그인 허용 및 기타 개발 편의 설정

### docker/docker-compose.yml
- `mago-ubuntu-macbook` 이미지를 빌드 및 실행
- 사용자별 UID/GID/USER 환경변수로 컨테이너 사용자 지정
- 호스트의 `/Users/${USERNAME}/workspace`를 컨테이너의 `/home/${USERNAME}/workspace`에 마운트
- 도커 소켓, 로컬타임 마운트
- `SYS_ADMIN` capability 부여

### connect.sh
- 실행 중인 컨테이너에 bash로 접속하는 스크립트

### docker/add_bash.txt
- git 브랜치 정보를 포함한 컬러풀한 bash 프롬프트 설정

### LICENSE
- Apache License 2.0 적용

---

## 사용 방법

1. **Docker 환경 준비**
   - `docker` 디렉토리에서 docker-compose로 컨테이너 빌드 및 실행
     ```sh
     docker-compose up --build -d
     ```
   - 컨테이너에 접속
     ```sh
     ./connect.sh
     ```

---

## 라이선스

본 프로젝트는 [Apache License 2.0](./LICENSE)을 따릅니다.
