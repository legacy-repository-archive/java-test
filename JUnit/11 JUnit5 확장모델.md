# JUnit5 확장모델.md   
              
JUnit5는 JUnit4에 비해서 확장 모델이 단순해졌다고 표현할 수 있다.              
JUnit4의 확장 모델은 `@RunWith(Runner)`, `TestRule`, `MethodRule` 3가지가 존재했다.      
개인적인 궁금증으로 아래에 한 번 정리해보았습니다.   

* **RunWith(Runner) :**   
테스트 실행 방법을 확장할 때 사용하는 어노테이션이다.            
필자 같은 경우는 주로, `SpringRunner.class` 를 사용했던 기억이 있다.             
`SpringRunner.class`를 사용하게 될 경우 스프링 실행자를 실행하여 서버를 구동합니다.           
        
* **Rule(Test/Method) :**   
테스트 케이스를 실행하기 전/후에 추가 코드를 실행할 수 있게 해준다.       
`@Before`와 `@After`가 있지만, 재사용 및 더 확장 가능한 기능으로 개발할 수 있는 장점이 있다.       

____ 
        
JUnit5로 넘어오면서 확장 모델은 단 하나, `@Extension`으로 간략화되었다.    
   
**확장팩 등록 방법**     
* 선언적인 등록 `@ExtendWith`          
* 프로그래밍 등록 `@RegisterExtension`     
* 자동 등록 자바 `ServiceLoader`      
     
**확장팩 만드는 방법**   
* 테스트 실행 조건     
* 테스트 인스턴스 팩토리   
* 테스트 인스턴스 후-처리기    
* 테스트 매개변수 리졸버     
* 테스트 라이프사이클 콜백    
* 예외 처리     
* 등등...    

# 실습을 통해 알아보기  
## LifeCycle Callback을 이용한 Extension    
   
실행하는데 오래 걸리는 테스트를 찾는데       
만약 `@Slow` 어노테이션이 없다면 이를 붙이라고 권장하는 `Extension`을 만들어보자    

```java

```
`getStore()`는 `NameSpace` 타입이 필요하므로 이를 만들어주는 팩토리 메서드 `create()`를 호출한다.     
`create()` 또한, `Object` 가변 변수를 통해 받아온 인자들을 합쳐서 `NameSpace` 를 만든다.      
        
우리는 테스트 클래스 이름과 메서드 이름을 조합한 `NameSpace` 를 만들 예정이다.      
사실, `ExtensionContext`는 호출한 곳에 대한 정보를 가지고 있는 클래스다.       
그렇기에 해당 인스턴스를 통해 우리는 테스트 클래스 이름과 메서드 이름을 가져올 수 있었던 것이다.       
`getRequiredTest이름()`을 통해 우리가 원하는 정보를 가져오자        
     
