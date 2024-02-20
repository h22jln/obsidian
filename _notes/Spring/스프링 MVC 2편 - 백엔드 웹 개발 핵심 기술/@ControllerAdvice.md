`@ControllerAdvice` / `@RestControllerAdvice`를 사용하면 정상코드와 에러코드를 분리할 수 있다

\*`@RestControllerAdvice `= `@ControllerAdvice` + `@ResponseBody`

```
@Slf4j
@RestControllerAdvice public class ExControllerAdvice {
	@ResponseStatus(HttpStatus.BAD_REQUEST)
	@ExceptionHandler(IllegalArgumentException.class)
	public ErrorResult illegalExHandle(IllegalArgumentException e) {
		log.error("[exceptionHandle] ex", e);
		return new ErrorResult("BAD", e.getMessage());
	}
}
```
별도의 클래스로 에러 코드를 분리하고 `@ControllerAdvice` / `@RestControllerAdvice`를 붙여주면 된다

#### 특정 어노테이션이 있는 컨트롤러에만 적용
```
@ControllerAdvice(annotations = RestController.class)
public class ExampleAdvice1 {}
```
`@RestController`에만 적용

#### 특정 패키지 하위에만 적용
```
@ControllerAdvice("org.example.controllers")
public class ExampleAdvice2 {}
```
해당 패키지 하위에만 적용됨

#### 특정 클래스에만 적용
```
@ControllerAdvice(assignableTypes = {ControllerInterface.class,
AbstractController.class})
public class ExampleAdvice3 {}
```
해당 클래스에만 적용됨

==생략시 모든 컨트롤러에 적용됨==
