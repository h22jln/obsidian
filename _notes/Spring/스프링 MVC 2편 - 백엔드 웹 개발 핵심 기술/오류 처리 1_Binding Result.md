```
public String addItemV1(@ModelAttribute Item item  
                        ,BindingResult bindingResult  
                        ,RedirectAttributes redirectAttributes  
                        ,Model model) {  
  
    //검증 로직  
    if(!StringUtils.hasText(item.getItemName())){  
        bindingResult.addError(new FieldError(("item"), "itemName", "상품 이름은 필수 입니다."));  
} 
  
    //특정 필드가 아닌 복합 룰 검증  
    if (item.getPrice() != null && item.getQuantity() != null) {  
        int resultPrice = item.getPrice() * item.getQuantity();  
        if (resultPrice < 10000) {  
            bindingResult.addError(new ObjectError("item", "가격 * 수량의 합은 10,000원 이상이어야 합니다. 현재 값 = " + resultPrice));  
        }  
    }  
  
    //검증에 실패하면 다시 입력 폼으로  
    if(bindingResult.hasErrors()){  
        log.info("errors = {}",bindingResult);  
        return "/validation/v2/addForm";  
    }  
  
    // 성공 로직  
    ...
}
```

==검증할 객체 뒤에== BindingResult를 매개변수로 적어준다

###  **필드 오류 - FieldError**
```
// 생성자
public FieldError(String objectName, String field, String defaultMessage) {}
```
필드에 오류가 있으면 `FieldError` 객체를 생성해서 `bindingResult`에 담는다

`objectName` : `@ModelAttribute`이름
`field` : 오류가 발생한 필드 이름
`defaultMessage` : 오류 기본 메시지

*타임리프에서의 사용*
```
<input type="text" id="itemName" th:field="*{itemName}" th:errorclass="field-error" class="form-control" placeholder="이름을 입력하세요">  
<div class="field-error" th:errors="*{itemName}">
	상품명 오류
</div>
```
`th:errors` : 해당 필드에 오류가 있는 경우 태그를 출력한다
`th:errorclass` : `th:field`에서 지정한 필드에 오류가 있다면 class에 해당 정보를 추가한다

#### 오류 발생시 사용자 입력 값을 유지하는 방법
`FieldError`는 두 가지 생성자를 제공한다

```
// 두 번째 생성자
public FieldError(String objectName, String field, @Nullable Object
rejectedValue, boolean bindingFailure, @Nullable String[] codes, @Nullable Object[] arguments, @Nullable String defaultMessage)
```
`objectName` : 오류가 발생한 객체 이름
`field` : 오류 필드
`rejectedValue` : 사용자가 입력한 값
`bindingFailure` : 타입 오류와 같이 바인딩 실패인지 여부
`codes` : 메시지 코드
`arguments` : 메시지에서 사용하는 인자
`defaultMessage` : 기본 오류 메시지

```
new FieldError("item", "price", item.getPrice(), false, null, null, "가격은 1,000 ~ 1,000,000 까지 허용합니다.")
```
rejectedValue에 사용자가 입력한 값을 넣으면 오류가 발생해도 입력 값이 유지된다

타임리프에서는
```
th:field="*{itemName}"
```
이 코드가 정상 상황에서는 모델 객체의 값을 사용하고, 오류 발생시 FieldError에서 값을 꺼내 출력한다


### **글로벌 오류 - ObjectError**
```
public ObjectError(String objectName, String defaultMessage) {}
```
필드를 넘어서는 오류가 있으면 `ObjectError` 객체를 생성해 `bindingResult`에 담는다

`objectName` : `@ModelAttribute`의 이름
`defaultMessage` : 오류 기본 메시지

*타임리프에서의 사용*
```
<div th:if="${#fields.hasGlobalErrors()}">
	<p class="field-error" th:each="err : ${#fields.globalErrors()}" th:text="${err}">글로벌 오류 메시지</p> 
</div>
```
`#fields`로 `BindingResult`가 제공하는 검증 오류에 접근할 수 있다

### 객체에 바인딩시 타입 오류
`@ModelAttribute`에 객체 바인딩시 타입 오류가 발생할 때, `BindingResult`가 있다면, 스프링이 오류 정보를 `BindingResult`에 담아서 컨트롤러를 호출한다

-> 위에서 직접 객체를 생성한 것과 같은 방식으로 스프링이 `FieldError`나 `ObjectError` 객체를 생성해서 `BindingResult`에 넣는다

