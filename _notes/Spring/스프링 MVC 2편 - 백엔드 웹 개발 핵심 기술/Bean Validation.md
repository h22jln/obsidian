검증 로직을 모든 프로젝트에 적용할 수 있게 공통화하고 표준화 한 것
Bean Validation을 구현한 기술 중 일반적으로 사용하는 구현체는 하이버네이트이다

*검증 어노테이션 모음 [[https://docs.jboss.org/hibernate/validator/6.2/reference/en-US/html_single/#validator-defineconstraints-spec]]*

`build.gradle`에 dependency추가
```
implementation 'org.springframework.boot:spring-boot-starter-validation'
```


객체의 필드 위에 붙여주면 된다
```
@NotBlank
private String itemName;

@NotNull
@Range(min = 1000, max = 1000000)
private Integer price;

@NotNull
@Max(9999)
private Integer quantity;
```
`@NotBlank` : 빈값 / 공백만 있는 경우를 허용하지 않음
`@NotNull` : null값을 허용하지 않음
`@Range` : 범위 안의 값만 허용
`@Max` : 최대 이 값까지 허용


```
@PostMapping("/add")
public String addItem(@Validated @ModelAttribute Item item, BindingResult
bindingResult, RedirectAttributes redirectAttributes) {
	...
}
```
검증할 객체 앞에 `@Validated`를 붙여주면 된다
(`@Valid`도 가능)


#### 실행 과정
1. 스프링 부트에 `spring-boot-starter-validation` 라이브러리를 넣으면 자동으로 `Bean Validator`를 인지하고 스프링에 통합한다
2. `LocalValidatorFactoryBean`을 글로벌 `Validator`로 등록한다
3. 이 `Validator`가 어노테이션을 보고 검증을 수행한다
4. 오류가 발생하면 `FieldError`/`ObjectError` 객체를 생성해서 `BindingResult`에 담는다

**이 때문에 직접 글로벌 `Validator`를 등록하면 `BeanValidator`를 글로벌 `Validator`로 등록하지 않는다**

#### Bean Validator의 검증 순서
1. `@ModelAttribute `각각의 필드에 타입 변환
2. 실패하면 `typeMismatch`로 `FieldError` 추가
3. 성공시 `Validator` 적용


***
## Bean Validation 에러 코드
`Bean Validation` 적용시 `bindingResult`에 등록된 검증 오류 코드는 어노테이션 이름으로 등록되고, 
`MessageCodeResolver`를 통해 메시지 코드가 순서대로 생성된다
[[오류 처리 2_rejectValue]] 메시지 생성 규칙과 비슷하다

```
@NotBlank
1. NotBlank.item.itemName
2. NotBlank.itemName 
3. NotBlank.java.lang.String 
4. NotBlank
```

이전과 같이 `errors.properties`에 메시지를 직접 등록할 수 있다

#### Bean Validation의 메시지 찾는 순서
1. 생성된 메시지 코드 순서대로 messageSource에서 메시지 찾기 
2. 어노테이션의 message 속성에 등록된 메시지  `@NotBlank(message = "공백! {0}")`
3. 라이브러리가 제공하는 기본값

***
## Bean Validation - groups
동일한 모델 객체를 다르게 검증하기 위한 방법 중 하나

```
public interface SaveCheck {...}

public interface UpdateCheck {...}
```
검증을 위해 인터페이스를 생성하고

```
@NotNull(groups = {SaveCheck.class, UpdateCheck.class}) 
@Max(value = 9999, groups = SaveCheck.class) //등록시에만 적용 
private Integer quantity;
```
객체의 필드에 groups를 사용해 검증하고자 하는 로직별로 분류한다

```
@PostMapping("/add")
public String addItemV2(@Validated(SaveCheck.class) @ModelAttribute Item item,
BindingResult bindingResult, RedirectAttributes redirectAttributes) {
	//...
}
```
`@Validated`에 검증하고자 하는 로직의 인터페이스명을 넣어주면 된다


그러나 이 방법은 번거로우므로, ==로직별로 객체를 분리하여 각각 검증 어노테이션을 달아주는 것이 좋다==
*(로직 객체로 받아와서 repository에 요청할 때에는 새로운 객체를 생성하는 식으로)*

***
## HTTP 메시지 컨버터
`@Validated`(`@Valid`)는 `HttpMessageConverter`(`@RequestBody`)에도 적용 가능하다

API 요청은 3가지 케이스를 생각할 수 있다

- 성공 요청 : 성공
- 실패 요청 : JSON 객체 생성 실패
	실패 요청시 컨트롤러 자체가 호출되지 않고, 그 전에 예외가 발생한다
	Validator도 실행되지 않는다
- 검증 오류 요청 : JSON 객체 생성은 성공했지만, 검증 실패


#### @ModelAttribute vs @RequestBody
`@ModelAttribute`는 필드 단위로 적용되므로 특정 필드에 타입이 맞지 않는 오류가 발생해도, 
나머지 필드는 정상 처리 가능. Validator도 사용 가능

`@RequestBody`는 `HttpMessageConverter`에서 JSON을 객체로 변경하지 못하면, 
이후 단계가 실행되지 않고 예외가 발생한다. 
컨트롤러도 호출되지 않고 Validator도 적용 불가

