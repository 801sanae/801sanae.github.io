---
layout: post
title: RestTemplate, WebClient
subtitle: RestTemplate, WebClient 코드 예시
tags: [RestTemplate, WebClient]
comments: true
---

### Overview

 Spring Framework에서 RESTful 웹 서비스(외부 API 사용방법)를 Spring 특정 버전 별로 여러가지 제공하였다.

- Spring 3.0 대는 RestTemplate

- Spring 4.0 대는 AsyncRestTemplate

- Spring 5.0 대는 WebClient

<br>

### RestTemplate는
1. 동기적이고 블로킹 방식으로 동작한다.
   
2. 대규모 트래픽을 처리하는 애플리케이션에서는 비효율적일 수 있다.

3. RestTemplate는 SingleTon 방식으로 사용하는것이 효율적이다.
    - @Bean 등록,
    - Builder? <<< 확인필요
4. 타임아웃 설정으로 응답지연에 대한 방어로직 필요.

5. 토큰 관련을 결합하여 사용.
   
<br>   

#### Restemplate 사용 방법 예시

```java
HttpHeaders headers = new HttpHeaders();
headers.set("X-Naver-Client-Id", id);
headers.set("X-Naver-Client-Secret", secret);
headers.setContentType(MediaType.APPLICATION_JSON);

HttpEntity request = new HttpEntity(headers);
MultiValueMap<String,String> param = new LinkedMultiValueMap<>();
param.add("query", query);
param.add("start", String.valueOf(start));
param.add("display", "20");
param.add("sort", "sim"); // sim 정확도, date 출간일순

URI uri = UriComponentsBuilder
        .fromHttpUrl(url)
        .queryParams(param)
        .encode()
        .build()
        .toUri();

ResponseEntity<NaverResultList> response = new RestTemplateBuilder().build()
        .exchange(uri, HttpMethod.GET, request, NaverResultList.class); //#

log.debug("{}", response.getBody());
```

<br>

### WebClient는
1. 비동기적이고 논블로킹 방식으로 동작한다.(동기적으로도 작동 가능)

2. 다양한 프로토콜 지원(HTTP/1.1뿐만 아니라 HTTP/2)

3. Spring WebFlux와 함께 사용되어 리액티브 프로그래밍 지원

4. 러닝커브가 있음.

<br>

#### WebClient 사용 방법 예시

```java
WebClient webClient = WebClient.builder() 
                .baseUrl(url)
                .defaultHeader("X-Naver-Client-Id", id)
                .defaultHeader("X-Naver-Client-Secret", secret)
                .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
                .build();

MultiValueMap<String,String> param = new LinkedMultiValueMap<>();
param.add("query", query);
param.add("start", String.valueOf(1));
param.add("display", "20");
param.add("sort", "sim"); 

NaverResultList response = webClient
        .method(HttpMethod.GET)
        .uri(uriBuilder -> 
                uriBuilder.queryParams(param).build())
        .retrieve() 
        .bodyToMono(NaverResultList.class) 
        .block();  
```