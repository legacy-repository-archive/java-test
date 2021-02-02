# 들어가면서 

# JUnit 소개

JUnit은 대다수의 자바 개발자가 이용하는 자바 테스팅 프레임워크입니다.      
실제로 [JetBrains의 설문조사](https://www.jetbrains.com/lp/devecosystem-2019/java/)에 따르면      
자바 개발자의 93%가 단위 테스트에 JUnit을 사용해본 경험이 있다는 결과를 발표했습니다.     
     
개발자라면 누구나 자신의 코드에 대한 테스트를 진행해야 합니다.           
그렇기 때문에 우리가 사용할 테스팅 프레임워크에 대해서 잘 알 필요가 있습니다.    

![image](https://user-images.githubusercontent.com/50267433/106532524-ade2ee00-6533-11eb-85ca-677c98e5d5a5.png)      
 
* JUnit Platform : 테스트를 실행해주는 런처 제공, TestEngine API 제공   
* Jupiter : TestEnigine API 구형체로 JUnit 5를 제공   
* Vintage : Junit 4와 3을 지원하는 TestEngine 구현체   
   
더 자세한 내용을 [JUnit5 공식 레퍼런스](https://junit.org/junit5/docs/current/user-guide/)를 통해 확인!   

# JUnit5 VS JUnit4 

우리가 공부하고자 하는 `JUnit5`는 2017년 2월 알파버전부터 시작하여    
현재까지도 많은 개발자에게 사랑을 받는 `테스팅 프레임워크`입니다.       

당연한 얘기지만 `JUnit5` 이전에는 `JUnit4`가 존재했습니다.      
그렇다면 무엇이 달라졌을까요?      

`JUnit5` 팀은 2가지 영역을 중점으로 개발을 진행했습니다.   
  
* 테스트 작성을 위한 **API 세트**        
* `tool Builder` 가 테스트를 `발견`, `필터링`,`실행`하는 데 사용할 수 있는   
**JUnit 플랫폼 런처 API**     
       
Junit5로 넘어와서 기존에는 다소 불명확한 이름을 갖지고 있던 어노테이션들도    
보다 명확성을 가질 수 있도록 새로운 이름으로 지정해주었습니다.      
       
|JUnit 5|JUnit 4|
|-------|-------|
|@BeforeEach|@Before|
|@AfterEach|@After|
|@BeforeAll|@BeforeClass|
|@AfterAll|@AfterClass|
|@Disabled|@Ignore|
	
	
	
	
	
