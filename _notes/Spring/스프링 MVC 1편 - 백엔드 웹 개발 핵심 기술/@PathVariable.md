```
@GetMapping("/mapping/{userId}")
 public String mappingPath(@PathVariable("userId") String data) {

     log.info("mappingPath userId={}", data);

     return "ok";
 }
```

http://localhost:8080/mapping/userA 호출 시 userId를 인식해서 로깅한다

`@PathVariable`의 이름과 파라미터 이름이 같으면 생략할 수 있다

```
@GetMapping("/mapping/{userId}")
 public String mappingPath(String userId) {

     log.info("mappingPath userId={}", data);

     return "ok";
 }
```
