# JUnit5 Assertion    
   
Assertion 은 우리가 실제 테스트에서 검증하고자하는 내용을 확인하는 기능이다.             
코드에서 Assertion은, `org.junit.jupiter.api.Assertions`에 들어있는 `assert-` static 메서드를 말한다.        
`org.junit.jupiter.api.Assertions`의 static 메서드이기에 `import static`을 이용해야 한다.    

## assert- 메서드들
`assert-` 메서드에 대한 내용을 정리하고자 한다.  

|assert- 메서드|설명|
|------------|---|
|assertEquals(expect, actual)|expect와 acutal이 일치하는지 확인한다.|
|assertArrayEquals(expect, actual)|배열 자료형인 expect와 acutal이 일치하는지 확인한다.|    
|assertFalse(condition)|condition이 false 인지 확인한다.|
|assertTrue(condition)|condition이 true 인지 확인한다.|
|assertNull(acutal)|acutal이 null인지 확인한다|
|assertNotNull(acutal)|acutal이 null이 아닌지 확인한다|
|assertSame(expect, actual)|expect와 actual가 같은 객체를 참조하고 있는지 확인한다.| 
|assertNotSame(expect, actual)|expect와 actual가 같은 객체를 참조하고 있지 않은지 확인한다.|
|assertAll(executables...)|()안에 있는 모든 구문이 테스트를 성공하는지 확인한다.|
|assertThrows(expectedType, executable)|예외가 발생하는지 확인한다.|
|assertTimeout(duration, executable)|특정 시간안에 실행이 완료되는지 확인한다.|

`assertEquals()`는 `equals()`메서드를 기준으로 두 객체의 값이 같은지 확인하고,        
`assertSame()`는 `== 연산자`를 기준으로 두 객체의 레퍼런스가 동일한가를 확인한다.        
    
**assert-** 메서드는 파라마터의 순서에따라 의미가 달라지니,        
이를 잘 이해하고 사용하도록 하자 (대부분 처음에 기대값, 실제값)        


## 기본 사용해보기  
`assert-` 메서드를 사용하기 위해서 몇 개의 소스 코드를 생성 및 수정하겠다.           
    
**main.me.kwj1270.thejavatest.Study**
```java
package me.kwj1270.thejavatest;

public class Study {
    private StudyStatus studyStatus;

    public StudyStatus getStatus() {
        return this.studyStatus;
    }
}
```
* Study의 상태를 나타내는 `StudyStatus 열거형`을 인스턴스 변수로 넣었다.      

**main.me.kwj1270.thejavatest.StudyStatus**    
```java
package me.kwj1270.thejavatest;
public enum StudyStatus {
    DRAFT, STARTED, ENDED;
}
```
* Study의 상태를 나타내는 `StudyStatus 열거형`을 생성했다.  

**test.me.kwj1270.thejavatest**
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import static org.junit.jupiter.api.Assertions.*;

class StudyTest {

    @DisplayName("스터디 모두 화이팅")
    @Test
    public void Study_테스트() {
        Study study = new Study();
        assertNotNull(study);
        assertEquals(StudyStatus.DRAFT, study.getStatus());
        System.out.println("Study_테스트");
    }
    
}
```
* 테스트 클래스에서는 Study의 상태가 `DRAFT`인지 확인한다.     
   
![JUnitAssertionTestBasic.png](./image/JUnitAssertionTestBasic.png)
         
위 사진에서 알 수 있듯이      
테스트에 실패하면, 예상값하고 실제값에 대한 정보가 나오면서         
**테스트에 실패한 이유를 어느정도 유추할 수 있다.**          


## 메세지 입력하기
`assert-` 메서드들은 공통적으로 메세지를 입력할 수 있도록 오버로딩 되어있다.             
그렇기에 `assert-` 메서드의 마지막 인자로 메세지를 입력하여 좀 더 쉽게 디버깅할 수 있다.          
현재는 테스트량이 적기에 티가 안날 수 있지만, 현업에서는 무수히 많은 테스트를 진행하므로        
**어느 테스트가 실패했는지 원인은 무엇인지 알기 위해서라도 메세지를 작성하는 것이 좋다.**             

```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import static org.junit.jupiter.api.Assertions.*;

class StudyTest {

    @DisplayName("스터디 모두 화이팅")
    @Test
    public void Study_테스트() {
        Study study = new Study();
        assertNotNull(study);
        assertEquals(StudyStatus.DRAFT, study.getStatus(), "스터디를 처음 만들면 상태값이 DRAFT여야 한다.");
        System.out.println("Study_테스트");
    }

}
```
![JUnitAssertionTestMessage.png](./image/JUnitAssertionTestMessage.png)  

**메시지 람다식으로 작성**
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import static org.junit.jupiter.api.Assertions.*;

class StudyTest {

    @DisplayName("스터디 모두 화이팅")
    @Test
    public void Study_테스트() {
        Study study = new Study();
        assertNotNull(study);
        assertEquals(StudyStatus.DRAFT, study.getStatus(), () -> "스터디를 처음 만들면 상태값이 DRAFT여야 한다.");
        System.out.println("Study_테스트");
    }
    
}
```
```java
assertEquals(StudyStatus.DRAFT, study.getStatus(), () -> "스터디를 처음 만들면 상태값이 DRAFT여야 한다.");
```
```java
assertEquals(Object expected, Object actual, String message)
assertEquals(Object expected, Object actual, Supplier<String> messageSupplier)
```
`assert-`메서드의 메시지구문 오버로딩은 `String`과 `Supplier<String>`으로 되어있다.    
그렇기에 `Supplier<String>` 오버로딩 메서드를 사용하면 람다식을 적용할 수 있다.         
   
**🤔 그렇다면 왜 람다식을 사용할까?**   
만약, 에러 메시지를 만들 때 만드는 방법이 복잡하다면? (실제 현업에서는 연산해서 만듬)                 
일반적인 String 메시지를 사용한다면 매번 복잡한 방법을 사용해야한다.                
이는 테스트가 성공하든, 실패하든 무조건 String으로 넘겨주기에 실행된다고 한다.          
         
하지만, 람다식으로 만든다면 테스트에 실패했을 경우에만 메시지를 넘겨준다.       
즉, 메시지를 만드는 비용이 조금 걱정되는 수준이면 람다식을 사용하는 것이 성능에서 유리하다.      

## JUnit5 의 assertAll
JUnit5에서는 `assertAll`이라는 메서드가 등장했다.          
네이밍에서 유추해보자면 모든 구문을 테스트한다는 것인데,         
일반적으로 메서드안에 테스트를 여러개 만들어서 검사하면 되지 않나 생각할 수 있다.      
하지만, `assertAll`은 매우 유용한 Assertion이므로 이를 한 번 알아보자   
   
### assertAll 사용 전

**main.me.kwj1270.thejavatest.Study**
```java
package me.kwj1270.thejavatest;

public class Study {
    private StudyStatus studyStatus;
    private int limit = -10;
    public StudyStatus getStatus() {
        return this.studyStatus;
    }

    public int getLimit() {
        return limit;
    }
}

```
   
**test.me.kwj1270.thejavatest.StudyTest**
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import static org.junit.jupiter.api.Assertions.*;

class StudyTest {

    @DisplayName("스터디 모두 화이팅")
    @Test
    public void Study_테스트() {
        Study study = new Study();
        assertNotNull(study);
        assertEquals(StudyStatus.DRAFT, study.getStatus(), () -> "스터디를 처음 만들면 상태값이 DRAFT여야 한다.");
        assertTrue(study.getLimit() > 0, () -> "스터디 최대 참석 가능 인원은 0보다 커야한다.");
        System.out.println("Study_테스트");
    }

}
```

![JUnitAssertAllBefore.png](./image/JUnitAssertAllBefore.png)

기존에는 테스트 중에 검사 결과가 잘못되었을 경우 도중에 멈춘다는 문제가 있다.       
그렇기에 여러 테스트에 문제가 있음에도 단 한 경우의 결과만 얻을 수 있었고            
매번 하나의 `assert-` 메서드를 수행하고 고치고 이를 반복하는 비효율적인 작업을 수행했다.


### assertAll 적용 후   
**me.kwj1270.thejavatest.StudyTest**   
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;


import static org.junit.jupiter.api.Assertions.*;

class StudyTest {

    @DisplayName("스터디 모두 화이팅")
    @Test
    public void Study_테스트() {
        Study study = new Study();
        assertAll(
                () -> assertNotNull(study),
                () -> assertEquals(StudyStatus.DRAFT, study.getStatus(), () -> "스터디를 처음 만들면 상태값이 DRAFT여야 한다."),
                () -> assertTrue(study.getLimit() > 0, () -> "스터디 최대 참석 가능 인원은 0보다 커야한다.")
        );
        System.out.println("Study_테스트");
    }

}
```
  
![JUnitAssertAllAfterOne.png](./image/JUnitAssertAllAfterOne.png)  
![JUnitAssertAllAfterTwo.png](./image/JUnitAssertAllAfterTwo.png)   


하지만, `assertAll()`을 사용하면, `()`안에 들어있는 모든 테스트에 대해서 검사를 진행한다.   
즉, 모든 테스트에 대한 검증 결과를 한번에 확인할 수 있게 되었다.     

```java
    public static void assertAll(Executable... executables) throws MultipleFailuresError {
```
```java
        assertAll(
                () -> assertNotNull(study),
                () -> assertEquals(StudyStatus.DRAFT, study.getStatus(), () -> "스터디를 처음 만들면 상태값이 DRAFT여야 한다."),
                () -> assertTrue(study.getLimit() > 0, () -> "스터디 최대 참석 가능 인원은 0보다 커야한다.")
        );
```
`assertAll()`의 파라미터를 보면 `Executable`자료형이 가변인자로 등록되어 있다.       
그렇기에, 다수의 `assert-` 메서드를 인자로 넘겨줄 수 있다.     
`assert-`다른 인자간의 구분은 기존과 마찬가지로 `,` 를 사용하고 `;`는 붙이지 않는다.      

## assertThrows
`assertThrows()` 는 특정 상황에서 특정 예외가 발생하는지 확인하는 `assert-` 메서드이다.      
    
```java
package me.kwj1270.thejavatest;

public class Study {
    private StudyStatus studyStatus;
    private int limit;

    public Study(int limit){
        if(limit < 0) throw new IllegalArgumentException("limit은 0보다 커야한다.");
        this.limit = limit;
    }

    public StudyStatus getStatus() {
        return this.studyStatus;
    }

    public int getLimit() {
        return limit;
    }
}

```
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;


import static org.junit.jupiter.api.Assertions.*;

class StudyTest {

    @DisplayName("스터디 모두 화이팅")
    @Test
    public void Study_테스트() {
        assertThrows(IllegalArgumentException.class, () -> new Study(-1));
        System.out.println("Study_테스트");
    }

}
```
![JUnitAssertThrowsOne.png](./image/JUnitAssertThrowsOne.png)

알맞은 예외클래스를 던졌기 때문에, 테스트는 성공적으로 끝났다.        
만약 내가 원치 않는곳에서 에러가 발생했다면, 테스트는 실패하고       
우리는 다른 예외가 발생한 이유를 찾아 수정을 진행하면 된다.       
   
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;


import static org.junit.jupiter.api.Assertions.*;

class StudyTest {

    @DisplayName("스터디 모두 화이팅")
    @Test
    public void Study_테스트() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> new Study(-1));
        String exceptionMessage = exception.getMessage();
        assertEquals("limit은 0보다 커야한다.",exceptionMessage);
        System.out.println("Study_테스트");
    }

}
```
또한, `assertThrows()` 첫번째 인자값으로 넘긴 예외 클래스 타입을 반환한다.        
그렇기에 해당 예외 클래스 타입을 가져와, 사용자가 입력한 메시지와 맞는지도 확인 가능하다.        
이렇게 하면, 보다 예외가 발생한 지점을 쉽게 찾을 수 있을 것이다.    
    
## assertTimeout       
`assertTimeout()`은 실행하는 코드가 특정 시간 이내에 완료되는지 확인한다.          
`assertTimeout()`는 첫번째 파라미터로 Duration, 두번째 인자로 테스트 대상을 입력해서            
해당 시간내에 테스트 대상이 끝나는지 확인하는 작업을 수행한다.           

```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import java.time.Duration;

import static org.junit.jupiter.api.Assertions.*;

class StudyTest {

    @DisplayName("스터디 모두 화이팅")
    @Test
    public void Study_테스트() {
        assertTimeout(Duration.ofMillis(10), () -> {
            Thread.sleep(300);
            new Study(10);
        });
    }

}
```
![JUnitAssertTimeoutOne.png](./image/JUnitAssertTimeoutOne.png)

위 코드에서는 일부러 `Thread.sleep()`을 이용해서 강제로 에러를 발생시켰다.      
한 가지 주의할 점은 시간이 지나면 바로 실패를 띄우는 것이 아니라,     
`{}` 코드 블럭을 전부 확인 한 시간을 기준으로 비교를 진행한다.         


```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import java.time.Duration;

import static org.junit.jupiter.api.Assertions.*;

class StudyTest {

    @DisplayName("스터디 모두 화이팅")
    @Test
    public void Study_테스트() {
        assertTimeoutPreemptively(Duration.ofMillis(10), () -> {
            Thread.sleep(300);
            new Study(10);
        });
    }

}
``` 
만약, 시간이 지났다면 더 불필요한 계산 없이 바로 탈출을 하고자 한다면          
`assertTimeoutPreemptively()` 메서드를 사용하면 된다.            
    
하지만, `assertTimeoutPreemptively()` 메서드도 주의할 점이 있다.      
테스트 코드 블럭은 별도의 쓰레드에서 사용하기 때문에,      
만약 쓰레드 로컬을 사용하는 코드가 있다면 예상치 못한 결과가 발생할 수 있다.     
     
JUnit 문서에서도 스프링 트랜잭션 처리 같은 경우 쓰레드 로컬을 기본적으로 사용하기에        
이 경우에는 스프링이 만든 트랜잭션 설정이 테스트에서 제대로 적용이 안될 수 있다.          
즉, **💣 롤백이 안되고 DB에 반영될 수 있는 문제가 발생할 수 있다.**           
(쓰레드 로컬은 다른 쓰레드와 자원을 공유하지 않기 때문이다.)        
그러므로 이 점을 주의하고 `assertTimeoutPreemptively()`을 사용해야한다.         
     
그래서 쓰레드와 전혀 관련이 없는 코드를 사용한다면 상관 없지만,         
쓰레드와 관련된 코드를 사용한다면, 시간이 더 걸리더라도 `assertTimeout()`이 더 낫다.          

## 이외에도   
AssertJ, Hemcrest, Truth등과 같은 써드파티 라이브러리들도 존재한다.        
각각의 써드파티들의 메서드들은 JUnit 에 조금 아쉬운 부분을 보안하여 제공된다.       
spring-boot-starter 를 의존하면 AssertJ, Hemcrest 가 들어온다.      

___ 
익명클래스에 `option + Enter(return)` 누르면 람다식으로 전환할 수 있다.    


