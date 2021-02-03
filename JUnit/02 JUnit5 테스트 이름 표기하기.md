# JUnit5 테스트 이름 표기하기

* `@DisplayNameGeneration` :     
  * Method와 Class 레퍼런스를 사용해서 테스트 이름을 표기하는 방법 설정    
  * 기본 구현체로 ReplaceUnderscores 제공한다.   
* `@DisplayName` :   
  * 어떤 테스트인지 테스트 이름을 보다 쉽게 표현할 수 있는 방법을 제공 
  * `@DisplayNameGeneration` 보다 우선순위가 높다.    
    
이 외에도 JUnit5 문서를 보면 더 다양한 이름짓기 방법들이 있다.   


![JUnitStart.png](./image/JUnitStart.png)      
테스트의 이름을 확인하는 방법은 실행 콘솔 왼쪽을 보면 알 수 있다.               
테스트 이름을 표기하는 전략은 기본적으로 테스트 메서드의 이름을 기준으로 진행된다.             
   
하지만, 만약 우리가 이름을 직접 정의하고 싶다면 위에 제시된 어노테이션을 사용하면 된다.      

## @DisplayNameGeneration   
`@DisplayNameGeneration`는 우리가 **전략**을 지정하는 어노테이션이다.   
메서드와 클래스에 사용할 수 있으며 사용 예시는 아래와 같다.  

```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import static org.junit.jupiter.api.Assertions.*;

@DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class)
class StudyTest {

    @Test
    public void Study_테스트() {
        Study study = new Study();
        assertNotNull(study);
        System.out.println("Study_테스트");
    }

    @Test
    public void 서브_테스트() {
        System.out.println("서브_테스트");
    }

    @Disabled
    @Test
    public void 미완성_테스트() {
        System.out.println("미완성_테스트");
    }

    @BeforeAll
    static void BeforeAll_테스트() {
        System.out.println("BeforeAll");
    }

    @BeforeEach
    public void BeforeEach_테스트() {
        System.out.println("BeforeEach");
    }

    @AfterEach
    public void AfterEach_테스트() {
        System.out.println("AfterEach");
    }

    @AfterAll
    static void AfterAll_테스트() {
        System.out.println("AfterAll");
    }


}
```

![JUnitDisplayNameGeneration](./image/JUnitDisplayNameGeneration.png)    
