# JUnit5 Assertion    
   
Assertion 은 우리가 실제 테스트에서 검증하고자하는 내용을 확인하는 기능이다.             
코드에서 Assertion은, `org.junit.jupiter.api.Assertions`에 들어있는 `assert-` static 메서드를 말한다.        
`org.junit.jupiter.api.Assertions`의 static 메서드이기에 `import static`을 이용해야 한다.    

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

    @DisplayName("☺️")
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
* 테스트 클래스에서는 Study의 상태가 `DRAFT`인지 확인한다.     



## 메세지 입력하기
