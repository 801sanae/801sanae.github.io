---
layout: post
title: File
subtitle: File에 관한 간략한 내용
tags: [FileSystem, Java]
comments: true
---

### Overview
File처리에 관해서 간략하게 글을 작성 하고자한다.


### Contents
#### 시나리오
webclient를 통해 특정파일(PDF)를 byte[]로 획득후 특정 파일 경로에 저장, 읽는다.

```java
byte[] pdfBytes = WebClient.create().get()
                        .uri("https://seoul.intercontinental.com/upload/file/commonfilelang/15/800.pdf")
                        .retrieve()
                        .bodyToMono(byte[].class)
                        .block();

String fileName = "output1.pdf";
String mmDD = LocalDate.now().format(DateTimeFormatter.ofPattern("MMdd"));
String userhome = System.getProperty("user.home");
String dirPath = userhome + "/" + mmDD + "/";
String filePath = dirPath + fileName;                        
```
> 파일을 획득후 byte[]로 가져온다.

**- 파일 쓰기**

```java
//#1 
File file = new File(filePath);

try (FileOutputStream fos = new FileOutputStream(file)) {
    assert pdfBytes != null;
    fos.write(pdfBytes);
}

//#2
Files.write(Paths.get(filePath), pdfBytes);
```
> filePath는 파일이 저장될 경로 + 파일명

**- 파일 읽기**

```java
try(FileInputStream fis = new FileInputStream(new File(filePath))){
    byte[] bytes = fis.readAllBytes();
    Files.write(Paths.get("output1.pdf"), bytes);
}
```
> FileInputStream으로 file을 읽어 후속처리를 할 수 있다.

<br>
### 마치며
InputStream으로 읽고, OutputStream으로 쓴다라는 기본으로 SpringFramework에서 지원하는 FileOutputStream, FileInputStream, Resource, FileSystemResource등을 활용하면 파일 다운로드까지 쉽게 구현할수 있다. 물론, 요센 LLM을 통해 검색하면 훌륭한 예시까지 제시해주지만, 다시한번 정리하는 겸 기록해보았다. 끝.