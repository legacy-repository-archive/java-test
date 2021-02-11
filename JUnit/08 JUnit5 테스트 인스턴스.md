# JUnit5 테스트 인스턴스

JUnit은 테스트 메소드 마다 테스트 인스턴스를 새로 만든다.
이것이 기본 전략.
테스트 메소드를 독립적으로 실행하여 예상치 못한 부작용을 방지하기 위함이다.
이 전략을 JUnit 5에서 변경할 수 있다.

@TestInstance(Lifecycle.PER_CLASS)
테스트 클래스당 인스턴스를 하나만 만들어 사용한다.
경우에 따라, 테스트 간에 공유하는 모든 상태를 @BeforeEach 또는 @AfterEach에서 초기화 할 필요가 있다.
@BeforeAll과 @AfterAll을 인스턴스 메소드 또는 인터페이스에 정의한 default 메소드로 정의할 수도 있다. 
