# Mockito가 야기한 혼란

다른 Layer에 영향을 받지 않고 순수 Service 코드를 검증하는 슬라이스 테스트를 하기 위해 Mockito를 사용했었다.
내가 테스트하고자 하는 부분을 제외하고는 다른 부분을 내가 기대한 동작을 하도록 만들 수 있기 때문이었다.

이 때, Repository를 Mock해서 행위를 정해주고 테스트하면 결과 값을 다 알고 테스트하는, 작위적인 느낌이 강하게 들었다.

Mockito 를 사용함으로써 이미 내부 구현이 어떻게 되고 있는지를 알고 있게 되며, 테스트의 성공을 의도하는 방향으로 흐를 수 있기 때문에 완벽한 테스트라고 볼 수 없다. 또한 프로덕션 내부 구현이 변경되었을 때
테스트가 여전히 통과할 수 있어 혼란을 야기할 수 있다.

결론적으로 리펙토링이나 구현의 변경으로부터 자유로울 수 없다.

최대한 Mockito를 사용하지 않도록 노력하고, 사용하게 된다면 엄격하게 사용하자.

# 엄격하게 사용한다는 것은?

- ```Random, Shuﬄe, LocalDate.now() • 외부 세계 • HTTP • 외부 저장소``` 등 개발자가 제어할 수 없는 영역에서 사용하는 것이다.

- 로컬 구성이 가능한 저장소는 Mock 하지 않는다. 저장소를 Mock 처리한다는 것은 굉장히 많은 것을 포기하는 것이기 때문에 신뢰할 수 없는 테스트가 된다. h2, embedded redis 등 다양한 로컬 셋업 환경이 있고, 비용도 작기 때문에 이를 사용하지 않고 Mock을 사용할 이유가 없다.

- 어플리케이션의 규모가 대규모일 때, 상당히 깊은 depth 의 레이어를 갖는 설계에서 응용 계층의 테스트를 작성하기 위해 하위 계층들의 테스트 셋업이 지나치게 방대해질 수 있다. 
  그렇기 때문에 이러한 레이어링을 갖는 설계에서는 레이어의 경계 영역에서 Mockito 를 사용할 수 있다.

# Reference

https://github.com/woowacourse/jwp-chess/pull/314
https://github.com/woowacourse/jwp-refactoring/pull/2