# Dependency Injection(의존성 주입) 이란?

객체를 생성하여 사용하는 것이 아닌, 주입받아서 사용하는 것이다. 의존성 == 사용이라고 봐도 될 듯하다.

DI는 이번에 스프링을 접하면서 듣게 된 주제지만, 넓게 보면 OOP 주제이기도 하다.

객체 내부에서 다른 객체를 생성하는 것은 ```강한 결합도```를 가지는 구조이다. 

왜냐하면

1. 현재 A 클래스 내부에서 B 클래스의 객체를 생성하고 있는 상태 
2. 만약 B 클래스의 객체를 C 클래스의 객체로 바꾸고 싶다면?
3. A 클래스 내부를 수정해야 한다.

객체를 주입받아 사용하게 되면 결합도를 낮출 수 있고, 런타임에 의존관계가 결정되기 때문에 유연한 구조를 가지게 된다.

DI는 확장에는 열려있고, 수정에 대해서는 닫혀 있어야한다는 OCP(Open Closed Principle) 원칙과 관계가 있다.

테스트 코드를 작성할 때에도, 의존성이 없는 객체에 의존성을 주입해가면서 테스트하기가 수월하다.

결국 의존성 주입 또한 클래스 간에 종속, 의존을 줄여 유지보수하기 편하도록 만드는 것이 핵심이라고 생각했다.

# Dependency Injection의 종류

의존성 주입은 ```Constructor Injection```, ```Setter Injection```, ```Field Injection```이 있다.

결론부터 말하면 ```Constructor Injection```이 가장 좋은 방법이며, IntelliJ 또한 다른 종류의 Injection을 수행할 경우 Warning 메시지를 뱉는다.

## Setter Injection

```Setter Injection```은 런타임에 주입할 수 있도록 낮은 결합도를 가지게 구현되었다. 

하지만 문제는 ```Setter Injection```을 통해서 Service 의 구현체를 주입해주지 않아도 객체는 생성가능하다. 

객체가 생성가능하다는 것은 내부에 있는 메소드도 호출 가능하다는 것인데, 이 때 NullPointerException 이 발생할 수 있다.

객체가 주입이 되지 않아도 얼마든지 객체를 생성할 수 있다는 것이 문제다.

## Constructor Injection

위 문제를 해결 할 수 있는 방법이 생성자 주입이다.

```Field Injection```은 ```Setter Injection```과 유사한 방식으로 이루어지기 때문에, ```Setter Injection```은 ```Field Injection``` 의 단점을 그대로 가진다.

```Setter Injection```은 스프링 컨테이너가 아닌 외부에서 수정자를 호출해서 주입할 수 있는 방법이라도 열려있지만, 

```Field Injection```은 스프링 컨테이너 말고는 외부에서 주입할 수 있는 방법이 없다.

또한 ```Field Injection```이나, ```Setter Injection```은 객체 생성시점에 순환참조가 일어나는지 아닌지 발견할 수 있는 방법이 없다.