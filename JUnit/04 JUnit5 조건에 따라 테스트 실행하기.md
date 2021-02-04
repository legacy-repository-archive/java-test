# JUnit5 조건에 따라 테스트 실행하기    

특정한 조건을 만족하는 경우에 테스트를 실행하는 방법 
즉, 조건에 따라 테스트를 실행하는 방법      

예를 들면 `특정 OS`, `특정 JDK 버전`, `특정 환경 변수`, `특정 시스템 변수`에 따라    
실행하거나 실행하지 말아야 되는 경우 이를 `assume-` 메서드를 통해 해결할 수 있다.      

* **`org.junit.jupiter.api.Assumptions.*`**
    * `import static`을 사용해야 한다.  
    * assumeTrue(boolean assumption);    
    * assumingThat(BooleanSupplier assumptionSupplier, Executable executable)      
       

* **`@Enabled___`** 와 **`@Disabled___`**    
    * OnOs
    * OnJre
    * IfSystemProperty
    * IfEnvironmentVariable
    * If  
    
