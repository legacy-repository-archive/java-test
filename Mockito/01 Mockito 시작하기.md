# Mockito 시작하기  
`Spring Initializr`의 스프링 부트 `2.2+`이상 부터는 프로젝트 생성시에       
자동으로 `spring-boot-starter-test`받게 되면서 `Mockito` 의존성을 추가해준다.   
     
하지만, 만약 `Spring Initializr`를 사용하지 않거나       
`spring-boot-starter-test`가 없는 경우, 의존성을 직접 추가해줘야 한다.     

```xml

```
```gradle
```

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
