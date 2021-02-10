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




{displayName}
{currentRepetition}
{totalRepetitions}
RepetitionInfo 타입의 인자를 받을 수 있다.

# @ParameterizedTest
테스트에 여러 다른 매개변수를 대입해가며 반복 실행한다.
{displayName}
{index}
{arguments}
{0}, {1}, ...
