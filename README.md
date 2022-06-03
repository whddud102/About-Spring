# 📌 스프링 관련 내용 정리 📌

### 📜 어노테이션
**@Controller**
-	해당 어노테이션이 적용된 클래스는 “Controller”임을 나타내고, bean으로 등록되며 해당 클래스가 Controller로 사용됨을  Spring Framework에 알림.


### 📜 MVC 패턴이란?
-	Model – View – Controller의 약자
-	개발을 할 때 3가지 형태로 역할을 나누어서 개발하는 방법

### 📜 Model
-	어플리케이션이 무엇을 할 것인지 정의.
-	즉, DB와 연동하여 사용자가 입력한 데이터나 사용자에게 출력할 데이터를 다룹니다.

### 📜 View 
-	사용자에게 시각적으로 보여주는 부분

### 📜 Controller 
-	Model이 데이터를 어떻게 처리할지 알려주는 역할
-	사용자의 클라이언트로 부터 서버에 보낸 데이터가 있으면, 모델을 호출하기 전에 컨트롤러가 적절히 가공을 하고 모델을 호출
-	그런 다음 모델이 업무 수행을 완료하면 그 결과를 전달받고 다시 View에게 전달해주는 역할


### 📜 MVC 패턴을 사용하는 이유
-	사용자가 보는 페이지, 데이터 처리, 그리고 이 2가지를 중간에서 제어하는 컨트롤러, 이 3가지로 어플리케이션을 나누어서 만들면 각각 맡은바에만 집중할 수 있게 됩니다.
-	어플리케이션의 유지보수성, 확장성, 유연성이 증가하고, 중복코딩 문제도 사라짐

### 📜 Controller 란?
-	사용자의 요청을 처리한 후, 지정된 View에 Model 객체를 넘겨주는 역할 수행
-	즉, 사용자의 요청이 진입하는 지점
-	요청에 따라 어떤 처리를 할지 결정을 Service에 넘겨준다.
-	그 후, Service에서 실질적으로 처리한 내용을 View에 넘겨준다.
-	Spring에서 컨트롤러를 지정해주기 위한 어노테이션은 @Controller와 @RestController가 있다.

### 📜 @Controller
-	전통적인 Spring MVC의 컨트롤러
-	주로 View를 반환하기 위해 사용
-	@ResponseBody 어노테이션과 같이 사용하면 @RestController와 똑 같은 기능을 수행

### 📜 @RestController (Spring Restful Controller)
-	@Controller에 @ResponseBody 어노테이션이 붙은 효과를 지니게 된다.
-	주요 용도는 JSON/XML 형태로 객체 데이터 반환을 목적으로 한다.

### 📜 Service란 ?
-	**웹 요청 처리 과정**
-	1. Client가 Request를 보낸다. (Ajax, Axios, Fecth 등등 …)
-	2. Request URL 경로에 해당하는 알맞은 Controller가 요청을 수신 받는다 (@Controller, @RestController)
-	3. Controller는 넘어온 요청을 처리하기 위해 Service를 호출한다.
-	4. Service는 알맞은 정보를 가공하여 Controller에게 데이터를 넘긴다.
-	5. Controller는 Service의 결과물을 CLient에게 전달해준다.

-	Service가 알맞은 정보를 가공하는 과정을 ‘비즈니스 로직을 수행한다’ 라고 한다.
-	Service가 비즈니스 로직을 수행하고 데이터베이스에 접근하는 DAO를 이용해서 결과값을 받아 온다.

### 📜 DAO 란?
-	DB 서버에 접근하여 SQL문을 실행할 수 있는 객체

### 📜 JAP란?
-	매우 적은 양의 코드로 DAO를 구현할 수 있도록 해준다.
-	즉, 인터페이스를 만드는 것 만으로도 Entity (@Entity) 클레스에 대한 insert, Update, Delete, Select 를 실행할 수 있게 해줍니다.
-	But, JPA가 만들 수 있는 코드는 매우 가볍고 쉬운 쿼리를 대처하는 것이라서 쿼리 복잡도가 높아지면 사용하기가 어렵다.
-	이때, JPA으로만 DB를 제어한다면 수행능력이 SQL을 직접 사용해서 개발하는 것보다 못한 상황이 벌어 질 수도 있다.
-	그래서 JPA를 깊게 공부해서 JPA로 복잡도가 높은 쿼리를 짜거나 아니면 복잡도가 높은 곳은 DAO로 같이 사용한다고 한다.

### 📜 Consumes 와 produces 속성
-	Mapping을 할 때, 우리는 받고 싶은 데이터를 강제 함으로써 오류 상황을 줄일 수 있다.
-	이 때 사용하는 것 중 하나가 Media Types 이다.
-	Consumes는 들어오는 데이터 타입을 정의할 때 이용한다.
  
  예를 들어서 내가 json 타입을 받고 싶다면 아래와 같이 처리가 가능
  
  ```java  
  @PostMapping(path = "/pests", consumes = MediaType.APPLICATION_JSON_VALUE) 
  Public void addPet(@RequestBody Pet pet) {
      // ...
  }
  
  ```
  이렇게 처리를 하게 되면 해당 uri를 호출하는 쪽에서 헤더에 보내는 데이터가 json 이라는 것을 명시해야 한다.
  ```
  Content-Type:application/json
  ```
  
  
  produces는 반환하는 데이터 타입을 정의한다.
  ```java
  @GetMapping(path = "/pets/{petId}", produces = MediaType.APPLICATIOPN_JSON_VALUE)
  @ResponseBody 
  public Pet getPet(@PathVariable String petId) {
    // ...
  }
  ```
  
  이럴 경우 반환 타입이 json으로 강제된다.
  내가 보내야 하는 타입이 정해져 있다면 해당 부분을 정의하면 된다.
  요청하는 입장에서 특정 타입의 데이터를 원한다면 아래와 같은 내용을 헤더에 추가한다.
  
  ```
  Accept:application/json
  ```
  
  요약정리 : 
  - consumes는 클라이언트가 서버에게 보내는 데이터 타입을 명시한다.
  - produces는 서버가 클라이언트에게 반환하는 데이터 타입을 명시한다.
  
<br>  
  
**📜 Spring의 Controller의 가장 편리한 기능은 파라미터가 자동으로 수집되는 기능이다**
- 이 기능을 이용하면 setter 메서드가 있는 객체가 컨트롤러의 파라미터로 지정이되면, 해당 객체의 setter 메서드가 자동으로 사용되어 파라미터를 수집해온다.
- Controller가 파라미터를 수집하는 방식은 파라미터 타입에 따라 자동으로 변환하는 방식을 이용합니다.
- 예를 들어 int 타입으로 선언된 age 필드는 자동으로 숫자로 변환되어 수집이 됩니다.
- 만일 기본 자료형이나 문자열 등을 이용한다면 파라미터 타입만을 맞게 선언해주는 방식을 사용할 수 있다.


### 📜 Model 이라는 데이터 전달자
- Controller의 메서드를 작성할 때는 특별하게 Model 이라는 타입을 파라미터로 지정할 수 있습니다. Model 객체는 JSP에 컨트롤러로 생성된 데이터를 담아서 전달하는 역할을 하는 존재입니다.
- 이를 이용해서 jsp와 같은 뷰로 전달해야하는 데이터를 담아서 보낼 수 있다.
- 메서드의 파라미터에 Model 타입이 지정된 경우에는 스프링은 특별하게 Model 타입의 객체를 만들어서 메서드에 주입하게 된다.
- 이처럼 특정 타입의 파라미터는 스프링에서 알아서 자동으로 객체를 넣어주는 것 같다.


### 📜 Servlet 이란?
- **동적인 웹 페이지를 만들 때 사용**되는 자바 기반의 **웹 어플리케이션 프로그래밍 기술**이다.
- 웹을 만들때는 다양한 요청(Reuqest)과 응답(Response)이 있기 마련이고 이러한 요청과 응답을 일일이 처리하려면 굉장히 번거로운데, 서블릿은 이러한 웹 요청과 응답의 흐름을 간단한 메서드 호출만으로 체계적으로 다룰 수 있게 해주는 기술이다.


### 📜 Servlet Container 란?
- 말 그대로 **서블릿을 담고 관리해주는 컨테이너**. 서블릿 컨테이너는 구현되어 있는 servlet 클래스의 규칙에 맞게 **서블릿을 관리**해주며, **클라이언트에서 요청**을 하면 컨테이너는 **HttpServletRequest, HttpServletResponse** 두 객체를 생성하여 post, get 여부에 따라 **동적인 페이지를 생성하여 응답을 보낸다**.

### 📜 HttpServletRequest
- Http 프로토콜의 **request 정보를 서블릿에게 전달하기 위한 목적으로 사용**되며 **헤더 정보, 파라미터, 쿠키, URI, URL 등의 정보**를 읽어 들이는 메서드와 Body의 Stream을 읽어 들이는 메서드를 가지고 있다.

### 📜 HttpServletResponse
- WAS는 어떤 클라이언트가 요청을 보냈는지 알고 있고, **해당 클라이언트에게 응답을 보내기 위한 HttpServletRequest 객체를 생성하여 서블릿에게 전달**하고 **이 객체를 활용하여 content Type, 응답 코드, 응답 메시지 등을 전송**한다.


### 📜 View의 참조 위치
- 기본적으로는 **src/main/webapp/WEB-INF/spring/Secret/servlet-context.xml**에 있는 **InternalResourceViewResolver가 설정한 prefix와 suffix 정보가 적용된 경로**의 .jsp 파일을 찾는다.
- 보통은 **/WEB-INF/view/*.jsp** 경로

### 📜 JSP 지시어 
- 지시어는 해당하는 JSP 파일의 속성을 기술 하는 곳으로, JSP 컨테이너에게 해당 페이지를 어떻게 처리해야 하는지 전달하기 위한 내용을 담는다.
- 크게 아래 세 종류의 지시어로 나눌 수 있으며, 각각의 속성이 다르다.
    1. page
    2. include
    3. taglib

    #### page 지시어
    - 현재의 JSP 페이지를 컨테이너에서 처리하는데 필요한 각종 속성을 기술하는 부분
    - 형식 지정에 필요한 contentType, 자바 클래스 사용에 필요한 import, 오류 페이지 관리에 필요한 errorPage, isErrorPage 등을 가장 많이 사용.
    <br>
    
    ![image](https://user-images.githubusercontent.com/59597955/171810006-5f261e1f-b268-41c8-98bf-6c6de49365e6.png)
    
    <br>
    🔎 사용 예
    
    ```JSP 
    <% page contentType="text/html;charset=UTF-8" errorPage="error.jsp" %>
    <% page import="java.util.*"%>
    ```

    #### include 지시어
    - 현재 JSP 파일에 다른 HTML이나 JSP 문서를 포함하기 위한 기능
    - include 액션과 비슷한 기능
    - 여러 페이지에 공통으로 들어가는 내용을 관리할 떄 유용
    - include 하는 개별 파일은 개별적으로 컴파일된 클래스를 생성하지 않는다.
    - 각각의 파일을 모두 포함한 뒤 하나의 컴파일 클래스를 생성함.
    <br>
    🔎 사용 예
    
    ```JSP 
    <% include file="파일명.jsp"%>
    ```
    
    #### taglib 지시어
    - JSP 기능을 확장하기 위해 만들어진 커스텀 태그 라이브러리를 JSP 파일에서 사용하기 위한 지시어
    - 태그 라이브러리는 복잡하고 반복되는 작업을 간단한 태그 형태로 만들어 파일 구성의 일관성을 유지하며, 유지보수의 효율성을 높이려는 목적으로 사용됨
    - 한편으로는, 커스텀 태크 라이브러리를 너무 많이 사용하면 호환성이 떨어지고, 태그 라이브러리에 대한 이해를 위해 별도 학습이 필요하다는 문제점이 있음.

     <br>
    🔎 사용 예
    
     ```JSP
     <% taglib uri="/META-INF/mytag.tld" prefix="mytag"%>
     ```
     
     ### Spring tiles (스프링 타일즈)
     - 뷰페이지의 jsp들을 상단, 사이드, 메인, 하단을 설정 상태로 include 처리해주는 구조의 템플릿을 말한다.
     - 페이지들을 일괄 관리할 수 있고, 공통사용하는 부분들을 매번 따로 등록 해주지 않아도 되기 때문에 편리하다.
     
     ### 설정 방법
     
     **1. pom.xml - 라이브러리 추가**
     
     ![image](https://user-images.githubusercontent.com/59597955/171816843-54bc2982-6e88-418a-af8e-41ca5b82d244.png)

     ![image](https://user-images.githubusercontent.com/59597955/171816904-ad1e1c4b-047a-4bb4-97a2-4eac06b6e527.png)
    
     **2. 상단, 사이드, 메인, 하단으로 사용할 jsp들을 생성, 나중에 타일즈 설정파일을 통해 연결 예정**. 
     
     WEB-INF/views/tiles라는 디렉토리를 만들고 안에 각각 JSP들을 구성
     
     ![image](https://user-images.githubusercontent.com/59597955/171817133-bee7ed36-4280-44e5-8bff-5e8ad09b68f0.png)


    **3. servlet-context.xml에 InternalResourceViewResolver를 변경 및 tiles설정을 추가.**

    servlet-context.xml
    
    ![image](https://user-images.githubusercontent.com/59597955/171817385-2db231b6-6329-494b-8177-98c124626a91.png)

    order 부분을 유의해서 설정. tiles가 우선순위가 되도록 해주어야 함.
    
    servlet-context.xml을 추가했으면 beans:value에 처리한 tiles-define.xml을 생성하고 설정
    
    ![image](https://user-images.githubusercontent.com/59597955/171818037-0101254b-68c3-4a36-a891-f1cd02c8ab08.png)

   
     **4. tiles-define.xml 설정**
     
     ![image](https://user-images.githubusercontent.com/59597955/171818307-3807553c-14e3-48b4-8556-e82e286788bd.png)
    
     - 이 부분에서 상단, 사이드, 바닥 메인으로 사용할 템플릿 등을 설정
     - definition에서 name은 변수처럼 사용할 이름을 지정
     - template은 사용할 jsp를 지정
     - 그 내부에는 따라오는 헤더, 사이드, 바닥 jsp들을 처리하고 마찬가지로 name은 사용할 이름, value에는 실제로 들어갈 jsp를 입력

     - 먼저 생성해 놓은 JSP 중 tiles-layout.jsp를 메인으로 사용하고 각각 header sidebar, footer 부분을 영역별로 사용할 예정.
     - 아래서 extends 부분은 위에서 선언한 jsp를 상속받아 실제로 사용하는 페이지에 이식하고 body 영역은 실제 페이지를 사용하기 위해 설정하는 부분으로 메인 body로 쓸 jsp 영역을 설정할 수 있으며, {1}/{2}.jsp을 통해 board/board.jsp로 갈 때 자연스럽게 이식되어 사용할 수 있게 합니다.
     
     ![image](https://user-images.githubusercontent.com/59597955/171819913-39883725-11dd-4149-ade3-1a67b5bcbf84.png)

    ![image](https://user-images.githubusercontent.com/59597955/171819946-08b86a78-9fee-41d8-8e95-d7f00f15eb47.png)

    ![image](https://user-images.githubusercontent.com/59597955/171819979-289e3a42-7728-4549-ba3b-2f8720198eb4.png)

    ![image](https://user-images.githubusercontent.com/59597955/171820116-2fe1a38e-0c2e-4084-ba25-c2d530f06cb1.png)

    공통변수 처리부분이나 script부분은 무시해도 된다.

    tiles-define.xml에서 각각 JSP들을 불러왔으니 사용. 해당페이지에서 원하는 영역에 삽입을 할 수 있음.

    **<tiles:insertAttribute name="사용할 이름" />** 을 통해 적용. 
    
    #### @Transactional(readOnly = true)
    - 읽기 전용으로 트랜잭션을 설정
    - 조회한 데이터를 return 한다고 해도 의도치 않게 데이터가 변경되는 일을 사전에 방지해줌.
    - DB 서버의 부하를 줄이고 약간의 최적화를 할 수 있다.
    - 코드를 접하는 사람들이 직관적으로 보기에도 해당 메서드는 READ 동작만 수행할 것이라고 예상 가능.
