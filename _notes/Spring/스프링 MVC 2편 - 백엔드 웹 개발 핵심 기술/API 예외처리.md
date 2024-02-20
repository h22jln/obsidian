요청이 JSON인 경우 응답값도 HTML이 아닌 JSON으로 하려면
```
@RequestMapping(value = "/error-page/500", produces = MediaType.APPLICATION_JSON_VALUE)
public ResponseEntity<Map<String, Object>> errorPage500Api(HttpServletRequest
request, HttpServletResponse response) {
	log.info("API errorPage 500");
	
	Map<String, Object> result = new HashMap<>();
	Exception ex = (Exception) request.getAttribute(ERROR_EXCEPTION);
	result.put("status", request.getAttribute(ERROR_STATUS_CODE));
	result.put("message", ex.getMessage());
	
	Integer statusCode = (Integer)
	request.getAttribute(RequestDispatcher.ERROR_STATUS_CODE);
	
	return new ResponseEntity(result, HttpStatus.valueOf(statusCode));
}
```
같은 요청 경로여도 `produces`에 타입을 지정하면 `accept`가 `application/json`으로 들어오는 요청은 여기서 처리하게 된다 ([[미디어 타입 조건 매핑]]참고)

#### 스프링부트 기본 오류처리
스프링 부트에서 이런 기본 오류처리도 지원한다
`Accept`가 `text/html`이면 `BasicErrorController`의 `errorHtml()`를 호출해 `view`를 제공하고, 그 외의 경우 `error()`를 호출해 `JSON`을 리턴한다

#### HandlerExceptionResolver
예외가 서블릿을 넘어 WAS까지 전달되면 HTTP 상태코드가 500으로 처리된다
이를 해결하고 동작 방식을 변경하기 위해 `HandlerExceptionResolver`(`ExceptionResolver`)를 사용한다
사용하게 되면 컨트롤러에서 `DispatcherServlet`으로 예외가 전달된 후 `ExceptionResolver`에서 예외 해결을 시도하고, WAS에는 정상 응답이 가게 된다
*\*예외가 해결되어도 `postHandle()`은 호출되지 않음*

```
@Slf4j
public class MyHandlerExceptionResolver implements HandlerExceptionResolver {
	@Override
	public ModelAndView resolveException(HttpServletRequest request,
	HttpServletResponse response, Object handler, Exception ex) {
		try {
			if (ex instanceof IllegalArgumentException) {
				log.info("IllegalArgumentException resolver to 400");
				response.sendError(HttpServletResponse.SC_BAD_REQUEST, ex.getMessage());
				return new ModelAndView();
			}
		} catch (IOException e) {
			log.error("resolver ex", e);
		}
	return null;
	}
}
```
`IllegalArgumentException`을 잡아서 해결하고 WAS에 `sendError`로 알린다

**반환 값에 따른 동작 방식**
- 빈 `ModelAndView` : 뷰를 렌더링 하지 않고 정상 흐름으로 서블릿이 리턴된다
- `ModelAndView` 지정 : 뷰를 렌더링 한다
- null : 다음 `ExceptionResolver`를 찾아서 실행한다. 다음 `ExceptionResolver`가 없으면 처리가 안되고 예외를 밖으로 던진다


#### ExceptionResolver 등록
```
@Override
public void extendHandlerExceptionResolvers(List<HandlerExceptionResolver>
resolvers) {
	resolvers.add(new MyHandlerExceptionResolver());
}
```



