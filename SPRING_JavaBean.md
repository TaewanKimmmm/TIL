# JavaBean

두 가지 관례를 따라 만들어진 오브젝트를 말한다. Bean이라고도 부른다.

- 디폴트 생성자: Javabean은 파라미터가 없는 디폴트 생성자를 갖고 있어야 한다. 툴이나 프레임워크에서 리플렉션을 이용해 오브젝트를 생성하기 때문에 필요하다.
- 프로퍼티: 자바빈이 노출하는 이름을 가진 속성을 프로퍼티라고 한다. 프로퍼티는 set으로 시작하는 수정자 메서드(setter)와 get으로 시작하는 접근자 메서드(getter)를 이용해 수정 또는 조회할 수 있다.



## 설계 규약

멤버변수마다 ```getter```와 ```setter```가 있어야 한다.

```getter```는 파라미터가 존재하지 않아야 한다.

```setter```는 반드시 하나 이상의 파라미터가 존재해야 한다.

생성자는 파라미터가 존재하지 않아야 한다.