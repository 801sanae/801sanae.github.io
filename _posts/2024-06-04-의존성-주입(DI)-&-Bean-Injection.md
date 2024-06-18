---
layout: post
title: 의존성 주입(D.I) & Bean Injection
subtitle: Bean Injection은 생성자 주입(Constructor Injection)을 사용하자.
date: 2024-06-04
tags: [Bean Injection, Lombok. SpringFramework]
comments: true
---

# Conclusion

Bean Injection은 3가지(Felid Injection, Constructor Injection, Setter Injection)가 있으며, 일반적인 상황에서 Constructor Injection을 사용하자.

<br>

# Why


1. 순환참조 방지
   - Constructor Injection은 Build시 오류를 Emit하기 때문에 순환참조에 대한 사전 방지 가능
<br><br>

2. POJO 기반 테스트 용이
    ```java
    TestObject testObject = new TestObject();
    TestClass testClsass = new TestClass(testObject);
    testClass.functionTest();
    ```
<br>

3. immutable( final )
   - final 선언을 통해 Bean 생성시 생성한 instance의 불변성을 지킬수 있다.

<br>
  
# Lombok
lombok annotation을 통해 편리하게 사용 가능.

```java
@RequireArgsConstructor
public TestController(){
    private final TestService testService;
}
```

<br>

## 참고

[Spring Beans and Dependecy](https://docs.spring.io/spring-boot/docs/2.0.x/reference/html/using-boot-spring-beans-and-dependency-injection.html)
