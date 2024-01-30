Controller의 method가 String을 반환하는 경우
- `@ResponseBody`가 없다면 View Resolver가 실행되어 뷰를 찾고 렌더링한다
- `@ResponseBody`가 있다면, View Resolver를 실행하지 않고 HTTP 메시지 바디에 직접 문자열을 입력한다

```
@ResponseStatus(HttpStatus.OK)
@ResponseBody
@GetMapping("/response-body-json-v2")
public HelloData responseBodyJsonV2() {

	HelloData helloData = new HelloData();
	helloData.setUsername("userA");
	helloData.setAge(20);

	return helloData;
}
```
`@ResponseBody`를 사용하면 HTTP 메시지 컨버터를 통해 메시지를 직접 입력할 수 있다
`@ResponseStatus`를 통해 응답코드도 설정 가능하다

이 때 응답 코드를 동적으로 변경할 수는 없으므로
응답코드를 동적으로 변경하고 싶다면
```
@GetMapping("/response-body-json-v1")
    public ResponseEntity<HelloData> responseBodyJsonV1() {

        HelloData helloData = new HelloData();
        helloData.setUsername("userA");
        helloData.setAge(20);

        return new ResponseEntity<>(helloData, HttpStatus.OK);
    }
```
`ResponseEntity`를 사용하면 된다


