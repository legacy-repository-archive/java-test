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
그렇다면 무엇이 달라졌을까?      

`JUnit5` 팀은 2가지 영역을 중점으로 개발을 진행했다.   
  
* 테스트 작성을 위한 **API 세트**        
* `tool Builder` 가 테스트를 `발견`, `필터링`,`실행`하는 데 사용할 수 있는   
**JUnit 플랫폼 런처 API**     
 
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
`BeforeEach`, `BeforeAll`을 보면, 메서드를 각각/전체적으로 실행하는 것을 알 수 있습니다.     
	
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
`assertAll()`로 `Lamda`를 사용할 수 있으며 이를, **Assertion Group**이라 말합니다.      
하지만, 이런 의문을 가집니다. **각기 따로 진행하면 되지 왜 한번에 진행하지?**       
         
**Assertion Group**은 하나의 작업을 의미합니다.   
그렇기에 하나의 작업안에 존재하는 기능들에 대한 테스트를 진행합니다.
  
하지만, 각각의 기능들을 따로 테스트하는 것과 다르게,    
실패하면 실패한 테스트에 대한 **자세한 결과를 얻을 수 있습니다.**     
물론, 옳게 된 테스트에 대한 결과도 얻을 수 있습니다.  
    
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



# JUnit 	
	
	
	
	
