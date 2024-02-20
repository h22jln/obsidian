`@ExceptionHandler `어노테이션을 선언하고 처리하고 싶은 예외를 지정하면 된다
해당 컨트롤러에서 예외가 발생하면 메소드가 호출된다

```
@ResponseStatus(HttpStatus.BAD_REQUEST)
@ExceptionHandler(IllegalArgumentException.class)
public ErrorResult illegalExHandle(IllegalArgumentException e) {
	log.error("[exceptionHandle] ex", e);
	return new ErrorResult("BAD", e.getMessage());
}
```
객체를 반환해도 `@RestController` 내에서는 JSON으로 리턴됨

```
@ExceptionHandler
public ResponseEntity<ErrorResult> userExHandle(UserException e) {
	log.error("[exceptionHandle] ex", e);
	ErrorResult errorResult = new ErrorResult("USER-EX", e.getMessage());
	return new ResponseEntity<>(errorResult, HttpStatus.BAD_REQUEST);
}
```
`@ExceptionHandler`의 `Exception` 값은 파라미터와 같다면 생략 가능

```
@ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
@ExceptionHandler
public ErrorResult exHandle(Exception e) {
	log.error("[exceptionHandle] ex", e);
	return new ErrorResult("EX", "내부 오류"); 
}
```

`@ExceptionHandler`는 지정한 `Exception`과 그 하위 자식 클래스를 모두 처리할 수 있다
부모클래스, 자식클래스를 처리하는 메소드가 있다면 자식클래스를 처리하는 메소드가 호출된다

```
@ExceptionHandler({AException.class, BException.class})
public String ex(Exception e) {
	log.info("exception e", e);
}
```
여러 예외 클래스를 한번에 처리할 수도 있다

``` 
@ExceptionHandler(ViewException.class)
public ModelAndView ex(ViewException e) {
	log.info("exception e", e);
	return new ModelAndView("error");
}
```
`ModelAndView`를 반환하면 View로 응답할수도 있다