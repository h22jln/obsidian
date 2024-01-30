#### 스프링 부트에서의 서블릿
`@ServletComponentScan`을 붙이면 서블릿을 스캔해 자동 등록할 수 있도록 한다

클래스에 
```
@WebServlet(name = "helloServlet", urlPatterns = "/hello")
```
붙여주면 해당 url을 잡아서 가져온다


#### HTTP Request Message
`application.propertie`에
```
logging.level.org.apache.coyote.http11=debug
```
넣어주면, 서버가 받은 HTTP 요청 메시지를 출력한다


#### HttpServletRequest
- Start line
	- HTTP Method
	- URL
	- Query String
	- Scheme, Protocol
- Header
- Body
정보를 편하게 조회할 수 있는 기능을 제공한다
`HttpServletRequest.getXXX()`

```
request.getHeaderNames().asIterator()  
        .forEachRemaining(headerName -> System.out.println(headerName + ": "  
+ request.getHeader(headerName)));
```
Header의 모든 값 조회시, Iterator를 사용하면 된다



#### HTTP POST HTML Form 
POST의 HTML Form을 전송하면, 웹브라우저는 content-type을 `application/x-www-form-urlencoded` 으로 하여 HTTP 메시지를 만든다

*=> POSTMAN으로 content-type을 위와 같이 보내면 form 테스트가 가능하다 (Body: x-www-form-urlencoded)*


#### JSON
```
private ObjectMapper objectMapper = new ObjectMapper();
		
ServletInputStream inputStream = request.getInputStream();
		
        String messageBody = StreamUtils.copyToString(inputStream,
StandardCharsets.UTF_8);
		
        HelloData helloData = objectMapper.readValue(messageBody,
HelloData.class);
```
ObjectMapper를 사용하면 JSON 형식의 데이터를 객체로 변환 가능하다

```
HelloData data = new HelloData();
 data.setUsername("kim");
 data.setAge(20);

 //{"username":"kim","age":20}

 String result = objectMapper.writeValueAsString(data);
```
반대로 객체를 JSON 형태로 변환도 가능하다


JSON 결과를 파싱해서 사용할 수 있는 자바 객체로 변환하려면 ==Jackson,Gson==과 같은 JSON 변환 라이브러리를 추가해서 사용해야 한다. 스프링부트로 Spring MVC를 선택하면 기본으로 Jackson 라이브러리를 제공한다



#### HttpServletResponse
- HTTP 응답코드
- 헤더
- 바디
- Content-Type
- 쿠키
- Redirect
를 설정할 수 있는 기능을 제공한다

**Redirect**
```
response.setStatus(HttpServletResponse.SC_FOUND); //302
response.setHeader("Location", "/basic/hello-form.html");
```
이전에는 302로 지정 후, 강제로 위치를 이동시켰다면

```
response.sendRedirect("/basic/hello-form.html");
```
sendRedirect 하나로 편리하게 가능하다