---
layout: post
title: XML 처리 방법 사용기
subtitle: XML
tags: [ObjectMapper, JAXB, XlmStreamReader]
comments: true
---

### Overview
데이터처리 및 전달은 전통적으로 XML을 많이 사용해왔다. 지금도 마찬가지이나, KeyValue 스타일(JSON)이 많이 각광받긴하나 XML형식 또한 많이 사용되고 있는 데이터포맷이다.
최근 회사에서 xml파일을 읽어서 해당 태그를 추출하고, 태그별 데이터를 바인딩할 일이 있어서 해당내용에 대해 정리해본다.
XML 처리를 위해 3가지 방법에 대한 내용이다.

- XMLStreamReader<br/>
    XMLStreamReader는 StAX(Streaming API for XML) API의 일부로, XML 문서를 순차적으로 읽는 pull-parsing 방식을 사용합니다.
  
  - ***장점***: 메모리 효율적이며 대용량 XML 파일 처리에 적합
  - ***단점***: 저수준 API로, 개발자가 직접 파싱 로직을 구현해야 함
- JAXB (Java Architecture for XML Binding)<br/>
    JAXB는 XML과 Java 객체 간의 매핑을 자동화합니다.

  - ***장점***: 사용이 간편하며 XML을 직접 Java 객체로 변환
  - ***단점***: 전체 문서를 메모리에 로드하므로 대용량 파일에는 부적합할 수 있음
- ObjectMapper (Jackson)<br/>
    Jackson의 ObjectMapper는 JSON 및 XML 데이터 처리를 위한 고수준 API를 제공합니다.

  - ***장점***: JSON과 XML 모두 지원하며 사용이 간편함
  - ***단점***: JAXB에 비해 유연성이 떨어질 수 있음

### 성능 비교
- 메모리 사용:<br/>
    XMLStreamReader < ObjectMapper < JAXB
- 처리 속도:<br/>
    소형 문서: JAXB ≈ ObjectMapper > XMLStreamReader<br/>
    대형 문서: XMLStreamReader > ObjectMapper > JAXB
- 개발 생산성:<br/>
    JAXB ≈ ObjectMapper > XMLStreamReader

<br/>

### 사용예졔
<br/>
#### 1. XMLStreamReader
```java
XMLInputFactory xmlInputFactory = XMLInputFactory.newInstance();
XMLStreamReader reader = xmlInputFactory.createXMLStreamReader(new FileInputStream("FileName.xml"));

while (reader.hasNext()) {
    int eventType = reader.next();

    switch (eventType) {
        case XMLStreamConstants.START_ELEMENT:
            System.out.println("Start Element :: " + reader.getLocalName());
            break;
        case XMLStreamConstants.CHARACTERS:
            String txt = reader.getText().trim();
            System.out.println("Element value :: "+txt);
            break;
        case XMLStreamConstants.END_ELEMENT:
            System.out.println("End Element :: " + reader.getLocalName());
            break;
    }
}
```

<br/>

#### 2. JAXB
```java
JAXBContext jaxbContext = JAXBContext.newInstance(Dto.class);
Unmarshaller jaxbUnmarshaller = jaxbContext.createUnmarshaller();
// XML을 Java 객체로 언마샬링
Dto dto = (Dto) jaxbUnmarshaller.unmarshal(new File("./FileName.xml"));
```

<br/>

#### 3. ObjectMapper
```java
ObjectMapper objectMapper = new ObjectMapper();
Dto dto = objectMapper.readValue(new File("./FileName.xml"), DTO.class);
```

<br/>

