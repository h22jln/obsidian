#### @ResponseStatus
예외에 따라서 HTTP 상태 코드를 지정한다

```
@ResponseStatus(code = HttpStatus.BAD_REQUEST, reason = "잘못된 요청 오류") 
public class BadRequestException extends RuntimeException {
}
```
`@ResponseStatus`를 사용하면 HTTP 상태코드를 변경한다

```
@ResponseStatus(code = HttpStatus.BAD_REQUEST, reason = "error.bad") 
public class BadRequestException extends RuntimeException {  
}
```
메시지 값을 MessageSource에서 찾는 기능도 제공한다


#### ResponseStatusException
`@ResponseStatus`는 어노테이션을 직접 넣어야 하므로 개발자가 변경할 수 없는 예외에는 적용할 수 없다

이런 경우 `ResponseStatusException`을 사용하면 된다
```
@GetMapping("/api/response-status-ex2")
public String responseStatusEx2() {
	throw new ResponseStatusException(HttpStatus.NOT_FOUND, "error.bad", new
	IllegalArgumentException());
}
```

