# Mockito 시작하기  
스프링 부트 `2.2+`이상 부터는 프로젝트 생성시에 
자동으로 `spring-boot-starter-test`받게 되면서 `Mockito` 의존성을 추가해준다.  


스프링 부트 쓰지 않는다면, 의존성 직접 추가.


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
