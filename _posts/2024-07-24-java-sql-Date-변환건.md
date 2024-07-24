---
layout: post
title: java.sql.Date로 변환
subtitle: 날짜데이터를 java.sql.Date로 변환
tags: [java.sql.Date, Oracle]
comments: true
---

## Overview
Oracle Database의 Date 값이 String으로 파라미터로 들어왔을떄 Pasing 및 데이터 타입변경을 하면서 알게 된 내용을 정리하고자 한다.

## Contents
#1 파라미터로 아래와 같이 전달받는다.

```java

String dateTime = "2024-07-24 00:00:00";

```

#2. 데이터포맷으로 formating을 하면 java.util.Date로 결과를 받는다.


```java

SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

java.util.Date date = sdf.parse(dateTime);

```

#3. java.sql.Date로 변환한다.

```java
// #1 
java.sql.Date resultDate = java.sql.Date.valueOf(date.toInstant()
                                                    .atZone(ZoneId.systemDefault())
                                                    .toLocalDate());
// #2
// Deprecated 되어서 위 방법을 추천한다.
// java.sql.Date resultDate = new Date(date.getTime());

```
<br>

## 추가적으로..
java.sql.Date 클래스는 Oracle Date 타입에 적합하여 해당 클래스를 쓰게 되었다.<br/>
java.util.Date는 java초기 부터 있엇지만 Deprecate된 메소드가 많다.<br>
java.sql.Date 클래스 역시 java.util.Date를 상속 받고 있어 유사하나, java.sql.Date는 시간정보를 소실하고 날짜정보만 보여주기때문에사용에 한정적이라고 생각한다.<br>
java8이상 부터는 LocalDate , LocalDateTime를 쓰는것이 손쉽게 날짜 시분초등을 표현하고 사용하는데 유용할것이다.

