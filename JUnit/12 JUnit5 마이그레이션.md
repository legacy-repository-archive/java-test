# JUnit5 마이그레이션
`JUnit5`가 제공하는 `JUnit4` 마이그레이션을 살펴보자   

**gradle Version**
```gradle
testRuntimeOnly("org.junit.platform:junit-platform-launcher:1.7.1")
testRuntimeOnly("org.junit.jupiter:junit-jupiter-engine:5.7.1")
testRuntimeOnly("org.junit.vintage:junit-vintage-engine:5.7.1")
```

**xml Version**
```xml
<!-- Only needed to run tests in a version of IntelliJ IDEA that bundles older versions -->
<dependency>
    <groupId>org.junit.platform</groupId>
    <artifactId>junit-platform-launcher</artifactId>
    <version>1.7.1</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.7.1</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.vintage</groupId>
    <artifactId>junit-vintage-engine</artifactId>
    <version>5.7.1</version>
    <scope>test</scope>
</dependency>
```

* junit-platform-launcher
* junit-jupiter-engine
* junit-vintage-engine
  
spring-boot-starter-test를 이용해 의존성을 주입받는다면       
위와 같이 3개의 Junit 모듈(라이브러리)를 다운받아 사용할 수 있게된다.      
           
그중에서, `junit-vintage-engine` 같은 경우 `Junit4`로 작성한 코드를 실행할 수 있다.            
JUnit5의 `JUnit Platform`이 `Vintage 구현체`를 구동시켜 `JUnit4` 코드를 실행시킨 것이다.      
반대로, 우리가 사용하던 JUnit5의 코드는 `Jupiter 구현체`를 구동시켜서 코드를 실행시킨 것이다.      

# `@Rule` 사용하기  
`Vintage`는 대부분의 JUnit4 코드는 실행시킬 수 있다.          
하지만, 대부분이라는 말 그대로 `@Rule`은 사용 불가능하다.            
`@Rule`을 사용하려면 `junit-jupiter-migrationsupport` 모듈이 제공하는 `@EnableRuleMigrationSupport`이 필요하다.             
`@EnableRuleMigrationSupport`를 사용하면 다음 타입의 Rule을 지원한다.     
     
* ExternalResource    
* Verifier   
* ExpectedException   
        
하지만, 위에서 알 수 있듯이       
3 종류만 지원하고 완전한 마이그레이션을 지원하지 못하기에 starter 구성에서 제외시켰다.            
      
추가로, JUnit4에서 Junit5로 마이그레이션을 진행하고자 한다면 아래 표를 확인하면 좋을 것이다.   

|JUnit 4|JUnit 5|
|-------|-------|
|@Category(Class)|@Tag(String)|
|@RunWith, @Rule, @ClassRule|@ExtendWith, @RegisterExtension|
|@Ignore|@Disabled|
|@Before, @After, @BeforeClass, @AfterClass|@BeforeEach, @AfterEach, @BeforeAll, @AfterAll|
 
# 이 외에      
`Junit4` 에서는 `@RunWith`과 같은 어노테이션을 메타 어노테이션으로 사용하지 못했다.        
하지만, `Junit5`로 넘어오면서 테스트 관련 어노테이션을 메타 어노테이션으로 사용할 수 있게 되어     
`@SpringBootTest`에서도 `@ExtenedWith(SpringExtension.class)`를 통해 서버를 구동시키고 있다.    
즉, 외부에서 서버를 구동시키기 위해 `@ExtenedWith(SpringExtension.class)`을 기술하지 않아도 된다.   



