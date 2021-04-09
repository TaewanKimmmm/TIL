# JDBC

Java DataBase Connectivity의 약자로, DB 관련 작업시에 사용하는 API

Mysql, MsSql, Oracle 등 DBMS(Database Management System)의 종류에 상관없이 JDBC API를 사용하여 DB 작업을 할 수 있다.

JDBC API는 JDBC Driver를 거쳐서 데이터베이스와 통신한다. 따라서 맨 처음에 load해줌

1. JDBC Driver 로드
2. Connection 객체 생성
3. Statement 객체 생성
4. Query 실행
5. ResultSet으로부터 Query 실행 결과 데이터 추출
6. Result 객체 Close
7. Statement 객체 Close
8. Connection 객체 Close

---

# JDBC Driver

자바 프로그램의 요청을 DBMS가 이해할 수 있는 프로토콜로 변환해준다.

Driver를 로딩한다는 것은 JVM이 어떤 DBMS를 사용하는지 인식시키는 작업이다.

DBMS 드라이버는 Class라는 클래스에 정의되어 있는 forName이라는 메소드를 이용해서 DBMS 드라이버의 핵심 클래스를 동적으로 메모리에 로딩한다.

---

# Connection

DB 연결 객체이다.

Connection객체의 역할은 DB와 애플리케이션 사이의 연결을 유지시켜주는 것이다.

DB와의 연결은 Connection 객체의 close() 메소드가 호출될때 까지 지속된다.

**Connection pool에서 얻어온 Connection객체는 connection.close()로 처리하는 것이 pool로의 반환을 의미하는 것이지 실제로 connetion을 close하는 것이 아니다**.

---

# Connection Pool

미리 커넥션 객체들을 생성하고, 관리하는것을 의미한다.

사용자의 요청에 따라 Connection 을 생성하다 보면 많은 수의 연결이 발생했을 때 서버에 과부하가 걸리게 된다 . 

이러한 상황을 방지하기 위해 미리 일정수의 Connection 을 만들어 pool 에 담아 뒀다가 사용자의 요청이 발생하면 연결을 해주고 연결 종료 시 pool 에 다시 반환하여 보관한다.

- JDBC를 통하여 DB에 연결하기 위해서는 드라이버(Driver)를 로드하고 커넥션(connection) 객체를 받아와야 한다.

- JDBC를 사용하면 사용자가 요청을 할 때마다 매번 드라이버를 로드하고 커넥션 객체를 생성하여 연결하고 종료하기 때문에 매우 비효율적이다.

위와 같은 문제를 해결하기 위해 Connection Pool을 사용한다.

---

# Statement, PreparedStatement

데이터베이스에 쿼리를 보내기 위해 필요한 객체 (SQL문을 실행하는 객체)

Connection 객체의 연결 정보를 이용하여 DB에 접근하므로 Connection 객체가 존재하고 있어야 한다.

---

# ResultSet

SELECT문의 결과를 가지는 객체

---

# Close

자바에서는 Close가 필요한 객체들이 여러 개 있고, 

Connection, Statement, ResultSet 객체도 그에 해당한다.

1. Statement를 닫지 않을 경우, 생성된 Statement의 개수가 증가하여 더 이상 Statement를 생성할 수 없게 된다.
2. 불필요한 자원(네트워크 및 메모리)을 낭비하게 된다.
3. Connection Pool을 사용하지 않는 상황에서 Connection을 닫지 않으면 새로운 Connection을 생성할 수 없게 된다.

---

JDBC 스펙에서는 Statement가 닫힐 때 ResultSet이 닫히고, 

Connection이 닫히면 Statement도 닫힌다고 되어있다.

Connection만 닫으면 Clean up이 되지 않을까?라고 생각했지만 그렇지 않다고 한다.

```
For example, if for some reason you are using a "primitive" type of database pooling and you call connection.close(), the connection will be returned to the pool and the ResultSet/Statement will never be closed and then you will run into many different new problems!

So you can't always count on connection.close() to clean up.
```

위와 같은 이유로 모든 걸 명시적으로 close하는 것을 권장한다.

---

# try-with-resources

Java 7 에서 `AutoCloseable` 인터페이스와 `try-with-resources` 가 등장했다.

탄생 배경: 

- close를 해줘야 하는 자원을 생성하고, 해당 자원을 이용한 작업을 진행 중이라고 가정

  ![img](https://blog.kakaocdn.net/dn/dAxaIV/btqSpGoDVuE/z8aEXKmkbRyqShcuJSI4vk/img.png)

- close()가 호출되지 않는다.

- try-finally를 이용하여 close() 호출 가능하도록 하면 아래와 같음

  ![img](https://blog.kakaocdn.net/dn/btTbJ3/btqR9l0JTB1/Gzrhs4t9ktbJ2Y1UiwFvAK/img.png)

- 자원을 여러 개 사용한다면..? 매우 장황해지고 디버깅도 어려워짐

  ![img](https://blog.kakaocdn.net/dn/cAWdEz/btqSATU0ht7/as2puZR7kdlCnMEOtMbvd0/img.png)

위와 같은 이유로 ```try-with-resource```가 등장했다.

## JAVA 7 의 try-with-resources

`try-with-resources` 를 사용하려면, try 블록의 소괄호 `()` 안에서 close() 메서드 호출이 필요한 (`AutoCloseable` 를 구현한) 객체를 할당한다.

try catch 절이 종료되면 객체의 close() 메서드가 자동으로 호출된다.

```java
public class Class1 {
    public method1() throws Exception {
      try (Connection conn = DriverManager.getConnection("...");
            Statement stat = conn.createStatement();
            ResultSet rs = stat.executeQuery("SELECT 1 from dual")) {
          
          // ...
      } catch (Exception e) {
          // ...
      } 
    }
}
```

## JAVA 9 의 try-with-resources

Java 7 에서 `try-with-resources` 를 사용할 때 close() 메서드 자동 호출을 위해 꼭 try 블록의 소괄호 안에서 자원 할당을 해야만 했다.

하지만 Java 9 부터는 `try-with-resources` 를 좀 더 유연하게 사용할 수 있게 되어서 try 블록의 밖에서 선언된 객체를 참조할 수 있다.

```java
public class Class1 {
    public method1() throws Exception {
      Connection conn = DriverManager.getConnection("...");
      Statement stat = conn.createStatement();
      ResultSet rs = stat.executeQuery("SELECT 1 from dual");

      try (conn; stat; rs) {
          // ...
      } catch (Exception e) {
          // ...
      } 
    }
}
```



# Reference

https://100100e.tistory.com/415

https://blog.benelog.net/1898928.html

https://stackoverflow.com/questions/4507440/must-jdbc-resultsets-and-statements-be-closed-separately-although-the-connection

https://m.blog.naver.com/PostView.nhn?blogId=jkssleeky&logNo=220493121966&proxyReferer=https:%2F%2Fwww.google.com%2F

https://hyoj.github.io/blog/java/basic/jdbc/jdbc-try-catch-tip.html#connection-preparedstatement-resultset-%EB%8B%AB%EB%8A%94-%EA%B0%80%EC%9E%A5-%EC%9D%B4%EC%83%81%EC%A0%81%EC%9D%B8-%EB%B0%A9%EC%8B%9D





