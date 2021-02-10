# JUnit5 커스텀 태그
JUnit5 에서 제공하는 어노테이션들은 메타어노테이션으로 사용할 수 있다.      
즉, 커스텀 어노테이션에서 JUnit5 어노테이션을 부가기능으로 사용할 수 있다.     

```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import static org.assertj.core.api.Assertions.*;

class StudyTest {

    @Tag("fast")
    @DisplayName("메인_테스트 fast")
    @Test
    public void 메인_테스트() {
        Study actual = new Study(10);
        assertThat(actual.getLimit()).isGreaterThan(0);
        System.out.println("메인_테스트 실행");
    }

    @Tag("slow")
    @DisplayName("서브_테스트 slow")
    @Test
    public void 서브_테스트() {
        System.out.println("서브_테스트");
    }
}
```
이전, [05 JUnit 태깅과 필터링.md](https://github.com/springframework-sprout/THE_JAVA_TEST/blob/main/JUnit/05%20JUnit%20%ED%83%9C%EA%B9%85%EA%B3%BC%20%ED%95%84%ED%84%B0%EB%A7%81.md)에서의 소스 코드와 동일하다 
