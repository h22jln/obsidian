```
@Component
public class ItemValidator implements Validator {

    @Override    
    public boolean supports(Class<?> clazz) {
        return Item.class.isAssignableFrom(clazz);
    }

    @Override    
    public void validate(Object target, Errors errors) {

        Item item = (Item) target;
        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "itemName","required");

        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice()> 1000000) {
            errors.rejectValue("price", "range", new Object[]{1000, 1000000},null);  
            }

		//특정 필드 예외가 아닌 전체 예외  
		if (item.getPrice() != null && item.getQuantity() != null) {
		  ...
		}

	} 
}
```
검증 부분을 Validator로 따로 빼서 만들 수 있다

`supports()` : 해당 검증기를 지원하는 여부 확인
`validate(Object target, Errors errors)` : 검증 대상 객체(`target`)와 `BindingResult`

해당 코드를 검증하고자 하는 곳에서 호출하면 된다
```
itemValidator.validate(item, bindingResult);
```
에러가 있다면 bindingResult에 담긴다

## WebDataBinder
`Validator`를 직접 호출하지 않고도 검증하려면, `WebDataBinder`를 사용하면 된다

```
 @InitBinder public void init(WebDataBinder dataBinder) {
	log.info("init binder {}", dataBinder); 
	dataBinder.addValidators(itemValidator);
 }
```
`WebDataBinder`에 생성한 `Validator`를 추가하면 컨트롤러에서 자동으로 적용한다

컨트롤러의 메소드가 실행될 때 마다 자동으로 실행되며, `@InitBinder`를 사용해서 `Validator`를 호출하는 경우에는 해당 컨트롤러에만 영향을 준다

```
@PostMapping("/add")
public String addItemV6(@Validated @ModelAttribute Item item, BindingResult
bindingResult, RedirectAttributes redirectAttributes)
```
직접 호출하는 대신, 검증하고자 하는 객체 앞에 `@Validated`를 붙여주면 된다


#### 실행 과정
1. `@Validated`를 붙이면 앞서 `WebDataBinder`에 등록한 검증기를 찾아서 실행한다
2. 이 때 여러 검증기가 등록되어 있다면, `support()`를 통해 실행 가능한지의 여부를 확인한다
3. 실행 가능하다면 `validate()`를 호출한다



## 모든 컨트롤러에 일괄 적용
```
@SpringBootApplication
public class ItemServiceApplication implements WebMvcConfigurer {
	public static void main(String[] args) {
		SpringApplication.run(ItemServiceApplication.class, args);
	}

	@Override     
	public Validator getValidator() {
		return new ItemValidator();
	}
}
```
메인 클래스에 `WebMvcConfigurer`를 `implements` 받고, `getValidator()`를 `Override` 하면 모든 컨트롤러에 대해 `@InitBinder`와 같이 동작한다
