`@RequestParam`, `@ModelAttribute`, `@PathVariable` 을 사용하여 값을 받으면 알아서 스프링에서 타입을 변환한다

#### 새로운 타입 변환
`org.springframework.core.convert.converter`의 `Converter`를 `implement`받아서 컨버터를 구현할 수 있다

**`Converter<대상 타입,변환하고자 하는 타입>`

```
 @Slf4j
 public class StringToIntegerConverter implements Converter<String, Integer> {
     @Override     
     public Integer convert(String source) {
         log.info("convert source={}", source);
         return Integer.valueOf(source);
     }
}
```
상세 동작을 정의해서 사용 가능하다

```
@Slf4j
 public class IpPortToStringConverter implements Converter<IpPort, String> {
     @Override     
     public String convert(IpPort source) {
         log.info("convert source={}", source);
         return source.getIp() + ":" + source.getPort();
     }
}
```
기본 타입뿐 아니라 직접 정의한 객체도 사용 가능하다


## ConversionService
타입 컨버터를 묶어서 편리하게 사용할 수 있는 기능

컨버터 서비스 인터페이스는 컨버팅 가능 여부 확인 기능과 실제로 컨버팅 하는 기능을 제공한다
-> **인터페이스 분리 원칙(ISP)**

```
// 컨버전 서비스에 컨버터 등록
DefaultConversionService conversionService = new
DefaultConversionService();

conversionService.addConverter(new StringToIntegerConverter());

// 실제로 사용
assertThat(conversionService.convert("10",Integer.class)).isEqualTo(10);
```
컨버터를 등록할 때에는 타입 컨버터를 알아야 하지만, 사용시에는 `ConversionService`를 통해 제공되므로 컨버터를 몰라도 된다
타입 변환을 원하는 사용자는 `ConversionService` 인터페이스에 의존하면 된다


## 스프링에서의 Converter
```
@Configuration
public class WebConfig implements WebMvcConfigurer {
	@Override     
	public void addFormatters(FormatterRegistry registry) {
		registry.addConverter(new StringToIntegerConverter());
	} 
}
```
스프링 내부에서 `ConversionService`를 제공하므로 `WebMvcConfigurer`가 제공하는 `addFormatters()`를 사용해 컨버터를 등록하면 된다

등록한 컨버터는 `@RequestParam`, `@ModelAttribute`, `@PathVariable`을 통해 컨트롤러에서 값을 받는 경우 자동으로 사용된다

웬만한 기본 타입끼리 변환을 위한 컨버터는 스프링이 제공한다
컨버터를 추가하면 추가한 컨버터가 기본 컨버터보다 우선순위가 높아 추가한 컨버터가 실행된다


#### 뷰에서의 컨버터
```
@GetMapping("/converter-view")
public String converterView(Model model) {
	model.addAttribute("number", 10000);
	model.addAttribute("ipPort", new IpPort("127.0.0.1", 8080));
	return "converter-view";
}
```

```
<ul>
     <li>${number}: <span th:text="${number}" ></span></li>
     <li>${{number}}: <span th:text="${{number}}" ></span></li>
     <li>${ipPort}: <span th:text="${ipPort}" ></span></li>
     <li>${{ipPort}}: <span th:text="${{ipPort}}" ></span></li>
</ul>
```
뷰에서 `${{}}` (대괄호 두번) 사용해서 값을 가져오면 컨버팅 된 값이 나온다

#### 폼에서의 컨버터
```
<form th:object="${form}" th:method="post">  
	th:field <input type="text" th:field="*{ipPort}"><br/>  
	th:value <input type="text" th:value="*{ipPort}">(보여주기 용도)<br/> 
	<input type="submit"/>
</form>
```
타임리프의 `th:field`는 컨버전 서비스도 함께 적용되므로 컨버팅을 하고싶지 않다면 `th:value`를 사용해야 한다


