---
title: JSP Servlet Forward
date: 2020-09-20 01:00:00 +0800
categories: [Develop, JSP-Servlet]
tags: [jsp-servlet]
---

# Forward
---
## forward란 무엇인가?
WAS의 서블릿이나 JSP가 요청을 받은 후  
추가적인 처리를 같은 웹 어플리케이션 안의 다른 서블릿이나 JSP에 맡기는 경우  
이를 위해 forward를 사용한다.  
<br><br>

## 작동원리

<img width="700px" src="https://user-images.githubusercontent.com/52627952/93671529-bd125380-fade-11ea-9d08-23162ad296f2.png">  

1. 웹 브라우저에서 Servlet1에게 요청을 보냄  
   
2. Servlet1은 요청을 처리한 후, 그 결과를 HttpServletRequest에 저장  

3. Servlet1은 결과가 저장된 HttpServletRequest와 응답을 위한 HttpServletResponse를  
같은 웹 어플리케이션 안에 있는 Servlet2에게 전송(forward)  

4. Servlet2는 Servlet1으로 부터 받은 HttpServletRequest와 HttpServletResponse를 이용하여  
요청을 처리한 후 웹 브라우저에게 결과를 전송  

<br><br>

## Redirect Forward의 차이점
`redirect`는 클라이언트가 서버에 요청을 보내고  
서버는 클라이언트에게 새로운 요청할 곳을 알려주는 것이다.  
그래서 `redirect` 후에는 url이 바뀌게 되는 것이다.  

`forward`는 서버에서 요청을 혼자 처리하는 것이 아니라  
다른 누군가에게 처리를 맡기는 것이다.  
클라이언트는 요청받은 Servlet1이  
혼자서 처리를 하는지 누군가에게 부탁을 하는지 알 필요가 없기 때문에  
`forward`가 실행되더라도 url이 바뀌지 않는 것이다.  

요청과 응답 측면에서 보면  
`redirect`는 여러번의 응답과 요청이 있다.  
맨 처음 클라이언트가 요청을 보내고  
`redirect`가 응답하여 클라이언트는 새로운 요청을 하게된다.  

하지만 `forward`는 요청과 응답이 한번만 존재한다.  
<br><br>

## 예제
1~6까지의 값을 `FirstServlet`에서 랜덤으로 만든 뒤  
`SecondSerlvet`에서 그 값 만큼 hello를 출력하는 예제  

`FirstSerlvet.java`  
```java
import java.io.IOException;
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/First")
public class FirstServlet extends HttpServlet {
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		int diceValue = (int)(Math.random() * 6) + 1; 

		// SecondServlet에게 값을 넘겨야 하기 때문에 request객체에 값을 맡긴다
        request.setAttribute("dice", diceValue);
        
		// request에 맡긴 값을 어디로 넘길건지 지정해준다
        RequestDispatcher requestDispatehcer = request.getRequestDispatcher("/Second");

		//forward 메소드에는 처음 요청할 때 받아왔던 request와 response 객체를 넘겨준다
        requestDispatehcer.forward(request, response);
	}
}

```  

`SecondServlet.java`  
```java


import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/Second")
public class SecondServelt extends HttpServlet {
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("<html>");
        out.println("<head><title>hello</title></head>");
        out.println("<body>");

		//getAttribute 메소드를 이용해 request에 맡겼던 값을 찾아온다
        int dice = (Integer)request.getAttribute("dice");
        out.println("dice : " + dice);
        for(int i = 0; i < dice; i++) {
            out.print("<br>hello");
        }
        out.println("</body>");
        out.println("</html>");
	}
}

```

<img width="700px" src="https://user-images.githubusercontent.com/52627952/93681414-21d2ab80-fae9-11ea-81aa-51205c52ab61.JPG">  
결과를 확인해보면 url이 First로 고정되어있고  
네트위크 상에서도 First만 나타나게 된다.  