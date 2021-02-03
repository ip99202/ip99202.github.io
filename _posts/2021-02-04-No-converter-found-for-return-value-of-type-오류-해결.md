---
title: 스프링 오류 No converter found for return value of type
date: 2021-02-04 18:00:00 +0800
categories: [Spring]
tags: [Spring]
---

# No converter found for return value of type
오류의 전문은  
Resolved [org.springframework.http.converter.HttpMessageNotWritableException: No converter found for return value of type: class com.teamkph.kph.user.dto.UserInfoDto]  

회원정보를 조회하여 보여주는 부분에서 해당 오류가 발생했다.  
회원조회를 하여 결과를 DTO 형태로 리턴해주는데  
DTO 형태에서 json으로 변환이 안돼서 발생하는 오류인 것 같다.  

DTO는 아래와 같이 작성되어 있었다.
```java
public class UserInfoDto {

    private String name;

    private String email;

    private String role;

    @Builder
    public UserInfoDto(User user) {
        this.name = user.getName();
        this.email = user.getEmail();
        this.role = user.getRole();
    }
}
```
<br>

# 해결 방법
생각보다 간단하지만 어이없는 문제였다.  
`@Getter` 어노테이션이 빠져있어서 오류가 발생한 것이다.  
객체에서 json 형태로 어떤식으로 변환되는지 자세히는 모르겠지만  
Getter를 사용해 내용을 빼오는 형식인 것 같다.  
그래서 Getter가 빠지면 json으로 변환이 불가능했다.  

```java
@Getter
public class UserInfoDto {

    private String name;

    private String email;

    private String role;

    @Builder
    public UserInfoDto(User user) {
        this.name = user.getName();
        this.email = user.getEmail();
        this.role = user.getRole();
    }
}
```
위와같이 어노테이션 하나만 붙여주니 문제가 해결되었다.  