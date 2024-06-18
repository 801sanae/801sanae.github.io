---
layout: post
title: openssl , Self Sign Certificate Generate
subtitle: openssl, 자체서명인증서(사설)
tags: [openssl, Certificate, JKS]
comments: true
---

## 1. KeyPair 생성 및 CSR, 인증서 생성

```bash
## 키페어를 생성 2048 bit key size
openssl genrsa -des3 -out test.key 2048
```
<br>

```bash
## 생성된 키페어로 CSR을 생성
openssl req -new -key test.key -out test.csr

cp test.key test.key.enc

openssl rsa -in test.key.enc -out test.key
```

<br>

```bash
## 365 유효기간인 인증서를 csr과 keyPair로 만든다.
openssl x509 -req -days 365 -in test.csr -signkey test.key -out test.crt

openssl x509 -text -in test.crt
```
<br>

## 2. Format 변경

```bash
openssl rsa -in test.key -text > testKey.pem

openssl x509 -inform PEM -inn test.crt > testCrt.pem
```
<br>

## 3. p12로 만들어서 keytool 사용 후 jks로 변환

```bash
cat testKey.pem testCrt.pem test.crt > test.pem

openssl pkcs12 -export -out test.p12 -in test.pem

keytool -importkeystore -srckeystore test.p12 -srcstoretype pkcs12 -destkeystore test.jks -deststoretype jks
```

<br>

## 4. etc

- PKCS12(Public-Key Cryptography Standards #12)
  - 비밀번호 기반 대칭 키로 보호되는 공개 키 인증서와 함께 개인 키를 저장하는 데 일반적으로 사용되는 파일 형식을 정의합니다.
- PEM(Privacy Enhanced Mail)
  - 사람이 이해할 수 있는 문자열로 구성되어 있다. 인증서나 개인 키는 BASE64 인코딩을 사용하여 이진 데이터를 ASCII 문자로 변환한 후에 인증서 정보와 함께 저장됩니다. 출력가능한 아스키 문자사용하고 Base64로 인코딩된 텍스트 형식의 파일이다. 주로 인증서나 개인키, 인증서 사인요청(CSR)에 사용된다.
  - header / body / footer 구성
  - body는 Base64로 인코딩된 데이터가 들어가며, 65byte? 개행이 되어야 된다.
  - 주된 확장자는 .pem, .crt, .key, .csr
- CSR(Certificate Sign Request)
  - 인증기관에 인증서 발급 요청을 하는 ASN.1 형식의 파일 (PKCS#10)
  - 공개키 정보, 알고리즘 정보 포함
- JKS(Java KeyStore), keytool
  - Java 애플리케이션에서 사용되는 저장소로, 키와 인증서를 안전하게 저장하고 관리하는 역할을 합니다. JKS 파일은 비밀 키, 공개 키, 인증서 등을 포함할 수 있으며, 보통 암호로 보호됩니다. JKS는 보안 통신, 인증 및 암호화를 위해 Java 기반 시스템에서 널리 사용됩니다.(feat ChatGPT3.5)
- X.509
  - [X.509는 암호학에서 공개키 인증서와 인증알고리즘의 표준 가운데에서 공개 키 기반(PKI)의 ITU-T 표준이다.](https://ko.wikipedia.org/wiki/X.509)