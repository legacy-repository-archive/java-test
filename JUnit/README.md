# 들어가면서 

# JUnit 소개

JUnit은 대다수의 자바 개발자가 이용하는 자바 테스팅 프레임워크다.      
실제로 [JetBrains의 설문조사](https://www.jetbrains.com/lp/devecosystem-2019/java/)에 따르면      
자바 개발자의 93%가 단위 테스트에 JUnit을 사용해본 경험이 있다는 결과를 발표했다.     
     
개발자라면 누구나 자신의 코드에 대한 테스트를 진행해야 한다.           
그렇기 때문에 우리가 사용할 테스팅 프레임워크에 대해서 잘 알 필요가 있다.    

![image](https://user-images.githubusercontent.com/50267433/106532524-ade2ee00-6533-11eb-85ca-677c98e5d5a5.png)      
 
* JUnit Platform : 테스트를 실행해주는 런처 제공, TestEngine API 제공   
* Jupiter : TestEnigine API 구형체로 JUnit 5를 제공   
* Vintage : Junit 4와 3을 지원하는 TestEngine 구현체   
   
더 자세한 내용을 [JUnit5 공식 레퍼런스](https://junit.org/junit5/docs/current/user-guide/)를 통해 확인!   

# JUnit5 VS JUnit4 

우리가 공부하고자 하는 `JUnit5`는 2017년 2월 알파버전부터 시작하여    
현재까지도 많은 개발자에게 사랑을 받는 `테스팅 프레임워크`이다.       

당연한 얘기지만 `JUnit5` 이전에는 `JUnit4`가 존재했다.      
**그렇다면 무엇이 달라졌을까?**        

```
JUnit5 팀은 2가지 영역을 중점으로 개발을 진행했다.   
  
* 테스트 작성을 위한 API 세트        
* `tool Builder` 가 테스트를 `발견`, `필터링`,`실행`하는 데 사용할 수 있는 JUnit 플랫폼 런처 API     
``` 
___   
  
`Junit4`에서는, 역할을 잘 표현하지 못하는 이름을 가진 어노테이션이 많았다.     
그렇기에 `Junit5` 에서는,역할을 잘 표현하도록 새로운 이름을 정의해주었다.     
       
|JUnit 5|JUnit 4|
|-------|-------|
|@BeforeEach|@Before|
|@AfterEach|@After|
|@BeforeAll|@BeforeClass|
|@AfterAll|@AfterClass|
|@Disabled|@Ignore|
	      
예시로, `Before`, `BeforeClass`만 보면 차이점을 제대로 이해하기 힘들지만,        
`BeforeEach`, `BeforeAll`을 보면, 메서드를 각각/전체적으로 실행하는 것을 알 수 있다.     
	
___

```java
public class SampleTest { 
  @Test
    void groupedAssertions() {
        // In a grouped assertion all assertions are executed, and any
        // failures will be reported together.
        assertAll("person",
            () -> assertEquals("John", person.getFirstName()),
            () -> assertEquals("Doe", person.getLastName())
        );
    }
}
```
`assertAll()`로 `Lamda`를 사용할 수 있으며 이를, **Assertion Group**이라 칭한다.      
하지만, **각기 따로 진행하면 되지 왜 한번에 진행하지?**         
            
**Assertion Group**은 하나의 작업을 의미한다.   
그렇기에 하나의 작업안에 존재하는 기능들에 대한 테스트를 진행한다.
    
하지만, 각각의 기능들을 따로 테스트하는 것과 다르게,      
실패하면 실패한 테스트에 대한 **자세한 결과를 얻을 수 있다.**     
    
**Assertion Group 메서드**
```java
Address address = unitUnderTest.methodUnderTest();
assertAll("Should return address of Oracle's headquarter",
    () -> assertEquals("Redwood Shores", address.getCity()),
    () -> assertEquals("Oracle Parkway", address.getStreet()),
    () -> assertEquals("500", address.getNumber())
);
```
   
**콘솔 결과**
```java
org.opentest4j.MultipleFailuresError:
    Should return address of Oracle's headquarter (3 failures)
    expected: <Redwood Shores> but was: <Walldorf>
    expected: <Oracle Parkway> but was: <Dietmar-Hopp-Allee>
    expected: <500> but was: <16>
```
   
___  

**JUnit4**
```java
public class TestClass {
    
    @Test
    public void run_test() {
        // 테스트
    }

}
```
**JUnit5**
```java
class TestClass {
    
    @Test
    void run_test() {
        // 테스트
    }

}
```
기존에는 `public 클래스`의 `public 메서드`만 테스트가 가능했던 것과 달리,    
접근 지정자가 없는 클래스/메서드에서도 `@Test`가 가능해졌다.         

___   


# 어노테이션
|어노테이션|기능|
|---------|----|
|@Test|메서드가 테스트 메서드임을 나타낸다.<br>`JUnit4`의 `@Test`과 달리,<br>JUnit Jupiter(JUnit5)전용으로 작동하기 때문에 attribute를 선언하지 않는다.<br>메서드를 오버라이딩하지 않는 경우 어노테이션은 상속되어 적용된다.|
|@ParameterizedTest|메서드가 매개 변수가 있는 테스트 메서드임을 나타낸다.<br>메서드를 오버라이딩하지 않는 경우 어노테이션은 상속되어 적용된다.|
|@RepeatedTest|메서드가 반복 테스트를 위한 테스트 템플릿임을 나타낸다.<br>메서드를 오버라이딩하지 않는 경우 어노테이션은 상속되어 적용된다.|
|@TestFactory|메서드가 동적 테스트를 위한 테스트 팩토리임을 나타낸다.<br>메서드를 오버라이딩하지 않는 경우 어노테이션은 상속되어 적용된다.|
|@TestTemplate|메서드가 등록 된 공급자가 반환한 호출 컨텍스트 수에 따라,<br>여러 번 호출되도록 설계된 테스트 케이스의 템플릿임을 나타낸다.<br>메서드를 오버라이딩하지 않는 경우 어노테이션은 상속되어 적용된다.|
|@TestMethodOrder|어노테이션이 달린 테스트 클래스에 대한 테스트 메서드 실행 순서를 구성하는 데 사용된다.<br>JUnit 4의 @FixMethodOrder<br>어노테이션은 메서드를 상속해도 적용된다.|
|@TestInstance|어노테이션이 달린 테스트 클래스에 대한 테스트 인스턴스 생명주기를 구성하는 데 사용된다.<br>어노테이션은 메서드를 상속해도 적용된다.|
|@DisplayName|테스트 클래스 또는 테스트 메서드에 대한 `사용자 지정 표시 이름` 을 선언합니다.<br>어노테이션은 메서드를 상속해도 적용되지 않는다.|
|@DisplayNameGeneration|테스트 클래스에 대한 사용자 지정 표시 이름 생성기를 선언한다.<br>어노테이션은 메서드를 상속해도 적용된다.|
|@BeforeAll|`@Test` , `@RepeatedTest`, `@ParameterizedTest`및 `@TestFactory`와 같은<br>테스트 메소드가 실행되기 전에 해당 어노테이션이 붙은 메서드를 딱 1번 수행한다.<br>테스트 인스턴스 생명주기가 적용되지 않은 경우 메서드에 `static`을 붙여야한다.<br>JUnit 4의 `@BeforeClass`<br>어노테이션은 메서드를 상속해도 무조건 적용된다. |
|@BeforeEach|`@Test` , `@RepeatedTest`, `@ParameterizedTest`및 `@TestFactory`와 같은<br>테스트 메소드가 실행되기 전에 해당 어노테이션이 붙은 메서드를 매번 수행한다.<br>JUnit 4의 @Before<br>메서드를 오버라이딩하지 않는다면, 어노테이션은 상속되어 적용된다.|
|@AfterEach|`@Test` , `@RepeatedTest`, `@ParameterizedTest`및 `@TestFactory`와 같은<br>테스트 메소드가 실행된 후에 해당 어노테이션이 붙은 메서드를 매번 수행한다.<br>메서드를 오버라이딩하지 않는다면, 어노테이션은 상속되어 적용된다.|


@AfterAll

주석 메소드가 실행되어야 함을 나타내고, 이후 모든 @Test , @RepeatedTest, @ParameterizedTest및 @TestFactory현재 클래스의 메소드; JUnit 4의 @AfterClass. 이러한 메서드는 상속 되며 ( 숨겨 지거나 재정의 되지 않는 한 ) 반드시 상속 되어야합니다 static( "클래스 별" 테스트 인스턴스 수명주기 가 사용 되지 않는 경우 ).

@Nested

주석이 달린 클래스가 정적이 아닌 중첩 테스트 클래스 임을 나타냅니다 . @BeforeAll및 @AfterAll방법은 직접 사용할 수 없습니다 @Nested은 "당 클래스"를 제외 테스트 클래스 테스트 인스턴스 라이프 사이클이 사용됩니다. 이러한 주석은 상속 되지 않습니다 .

@Tag

클래스 또는 메서드 수준에서 테스트 필터링을위한 태그 를 선언하는 데 사용됩니다 . TestNG의 테스트 그룹 또는 JUnit 4의 Categories와 유사합니다. 이러한 주석은 클래스 수준에서 상속 되지만 메서드 수준 에서는 상속 되지 않습니다.

@Disabled

테스트 클래스 또는 테스트 메서드 를 비활성화 하는 데 사용됩니다 . JUnit 4의 @Ignore. 이러한 주석은 상속 되지 않습니다 .

@Timeout

실행이 주어진 기간을 초과하는 경우 테스트, 테스트 팩토리, 테스트 템플릿 또는 수명주기 메서드를 실패하는 데 사용됩니다. 이러한 주석은 상속 됩니다.

@ExtendWith

확장을 선언적 으로 등록하는 데 사용됩니다 . 이러한 주석은 상속 됩니다.

@RegisterExtension

필드를 통해 프로그래밍 방식으로 확장 을 등록하는 데 사용됩니다 . 이러한 필드는 음영 처리 되지 않는 한 상속 됩니다 .

@TempDir

라이프 사이클 방법 또는 테스트 방법에서 필드 주입 또는 매개 변수 주입을 통해 임시 디렉토리 를 제공하는 데 사용됩니다 . 에있는 org.junit.jupiter.api.io패키지.

# JUnit 	
	
	
	
	
