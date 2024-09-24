---
layout: post
title: AWS SNS를 이용한 서버 푸쉬 개발에 대한 기록(1/2)
subtitle: AWS SNS SDK를 활용한 서버 푸쉬 개발기록
tags: [AWS, SNS, FCM]
comments: true
---

### Overview
AWS에는 여러가지 서비스가 많다. 그중 SNS Simple Notification Service의 SDK를 통해 서버 푸쉬, Notification을 개발했던 내용을 기록하고자한다.
AWS SNS는 Apache Kafka랑 뭔가 비슷한느낌이다. 토픽을 등록하고, 토픽을 구독(Subscribe)하고 토픽 구독한 대상에게 publish를 하는 구조이며, publish를 하게 되면 Push Notification이 나간다?라고 표현하겠다.
토픽 또한 여러가지 기능을 제공하지만 특히 Push Notification에 대한 내용을 기술하고자한다.

### 구조
SNS는 아래와 이미지와 같은 구조를 가진다.
1. Application - to application(A2A)
<img src="../assets/img/A2A.png"  width="800px"/>
2. Application - to person(A2P)
<img src="../assets/img/A2P.png"  width="800px"/>
<br>

### 설정

#### 1.build.gradle 추가.

```groovy
// BOM for AWS SDK For Java
implementation platform('software.amazon.awssdk:bom:2.20.56')

// We only need to get SNS SDK in our case
implementation 'software.amazon.awssdk:sns' 
```
<br>
#### 2.IAM - 엑세스 관리 - 사용자

<img src="../assets/img/iam1.png"  width="800px"/>

<img src="../assets/img/iam2.png"  width="800px"/>

<img src="../assets/img/iam3.png"  width="800px"/>
<br>
#### 3.생성 후 해당 사용자정보 확인 - 액세스 키 칸들기

<img src="../assets/img/key1.png"  width="800px"/>

<img src="../assets/img/key2.png"  width="800px"/>

<img src="../assets/img/key3.png"  width="800px"/>

> 액세스 키와, 비밀 엑세스 키를 csv로 보관 필요..

### SnsClient Bean 생성
#### 1. 로컬테스트시
```java
@Getter
@Configuration
public class AWSConfig {

    @Value("${cloud.aws.credentials.access-key}")
    private String awsAccessKey;

    @Value("${cloud.aws.credentials.secret-key}")
    private String awsSecretKey;

    @Value("${cloud.aws.region.static}")
    private String awsRegion;

    @Bean
    public SnsClient getSnsClient(){
        return SnsClient.builder()
                .credentialsProvider(getAwsCredentials(awsAccessKey,awsSecretKey))
                .region(Region.of(awsRegion))
                .build();
    }

    public AwsCredentialsProvider getAwsCredentials(String accessKey, String secretKey){
        AwsBasicCredentials awsBasicCredentials = AwsBasicCredentials.create(accessKey,secretKey);
        return () -> awsBasicCredentials;
    }
}
```
> SnsClient를 통해 전반적인 Api 호출을 진행한다. 
<br>
```groovy
cloud:
  aws:
    stack:
      auto: false
    credentials:
      #KMY
      access-key: AKIA4PCVVHWUFV4CGF76
      secret-key: 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
    region:
      auto: false
      static: ap-northeast-2
```
> IAM에서 생성한 User의 AccessKey, SecretKey를 Setting

#### 2. 서버 배포시 추천
```java
@Getter
@Configuration
public class AWSConfig {

    @Value("${cloud.aws.region.static}")
    private String awsRegion;

    @Bean
    public SnsClient getSnsClient(){
        return SnsClient.builder()
                .credentialsProvider(InstanceProfileCredentialsProvider.create())
                .region(Region.of(awsRegion))
                .build();
    }
}
```
<img src="../assets/img/iamrole.png"  width="800px"/>
[https://docs.aws.amazon.com/ko_kr/sdk-for-java/latest/developer-guide/ec2-iam-roles.html](https://docs.aws.amazon.com/ko_kr/sdk-for-java/latest/developer-guide/ec2-iam-roles.html)<br>
[https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/ec2-iam-roles.html](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/ec2-iam-roles.html)

<br>

<img src="../assets/img/policy1.png"  width="800px"/>

<img src="../assets/img/policy2.png"  width="800px"/>

<img src="../assets/img/policy3.png"  width="800px"/>

<img src="../assets/img/policy4.png"  width="800px"/>

<img src="../assets/img/policy5.png"  width="800px"/>

<br>

[참고](https://docs.quarkiverse.io/quarkus-amazon-services/dev/amazon-sns.html)

## 마치며
AWS의 많은 서비스중 SDK를 이용하여 처음 사용해본 SNS를 정리하였다. 많은 시행 착오와 검색을 통해 진행하였는데, 결국 공식 홈페이지의 Document가 제일 도움이 되었다.
인상깊었던건, 서버(EC2)에 권한?을 위임하여 SnsClient를 생성하는 점이 생소했는데 알고보면 이미 많은 사이트에서 이런 방식으로 운영, 개발되고 있음을 알게 되었다.(github action,,)
2편에서는 사용한 SDK에 관련된 부분을 작성할 예정이다.
