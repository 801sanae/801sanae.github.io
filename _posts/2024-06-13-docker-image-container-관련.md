---
layout: post
title: docker 이미지, 컨테이너 import,export,save,load
subtitle: docker 이미지 처리 관련
tags: [docker, container, image]
---

## docker 이미지를 로컬환경에 백업 또는 전송가능.

Docker에는 이미지와 컨테이너를 저장하고 불러오는 다양한 명령어가 있음. 
이 명령어들은 주로 Docker 이미지를 백업하거나 다른 시스템으로 전송하는 작업에서 사용됨.

## `docker save`

- **기능**: docker 이미지를 tar파일로 저장
- **용도**: docker 이미지를 백업하거나 다른 시스템으로 전송할 때 사용
- **예시**:
```bash
  docker save -o my_image.tar my_image:latest
```
> my_image:latest 이미지를 my_image.tar 파일로 저장

<br>
## `docker export`

- **기능**: 실행 중인 docker 컨테이너의 파일 시스템을 tar 파일로 저장
- **용도**: docker 컨테이너의 현재 상태를 저장하거나 다른 시스템으로 전송할 때 사용
- **예시**:
```bash
  docker export -o my_container.tar my_container
```
> my_container 컨테이너의 파일 시스템을 my_container.tar 파일로 저장

<br>
## `docker import`

- **기능**: `docker export`로 생성된 tar 파일에서 이미지로 가져옴
- **용도**: 저장된 컨테이너 파일 시스템을 새로운 이미지로 만들 때 사용
- **예시**:
```bash
  docker import my_container.tar my_new_image:latest
```
> my_container.tar 파일을 새로운 이미지 my_new_image:latest로 가져옴

<br>
## `docker load`

- **기능**: `docker save`로 생성된 tar 파일에서 이미지를 로드함
- **용도**: 저장된 이미지를 Docker 데몬에 다시 로드할 때 사용
- **예시**:
```bash
  docker load -i my_image.tar
```
> my_image.tar 파일에서 이미지를 로드하여 Docker 데몬에 추가

<br>

## 정리
- 연관성 정리
  - **저장 및 로드 :**
    - `docker save` : Docker 이미지를 tar 파일로 저장
    - `docker load` : docker save로 저장한 이미지를 로드
  - **내보내기 및 가져오기 :**
    - `docker export` : 컨테이너의 파일 시스템을 tar 파일로 저장
    - `docker import` : docker export로 저장한 컨테이너 파일 시스템을 이미지로 가져옴

- 차이점
  - docker save와 docker load는 Docker 이미지를 직접 다룸
  - docker export와 docker import는 컨테이너의 파일 시스템을 다룸