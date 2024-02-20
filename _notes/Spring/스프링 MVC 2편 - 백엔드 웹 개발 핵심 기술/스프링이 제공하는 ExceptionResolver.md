스프링 부트가 기본으로 제공하는 ExceptionResolver는 다음과 같은 순서로 실행된다
1. [[@ExceptionHandler||ExceptionHandlerExceptionResolver]]
2. [[ResponseStatusExceptionResolver]]
3. `DefaultHandlerExceptionResolver`
	스프링 내부에서 발생하는 스프링 예외를 해결한다
	대부분의 `Exception`을 잡아 알맞은 상태코드로 변경해서 `sendError`를 보낸다

