# JUnit5 테스트 반복하기    
> 테스트를 반복적으로 실행하는 방법에 대해서 알아보자   
   
주로, 아래와 같은 상황에서 **테스트를 반복적으로 사용한다.**            
   
* 매번 랜덤값을 사용한다.              
* 실행되는 타이밍에 따라 조건이 달라진다.                
    
    
**@RepeatedTest**
* 반복 횟수와 반복 테스트 이름을 설정할 수 있다.
  * {displayName}
  * {currentRepetition}
  * {totalRepetitions}
* RepetitionInfo 타입의 인자를 받을 수 있다.

**@ParameterizedTest**
* 테스트에 여러 다른 매개변수를 대입해가며 반복 실행한다.
  * {displayName}
  * {index} 
  * {arguments}
  * {0}, {1}, ...

# @RepeatedTest
반복 횟수와 반복 테스트 이름을 설정할 수 있다.
{displayName}
{currentRepetition}
{totalRepetitions}
RepetitionInfo 타입의 인자를 받을 수 있다.

# @ParameterizedTest
테스트에 여러 다른 매개변수를 대입해가며 반복 실행한다.
{displayName}
{index}
{arguments}
{0}, {1}, ...
