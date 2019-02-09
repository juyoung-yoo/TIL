
### TDD ( Test Driven Development )
- 테스트 코드를 주도적으로 작성하고, 후에 실제 코드를 작성하는 설계방법
- 실제코드보다 당연히 먼저 작성함으로 첫 테스트 코드는 실패할 것

######  TDD 3가지 법칙
1. 테스크 코드를 작성할 때까지 실제 코드 작성하지 않는다.
2. 컴파일은 실패하지 않으면서 실패하는 정도로만 테스트 코드 작성
3. 현재 작성된 테스트 코드를 성공시킬 정도의 코드만 작성

##### 테스트 코드
- 실제 코드만큼이나 리팩토링 대상이 되어야한다.
- 간단하게 작성한다.
- 해당 도메인에 대한 테스크 코드 API를 만들어 가독성을 살리자.
- 가독성은 중요하나 효율성은 저하시켜도 됨. (최적화 < 가독성을 중시하자)
- 관레 : Given - When - Then 패턴을 지키자.
- Assert는 1개가 일반적이지만, 2개이상이여도된다.
FIRST를 지키자.
  - Fast : 빠르게 돌아햐 한다.
  - Independent : 각 테스트케이스는 모두 독립적으로  실행되어야 한다.
  - Repeatable : 어떤 환경에서도 반복해서 실행할 수 있어야한다.
  - Self Validating : 스스로 검증할 수 있어야한다. log 이용하는것은 비추. 스스로 검증하여 boolean 타입으로 리턴한다.
  - Timely : 테스트 케이스는 적시에 작성되어야한다. 실제 코드를 작성하기 전에 작성.

###### 효과적인 이유
 TDD는 더 고품질의 프로그래밍을 실현한다. TDD를 사용하면 테스트를 충족하기에 충분한 코드만 작성하게 된다. 그 결과 테스트를 거쳤을 뿐만 아니라 확장성과 유지보수 편의성도 높은, 더 모듈성이 강하고 군더더기 없는 코드가 생산된다. TDD는 또한 총소유비용(TCO) 절감으로도 이어진다. 결함의 수가 더 적다. 결함 수정 비용이 덜 드는 사이클의 조기에 결함을 포착할 수 있다.
###### 효과를 거두지 못하는 경우
 TDD는 숙련되기까지 4~6개월이 소요된다. 이 램프업 기간 동안 TDD는 팀의 속도를 떨어트린다. 또한 선행 투자도 더 많이 필요하다. 그러나 장기적인 비용이 절감되므로 이 비용은 결국 상쇄된다.

```
유닛 테스트를 한다면, 테스트할  주제, 대상 클래스를 정하고 대상 클래스가 사용하는 의존성 패키지,함수들은 테스트 더블(Mock, Stub)으로 만든다.
처음에는 작성한 모듈이나 클래스 비율이 높게 테스트를 작성하는데 기본적인 부분만 작성하고 이후 리팩토링을 통해 단계적으로 추가한다.
위 원칙적으로는 테스트를 먼저 작성하고, 충족 시키기 위해 코드를 작성하고 리팩토링하는 것이 바람직하나 실제로는 쉽지 않다. ( 초심자는 작성이 어렵고, 실무에서 또한 TDD 원칙을 따르기 어렵다.) 코드를 어느정도 작성 후 테스트,Mock을 붙이는 것을 권하고 있다.
맨 처음 스펙 도출 이후 테스트 작성, 단언, Mocking등 순차적으로 내려가는 것이 좋지만, 일반적인 테스팅 FrameWork를 적용해서 작은 단위로 테스트를 단순하고 쉽게 접근할 수 있기 때문에 개별 모듈별로 먼저 단위 테스트를 시도해 보는 것을 권장한다.

1. 작성 시 우선 순위는 외부싯템과 의존성이 가장 낮은 메소드부터 작성하고 확장한다.
2.   예상된 예외사항은 try-catch구문이 아닌, 어노테이션을 사용하자 ex) @Test(expected=SomDomainSpecificException.SubException.class)
3. 테스트 결과는 XML형태로 출력하며, 추가적인 포맷팅 작업으로 테스트 결과를 보기좋게 변경 가능하다.
```