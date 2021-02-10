# JUnit 5: 테스트 반복하기 1부

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
