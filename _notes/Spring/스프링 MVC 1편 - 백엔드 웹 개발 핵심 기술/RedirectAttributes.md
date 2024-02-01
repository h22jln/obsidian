#### PRG (Post/Redirect/Get)
웹에서 POST로 마지막으로 실행한 작업이 있는데 새로고침을 하면 작업이 한번 더 실행되게 된다

이 때 다른 화면으로 Redirect 시켜주는 것이 좋다
```
return "redirect:/basic/items/" + item.getId();
```


#### RedirectAttributes
URL에 +로 파라미터 값을 넘겨주면, 파라미터값이 이상한 경우에 에러가 날 수 있다

```
public String addItemV6(Item item, RedirectAttributes redirectAttributes) {

     Item savedItem = itemRepository.save(item);
     redirectAttributes.addAttribute("itemId", savedItem.getId());
     redirectAttributes.addAttribute("status", true);
     return "redirect:/basic/items/{itemId}";

}
```
RedirectAttributes를 사용하면 URL 인코딩, pathVariable, 쿼리 파라미터를 처리할 수 있다

또한 redirectAttribute에 넣은 값을 view에서 확인 가능하다

```
<h2 th:if="${param.status}" th:text="'저장 완료!'"></h2>
```

