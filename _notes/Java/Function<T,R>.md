T타입의 값을 받아서 R타입의 값을 리턴
https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html


- **apply**
```
public class Plus10 implements Function<Integer,Integer> {  
	@Override  
	public Integer apply(Integer integer) {  
			return integer + 10;  
	}  
}
```
Function<T,R>의 method를 override 해서 만들어도 되고,

```
Function<Integer,Integer> plus10_lambda = (n) -> n+10;
```
람다식으로 구현해도 된다

```
plus10_lambda.apply(1)
```
실행은 두 방식 동일하게 apply method를 호출한다


- **compose**
	A.compose(B) -> B의 결과값을 A의 입력값으로 사용하겠다
	
	```
	// 람다식 구현  
	Function<Integer, Integer> plus10_lambda = (n) -> n + 10;  
	Function<Integer, Integer> multifly2 = (n) -> n * 2;  
	  
	// compose 안의 함수부터 실행하겠다  
	// multifly2 의 결과값을 plus10_lambda의 입력값으로 사용한다  
	Function<Integer, Integer> multifly2AndPlus10 
		= plus10_lambda.compose(multifly2);  
	System.out.println(multifly2AndPlus10.apply(2));
	```


- **andThen**
	A.andThen(B) -> A의 결과값을 B의 입력값으로 사용하겠다
	```
	System.out.println(plus10_lambda.andThen(multifly2).apply(2));
	```
