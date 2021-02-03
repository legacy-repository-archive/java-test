# JUnit5 테스트 이름 표기하기

* `@DisplayNameGeneration` :     
  * Method와 Class 레퍼런스를 사용해서 테스트 이름을 표기하는 방법 설정    
  * 기본 구현체로 ReplaceUnderscores 제공한다.   
* `@DisplayName` :   
  * 어떤 테스트인지 테스트 이름을 보다 쉽게 표현할 수 있는 방법을 제공 
  * `@DisplayNameGeneration` 보다 우선순위가 높다.    

