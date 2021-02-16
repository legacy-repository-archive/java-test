# 💻 THE_JAVA_TEST
백기선, 더 자바, 애플리케이션을 테스트하는 다양한 방법

💡 이 레포지토리는 백기선님의 인프런 강의 [더 자바, 애플리케이션을 테스트하는 다양한 방법](https://www.inflearn.com/course/the-java-application-test)을 팔로우하고 있습니다.   
🔌 보다 자세한 내용과 설명은 백기선님의 온라인 강의를 듣는 것을 적극 추천드립니다.         
   
# Mockito   
**Mock**이란 진짜 객체와 비슷하게 동작하지만 **프로그래머가 직접 그 객체의 행동을 관리하는 모형 객체**를 의미한다.            
**Mockito**는 **`Mock 객체`를 쉽게 만들고 관리하고 검증**할 수 있는 방법을 제공하는 **테스팅 프레임워크다.**            
      
[JetBrains의 설문조사](https://www.jetbrains.com/lp/devecosystem-2019/java/)에 의하면 **자바 개발자가 가장 많이 사용한 Mock 프레임워크다.**       
          
**🤔 그렇다면 JUnit도 있는데 Mock 은 왜 필요한걸까?**    
   
애플리케이션이 데이터베이스를 사용, 외부 API 호출, 자사 플랫폼의 API 를 호출할만큼 복잡해졌다 가정한다.
애플리케이션의 기능들을 검증하기 위해 `외부 API 호출`과 같은 작업들도 테스트해야 한다.       
       
하지만, 테스트를 진행함에 있어 **매번 `외부 API 호출`과 같은 작업을 하는 것은 매우 복잡하고 번거로운 일이다.**                    
특히 이러한 서비스를 테스팅하기 위해서는 **실제 서비스와 동일한 수준의 코드를 작성**해야하며                     
또한, 이러한 서비스 클래스들이 **미리 정의되어 있지 않는다면 테스트를 진행할 수도 없다.**                            
결국, **테스트라는 본질을 잃어버리고 하나의 서비스를 만드는 작업이 되어버린다.**           
     
Mock은 테스트에 필요한 서비스를 호출한다고 가정을 한 **모형 객체이다**            
Mock 객체를 이용해서 우리는 **실제 서비스를 이용하는 것처럼 기능을 테스트** 할 수 있다.            
Mock 객체를 이용해서 **구현되어 있지 않은 서비스를 예상하고 기능을 테스트** 할 수 있다.       
      
그렇기 때문에, `Mock`은 단순히 모형 객체가 아닌 **`테스트`라는 본질을 찾기 위한 객체이다.**     

**Mock이 필요한 상황을 정리해보면 아래와 같다.**
   
- 테스트 환경을 구축하는데 시간이 오래걸리는 경우   
- 특정 모듈을 갖고 있지 않아 테스트 환경을 구축하기 어려운 경우   
- API 호출 및 사용에 대한 협의나 정책이 필요한 경우  
- 테스트가 특정 경우나 순간에 의존적인 경우
- 테스트 시간이 오래 걸리는 경우        
- 개인 PC의 성능이나 서버의 성능문제로 테스트가 오래 걸릴수 있는 경우    
            
    



테스트를 만들고 이에 맞춰서 코드로 구현하는 TDD의 







위처럼 CellPhoneMmsSender를 구현하는 상황이라고 해보자. 
그 과정에서 CellphoneService라는 객체의 sendMMS 함수를 호출한다. 
여기서 내가 구현한 send 함수를 테스트 한다면, 적절한 파라미터로 sendMMS 함수를 호출하는지만 확인하면 된다. 
sendMMS가 제대로 구현되었는지는 중요하지 않기 때문이다. 테스트의 주된 관심사가 아니다!

그렇다면 굳이 테스트를 위해 CellphoneService 객체를 생성해서 사용할 필요가 없다. 
적당히 sendMMS의 정상 리턴값을 던져주는 Fake CellphoneService를 사용하면 되는 것이다. 
왜? CellphoneMmsSender의 behavior를 테스트하는데 집중하기 위해! 이것이 Mock Object를 사용하는 이유이다.






[단위 테스트에 고찰](https://martinfowler.com/bliki/UnitTest.html)     


Mockito 시작하기
스프링 부트 2.2+ 프로젝트 생성시 spring-boot-starter-test에서 자동으로 Mockito 추가해 줌.

스프링 부트 쓰지 않는다면, 의존성 직접 추가.
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

다음 세 가지만 알면 Mock을 활용한 테스트를 쉽게 작성할 수 있다.
Mock을 만드는 방법
Mock이 어떻게 동작해야 하는지 관리하는 방법
Mock의 행동을 검증하는 방법

Mockito 레퍼런스
https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html

Mock 객체 만들기
Mockito.mock() 메소드로 만드는 방법
        MemberService memberService = mock(MemberService.class);
        StudyRepository studyRepository = mock(StudyRepository.class);

@Mock 애노테이션으로 만드는 방법
JUnit 5 extension으로 MockitoExtension을 사용해야 한다.
필드
메소드 매개변수

@ExtendWith(MockitoExtension.class)
class StudyServiceTest {

    @Mock MemberService memberService;

    @Mock StudyRepository studyRepository;

@ExtendWith(MockitoExtension.class)
class StudyServiceTest {
    
    @Test
    void createStudyService(@Mock MemberService memberService,
                            @Mock StudyRepository studyRepository) {
        StudyService studyService = new StudyService(memberService, studyRepository);
        assertNotNull(studyService);
    }

}

Mock 객체 Stubbing
모든 Mock 객체의 행동
Null을 리턴한다. (Optional 타입은 Optional.empty 리턴)
Primitive 타입은 기본 Primitive 값.
콜렉션은 비어있는 콜렉션.
Void 메소드는 예외를 던지지 않고 아무런 일도 발생하지 않는다.

Mock 객체를 조작해서
특정한 매개변수를 받은 경우 특정한 값을 리턴하거나 예뢰를 던지도록 만들 수 있다.
How about some stubbing?
Argument matchers
Void 메소드 특정 매개변수를 받거나 호출된 경우 예외를 발생 시킬 수 있다.
Subbing void methods with exceptions
메소드가 동일한 매개변수로 여러번 호출될 때 각기 다르게 행동호도록 조작할 수도 있다.
Stubbing consecutive calls

Mock 객체 Stubbing 연습 문제
다음 코드의 // TODO에 해당하는 작업을 코딩으로 채워 넣으세요.

Study study = new Study(10, "테스트");

// TODO memberService 객체에 findById 메소드를 1L 값으로 호출하면 Optional.of(member) 객체를 리턴하도록 Stubbing
// TODO studyRepository 객체에 save 메소드를 study 객체로 호출하면 study 객체 그대로 리턴하도록 Stubbing

studyService.createNewStudy(1L, study);

assertNotNull(study.getOwner());
assertEquals(member, study.getOwner());

git checkout 216112f5706fef76f56c735faed200f311b8d919


Mock 객체 확인
Mock 객체가 어떻게 사용이 됐는지 확인할 수 있다.
특정 메소드가 특정 매개변수로 몇번 호출 되었는지, 최소 한번은 호출 됐는지, 전혀 호출되지 않았는지
Verifying exact number of invocations
어떤 순서대로 호출했는지
Verification in order
특정 시간 이내에 호출됐는지
Verification with timeout
특정 시점 이후에 아무 일도 벌어지지 않았는지
Finding redundant invocations

Mockito BDD 스타일 API
BDD: 애플리케이션이 어떻게 “행동”해야 하는지에 대한 공통된 이해를 구성하는 방법으로, TDD에서 창안했다.

행동에 대한 스팩
Title
Narrative
As a  / I want / so that
Acceptance criteria
Given / When / Then

Mockito는 BddMockito라는 클래스를 통해 BDD 스타일의 API를 제공한다.

When -> Given
given(memberService.findById(1L)).willReturn(Optional.of(member));
given(studyRepository.save(study)).willReturn(study);

Verify -> Then
then(memberService).should(times(1)).notify(study);
then(memberService).shouldHaveNoMoreInteractions();

참고
https://javadoc.io/static/org.mockito/mockito-core/3.2.0/org/mockito/BDDMockito.html
https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#BDD_behavior_verification



Mockito 연습 문제
다음 StudyService 코드에 대한 테스트를 Mockito를 사용해서 Mock 객체를 만들고 Stubbing과 Verifying을 사용해서 테스트를 작성하세요.

StudyService.java
public Study openStudy(Study study) {
    study.open();
    Study openedStudy = repository.save(study);
    memberService.notify(openedStudy);
    return openedStudy;
}

StudyServiceTest.java

@DisplayName("다른 사용자가 볼 수 있도록 스터디를 공개한다.")
@Test
void openStudy() {
    // Given
    StudyService studyService = new StudyService(memberService, studyRepository);
    Study study = new Study(10, "더 자바, 테스트");
    // TODO studyRepository Mock 객체의 save 메소드를호출 시 study를 리턴하도록 만들기.

    // When
    studyService.openStudy(study);

    // Then
    // TODO study의 status가 OPENED로 변경됐는지 확인
    // TODO study의 openedDataTime이 null이 아닌지 확인
    // TODO memberService의 notify(study)가 호출 됐는지 확인.
}

git checkout d52c6af44e82ca1c474bd45290e4a44340ffd483


	

