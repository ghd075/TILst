# 실전 JSP(renew ver.)
> [인프러 강의](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-jsp_renew#)를 듣고 내용 정리

> 이 강의는 JSP를 입문하기 전에 보면 딱 좋을 강의라고 생각한다.

### 웹프로그램이란?

인터넷 서비스를 이용해서 서로 다른 구성들(PC등)이 통신할 수 있는 프로그램이다.

### 프로토콜이란

* 통신을 하기위한 규약으로 HTTP, FTP, SMTP, POP 등이 있다.
* <img width="90%" src="../img/procol.jpg"/>

## JSP(Java Server Page)

* HTML 파일 내에 Java 언어를 삽입한 문서
* 동적 웹 어플리케이션 컴포넌트
* .jsp 확장자
* 클라이언트의 요청에 동적으로 작동하고, 응답은 html을 이용
* jsp는 servlet으로 변환되어 실행

### Servlet(Server Applet)

* Java 언어로 이루어진 웹 프로그래밍 문서
* 동적 웹 어플리케이션 컴포넌트
* .java 확장자
* 클라이언트의 요청에 동적으로 동작하고, 응답은 html을 이용
* HttpServlet 클래스를 상속받는다.
  * HttpServlet 클래스 : HTTP 프로토콜의 요청과 응답 처리를 편리하게 한다.

### JSP 처리 과정

JSP가 실행되면 컴파일 후 Servlet코드로 변환되어 웹 어플리케이션 서버에서 동작되면서 필요한 기능을 수행한 뒤, 데이터와 웹페이지를 함께 클라이언트로 보여준다.

1. 클라이언트가 어떤 동작을 함으로써 hello.jsp를 요청
2. JSP 컨테이너가 JSP 파일을 읽는다.
3. JSP 컨테이너가 Generete(변환) 작업을 통해 Servlet(.java) 파일을 생성한다.
4. .java 파일은 다시 .class 파일로 컴파일된다.
5. Execute(실행)을 통해 HTML 파일을 생성하여 JSP 컨테이너에게 전달한다.
6. JSP는 HTML 프로토콜을 통해 HTML 페이지를 클라이언트에게 전달한다.

### Servlet 맵핑이란?

* 서블릿의 URL 주소를 보다 쉽고 간결하게 표시하기 위해서 다른 이름을 붙여주는 것

* web.xml 파일 이용

  ```xml
  <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
    <welcome-file-list>
      <welcome-file>index.html</welcome-file>
      <welcome-file>index.htm</welcome-file>
      <welcome-file>index.jsp</welcome-file>
      <welcome-file>default.html</welcome-file>
      <welcome-file>default.htm</welcome-file>
      <welcome-file>default.jsp</welcome-file>
    </welcome-file-list>
    
    <servlet>
    	<servlet-name>servletEx</servlet-name>
    	<servlet-class>com.servlet.ServletEx</servlet-class>
    </servlet>
    <servlet-mapping>
    	<servlet-name>servletEx</servlet-name>
    	<url-pattern>/SE</url-pattern>
    </servlet-mapping>
     
  </web-app>
  ```

* Java Annotaion을 이용

  ```java
  @WebServlet(name="servletEx", urlPatterns= {"/Hello", "/SE"})
  public class ServletEx extends HttpServlet {
  
  	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  		
  		PrintWriter out = response.getWriter();
  		out.print("<html>");
  		out.print("<head><title>ServletEX</title></head>");
  		out.print("<body>");
  		out.print("Hello Servlet~");
  		out.print("</body>");
  		out.print("</html>");
  	}
  
  }
  ```

### Servlet Life-Cycle

* 서블릿은 최초 요청시 객체가 만들어져 메모리에 로딩되고, 그 이후의 요청에는 기존 객체를 재활용한다.
* <img width="90%" src="../img/servlet_life_cycle.jpg"/>
  * 서블릿 객체 생성(최초에 한 번)
    * 선처리 할 경우 (@PostConstruct)
  * Init() 호출(최초에 한 번, 초기화 단계)
  * Service(), doGet(), doPost() 호출 (요청 시 매 번, 작업 단계)
  * destroy() 호출(마지막 한 번, 자원해제(servlet수정시, 서버 재가동시), 종료 단계)
    * 후처리 할 경우 (@PreDestrooy)

### form 데이터 처리

* form 태그에서 주요한 속성
  * action : 수신 대상
  * method : 전송 방식
* Get 방식
  * 클라이언트에서 서버로 데이터를 전달할 때, **주소 뒤에 '이름'과 '값'이 결합된 스트링 형태**로 전달
  * 주소창에 쿼리 스트링이 그대로 보여지기 때문에 **보안성이 떨어진다.**
  * 전달되는 데이터가 **255개의 문자**를 초과하면 문제가 발생할 수 있다.
  * **Post방식보다 상대적으로 전송 속도가 빠르다.**
* Post 방식
  * 일정 크기 이상의 데이터를 보내야 할 때 사용한다.
  * 서버로 보내기 전에 인코딩하고, 전송 후 서버에서는 다시 디코딩 작업을 한다.
  * 주소창에 전송하는 데이터의 정보가 노출되지 않아 **Get방식에 비해 보안성이 높다.**
  * 속도가 Get방식보다 느리다.
  * 쿼리스트링(문자열)데이터 뿐만 아니라, 라디오 버튼, 텍스트 박스 같은 **객체들의 값도 전송가능**

### JSP 주요 스크립트

* <%! %> : 선언태그 
  * JSP 페이지에서 Java의 멤버변수 또는 메서드를 선언
* <%-- --%> : 주석태그
  * JSP 주석은 jsp 파일이 서블릿 파일로 변환될 때 제외됨
* <% %> : 스크립트릿태그
  * JSP 페이지에서 Java 코드를 넣기 위한 태그
* <%= %> : 표현식 태그
  * Java의 변수 및 메서드의 반환값을 출력하는 태그
* 지시어 : 서버에서 jsp 페이지를 처리하는 방법에 대한 정의
  * <% page %> : 페이지 기본 설정
  * <% include %> : include file 설정
  * <% taglib %> : 외부라이브러리 태그 설정

- - -
### [뒤로 가기](./../../..)
