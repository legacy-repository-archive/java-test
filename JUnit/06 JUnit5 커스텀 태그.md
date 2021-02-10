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
이전, [05 JUnit5 태깅과 필터링.md](https://github.com/springframework-sprout/THE_JAVA_TEST/blob/main/JUnit/05%20JUnit5%20%ED%83%9C%EA%B9%85%EA%B3%BC%20%ED%95%84%ED%84%B0%EB%A7%81.md)에서의 소스 코드와 동일하다.   
                
**🤔 위 코드에서의 문제점은 무엇일까?**                        
우선, 어노테이션의 중복? 많은 어노테이션을 한 번에 관리해야 하기에 그것도 맞는 말이다.                    
하지만 무엇보다도 ` @Tag()` 사용시에 멤버 값 문자열에 대한 안정성이 보장되지 않는다는 점이다.                  
즉, 내가 `String` 멤버 값을 넣는데 **오타가 나더라도 이에 대한 안전장치가 없다는 것이다.**                
        
**그래서 이를 해결할 수 있는 커스텀 어노테이션을 한 번 만들어보겠다.**         

# 커스텀 태그 어노테이션 만들기 
**test.me.kwj1270.thejavatest.FastTest**
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.Tag;
import org.junit.jupiter.api.Test;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;


@Tag("fast")
@Test
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface FastTest {
}
```
* `@Test`와 `@Tag("fast")`를 합친 `@FastTest` 어노테이션을 만들었다.       
    
**test.me.kwj1270.thejavatest.SlowTest**
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.Tag;
import org.junit.jupiter.api.Test;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;


@Tag("slow")
@Test
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface SlowTest {
}
```
* `@Test`와 `@Tag("slow")`를 합친 `@SlowTest` 어노테이션을 만들었다.     
       
**test.me.kwj1270.thejavatest.StudyTest.java**
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import static org.assertj.core.api.Assertions.*;

class StudyTest {

    @FastTest
    @DisplayName("메인_테스트 fast")
    public void 메인_테스트() {
        Study actual = new Study(10);
        assertThat(actual.getLimit()).isGreaterThan(0);
        System.out.println("메인_테스트 실행");
    }

    @SlowTest
    @DisplayName("서브_테스트 slow")
    public void 서브_테스트() {
        System.out.println("서브_테스트");
    }

}
```
기존 코드와 다르게 어노테이션의 중복이 없어졌고 어노테이션의 이름도 명확해졌다!        
그리고 무엇보다도 `@Tag()`에 한해서 멤버값 오타를 일으킬 수 있는 문제가 사라졌다.       
        
사실, `@Tag()`를 위한 것 뿐만 아니라,           
`JUnit5 어노테이션`을 활용한 커스텀 어노테이션을 통해       
개발자는 더 가독성이 좋고 유지보수하기 쉬운 어노테이션을 만들어 사용할 수 있다.    



