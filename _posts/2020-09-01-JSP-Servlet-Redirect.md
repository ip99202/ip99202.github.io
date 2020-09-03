---
title: JSP Servlet Redirect
date: 2020-09-01 22:00:00 +0800
categories: [Develop, JSP-Servlet]
tags: [jsp-servlet]
seo:
  date_modified: 2020-09-02 01:53:03 +0900
---

# Redirect
---
## redirect란 무엇인가?
`redirect`란 서버가 클라이언트에게  
특정 URL로 이동하라는 요청을 보내는 것을 말한다.  
`redirect`는 HTTP 프로토콜로 정해진 규칙이다.  

그렇다면 이 `redirect`는 어디에 쓰일까?  
예를들어 게시판을 만든다고 생각해보자  
게시판에 글을 작성한 후 작성완료 버튼을 누르게되면  
다시 글 목록으로 돌아오도록 만들어야 한다.  
이러한 경우에 버튼을 누르면 `redirect`를 하도록 만들면 된다.  
<br><br>

## 작동원리

1. 서버가 리다이렉트 요청한 페이지를 받는다.  
   
2. 서버는 클라이언트에게 HTTP 상태코드 302로 응답한다.  
또한 헤더 내에 Location 값에 이동할 URL을 추가한다.  

3. 클라이언트는 서버로부터 받은 상태코드가 302라면 Location 해더값으로 재 요청을 보낸다.  

4. 브라우저의 주소창이 전송받은 URL로 바뀐다.  

<br>

실제 코드로 확인해보자  
`Redirect1.jsp`  
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	response.sendRedirect("Redirect2.jsp");
%>
```  

`Redirect2.jsp`  
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title></title>
</head>
<body>
	redirect된 페이지
</body>
</html>
```

<img width="700px" src="https://user-images.githubusercontent.com/52627952/91882530-0c264f00-ecbe-11ea-9e26-9a5e3da85e94.JPG">  

실행 결과를 보게되면  
`Redirect1.jsp`의 HTTP 상태코드가 302임을 확인할 수 있다.  
<br>
<img width="500px" src="https://user-images.githubusercontent.com/52627952/91882792-750dc700-ecbe-11ea-8420-e5d02d23453e.JPG">  
또한 응답헤더의 Location에 `Redirect2.jsp`가 있는것도 확인할 수 있다.  

위의 과정이 있기 때문에 현재 페이지는 `Redirect2.jsp`가 되는 것이다.