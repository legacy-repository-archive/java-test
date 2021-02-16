# Mockito 시작하기  
`Spring Initializr`의 스프링 부트 `2.2+`이상 부터는 프로젝트 생성시에       
자동으로 `spring-boot-starter-test`받게 되면서 `Mockito` 의존성을 추가해준다.   
     
하지만, 만약 `Spring Initializr`를 사용하지 않거나       
`spring-boot-starter-test`가 없는 경우, 의존성을 직접 추가해줘야 한다.     

**pom.xml**
```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```  
**build.gradle**
```gradle
dependencies {
    ...
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```
위와 같이 `spring-boot-starter-test`를 의존받아 한번에 설정해줘도 되며   


**pom.xml**
```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>3.1.0</version>
    <scope>test</scope>
</dependency>


<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-junit-jupiter</artifactId>
    <version>3.1.0</version>
    <scope>test</scope>
</dependency>
```
**build.gradle**
```gradle
dependencies {
    ...
    testImplementation 'org.mockito:mockito-core'
    testImplementation 'org.mockito:mockito-junit-jupiter'
}
```
`mockito-core`와 `mockito-junit-jupiter`를 의존받아 사용할 수 있다.        
`mockito-junit-jupiter`는 Mockito와 JUnit을 연결시켜주는 라이브러리다.     

# 앞으로 공부하면서  
`Mockito`는 다음 3 가지만 알면 **Mock을 활용한 테스트를 쉽게 작성할 수 있다.**     
  
* Mock을 만드는 방법
* Mock이 어떻게 동작해야 하는지 관리하는 방법
* Mock의 행동을 검증하는 방법

조금 더 많은 자료가 필요하다면 [Mockito 레퍼런스](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html)를 참조하자    

# 테스트를 진행하기 위한 추가 환경 설정  
## 의존성 주입 
```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
```
강의에서는 `lombok`, `jpa`를 사용하기에 이와 관련된 라이브러리를 추가로 의존시켰다.   


## 소스코드  
**src.main.java.me.kwj1270.thejavatest.domain.Member**
```java
package me.kwj1270.thejavatest.domain;

import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
@Getter @Setter @NoArgsConstructor
public class Member {

    @Id @GeneratedValue
    private Long id;

    private String email;

}
```
**src.main.java.me.kwj1270.thejavatest.domain.Study**
```java
package me.kwj1270.thejavatest.domain;

import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import java.time.LocalDateTime;

@Entity
@Getter @Setter @NoArgsConstructor
public class Study {

    @Id @GeneratedValue
    private Long id;
    private StudyStatus status = StudyStatus.DRAFT;
    private int limitCount;
    private String name;
    private LocalDateTime openedDateTime;
    private Long ownerId;

    public Study(int limit, String name) {
        this.limitCount = limit;
        this.name = name;
    }

    public Study(int limit) {
        if (limit < 0) {
            throw new IllegalArgumentException("limit은 0보다 커야 한다.");
        }
        this.limitCount = limit;
    }

    public void open() {
        this.openedDateTime = LocalDateTime.now();
        this.status = StudyStatus.OPENED;

    }

}
```
**src.main.java.me.kwj1270.thejavatest.domain.StudyStatus**
```java
package me.kwj1270.thejavatest.domain;

public enum StudyStatus {
    DRAFT, OPENED, STARTED, ENDED
}

```

**src.main.java.me.kwj1270.thejavatest.member.MemberService**
```java
package me.kwj1270.thejavatest.member;

import me.kwj1270.thejavatest.domain.Member;

import java.util.Optional;

public interface MemberService {

    Optional<Member> findById(Long memberId);
}
```

**src.main.java.me.kwj1270.thejavatest.study.StudyController**
```java
package me.kwj1270.thejavatest.study;

import lombok.RequiredArgsConstructor;
import me.kwj1270.thejavatest.domain.Study;
import org.springframework.web.bind.annotation.*;

@RestController
@RequiredArgsConstructor
public class StudyController {

    final StudyRepository repository;

    @GetMapping("/study/{id}")
    public Study getStudy(@PathVariable Long id) {
        return repository.findById(id)
                .orElseThrow(() -> new IllegalArgumentException("Study not found for '" + id + "'"));
    }

    @PostMapping("/study")
    public Study createsStudy(@RequestBody Study study) {
        return repository.save(study);
    }

}
```

**src.main.java.me.kwj1270.thejavatest.study.StudyRepository**
```java
package me.kwj1270.thejavatest.study;

import me.kwj1270.thejavatest.domain.Study;
import org.springframework.data.jpa.repository.JpaRepository;

public interface StudyRepository extends JpaRepository<Study, Long> {

}
```

**src.main.java.me.kwj1270.thejavatest.study.StudyService**
```java
package me.kwj1270.thejavatest.study;


import me.kwj1270.thejavatest.domain.Member;
import me.kwj1270.thejavatest.domain.Study;
import me.kwj1270.thejavatest.member.MemberService;

import java.util.Optional;

public class StudyService {

    private final MemberService memberService;
    private final StudyRepository repository;

    public StudyService(MemberService memberService, StudyRepository repository) {
        assert memberService != null;           // assert 키워드를 사용하면, null임을 확인할 수 있다. (AssertException)
        assert repository != null;
        this.memberService = memberService;
        this.repository = repository;
    }

    public Study createNewStudy(Long memberId, Study study) {
        Optional<Member> member = memberService.findById(memberId);
        if (!member.isPresent()) {
            throw new IllegalArgumentException("Member doesn't exist for id: '" + memberId + "'");
        }
        study.setOwnerId(memberId);
        Study newstudy = repository.save(study);
        return newstudy;
    }

}
```
**src.main.java.me.kwj1270.thejavatest.TheJavaTestApplication**  
```java
package me.kwj1270.thejavatest;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class TheJavaTestApplication {

    public static void main(String[] args) {
        SpringApplication.run(TheJavaTestApplication.class, args);
    }

}
```
