---
layout: post
title: 도커 이미지, 컨테이너
subtitle: 도커 기본적인 Command
tags: [docker]
---

## 이미지

- 컨테이너를 생성하는 필수 요소.

- 여러 개의 계층으로 된 바이너리 파일로 존재.

- 컨테이너를 생성하고 실행할 때 읽기 전용으로 사용.

- 이미지와 컨테이너는 1:N 구조.

- 바이너리와 의존성이 설치되어 있으며, 격리되어 있음.

## 컨테이너

- 호스트로부터 격리된 프로세스.

- 이미지는 읽기 전용, 변경사항을 컨테이너 계층에 저장됨.
  

### 구성 
- [저장소이름]/[이미지이름]:[태크]

- 태크 생략시 최신버전 lastest.

- 저장소 생략시 Docker Hub.

### 이미지 저장소

- Public
  - [Docker hub](https://hub.docker.com/), [quay](https://quay.io/)

- Private
  - [AWS ECR](https://aws.amazon.com/ko/ecr/), [Docker Registry](https://hub.docker.com/_/registry), [Harbor](https://goharbor.io/)

<br>

```bash
## 우분투 이미지가 도커엔진에 없을 경우 docker hub에서 이미지를 pull 해온다.
## -i 상호입출력
## -t tty 활성화 하여 bash 셀 사용
docker run -it ubuntu:14.04
```

<br>

### docker pull
```bash
## docker hub에서 centos를 엔진으로 이미지를 내려받는다.
docker pull centos:7
```
<br>

### docker images

```bash
## docker engine에 내려받은 이미지들을 조회힌다. 
docker images
```

<br>

### docker run
이미지가 존재하지 않으면 pull -> create -> start -> attach
```bash
## 컨테이너 생성
docker create -it --name myCentos centos:7
## 컨테이너 시작
docker start myCentos
## 컨테이너 내부로 들어가는 명령어
docker attach myCentos
```
<br>
### docker create
이미지가 존재하지 않으면 pull -> create
<br>


## 컨테이너 목록 확인

```bash
## 정지되지 않은 컨테이너 출력
docker ps
docker container ls
## 컨테이너 이름 변경
docker rename A B
## 컨테이너 삭제
docker rm
## 컨테이너 정지
docker stop 'Container ID'
docker stop ${docker ps -aq}
```
> docker ps -aq 컨테이너 ID만 출력