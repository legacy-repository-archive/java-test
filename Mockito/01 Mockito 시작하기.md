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

 
