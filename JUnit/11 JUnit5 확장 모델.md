# JUnit5 확장 모델 
              
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
        
JUnit5로 넘어오면서 확장 모델은 단 하나, `Extension`으로 간략화되었다.    
   
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
     
**test.java.me.kwj1270.thejavatest.FindSlowTestExtension 생성**    
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.extension.AfterTestExecutionCallback;
import org.junit.jupiter.api.extension.BeforeTestExecutionCallback;
import org.junit.jupiter.api.extension.ExtensionContext;

public class FindSlowTestExtension implements BeforeTestExecutionCallback, AfterTestExecutionCallback {
    private static final long THRESHOLD = 1000L;


    @Override
    public void beforeTestExecution(ExtensionContext extensionContext) throws Exception {
        ExtensionContext.Store store = getStore(extensionContext);
        store.put("START_TIME", System.currentTimeMillis());
    }
    
    @Override
    public void afterTestExecution(ExtensionContext extensionContext) throws Exception {
        String testMethodName = extensionContext.getRequiredTestMethod().getName();
        ExtensionContext.Store store = getStore(extensionContext);
        long start_Time = store.remove("START_TIME", long.class);
        long duration = System.currentTimeMillis() - start_Time;
        if (duration > THRESHOLD) {
            System.out.printf("Please consider mark method [%s] with @SlowTest.\n", testMethodName);
        }
    }

    private ExtensionContext.Store getStore(ExtensionContext extensionContext) {
        String testClassName = extensionContext.getRequiredTestClass().getName();
        String testMethodName = extensionContext.getRequiredTestMethod().getName();
        ExtensionContext.Store store = extensionContext.getStore(ExtensionContext.Namespace.create(testClassName, testMethodName));
        return store;
    }
}

```   
* **ExtensionContext :**          
`Extension` 클래스를 사용하는 곳에 대한 정보를 가지고 있는 클래스다.            
또한, `Key-Value`와 비슷한 구조로 값들을 저장하는 `Store` 인터페이스가 있다.         

## getStore() 정의하기    

```java
    private ExtensionContext.Store getStore(ExtensionContext extensionContext) {
        String testClassName = extensionContext.getRequiredTestClass().getName();
        String testMethodName = extensionContext.getRequiredTestMethod().getName();
        ExtensionContext.Store store = extensionContext.getStore(ExtensionContext.Namespace.create(testClassName, testMethodName));
        return store;
    }
```
우선 우리는 `Store` 인터페이스의 구현체를 `Extension` 인스턴스의 `getStore()`를 통해 얻어와야 한다.  
`Extension` 인스턴스의 `getStore()`는 `ExtensionContext.NameSpace` 타입이 필요하다.        
다행스럽게도 `ExtensionContext.Namespace` 타입에서 팩토리 메서드 `create()`를 제공해준다.       
`create()`는 `Object` 가변 변수를 통해 받아온 인자들을 합쳐서 `NameSpace` 를 만든다.        
                      
우리는 테스트 클래스 이름과 메서드 이름을 조합한 `NameSpace` 를 만들 예정이다.                     
그렇기에 테스트 클래스 이름과 메서드 이름 또한, `Extension` 인스턴스에서 가져온다.          
이렇게, `NameSpace`를 정의했으므로 이에 대응되는 `Store` 구현체를 하나 생성했다.     
        
## beforeTestExecution 정의하기        
`beforeTestExecution`는 테스트 케이스를 실행하기 전에 동작하는 메서드이다.             

```java
    @Override
    public void beforeTestExecution(ExtensionContext extensionContext) throws Exception {
        ExtensionContext.Store store = getStore(extensionContext);
        store.put("START_TIME", System.currentTimeMillis());
    }
```
우리가 앞서 정의한 `getStore(extensionContext)`를 통해 `Store` 구현체를 가져온다.    
앞서 말했듯이 `Store`는 `Key-Value`와 비슷한 구조를 가진다.     
`Key`의 값으로는 **`"START_TIME"`** 을 `Value`의 값으로는 **현재 시간**을 구해서 넣어준면 된다.     

## afterTestExecution 정의하기   
`afterTestExecution`는 테스트 케이스를 실행한 후에 동작하는 메서드이다.                
    
```java
    @Override
    public void afterTestExecution(ExtensionContext extensionContext) throws Exception {
        String testMethodName = extensionContext.getRequiredTestMethod().getName();
        ExtensionContext.Store store = getStore(extensionContext);
        long start_Time = store.remove("START_TIME", long.class);
        long duration = System.currentTimeMillis() - start_Time;
        if (duration > THRESHOLD) {
            System.out.printf("Please consider mark method [%s] with @SlowTest.\n", testMethodName);
        }
    }
```  
메시지 출력을 위해, 메서드의 이름을 `ExtensionContext`로부터 가져온다.          
마찬가지로 이전에 사용했던 `Store` 구현체를 사용해야하므로 `getStore(extensionContext)`를 사용한다.         
시간 비교를 위해 기존 Store 구현체에 있던 `"START_TIME"`의 값을 long 타입으로 빼오면서 삭제한다.       
현재 시간을 구하고 특정 시간이 초과되는지를 검증한다.       
만약, 앞서 말했듯이 시간이 초과된다면 `@SlowTest`를 붙이는 것을 권장하는 메시지를 출력한다.       
   
