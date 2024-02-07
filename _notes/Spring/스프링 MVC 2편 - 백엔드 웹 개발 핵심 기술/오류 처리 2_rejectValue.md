[[오류 처리 1_Binding Result]]에서의 방법은 번거롭다

컨트롤러에서 `BindingResult`는 검증해야 할 객체인 `target` 바로 다음에 오기 때문에 검증해야 할 객체인 `target`을 알고 있다

```
bindingResult.rejectValue("itemName", "required");
```
이 코드는 
```
bindingResult.addError(new FieldError(("item"), "itemName", "상품 이름은 필수 입니다.")); 
```
[[오류 처리 1_Binding Result]]에서 만든 객체와 같은 역할을 한다


## rejectValue()
```
void rejectValue(@Nullable String field, String errorCode, @Nullable Object[] errorArgs, @Nullable String defaultMessage);
```
`field` : 오류 필드명
`errorCode` : 오류 코드(메시지에 등록된 코드가 아니다)
`errorArgs` : 매개변수
`defaultMessage` : 오류 메시지를 찾을 수 없을 때 사용하는 기본 메시지


## rejectValue() 의 errorCode
```
bindingResult.rejectValue("itemName", "required");
```

`errors.properties`에 `required.item.itemName`이라는 메시지가 있을 때, `"required"`만 입력해도 동작한다

이는 메시지에 required.* 로 된 키의 값을 모두 가져와서 사용한다
세밀한 메시지 코드일수록 우선순위가 높다
```
#Level1
required.item.itemName: 상품 이름은 필수 입니다. 

#Level2
required: 필수 값 입니다.
```


## MessageCodesResolver
```
//(1)
MessageCodesResolver codesResolver = new DefaultMessageCodesResolver();

//(2)
@Test
void messageCodesResolverObject() {
	String[] messageCodes = codesResolver.resolveMessageCodes("required","item");
	assertThat(messageCodes).containsExactly("required.item", "required");
}

//(3)
@Test
void messageCodesResolverField() {
	String[] messageCodes = codesResolver.resolveMessageCodes("required","item", "itemName", String.class);
	assertThat(messageCodes).containsExactly(
		"required.item.itemName",
		"required.itemName",
		"required.java.lang.String",
		"required"
	); 
}
```

(1) `MessageCodesResolver`의 구현체로 `DefaultMessageCodesResolver`를 선택했다

(2) `ObjectError`에서 처리하는 에러 메시지는 `"required.item"`, `"required"` 임을 확인할 수 있다

(3) FieldError에서 처리하는 에러 메시지는 `"required.item.itemName"`,`"required.itemName"`,`"required.java.lang.String"`,`"required"` 임을 확인할 수 있다


#### DefaultMessageCodesResolver의 기본 메시지 생성 규칙

- Object Error
```
1.: code + "." + object name 
2.: code

예) 오류 코드: required, object name: item

1.: required.item

2.: required
```

- Field Error
```
1.: code + "." + object name + "." + field 
2.: code + "." + field  
3.: code + "." + field type  
4.: code

예) 오류 코드: typeMismatch, object name "user", field "age", field type: int 
1. "typeMismatch.user.age"  
2. "typeMismatch.age"  
3. "typeMismatch.int"
4. "typeMismatch"
```


## ValidationUtils
```
if(!StringUtils.hasText(item.getItemName())) { 
	bindingResult.rejectValue("itemName", "required", "기본: 상품 이름은 필수입니다.");
}
```
이 코드를

```
ValidationUtils.rejectIfEmptyOrWhitespace(bindingResult, "itemName",
"required");
```
한줄로 정리 가능하다
==그러나 Empty, 공백 체크와 같은 단순한 기능만 제공한다==

## 스프링이 직접 만든 오류 처리
바인딩 에러과 같은 경우 스프링이 정의한 오류 메시지를 보여준다

```
log.info("errors ={}",bindingResult);
```
`bindingResult`를 확인해보면 `codes[typeMismatch.item.price,typeMismatch.price,typeMismatch.java.lang.Integer,ty peMismatch]` 위의 규칙과 동일한 레벨로 메시지 코드가 생성되어있다

`errors.properties`에 처리하고자 하는 코드의 에러 메시지를 넣어두면 원하는 메시지를 노출시킬 수 있다

# 오류 메시지의 처리 과정 정리
1. `rejectValue()` 호출
2. `MessageCodeResolver`를 사용해 메시지 코드들을 모두 생성
3. `new FieldError()` 로 메시지 코드들을 보관
4. `th:errors`에서 메시지 코드들로 메시지를 순서대로 찾고, 있으면 노출


