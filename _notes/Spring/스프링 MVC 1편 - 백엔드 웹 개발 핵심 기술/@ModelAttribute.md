```
@ResponseBody
 @RequestMapping("/model-attribute-v1")
 public String modelAttributeV1(@ModelAttribute HelloData helloData) {

     log.info("username={}, age={}", helloData.getUsername(),
 helloData.getAge());

     return "ok";
 }
 ```
 객체와 파라미터의 값을 바인딩해서 객체에 담아준다
*이 때 setter를 사용하므로 객체에 setter가 구현되어 있어야 한다*

```
 @ResponseBody
 @RequestMapping("/model-attribute-v2")
 public String modelAttributeV2(HelloData helloData) {

     log.info("username={}, age={}", helloData.getUsername(),
 helloData.getAge());

     return "ok";
 }
```
[[@RequestParam]]과 같이 `@ModelAttribute`도 생략 가능한데, 

스프링은 어노테이션 생략시 
String, int, Integet 등등 단순 타입은 `@RequestParam`으로, 나머지는 `@ModelAttribute`로 처리한다.
(*\*단 argument resolver로 지정한 타입 제외)*

***
```
@ModelAttribute("item") Item item
```

`@ModelAttribute`로 지정하면
Item 객체에 파라미터로 넘어온 값들을 set 해주고, Model에 "item" 이름으로 자동으로 addAttribute 한다

*괄호 안의 이름을 생략하면 클래스 명의 첫자를 소문자로 해서 model에 넣어준다*

