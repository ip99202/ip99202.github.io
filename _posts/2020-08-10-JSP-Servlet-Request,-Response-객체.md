---
title: JSP Servlet Request, Response 객체
date: 2020-08-11 19:40:00 +0800
categories: [Develop, JSP&Servlet]
tags: [jsp&servlet]
---

# Request, Response  
<img width="700px" src="https://user-images.githubusercontent.com/52627952/89890005-8eff4100-dc0d-11ea-862e-cb93fe8e53b7.jpg">  

## Request, Response 과정
1. 웹 브라우저에 URL을 입력
2. 웹 브라우저는 도메인과 포트 번호를 이용해서 서버에 접속
3. path 정보, 클라이언트의 IP 등의 요청 정보를 서버에 전송
4. WAS가 `HttpServletRequest`라는 객체와 `HttpServletResponse`라는 객체를 생성
5. 생성된 두 개의 객체를 요청 정보에 있는 path로 매핑된 서블릿에게 전달
6. 전달된 객체는 `service()`, `doGet()`, `doPost()` 같은 메서드에 파라미터로 전달되서 사용  
<br><br>


## HttpServletRequest  

* http 프로토콜의 request 정보를 서블릿에게 전달하기 위한 목적으로 사용한다.
* 헤더정보, 파라미터, 쿠키, URI, URL 등의 정보를 읽는 메소드를 가지고 있다.
* Body의 Stream을 읽어 들이는 메소드를 가지고 있다.  
<br><br>

## HttpServletResponse

* WAS는 요청을 보낸 클라이언트에게 응답을 보내기 위한 HttpServletResponse 객체를 생성하여 서블릿에게 전달한다.
* 서블릿은 해당 객체를 이용하여 content type, 응답코드, 응답 메시지등을 전송한다.
