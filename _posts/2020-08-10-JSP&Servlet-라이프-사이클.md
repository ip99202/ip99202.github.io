---
title: JSP/Servlet 라이프 사이클
date: 2020-08-10 17:00:00 +0800
categories: [Develop, JSP&Servlet]
tags: [jsp&servlet]
---

## 라이프 사이클이란?  
어떤 객체의 생성부터 소멸까지의 과정을 Life Cycle이라고 한다.  
그 중에 JSP와 Servlet의 라이프 사이클을 살펴보자.  
<br>


## 라이프 사이클을 살펴보기 위한 코드  
```java
import java.io.IOException;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/LifeCycle")
public class LifeCycle extends HttpServlet {
	private static final long serialVersionUID = 1L;
       

    public LifeCycle() {
    	System.out.println("LifecycleServlet 생성!!");
    }

	public void init(ServletConfig config) throws ServletException {
		System.out.println("init test 호출!!");
	}

	public void destroy() {
		System.out.println("destroy 호출!!");
	}

	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("service호출!!");
	}
}
```
`init`은 서블릿이 처음 만들어졌을 때 실행되는 함수이고  
`destroy`는 서블릿이 종료 되었을 때  
`service`는 서블릿이 `init`후 동작할 때 실행되는 함수이다.  
<br>


## 코드의 실행 결과 & 라이프 사이클의 과정  
<img width="400px" src="https://user-images.githubusercontent.com/52627952/89768461-969fe680-db36-11ea-989b-1e62c1c27205.JPG">  

처음 서블릿이 실행되면  
서버는 메모리에 해당 클래스가 있는지 확인 후  
없다면 서블릿을 생성하게 된다.  
이 때 생성자에 넣어 둔 `LifecycleServlet 생성!!`이라는 메시지가 뜬 것이다.  
그리고나서 `init()`메서드를 호출하고  
`service()`메서드를 호출한다.  

새로고침을 해보니 다시 `service()` 메소드가 실행된 것을 확인할 수 있다.  
서블릿은 서버에 서블릿 객체를 여러개 만드는 것이 아니라서  
요청이 여러번 들어온다고 해서 여러번 객체를 생성하지 않는다.  
그래서 `service()` 메서드만 실행된 것이다.  

코드를 조금 고치고 다시 저장해보니  
이번에는 `destroy` 메서드가 호출되었다.  
코드를 수정해서 지금 메모리에 올라가있는 객체는 더 이상 사용할 수 없기 때문에  
`destroy` 메서드가 호출되어 서블릿이 종료된 것이다.  

이후 다시 새로고침을 해보니  
처음 생성했던 것처럼 순서대로  
서블릿이 생성되고 `init` `service`가 호출되었다.  
<br>


## service() 메서드란?  
위에 라이프 사이클에서 확인했듯이  
요청이 들어왔을 때 응답해야 되는 모든 내용은  
`service()` 메서드에 작성해야 한다.  

하지만 `service()` 메서드는 실제 코드 작성을 할 때 본적이 거의 없을것이다.  
보통 서블릿을 작성할 때 `doGet()`과 `doPost()` 메서드를 이용한다.  
사실 WAS는 `service()` 메서드만 실행시킨다.  

하지만 우리가 `service()` 대신 `doGet()`과 `doPost()` 메서드만 오버라이드했기 때문에  
`service()`의 부모인 `HttpServlet의 service()`가 실행된다.  

`HttpServlet의 service()`은  
클라이언트의 요청이 `GET`일 경우에는 가지고있는 `doGET()` 메서드를 호출하고  
클라이언트의 요청이 `POST`일 경우에는 가지고있는 `doPOST()` 메서드를 호출한다.  

그렇기때문에 우리가 `service()` 메서드를 오버라이드 하지 않더라도  
`doGet()`이나 `doPOST()`가 정상적으로 실행될 수 있었던 것이다.
