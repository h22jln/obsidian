null이 return되어 NPE가 발생할 가능성이 있는 경우, return 부분에
```
public Optional<Progress> getProgress() {  
    return Optional.ofNullable(progress);  
}
```
Optional을 걸어주면 NPE가 발생하지 않고 안전하게 처리할 수 있다


- **return 타입에만 쓰는 것이 ==권장사항==이다**
	매개변수로도 Optional 타입을 넘길 수 있지만, 매개변수에는 null값을 넘길 수 있기 때문에 NPE가 발생 할 수 있고, 번거로워진다

- 프리미티브 타입용 Optional이 따로 있다
	Int, Long 등 따로 Optional이 있는 경우 사용하는 것이 효율적이다
	기본 Optional을 사용한다고 에러가 나진 않지만 Boxing, UnBoxing이 타입용에 비해 많이 실행된다

- Optional을 리턴하는 메소드에서 null을 리턴하지 말자
	Optional을 리턴하는 메소드를 밖에서 사용할 때엔 Optional이 제공하는 메소드를 이용하므로, 리턴값이 없는 경우 null 대신 ``Optional.empty()``를 사용하자

- Collection, Map, Stream, Array, Optional은 Optional로 감싸지 말자
	해당 타입들이 비어있는지, 아닌지를 제공하므로 굳이 감쌀 필요가 없다


### 실습 예제
```
package ex05;  
  
import ex05.OnlineClass;  
  
import java.util.ArrayList;  
import java.util.List;  
import java.util.Optional;  
  
public class main {  
    public static void main(String[] args) {  
  
        List<OnlineClass> springClasses = new ArrayList<>();  
        springClasses.add(new OnlineClass(1, "spring boot", true));  
        springClasses.add(new OnlineClass(5, "rest api development", false));  
  
        Optional<OnlineClass> optional = springClasses.stream()  
                .filter(s -> s.getTitle().startsWith("spring"))  
                .findFirst();  
  
        boolean present = optional.isPresent(); // 있는지 체크  
        boolean empty = optional.isEmpty(); // 없는지 체크  
  
        // optional이 있다면 괄호안의 문장을 실행하라  
        optional.ifPresent(s->{  
            System.out.println(s.getTitle());  
        });  
  
        // optional이 있으면 넣고, 없으면 괄호 OnlineClass 타입의 새 클래스를 만들어라  
        // optional이 있더라도 orElse 안의 내용은 반드시 실행된다  
        // 이미 상수로 만들어진 것 등을 참고하는 경우에 적합  
        OnlineClass onlineClass = optional.orElse(createNewClass());  
        System.out.println(onlineClass.getTitle());  
  
        // orElseGet의 파라미터 타입은 Supplier        // optional이 없으면 orElseGet 안의 내용은 실행되지 않는다  
        // 동적으로 생성할 때 적합  
        OnlineClass onlineClass2 = optional.orElseGet(() -> createNewClass());  
  
        // optional이 없다면 Error를 던짐  
        OnlineClass onlineClass3 = optional.orElseThrow();  
        // 파라미터가 Supplier이므로 원하는 에러 정의 가능  
        OnlineClass onlineClass4 = optional.orElseThrow(()->{  
            return new IllegalStateException();  
        });  
        OnlineClass onlineClass5 = optional.orElseThrow(IllegalStateException::new);  
  
        // optional에 값이 있다는 가정 하에 값을 걸러낼 수 있다  
        // 값이 없는 경우 Optional.empty()로 나온다  
        Optional<OnlineClass> onlineClass1 = optional.filter(s -> s.getId() > 10);  
  
        // map으로 opptional의 Type을 바꿀 수 있다  
        Optional<Integer> i = optional.map(OnlineClass::getId);  
  
        // optional type을 map하게 되면 optoinal을 꺼내줘야 한다  
        Optional<Optional<Progress>> progress = optional.map(OnlineClass::getProgress);  
        Optional<Progress> progress1 = progress.orElseThrow();  
        Progress progress2 = progress1.orElseThrow();  
  
        // -> 이런 경우 유용한 flatMap        Optional<Progress> progress3 = optional.flatMap(OnlineClass::getProgress);  
        Progress progress4 = progress3.orElseThrow();  
  
    }  
  
    private static OnlineClass createNewClass() {  
        System.out.println("create new class");  
        return new OnlineClass(10, "New class", false);  
    }  
}
```