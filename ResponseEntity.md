# ResponseEntity

@RestController는 View가 아닌 데이터를 리턴한다.

비정상적인 데이터를 리턴할 경우 문제가 생길 수 있으므로, 데이터와 함께 HTTP 상태 코드를 전송할 수 있도록 스프링에서 제공하는 타입이 바로 ResponseEntity이다.

