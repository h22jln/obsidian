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
