# TestContainer 소개

테스트에서 도커 컨테이너를 실행할 수 있는 라이브러리   
* 테스트 실행시 DB를 설정하거나 별도의 프로그램 또는 스크립트를 실행할 필요 없다.      
* 보다 Production에 가까운 테스트를 만들 수 있다.     
* 다만, 매번 도커 이미지를 빌드하고 컨테이너화하기 때문에 속도가 매우 느려진다는 단점이 있다.    
      
A라는 DB를 사용하고 있지만, 테스트에서는 B라는 Inmemory DB를 사용하고 있다고 가정한다.         
인메모리 DB가 가볍고 빠르지만, 스프링의 Transactional은 Isolation 과 Propagation을 DB의 설정값으로 하기에         
실제 테스트 서버에서의 결과가 실제 서버에서의 결과과 일치하지 않고 문제가 발생할 가능성이 있다.      
    
그렇기에, 이 같은 문제를 해결하기 위해 도커로 동일한 DB를 올려서 작업할 수 있다.       
물론, DB에 다른 스키마를 사용할 수 있지만 별로 추천하지는 않는 방법이다.      
  
`테스트 컨테이너즈`를 사용하면 이같은 컨테이너를 만들고 해제하는 역할을 해주는 라이브러리다.       
테스트 코드에 도커를 띄우고 삭제하고 스크립트까지도 안 만들 수 있게끔 해준다.     


