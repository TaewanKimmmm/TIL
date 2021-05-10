# produces 지정이 꼭 필요한 요청?

```java
@GetMapping(value = "/lines/{id}", produces = MediaType.APPLICATION_JSON_VALUE)
```

```java
@GetMapping(value = "/lines/{id}", produces = MediaType.TEXT_PLAIN)
```
이런 식으로

-http method도 같고
-요청 URI도 같고
-반환하는 객체도 같지만

json 타입으로 보낼 것인지, 텍스트 타입으로 보낼 것인지 갈릴 수 있을 때(클라이언트에서의 Accept 요청헤더가 다를 수 있을 때)에만 produces가 필요할 것..!

굳이 produces를 쓰지 않아도 mapping이 되는데 굳이 produces를 통해 매핑을 narrow할 필요가 없기 때문에 필요가 없다.