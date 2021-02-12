# JUnit5 테스트 인스턴스
       
JUnit의 기본 전략으로 테스트 메서드 마다 테스트 인스턴스를 새로 만든다.          
이는, 테스트 메서드간의 의존성을 낮추기 위한 장치로,          
테스트 메소드를 독립적으로 실행하여 예상치 못한 부작용을 방지하기 위함이다.       

```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;
import org.junit.jupiter.api.extension.ParameterContext;
import org.junit.jupiter.params.aggregator.ArgumentsAccessor;
import org.junit.jupiter.params.aggregator.ArgumentsAggregationException;
import org.junit.jupiter.params.aggregator.ArgumentsAggregator;
import org.junit.jupiter.params.converter.ArgumentConversionException;
import org.junit.jupiter.params.converter.SimpleArgumentConverter;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

class StudyTest {

    int value = 0;

    @FastTest
    @DisplayName("메인_테스트 fast")
    public void 메인_테스트() {
        Study actual = new Study(10);
        assertThat(actual.getLimit()).isGreaterThan(0);
        System.out.println();
        System.out.println("메인_테스트 실행");
        System.out.println("value = " + ++value);
        System.out.println();
    }

    @SlowTest
    @DisplayName("서브_테스트 slow")
    public void 서브_테스트() {
        System.out.println();
        System.out.println("서브_테스트 실행");
        System.out.println("value = " + ++value);
        System.out.println();
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
![JUnitTestInstanceDefaultStrategy.png](./image/JUnitTestInstanceDefaultStrategy.png)     
        
위에서 보이는 결과로 인스턴스 변수 value 의 값은 1로만 출력이 되는데         
이는 테스트 메서드 마다 테스트 인스턴스를 새로 만들어 사용하기 때문이다.          

```java
    @BeforeAll
    static void BeforeAll_테스트() {
        System.out.println("BeforeAll");
    }

    @AfterAll
    static void AfterAll_테스트() {
        System.out.println("AfterAll");
    }
```
참고로, `@BeforeAll`과 `@AfterAll` 어노테이션을 사용하려면              
어노테이션을 선언한 테스트 메서드에서 `static`을 선언해줘야 했다.             
그 이유도, `@BeforeAll`과 `@AfterAll`는 모든 테스트 과정에서       
딱 1번만 수행해야하기 때문에, 인스턴스 메서드가 아닌 static 정적 메서드로 정의한 것이다.    

# @TestInstance   
             
JUnit5 부터 테스트 인스턴스 LifeCycle에 대한 설정을 개발자가 할 수 있게 되었다.           

      
@TestInstance(Lifecycle.PER_CLASS)   
테스트 클래스당 인스턴스를 하나만 만들어 사용한다.    
경우에 따라, 테스트 간에 공유하는 모든 상태를 @BeforeEach 또는 @AfterEach에서 초기화 할 필요가 있다.   
@BeforeAll과 @AfterAll을 인스턴스 메소드 또는 인터페이스에 정의한 default 메소드로 정의할 수도 있다.    
