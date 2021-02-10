# JUnit5 테스트 반복하기    
> 테스트를 반복적으로 실행하는 방법에 대해서 알아보자   
   
주로, 아래와 같은 상황에서 **테스트를 반복적으로 사용한다.**            
     
* 매번 랜덤값을 사용한다.                
* 실행되는 타이밍에 따라 조건이 달라진다.                   

# @RepeatedTest
`@RepeatedTest`는 반복 횟수와 반복 테스트 이름을 설정할 수 있는 테스트이다.    


```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import static org.assertj.core.api.Assertions.*;

class StudyTest {

    @RepeatedTest(10)
    void 반복_테스트() {
        System.out.println("test");
    }

}
```   
`@RepeatedTest()`에 `value 멤버`에 **`int`형 정수**를 입력하면       
입력한 숫자만큼 테스트를 반복하도록 동작시킨다.      
그리고 `value`멤버는 어노테이션 문법상 생략가능하므로 주로, 정수만 입력한다.        

```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import static org.assertj.core.api.Assertions.*;

class StudyTest {

    @RepeatedTest(10)
    void 반복_테스트(RepetitionInfo repetitionInfo) {
        System.out.println("test" + repetitionInfo.getCurrentRepetition()+"/"+repetitionInfo.getTotalRepetitions());
    }

}
```   
`@RepeatedTest`를 선언한 테스트 메서드는              
`RepetitionInfo` 인스턴스를 매개변수로 받아 사용할 수 있다.        

`RepetitionInfo`은 인터페이스로 아래와 같은 2가지 추상 메서드를 가진다.    

* `int getCurrentRepetition()` : 반복중인 **현재 횟수**를 리턴한다.       
* `int getTotalRepetitions()` : 반복되어야할 **전체 횟수**를 리턴한다.     
      
구현체가 무엇인지는 자세히 알 수 없었지만,        
이를 통해, 테스트 로직에 반복에 대한 현재 횟수와, 전체 횟수를 사용할 수 있다.    
  
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import static org.assertj.core.api.Assertions.*;

class StudyTest {

    @DisplayName("반복_테스트")
    @RepeatedTest(value = 10, name = "{displayName}, {currentRepetition}/{totalRepetitions}")
    void 반복_테스트(RepetitionInfo repetitionInfo) {
        System.out.println("test" + repetitionInfo.getCurrentRepetition()+"/"+repetitionInfo.getTotalRepetitions());
    }

}    
```  
`@RepeatedTest`의 멤버로는 `value`외에도 `name`이 존재한다.          
`@RepeatedTest`의 `name`을 선언할 경우 `@DisplayName`처럼 **반복 테스트의 이름을 정의할 수 있다.**      
      
기본적으로 **문자열을 사용할 수 있으며** 추가로, `{}`값을 사용할 수 있다.       
   
* **`{displayName}` :** 메서드의 이름을 나타내며 `@DisplayName`이 있을 경우 어노테이션에 정의된 이름을 우선시한다.        
* **`{currentRepetition}` :** 반복중인 `현재 횟수`를 의미한다.  
* **`{totalRepetitions}` :** 반복되어야할 `전체 횟수`를 의미한다.   
     
`name`의 디폴트 값으로는 `"repetition {currentRepetition} of {totalRepetitions}"`이 들어있다.   

    
# @ParameterizedTest      
`@ParameterizedTest`는 테스트에 여러 다른 매개변수를 대입해가며 반복적으로 실행한다.       
`JUnit4`에서는 써드파티 라이브러리에서 불러와 사용했지만,       
`JUnit5`에서 부터 기본으로 제공해주기 시작했다.           
      
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

import static org.assertj.core.api.Assertions.*;

class StudyTest {

    @ParameterizedTest
    @ValueSource(strings = {"날씨가", "많이", "추워지고", "있네요"})
    void ParameterizedTest(String message){
        System.out.println(message);
    }

}
```

```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

import static org.assertj.core.api.Assertions.*;

class StudyTest {

    @DisplayName("파라미터_TEST")
    @ParameterizedTest(name = "{index} {displayName} {0}")
    @ValueSource(strings = {"날씨가", "많이", "추워지고", "있네요"})
    void ParameterizedTest(String message){
        System.out.println(message);
    }

}

```

`@ParameterizedTest`의 멤버로 `name`이 존재한다.          
`@ParameterizedTest`의 `name`을 선언할 경우 `@DisplayName`처럼 **반복 테스트의 이름을 정의할 수 있다.**         
         
* **`{displayName}` :** 메서드의 이름을 나타내며 `@DisplayName`이 있을 경우 어노테이션에 정의된 이름을 우선시한다.      
* **`{index}` :** 현재 반복 횟수를 나타내며 1부터 시작한다.                
* **`{arguments}` :** 파라미터로 들어오는 모든 값들을 한번에 출력한다.(여러 매개변수들을 한번에)             
* **`{0}`, `{1}`, .. :** 파라미터로 들어오는 값들을 정의된 순서에 맞게끔 가져오며 0부터 시작한다.        
* **`{argumentsWithNames}` :** 매개변수의 이름과 값을 `이름=값` 형태로 한번에 표현한다.              
    
`name`의 디폴트 값으로는 `default "[{index}] {argumentsWithNames}"`이 들어있다.     
