# JUnit5 조건에 따라 테스트 실행하기    

특정한 조건을 만족하는 경우에 테스트를 실행하는 방법    

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
    
