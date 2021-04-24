# Checked Exception vs Unchecked Exception

먼저 Exception(예외), Error(오류)는 다르다는 것을 알아야 한다.

## Exception (예외)

입력 값에 대한 처리가 불가능하거나, 프로그램 실행 중에 참조된 값이 잘못된 경우 등 정상적인 프로그램의 흐름을 어긋나는 것을 말한다. 개발자가 예측하여 처리할 수 있다.

## Error (오류)

시스템에 무엇인가 비정상적인 상황이 발생한 경우를 말한다. JVM에서 발생하기 때문에 개발자가 처리할 수 없다.



# RuntimeException

요약하면, 개발자가 코드를 잘못 만들어서 생기는 예외이다.

컴파일 시 문제가 없고, 실행 중에 문제가 발생한다.

# Checked Exception

- ```RuntimeException```을 상속하지 않는다.
- 컴파일 시점에 확인한다. ==> 프로그램이 돌기 전에 컴파일러가 예외 처리를 했는지 체크하니까 ```Checked```라는 이름이 붙음
- Checked Exception이 발생할 가능성이 있다면 throws를 이용해 상위 클래스로 처리를 위임하거나, try-catch를 이용하여 반드시 명시적으로 예외처리해야 한다.
- 예외 발생 시 트랜잭션 roll back 하지 않는다.
- 대표 Exception :IOException, SQLException

# Unchecked Exception

- ```RuntimeException```을 상속한다.
- 런타임 시점에 확인한다. ==> 컴파일러가 체크하지 않았으므로 ```Unchecked```라는 이름이 붙음

- 명시적으로 예외처리하지 않아도 된다.
- 예외 발생 시 트랜잭션 roll back 한다.
- 대표 Exception : IllegalArgumentException, NullPointerException

# Spring의 예외 처리

스프링의 JdbcTemplate은 JdbcTemplate 템플릿과 콜백 안에서 발생하는 Checked Exception에 속하는 SQLException을 RuntimeException인 DataAccessException으로 포장해서 던져준다.

# Reference

https://stackoverflow.com/questions/13104251/why-are-exceptions-named-checked-and-unchecked

https://gunju-ko.github.io/toby-spring/2018/11/07/%EC%98%88%EC%99%B8-%EC%B2%98%EB%A6%AC.html