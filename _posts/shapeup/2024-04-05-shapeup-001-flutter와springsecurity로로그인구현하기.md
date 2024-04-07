---
title: "[회원가입/로그인] 01. Flutter와 Spring Security로 로그인 구현하기 (with JWT)"
excerpt: "Flutter와 Spring Security로 로그인 구현하고 JWT 발급하기 01"

categories:
  - Shape-up
tags:
  - [develop, shapeup]

permalink: /develop/shapeup/flutter와springsecurity연동하여로그인구현

toc: true
toc_sticky: true

date: 2024-04-07
last_modified_at: 2024-04-05
---

## 들어가며

앱을 개발하는 <b>Flutter</b>와 서버를 개발하는 <b>Spring Boot</b>를 연동하여 소셜 로그인 기능을 구현하고자 합니다.  

가능한 방법은 다음 2가지로 확인되는데, 이 중에서 2번을 권장하고 있습니다. ([카카오 개발자 피셜](https://devtalk.kakao.com/t/topic/129605/4))   

1. Flutter에서는 로그인 요청만 하고 spring boot에서 OAuth2를 이용해 소셜 로그인을 처리하는 방법
2. Flutter에서 소셜 로그인 처리를 하고, 관련 유저 정보를 spring boot로 넘겨 token을 반환하는 방법

![001.png](/assets/images/posts_img/shapeup/001.png)

![002.png](/assets/images/posts_img/shapeup/002.png)

<br>

### Flutter에서 소셜 로그인 처리를 하는 이유

웹 서비스에서 소셜 로그인을 구현하는 방식에 <b>Redirect</b>를 사용합니다.  

하지만 "네이티브 앱 서비스"에서는 이 방법을 사용할 수 없기 때문에 <b>웹뷰</b>를 띄우고 로그인 처리 후 닫아야 하는 번거로움이 
있습니다.  

Flutter에서는 SDK로 소셜 로그인 기능을 제공하기 때문에 
"네이티브 앱 서비스" 구축 시에는 플러터 SDK가 하고, 
백엔드에서는 회원 정보를 받아 회원가입 및 로그인을 처리하는 것이 권장됩니다.  

<br>

> 이번 프로젝트에서는 2번처럼  
> Flutter에서 소셜 로그인 처리를 하고, spring boot로 넘겨 token을 반환하는 기능을 구현

<br>

## 프로젝트 설명

### 목표

먼저, 비밀번호를 사용하지 않을 것입니다.  

사용자 입장에서 소셜 로그인을 사용하기 때문에 또 다른 비밀번호를 입력하지 않도록 하기 위함입니다.  

아래와 같이 요청 정보와 함께 api를 요청하면 token 정보를 응답으로 보내주도록 구현해 보겠습니다.  

<br>

### 환경

| 라이브러리 이름        | 버전     | 설명           |
|-----------------|--------|--------------|
| spring boot     | 3.2.1  |  |
| spring security | 6.2.1  |  |
| jwt             | 0.12.3 |             |

<br>

### 기능 01 : 회원가입/로그인

#### 요청 본문

| 이름             | 타입     | 설명                      | 필수 |
|----------------|--------|-------------------------|----|
| socialId       | String | 소셜 로그인 서비스에서 제공하는 고유 ID | O  |
| socialProvider | String | 소셜 로그인 서비스              | O  |

#### 응답 본문

| 이름           | 타입     | 설명            | 필수 |
|--------------|--------|---------------|----|
| accessToken  | String | Access token  | O  |
| refreshToken | String | Refresh token | O  |

#### 서버 동작

![003.png](/assets/images/posts_img/shapeup/003.png)

<br>

### 기능 02 : 토큰 재발급

#### 요청 헤더

| 이름   | 타입     | 설명            | 필수 |
|------|--------|---------------|----|
| Authorization | String | Refresh Token | O  |

#### 응답 본문

| 이름           | 타입     | 설명            | 필수 |
|--------------|--------|---------------|----|
| accessToken  | String | Access token  | O  |
| refreshToken | String | Refresh token | O  |

#### 서버 동작

![005.png](/assets/images/posts_img/shapeup/005.png)

<br>

### 최종 아키텍처(의존 관계)

![004.png](/assets/images/posts_img/shapeup/004.png)

<br>

### Database : table

| 이름             | 타입     | 설명                        | 필수 |
|----------------|--------|---------------------------|----|
| uuid           | String | 회원 구분을 위한 uuid            | O  |
| social_id      | String | 소셜 로그인 고유 id              | O  |
| social_provider |   String     | 소셜 로그인 제공하는 곳 (ex. KAKAO) | O  |
| refresh_token  |   String     | JWT 재발급에 사용함              | O  |

<br>


다음 게시물부터 각 클래스의 역할과 코드를 알아보겠습니다.  

<hr>
<b>Reference</b>  
[spring boot와 flutter를 연동한 소셜로그인 구현(feat. spring security)-1](https://velog.io/@kgb/spring-boot%EC%99%80-flutter%EB%A5%BC-%EC%97%B0%EB%8F%99%ED%95%9C-%EC%86%8C%EC%85%9C%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EA%B5%AC%ED%98%84feat.-spring-security)  
[플러터 - 스프링 카카오 로그인 구현방법](https://devtalk.kakao.com/t/topic/129605/3)