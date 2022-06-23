# 📌 스프링 관련 내용 정리 📌

### DI(Dependency Injection)란?
- DI(의존성 주입)란, 클래스간의 의존관계를 스프링 컨테이너가 자동으로 연결해주는 것을 말한다.
* Dependancy란, 객체가 다른 객체와 상호작용하는 것을 말한다.
* 클래스 A가 클래스 B,C와 상호작용한다면 객체 A는 객체B,C와 의존관계이다.

- Inversion of Control (제어 역전) 이라고도 하는 의존 관계 주입(Depenecy Injection)이라고도 하며, 어떤 객체가 사용하는 의존 객체를 사용자가 직접 만들어서 사용하는게 아니라, 주입 받아 사용하는 방법이다. (new 연산자를 이용해서 객체를 생성하는 것이라고 생각하면 된다)


**의존성 예시와 정리**

```java
@Service
public class BookService {

    private BookRepository bookRepository;

    public BookService(BookRepository bookRepository) {
        this.bookRepository = bookRepository;  // 생성시 BookRepository가 필요, BookRepository와 의존 관계
    }
}

@Service
public class BookRepository {
      // DI Test
}
```

- 위 코드와 같이 BookService 클래스가 만들어 지기 위해서는 BooRepository 클래스를 필요로 한다. 이것을 BookService 클래스는 BookRepository 클래스의 의존성을 가진다 라고 한다. 
- 이와 같이 코드를 설계하였을 때, 코드의 재활용성이 떨어지고, 위 예제에서 BookRepository 클래스가 수정 되었을 때, BookService 클래스도 함께 수정해야하는 문제가 발생한다.
- 즉, 결합도(coupling)가 높아지게 된다.
- 그리고 위의 코드에서, BookRepository라는 클래스를 직접 new를 사용하여 객체를 주입하는 것이 아니라 생성자를 사용하여 주입받는 것을 Inversion of Control 이라고 한다.

**강한 결합**
- 객체 내부에서 다른 객체를 생성하는 것은 강한 결합도를 가지는 구조이다. A 클래스 내부에서 B 라는 객체를 직접 생성하고 있다면, B 객체를 C 객체로 바꾸고 싶은 경우에 A 클래스도 수정해야 하는 방식이기 때문에 강한 결합이다.

**느슨한 결합**
- 느슨한 결합은 외부에서 생성된 객체를 인터페이스를 통해서 넘겨받는 것이다. 이렇게 하면 결합도를 낮출 수 있고, 런타임시에 의존관계가 결정되기 때문에 유연한 구조를 가진다.

- 추가적으로 BookService와 BookRepository가 둘다 Bean으로 등록되어 있을 때 BookService의 생성자만 만들어주면 스프링 IoC 컨테이너가 BookRepository에 의존성 주입을 알아서 해준다.(스프링 4.3 이후부터는 생성자가 하나인 경우는 @Autowired를 사용하지 않아도 된다)

- 생성자가 하나인 경우는 자동으로 @Autowired 어노테이션이 적용이 되고, 생성자의 매개변수가 bean으로 등록이 되어 있다면 생성시점에, 스프링에서 알아서 의존성을 주입해준다.


- 스프링의 의존성 주입이 없다면 직접 new 연산자로 의존성을 주입해주어야 한다.
- 
```java
import org.junit.Test;

public class BookServiceTest {

    @Test
    public void save() {
    	// 직접 BookService에 필요한 BookRepository 생성 후, 의존성 주입
        BookRepository bookRepository = new BookRepository();
        BookService bookService = new BookService(bookRepository);	
    }
}
```
- 이런식으로 코드를 사용하지 않고 스프링이 제공하는 IoC 컨테이너를 사용하는 이유는 지금까지의 개발자의 노하우와 여러 가장 좋은 기능들이 IoC 컨테이너에 쌓여있기 때문에 사용하는 것이다. 

### 스프링 IoC 컨테이너란? 
- 가장 중요한 인터페이스는 BeanFactory, ApplicatonContext이다
- 애플리케이션 컴포넌트의 중앙 저장소이다.
- bean 설정 소스파일로 부터 bean 정의를 읽어들이고, bean을 구성하고 제공한다.
- bean들의 의존 관계를 설정해준다.(객체의 생성을 책임지고, 의존성을 관리한다)

![image](https://user-images.githubusercontent.com/59597955/172166054-7277bb38-c174-468f-84d6-616b1a535000.png)



**POJO**
![image](https://user-images.githubusercontent.com/59597955/172166177-7f3f8870-0473-4a8f-bea3-a95237ca0e10.png)

- Plain Old Java Object, 직역하면 오래된 방식의 자바 객체라는 뜻으로 평범하고 단순한 자바 클래스를 의미한다. 
- 종속되지 않는다는 것은 코드를 간결히 할 수 있고, 객체지향 설계를 충실히 이행하고 있음을 보여준다.

- 스프링 특징을 보다보면 POJO라는 단어가 존재한다. 과거에는 자바로 웹 어플리케이션을 개발하기 위해서는 Servlet 클래스를 상속받아서 사용했다. 이 Servlet 클래스는 POJO가 아닌 것이다. 
- 즉 Servlet 클래스를 작성하지 않고, Servlet 클래스의 상속 없이 POJO만으로 웹 어플리케이션을 개발할 수 있다는 것이 스프링의 특징이다. 

### 스프링 컨테이너 종류

**BeanFactory**

- 스프링 빈 컨테이너에 접근하기 위한 최상위 인터페이스이다.

- Bean 객체를 생성하고 관리하는 인터페이스이다. 디자인패턴의 일종인 팩토리 패턴을 구현한 것이다. 
- BeanFactory 컨테이너는 구동될 때 Bean 객체를 생성하는 것이 아니라. 클라이언트의 요청이 있을 때(getBean()) 객체를 생성한다.

**ApplicationContext**
- ListableBeanFactory(BeanFactory에 하위 인터페이스이며, Bean을 Listable하게 보관하는 인터페이스를 말한다. 
- 대표적으로 DefaultListableBeanFactory 클래스)를 상속하고 있으며, 여러 기능(ResourceLoader, ApplicationEventPublisher, MessageSource, Bean Lifecycle)을 추가로 제공한다.

- BeanFactory를 상속받은 interface이며, ApplicationContext 컨테이너는 구동되는 시점에 등록된 Bean 객체들을 스캔하여 객체화한다


### 빈(Bean)이란 무엇인가?

- 스프링 IoC 컨테이너가 관리하는 객체
- 빈으로 등록됐을 때의 장점
	-스프링 IoC 컨테이너에 등록된 Bean들은 의존성 관리가 수월해진다. 
	-스프링 IoC 컨테이너에 등록된 Bean들은 싱글톤의 형태이다.
	
* singleton : 기본(Default) 싱글톤 스코프. 하나의 Bean 정의에 대해서 Container 내에 단 하나의 객체만 존재한다.
* prototype : 어플리케이션에서 요청시 (getBean()) 마다 스프링이 새 인스턴스를 생성


### 빈을 등록하는 방법

**1. xml 설정파일을 통한 등록**

예전에 스프링에서는 xml에서 하나하나 bean을 등록해 사용하였다 예를들면 아래 코드와 같다.

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd">

    <bean id="bookService" class="com.example.demo.BookService">
        <property name="bookRepository" ref="bookRepository" />
    </bean>

    <bean id="bookRepository" class="com.example.demo.BookRepository">
    </bean>

</beans>
```

- 위와 같은 xml 파일에서 bean을 하나하나 등록하여 사용하였다. 예전에는 그냥 이런식으로 했구나 정도만 받아들이자. 
- 이렇게 하나하나 Bean으로 등록하는 방법은 굉장히 번거롭기 때문에 새로운 방법이 등장했는데 그것이 바로 component-scan이다. 

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.example.demo"/>

</beans>
```

- 이번에도 xml 파일 안에서 component-scan을 사용하여 base-package에 현재 패키지 이름을 작성하면 Class-Path 아래에 **@Repository**, **@Service** 등의 **Bean으로 등록할 수 있는 어노테이션을 찾아서 Bean으로 등록을 해준다**.(Component-Scan은 아래에서 다시 설명한다) 하지만 여기서 다시 자바코드로 빈을 등록할 수 있는 없을까? 라는 의문이 생겼다.


**2. Java 코드를 이용해서 Bean 등록**

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ApplicationConfig {

    @Bean
    public BookRepository bookRepository() {
        return new BookRepository();
    }

    @Bean
    public BookService bookService() {
        BookService bookService = new BookService();
        bookService.setBookRepository(bookRepository());
        return bookService;
    }
}
```

- 위와 같이 ApplicationConfig 라는 자바파일을 만들 후에, @Configuration이라는 어노테이션을 달고 빈으로 등록할 곳에 @Bean 어노테이션과 함께 코드를 작성하면 빈으로 등록이 된다. 하지만 이것도 하나하나 빈으로 등록해야 하는 번거로움이 있기 때문에 좋은 것 같지 않다. 그래서 하나 더 나온 방법이 현재 스프링부트에 가장 가까운 방법이다.
- Spring-Boot는 어노테이션을 통해 Bean을 설정하고 주입받는 것을 표준으로 삼는다.


#### Container에 Spring Bean으로 등록시켜주는 Annotation
- ex) @Bean, @Component, @Controller, @Service, @Repository 
- **@Bean**은 개발자가 컨트롤 할 수 없는 **외부 라이브러리**를 Bean으로 등록하고 싶은 경우 (메소드로 return 되는 객체를 Bean으로 등록) 
- **@Component**는 개발자가 직접 컨트롤할 수 있는 클래스(**직접 만든**)를 Bean으로 등록하고 싶은 경우 (선언된 Class를 Bean으로 등록) 

- **@Controller, @Service, @Repository** 등 은 **@Component를** 비즈니스 레이어에 따라 **명칭을 달리 지정해준 것** 

#### Container에 있는 Spring Bean을 찾아서 주입시켜주는 Annotation
- @Autowired : IoC 컨테이너에 있는 참조할 Bean을 찾아 주입한다. (SPRING 표준)


### 📜 어노테이션
**@Controller**
-	해당 어노테이션이 적용된 클래스는 “Controller”임을 나타내고, bean으로 등록되며 해당 클래스가 Controller로 사용됨을  Spring Framework에 알림.


### 📜 @Autowired ###
**@Autowired란?**
- @Autowired는 의존성 주입을 할 때 사용하는 어노테이션으로 **객체의 타입**에 해당하는 bean을 찾아 주입하는 역할을 한다.

**@Autowired를 사용할 수 있는 위치**
- @Autowired는 기본적으로 아래의 위치에서 사용할 수 있다.
- 생성자(스프링 4.3 부터는 생략 가능)
- Setter
- 필드

- 위의 3가지의 경우에 Autowired를 사용할 수 있다. 
- 그리고 Autowired는 기본값이 true이기 때문에 의존성 주입을 할 대상을 찾지 못한다면 애플리케이션 구동에 실패한다. 
- 그러면 Autowired를 사용할 때의 경우의 수가 존재하는데 각각의 상황에 대해서 정리해보자.

**사용시 주의점**
- 해당 타입의 bean이 없거나 1개인 경우

#### 생성자로 의존성 주입

- 생성자 주입은 생성자에 의존성 주입을 받고자 하는 field를 나열하는 방법으로, 권고되는 방법의 하나 이다.

**장점**
- 필수적으로 사용해야 하는 레퍼런스 없이는 **인스턴스를 아예 만들지 못하도록 강제함**
- Spring 4.3 이상부터는 생성자가 하나인 경우 @Autowired를 사용하지 않아도 됨
- 생성자에 점차 많은 의존성이 추가 될 경우 리팩토링 시점을 감지 할 수 있음
- 의존성 주입 대상 필드를 final로 불편 객체 선언할 수 있음
- 테스트 코드 작성시 생성자를 통해 의존성 주입이 용이함

**단점**
- 어쩔 수 없는 순환 참조는 생성자 주입으로 해결하기 어려움
- 이러한 경우에는 나머지 주입 방법 중에 하나를 사용
- 가급적이면 순환 참조가 발생하지 않도록 하는 것이 더 중요


#### 필드 의존성 주입

- **멤버 필드에 @Autowired annotation을 선언**하여 주입받는 방법이다.

**장점**
- 가장 간단한 선언 방식

**단점**
- 의존 관계가 눈에 잘 보이지 않아 추상적이고, 이로 인해 의존성 관계가 과도하게 복잡해질 수 있음
- 반대로 생성자 주입과 Setter 주입은 의존성을 명확하게 커뮤니케이션 함
- DI Container와 강한 결합을 가져 외부 사용이 용이하지 않음
- 단위 테스트시 의존성 주입이 용이하지 않음
- 의존성 주입 대상 필드가 final 선언 불가


#### Setter 의존성 주입

- **setter 메소드에 @Autowired annotation을 선언**하여 주입받는 방법이다.(메소드 이름을 setter 대신에 다른 걸로 하여도 주입은 가능하지만 좋은 방법은 아니다) 

**장점**
- 의존성이 **선택적으로 필요한 경우에 사용**
- **생성자에 모든 의존성을 기술하면 과도하게 복잡해질 수 있는 것**을 선택적으로 **나눠 주입 할 수 있게 부담을 덜어줌**
- **생성자 주입 방법**과 **Setter 주입 방법**을 적절하게 상황에 맞게 분배하여 사용

**단점**
- 의존성 주입 대상 필드가 final 선언 불가




**생성자에 @Autowired 명시 (스프링 4.3 부터는 생략가능)**

```java
@Service
pbulic class TestService {
  TestRepository testRepository;
  
  @Autowired
  public TestService(TestRepository testRepository) {
     this.testRepository = testRepository;
  }
}
```
```java
public class TestRepository {
 ....
}
```
- 위의 코드에서 TestRepository의 의존성 주입이 작동할까? **당연히 작동하지 않는다.**
- @Auotowired는 **의존 객체의 타입**에 해당하는 **bean을 찾아서 주입**하기 때문이다.
- 즉, TestRepository는 **bean으로 등록되어 있지 않기 때문**에 스프링이 **bean을 찾지 못해 의존성을 주입할 수 없다**.

- 이를 작동하게 하기 위해서는 TestRepository를 bean으로 등록하기 위해 @Repository 혹은 @Componet 어노테이션을 이용해 TestRepository를 bean으로 등록해주면 된다. 

```java
@Repository
public class TestRepository {
 ....
}
```

**Setter에 @Autoriwred 명시**

```java
@Service
pbulic class TestService {
  TestRepository testRepository;
  
  @Autowired
  //@Autowired(required = false)
  public void setTestRepository(TestRepository testRepository) {
     this.testRepository = testRepository;
  }
}
```
```java
public class TestRepository {
 ....
}
```

- 이번엔 Setter를 이용해 의존성 주입을 시도해보자. 하지만 이역시 작동하지 않는다.
- Setter로 TestService 자체의 인스턴스를 만들었는데 왜 작동을 안 할까?
- 그 이유는 @Autowired Annotation 때문이다.
- @Autowired가 명시되어 있기 때문에 스프링은 의존성을 주입하려고 시도한다.
- setter를 이용해 인스턴스를 생성할 수 있지만, 의존성 주입에는 실패하는 것이다.

- @Autowired(requred = false) 옵션을 주면 의존성 주입을 받지 않고 인스턴스를 만들어서 bean으로 등록할 수 있다.
- 즉, TestRepository는 의존성 주입이 되지 않은 상태로 bean이 등록된다.

**필드에 @Autowired 명시**

```java
@Service
pbulic class TestService {
 
 @Autowired
 //@Autowired(required = false)
 TestRepository testRepository;
}
```

```java
public class TestRepository {
   ...
}
```

- 필드 위에 @Autowired 명시
- setter나 필드 injection을 사용할 때는 Optional 설정(Requied = false)을 통해 TestService가 해당하는 의존성 없이도 bean으로 등록할 수 있다.

**해당하는 타입의 bean이 여러 개인 경우**
```java
@Service
public class TestService {
	
    @Autowired
    TestRepository testRepository;
}
```

```java
public interface TestRepository {
	....
}

@Repository
public class MyTestRepository implements TestRepository {
  // TestRepository의 구현체
}

@Repository
public class ExampleRepository implements TestRepository {
  // TestRepository의 구현체
}
```

- TestService는 TestRepository를 사용하는데, TestRepository는 인터페이스이고 이를 구현하는 Repository bean 두 개가 존재한다고 생각해보자.
- 스프링은 TestService에는 어떤 Repository를 주입해줄까? 스프링은 개발자가 원하는 의존성을 알지 못하기 때문에 주입을 못해준다.
- 따라서 같은 타입의 bean을 두 개 발견했다는 에러 메시지를 뱉고, 다음과 같은 액션을 추천해준다.

- @Primary
- 해당 타입의 bean 모두 주입받기
- @Qulifier(bean id)

<br>
**1. @Primary**
- @Primary Annotation은 같은 타입의 bean이 여러 개 주입될 경우 @Primary가 붙은 bean을 주입하겠다는 의미를 가지고 있다.

```java
public interface TestRepository {
	....
}

@Repository @Primary   // 이 구현체가 주입이 됌
public class MyTestRepository implements TestRepository {
  // TestRepository의 구현체
}

@Repository
public class ExampleRepository implements TestRepository {
  // TestRepository의 구현체
}
```

- @Repository 옆에 명시
- 같은 타입의 TestRepository가 주입되었을 때 MyTestRepository가 주입된다.

<br>
**2. @Qulifier**

- bean의 ID로 지정을 해주는 것 같다.

```java
@Service
public class TestService {
	
    @Autowired @Qulifier("myTestRepository")   // bean의 Id는 lower Camel Case를 사용한다.
    TestRepository testRepository;
}
```

- @Autowired 옆에 명시
- @Primary와 마찬가지로 MyTestRepository가 주입된다.
- @Primary가 @Qulifier보다 Type safe 하기 때문에 @Primary를 사용하는 것을 추천

<br>
**3. 해당 타입의 bean 모두 주입**

'''java
@Service
public class TestService {
	
    @Autowired
    List<TestRepository> testRepositoies;
}
```

- List로 같은 타입의 모든 bean을 주입받을 수 있다.


#### @Autowired 사용시 주의 사항

**1) 해당 타입의 빈이 없는 경우**

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class BookService {

    BookRepository bookRepository;

    @Autowired
    public BookService(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }
}
```

- 생성자에 대해 Autowired 어노테이션이 있는 상태이다. 
- **BookRepository**가 빈으로 등록되어 있지 않다고 가정하면 **생성자에서 에러가 발생**한다. 
- 이유는 Autowired가 의존성을 주입하기 위해서는 **빈이 등록되어 있는 객체중에서 찾는데** BookRepository라는 객체는 빈으로 등록되어 있지 않기 때문에 의존성 주입에 실패해서 에러가 발생한다. (Autowired의 기본값이 true이기 때문이다) 

 
 ```java
 import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class BookService {

    BookRepository bookRepository;

    @Autowired
    public void setBookRepository(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }
}
 ```
 
 - 현재 BookRepository는 빈으로 등록되어 있지 않다고 가정하고, **Autowired가 붙은 setter에서 여전히 에러가 발생**한다. 
 - 여기서 하나 궁금한 것은 **이번에는 setter인데** BookService의 빈은 등록할 수 있지 않나? 
 * (생성자에 @Autowired가 설정되어 있고, 필요한 의존 객체가 bean으로 등록되어 있지 않은 경우는, 애초에 생성자의 실행이 불가능하여 생성 자체가 안되어서 빈으로 생성할 수 없어서 빈으로 등록 자체가 안되는 듯)
 
 - 맞다. **BookSevice는 빈으로 등록할 수 있다.** 그리고 Autowired가 기본값이 true라고 했는데 그러면 false로 하면 어떻게 될까? 

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class BookService {

    BookRepository bookRepository;
    
    @Autowired(required = false)   // @Autorired의 required 속성을 false로 지정.
    public void setBookRepository(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }
}
```
 
- 위와 같이 쓴다면 **BookService의 객체는 빈으로 등록이 되지만** (@Service 어노테이션 때문에) BookRepository는 빈으로 등록되지 않게 된다. (required = false이기 때문이다) 그리고 Autowired를 사용할 수 있는 곳이 필드가 하나 더 있다. 

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class BookService {

    @Autowired(required = false)
    BookRepository bookRepository;
}
```

- 위와 같이 필드에도 사용할 수 있다. 
- setter와 필드에 Autowired를 사용하게 되면 만약 **BookRepository가 빈으로 등록되어 있지 않다 해도** BookService는 빈으로 등록이 가능하다. 
- 하지만 **생성자에 Autowired를 쓴 상황에서** **BookRepository가 빈으로 등록되어 있지 않다면 BookService도 빈으로 등록되지 못하는 경우가 생긴다. 
**

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


#### 📜 MyBatis

**MyBatis에서 resultType에 사용할 수 있는 값들**
- 자주 사용하는 자료형에 대해서 미리 지정된 별칭을 이용
    ex) string -> String
        arrayList -> ArrayList
	hashMap -> HashMap
- 또는 특정 클래스의 풀 경로를 입력하여 해당 클래스의 객체에 데이터를 담아서 반환
    ex) resultType = "com.dao.MyDAO"
    
- 또는 MyBatis-config.xml 에 특정 클래스에 대해 별칭을 지정 후, 그 별칭을 이용해서 사용가능

```xml
  <typeAlias>
    <typeAlias alias = "DAO" type = "com.dao.MyDAO"/>	  
  </typeAlias>	  
```
<br>
  별칭 지정 후, ex) resultType = "DAO" 로 사용 가능



#### Reuqset 전처리, 후처리

**Filter** : dispatcherServlet 전에 수행
**Interceptor** : dispatcherServlet 후에 수행

대략적인 프로세스 :
	1. Interceptor or Filter 동작 구현
	2. xml 설정 파일에 등록 및 탐지 패턴 작성


### JSTL 사용법
#### IF문 
- 'test' 속성의 값이 참이면 if문 태그 안의 코드가 실행됌 

```
<%@ taglib uri = "http://java.sun.com/jsp/jstl/core" prefix = "c" %>

<html>
   <head>
      <title><c:if> Tag Example</title>
   </head>

   <body>
      <c:set var = "salary" scope = "session" value = "${2000*2}"/>
      <c:if test = "${salary > 2000}">		// test 의 값이 true이면 안의 코드가 실행
         <p>My salary is:  <c:out value = "${salary}"/><p>
      </c:if>
   </body>
</html>
```

IF Else문
- java에서 많이 사용하는 if~else 문의 경우 jstl에서는 <c:choose>를 이용합니다.
```
<%@ taglib uri = "http://java.sun.com/jsp/jstl/core" prefix = "c" %>

<html>
   <head>
      <title><c:choose> Tag Example</title>
   </head>

   <body>
      <c:set var = "salary" scope = "session" value = "${2000*2}"/>
      <p>Your salary is : <c:out value = "${salary}"/></p>
      <c:choose>

         <c:when test = "${salary <= 0}">	// IF ELSE 문의 역할
            Salary is very low to survive.
         </c:when>

         <c:when test = "${salary > 1000}">     // IF ELSE 문의 역할
            Salary is very good.
         </c:when>

         <c:otherwise>                          // Default의 역할
            No comment sir...
         </c:otherwise>
      </c:choose>

   </body>
</html>
```


#### 이슈 해결 기록

갑자기 mybatis에서 type Alaias를 등록하는 부분에서 잘 되던 typeAlias가 등록이 안되며 **Error registering typeAlias for 'xxx'. Cuase:java. ....** 에러 발생
-> 해결 방법 : 프로젝트 클린 후 재빌드 하니깐 별도의 수정없이 해결

* 이슈있는게 아니라 코드 수정 사항이 적용이 안돼는 것 같을 때에도 clean 후 재 빌드하면 적용이 됀다.
* c:choose를 쓸 때 안에 주석이 있으면 원인모를 500에러가 뜬다.. c:choose 사용시 주석을 주의하자



