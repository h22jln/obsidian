```
@GetMapping("/")
public String homeLoginV3ArgumentResolver(@Login Member loginMember, Model
model) {...}
```
`@Login `어노테이션을 통해 `ArgumentResolver`가 자동으로 세션에 있는 회원을 찾아주고 없으면 null을 반환하도록 


#### @Login
```
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface Login {
}
```
- `@Target(ElementType.PARAMETER)` : 파라미터에만 사용
- `@Retention(RetentionPolicy.RUNTIME)`: 런타임까지 어노테이션 정보 유지

#### HandlerMethodArgumentResolver
```
@Slf4j

//(1)
public class LoginMemberArgumentResolver implements
HandlerMethodArgumentResolver {
	
	//(2)
    @Override    
    public boolean supportsParameter(MethodParameter parameter) {

		log.info("supportsParameter 실행");
		
		boolean hasLoginAnnotation = parameter.hasParameterAnnotation(Login.class);
		
		boolean hasMemberType = Member.class.isAssignableFrom(parameter.getParameterType());
		
		return hasLoginAnnotation && hasMemberType;
    }
    
	//(3)
	@Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest,
WebDataBinderFactory binderFactory) throws Exception {

	log.info("resolveArgument 실행");
	
	HttpServletRequest request = (HttpServletRequest) webRequest.getNativeRequest();
	
	HttpSession session = request.getSession(false);
	if (session == null) {
		return null;
	}
	return session.getAttribute(SessionConst.LOGIN_MEMBER);
}

}
```
(1) `HandlerMethodArgumentResolver`를 구현하여 `ArgumentResolver`를 만든다

(2) `supportsParameter` 메소드에서 true인 경우만 `resolveArgument` 메소드가 실행된다
위의 경우 `parameter`에 `@Login` 어노테이션이 붙어있고, `Member`클래스인 경우에만 실행된다

(3) 위의 결과를 만족한 경우, `session`에서 `loginMember`를 찾아서 반환한다

#### ArgumentResolver 등록
```
@Override
public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
	resolvers.add(new LoginMemberArgumentResolver());
 }
```