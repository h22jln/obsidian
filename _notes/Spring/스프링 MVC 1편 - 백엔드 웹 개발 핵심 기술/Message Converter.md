스프링 MVC는
HTTP 요청이 `@RequestBody / HttpEntity(RequestEntity)` 이거나,
HTTP 응답이 `@ResponseBody / HttpEntity(ResponseEntity) `이면 
HTTP Message Converter를 적용한다

스프링 부트는 다양한 컨버터를 제공하는데, ==대상 클래스 타입과 미디어 타입 둘 다 만족하는== 컨버터를 찾는다
만족하지 않으면, 다음 메시지 컨버터로 우선순위가 넘어간다
(끝까지 찾지 못하면 실패)

***

이 컨버터는 Argument Resolver가 호출하게 되는데,
Argument Resolver는 핸들러 어댑터가 핸들러를 호출할 때 필요한 파라미터를 찾는 역할을 한다

따라서 ==핸들러 어댑터가 Argument Resolver를 통해 필요한 파라미터를 찾을 때 사용하는 도구가 메시지 컨버터이다==

*응답값을 생성할 때 컨버터를 호출하는 것은 ReturnValueHandler*

