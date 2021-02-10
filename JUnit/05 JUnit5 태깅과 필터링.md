# JUnit 태깅과 필터링   
> JUnit5 가 제공하는 **Test Tagging**과 **Filtering**에 대해서 알아보자   
    
**Test Tagging**은 우리가 만든 **여러 Test 메서드를 태그를 통해 그룹화시키는 것**을 말한다.  
그룹화의 기준은 전적으로 개발자에 있으며   
가령 예를 들어, `단위 테스트/통합 테스트`와 같은 모듈별로 진행하던지      
테스트가 `빠른지/안 빠른지`와 같은 속도를 기준으로 그룹화 시킬 수 있다.     
         
쉽게 말하면, **테스트 그룹을 만들고 원하는 테스트 그룹만 테스트를 실행할 수 있는 기능이다.**            
   
# @Tag        
**`@Tag`** 를 통해 테스트 메소드에 태그를 추가할 수 있다.        
`@Tag` 의 `()` **멤버 값으로 String 문자열**이 오며, 이 문자열을 기준으로 그룹화를 진행한다.        
참고로, 하나의 `@Test 메소드`에 여러개의 `@Tag`를 사용할 수 있다.           
       
우선, `@Tag`에는 네이밍 규칙이 있다.  
* 태그는 `null`일 수 없다.
* 태그는 공백을 포함 할 수 없다.
* 태그는 ISO 제어 문자를 포함 할 수 없다.    
* 태그는 아래와 같은 예약 문자를 포함할 수 없다.     
  * `,` : 쉼표   
  * `(` : 왼쪽 괄호   
  * `)` : 오른쪽 괄호  
  * `&` : 앰퍼샌드     
  * `|` : 세로 막대    
  * `!` : 느낌표    
    
    
가정을 하나 해보려 한다.          
   
* `메인_테스트()` 테스트 케이스는 실행하는데 얼마 걸리지 않아 로컬에서 수행하고자 한다.      
* `서브_테스트()` 테스트 케이스는 실행하는데 오래 걸리므로 CI(빌드)환경에서 수행하고자 한다. (빌드 서버)   
           
이러한 경우, 테스트를 각각의 상황에 맞춰서 실행시키고 싶으므로 테스트 태깅을 사용한다.   
    
```java
package me.kwj1270.thejavatest;

import org.junit.jupiter.api.*;

import static org.assertj.core.api.Assertions.*;

class StudyTest {

    @Tag("fast")
    @DisplayName("메_테스트 fast")
    @Test
    public void 메인_테스트() {
        Study actual = new Study(10);
        assertThat(actual.getLimit()).isGreaterThan(0);
    }

    @Tag("slow")
    @DisplayName("서브_테스트 slow")
    @Test
    public void 서브_테스트() {
        System.out.println("서브_테스트");
    }

}
```
Mac 에서 **`ctrl`**`+`**`shift`**`+`**`r`** 을 눌러서 테스트를 진행해보자             
하지만, 코드를 위와 같이 작성했음에도 테스트를 실행하면 그룹별로가 아닌 모든 테스트가 수행된다.         
       
사실, 그룹별로 테스트를 수행하고자 한다면 추가적인 환경설정이 필요하다.     
`IntelliJ` 에서는 테스트에 대해서 디폴트로 클래스 단위로 수행하기에          
이를 태그 단위로 수행할 수 있도록 설정을 바꿔줘야한다.       
   


1. `[Run/Debug Configuration](Edit configuration)` 으로 들어간다. 
2. 
3.
4.
5.





