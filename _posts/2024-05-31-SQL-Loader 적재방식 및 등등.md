---
layout: post
title: SQLLoader 적재방식 및 등등
subtitle: Oracle Sqlloader 관련 정리
date: 2024-05-31
tags: [Oracle, sqldr]
---

### SQL Loader란
Oracle 설치시 기본적으로 설치되는 유틸리티로 데이터 이관에 특장정을 가지고 있다.
최근 CDC로 실시간 무중단 데이터 마이그레이션 하는 트랜드가 있다.
그럼에도 SQLLOADER를 사용하여 운영데이터 -> 개발데이터 이관시 사용되고 있다.
Control File을 통해 log, bad, dicard file 경로, 적재방식 등등을 필요 내용을 기입한다. 그리고 console상에서 컨트롤파일, 옵션으로 실행한다.

### SqlLoader 적재방식

|Conventional path|Direct path|
|:---:|:---:|
|SQL Loader의 rows & bindsize option으로 설정된 일정량의 버퍼를 채우면 SQL Insert 문장을 이용해 로딩하는 방식|Direct path load Engine을 사용해 오라클의 버퍼 캐시를 거치지 않고 디스크에 직접 쓰기 때문에 Conventional path 방식에 비해 부하 및 상호 간의 경쟁 없이 빠르게 수행|
|Row level locking|Table level locking|
|데이터 로딩 중  DML 사용 가능|로딩 중 lock|
|속도 느림, Insert문|속도 빠름 오라클 캐시 안씀|
||Loader 사용시 direct=true 옵션 필요|
