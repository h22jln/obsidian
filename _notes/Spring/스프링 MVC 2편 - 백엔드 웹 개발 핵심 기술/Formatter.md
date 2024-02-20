
`Formatter`는 문자에 특화된 [[TypeConverter||Converter]]의 일종이라고 볼 수 있다

```
@Slf4j
public class MyNumberFormatter implements Formatter<Number> {
	@Override     
	public Number parse(String text, Locale locale) throws ParseException {
		log.info("text={}, locale={}", text, locale);
		NumberFormat format = NumberFormat.getInstance(locale);
		return format.parse(text);
	}

	@Override     
	public String print(Number object, Locale locale) {
		log.info("object={}, locale={}", object, locale);
		return NumberFormat.getInstance(locale).format(object);
	}
}
```

*\*`1,000`과 같이 숫자 중간에 쉼표를 적용하려면 자바가 기본으로 제공하는 `NumberFormat`객체를 사용하면 된다*

## Fomatter를 지원하는 ConversionService
`FormattingConversionService`는 `Fomatter`를 지원하는 `ConversionService`이다

```
 @Test     
 
void formattingConversionService() {
	DefaultFormattingConversionService conversionService = new DefaultFormattingConversionService();
	
	//컨버터 등록  
	conversionService.addConverter(new StringToIpPortConverter()); conversionService.addConverter(new IpPortToStringConverter()); 
	//포맷터 등록  
	conversionService.addFormatter(new MyNumberFormatter());
	
	//컨버터 사용
	IpPort ipPort = conversionService.convert("127.0.0.1:8080", IpPort.class);
	assertThat(ipPort).isEqualTo(new IpPort("127.0.0.1", 8080)); 
	//포맷터 사용  
	assertThat(conversionService.convert(1000, String.class)).isEqualTo("1,000");
	assertThat(conversionService.convert("1,000", Long.class)).isEqualTo(1000L);
}
```

`FormattingConversionService`는 `ConversionService` 기능을 상속받기 때문에 `Converter`와 `Formatter` 모두 등록 가능하다
스프링 부트는 내부에서 `DefaultFormattingConversionService`를 상속받은 `WebConversionService`를 사용한다

#### 스프링 등록
```
@Configuration
public class WebConfig implements WebMvcConfigurer {

@Override     
public void addFormatters(FormatterRegistry registry) {
	registry.addConverter(new StringToIpPortConverter()); 
	registry.addConverter(new IpPortToStringConverter());
	
	//Formatter 추가
	registry.addFormatter(new MyNumberFormatter());
}
```


## 스프링이 제공하는 기본 Formatter
- `@NumberFormat` : 숫자 관련 형식 지정 `Formatter` 사용
- `@DateTimeFormat` : 날짜 관련 형식 지정 `Formatter `사용

```
@Data
static class Form {
	@NumberFormat(pattern = "###,###")
	private Integer number;
	
	@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	private LocalDateTime localDateTime;
}
```


==메시지 컨버터(`HttpMessageConverter`)에는 `ConversionService`가 적용되지 않는다==

