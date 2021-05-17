# @Transactional

스프링에서의 트랜잭션 처리를 지원해주는 어노테이션이다.

클래스나 메서드 위에 @Transactional을 추가하면 해당하는 클래스나 메서드에 트랜잭션 기능이 적용된 프록시 객체가 생성된다.

그 후 PlatformTransactionManager를 사용하여 트랜잭션을 시작하고, 정상 동작 여부에 따라 Commit 혹은 Rollback한다.

이 중 readOnly 속성을 사용해 봤는데, 이 속성은 다음과 같은 효과가 있다.

- 트랜잭션을 읽기 전용으로 설정할 수 있다.

- 성능을 최적화하기 위해 사용할 수도 있고 특정 트랜잭션 작업 안에서 쓰기 작업이 일어나는 것을 의도적으로 방지하기 위해 사용할 수도 있다.

조회 (SELECT) 쿼리를 사용하는 메서드의 경우는 읽기 전용으로 지정하고, 테이블에 변경을 일으킬 수 있는 메서드는 따로 속성을 주지 않고자 하였다.

readOnly 속성의 default 값이 false이기 때문이다.

따라서 클래스 레벨에서 @Transactional(readOnly = "true")를, 변경을 일으키는 메서드 레벨에서는 @Transactional을 사용하였다.

# Reference
https://goddaehee.tistory.com/167