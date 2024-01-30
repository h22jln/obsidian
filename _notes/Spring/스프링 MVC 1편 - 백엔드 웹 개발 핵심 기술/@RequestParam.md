```
 @ResponseBody
 @RequestMapping("/request-param-v2")
 public String requestParamV2(
         @RequestParam("username") String memberName,
         @RequestParam("age") int memberAge) {

     log.info("username={}, age={}", memberName, memberAge);

     return "ok";
 }
 ```
` @ResponseBody` : View 조회를 무시하고 HTTP message body에 값을 직접 입력

`@RequestParam`의 괄호 안의 값이 파라미터의 이름으로 사용된다

```
 @ResponseBody
 @RequestMapping("/request-param-v3")
 public String requestParamV3(
         @RequestParam String username,
         @RequestParam int age) {
     log.info("username={}, age={}", username, age);
     return "ok";

}
```
==파라미터 이름과 변수 이름이 같다면 `@RequestParam` 생략 가능==