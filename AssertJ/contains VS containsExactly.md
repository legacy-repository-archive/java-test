# contains()
```java
@Test
void containsTest() {
	List<Integer> integers = Arrays.asList(1, 2, 3);
	assertThat(integers).contains(1, 2, 3);	//테스트 통과
	assertThat(integers).contains(2, 1, 3);	//테스트 통과
}
```
contains는 순서와 상관 없이 실제 그룹이 주어진 값들을 포함하고 있는지를 테스트한다.    
그래서 위 두 줄의 테스트는 모두 통과한다.   
   
# containsExactly()   
```java
@Test
void containsExactlyTest() {
	List<Integer> integers = Arrays.asList(1, 2, 3);
	assertThat(integers).containsExactly(1, 2, 3);	//테스트 통과
	assertThat(integers).containsExactly(2, 1, 3);	//테스트 통과 X
	assertThat(integers).containsExactly(1, 2);	//테스트 통과 X
}
```
contains는 순서까지 고려해서 실제 그룹이 주어진 값들을 포함하고 있는지를 테스트한다    
그래서 첫번째 줄의 테스트는 통과하지만, 두번째 줄의 테스트는 통과하지 못한다.    
   
이때 주의할 점은 원소 하나라도 빠지면 테스트를 통과하지 못한다. 정말로 정확하게 일치하는 list여야 하는 것!       
