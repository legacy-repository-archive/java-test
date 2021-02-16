# Mock 객체 만들기   

초기 환경 설정은 [링크](https://github.com/springframework-sprout/THE_JAVA_TEST/blob/main/Mockito/01%20Mockito%20%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0.md#%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%A5%BC-%EC%A7%84%ED%96%89%ED%95%98%EA%B8%B0-%EC%9C%84%ED%95%9C-%EC%B6%94%EA%B0%80-%ED%99%98%EA%B2%BD-%EC%84%A4%EC%A0%95)를 참조

## StudyServiceTest 만들기   
이전 JUnit에서 다루었듯이   
`StudyService` 클래스에서 **`command`**`+`**`shift`**`+`**`t`** 를 눌러 `StudyServiceTest` 클래스를 만든다.      

**src.test.java.me.kwj1270.thejavatest.study**   
```java
package me.kwj1270.thejavatest.study;

import static org.junit.jupiter.api.Assertions.*;

class StudyServiceTest {

}
```

## Mock 사용하기   
`StudyService` 를 테스트하려면 StudyService 인스턴스를 만들어야 한다.     
   
```java
package me.kwj1270.thejavatest.study;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

class StudyServiceTest {

    @Test
    void createStudyService() {
        StudyService studyService = new StudyService();
    }
}
```

![MockitoNotMakeStudyService.png](./images/MockitoNotMakeStudyService.png)
  
new를 이용해 StudyService 인스턴스를 만들고자 하지만 불가능하다.       
`StudyService` 의 생성자는 `MemberService`와 `StudyRepository`의 구현체를 주입 받지만        
위 그림에서 알 수 있듯이 실제로는 인터페이스만 존재하고 이를 구현한 구현체 클래스가 없기 때문이다.    
         
그리고 바로 이러한 상황이 **`Mock`을 사용하기 아주 좋은 예시이다.**          

