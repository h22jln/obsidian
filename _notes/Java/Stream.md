Stream은 중개 오퍼레이션(intermediate operataion)과 종료 오퍼레이션(terminal operation)이 이어진 구조인데, 중개 오퍼레이션은 0..n개로 구성되어 있으며 끝에는 항상 하나의 종료 오퍼레이션이 존재한다.

종료 오퍼레이션에 도달하기 전에는 중개 오퍼레이션이 실행되지 않으므로 중개 오퍼레이션은 lazy하다.

``stream()`` 대신 ``parallelStream()`` 을 사용하면 병렬 처리가 가능하지만, 병렬 처리를 한다고 해서 속도가 빨라짐을 보장하지는 않는다.

- 중개 오퍼레이션
	return type이 Stream인 경우

- 종료 오퍼레이션
	return type이 Stream이 아닌 경우

### flatMap
Stream 내부에 Type이 Collection인 경우, flat하게 만들 수 있다 = Collection에 담긴 데이터를 꺼내 Stream을 구성할 수 있게 된다
```
List<List<OnlineClass>> myClasses = new ArrayList<>();

myClasses.stream().flatMap(list->list.stream())  //(1)
                .forEach(s-> System.out.println(s.getId()));
```

(1) 시점에서 flat하게 되어 stream의 Type이 List가 아닌 OnlineClass가 된다





## 실습 예제
```
package ex04;  
  
import java.awt.*;  
import java.sql.SQLOutput;  
import java.util.ArrayList;  
import java.util.Comparator;  
import java.util.List;  
import java.util.function.Predicate;  
import java.util.stream.Collectors;  
import java.util.stream.Stream;  
  
public class main {  
    public static void main(String[] args) {  
        List<OnlineClass> springClasses = new ArrayList<>();  
        springClasses.add(new OnlineClass(1, "spring boot", true));  
        springClasses.add(new OnlineClass(2, "spring data jpa", true));  
        springClasses.add(new OnlineClass(3, "spring mvc", false));  
        springClasses.add(new OnlineClass(4, "spring core", false));  
        springClasses.add(new OnlineClass(5, "rest api development", false));  
  
        System.out.println("1.spring으로 시작하는 수업");  
        springClasses.stream()  
                .filter(s -> s.getTitle().startsWith("spring"))  
                .forEach(s -> System.out.println(s.getTitle()));  
        System.out.println();  
  
        System.out.println("2.close 되지 않은 수업");  
        springClasses.stream().filter(s->!s.isClosed()) //s->!s.isClosed() == Predicate.not(OnlineClass::isClosed)  
                        .forEach(s-> System.out.println(s.getTitle()));  
        System.out.println();  
  
        System.out.println("3.수업 이름만 모아서 스트림 만들기");  
        springClasses.stream().map(OnlineClass::getTitle)  
                .forEach(System.out::println);  
        System.out.println();  
  
        List<OnlineClass> javaClasses = new ArrayList<>();  
        javaClasses.add(new OnlineClass(6, "The Java, Test", true));  
        javaClasses.add(new OnlineClass(7, "The Java, Code manipulation", true));  
        javaClasses.add(new OnlineClass(8, "The Java, 8 to 11", false));  
  
        List<List<OnlineClass>> myClasses = new ArrayList<>();  
        myClasses.add(springClasses);  
        myClasses.add(javaClasses);  
  
        System.out.println("4.두 수업 목록에 들어있는 모든 수업 아이디 출력");  
        myClasses.stream().flatMap(list->list.stream())  
                        .forEach(s-> System.out.println(s.getId()));  
        System.out.println();  
  
        System.out.println("5.10부터 1씩 증가하는 무제한 스트림 중에서 앞에 10개 빼고 최대 10개 까지만");  
        Stream.iterate(10, i -> i + 1) //무제한 스트림  
                .skip(10)  
                .limit(10)  
                .forEach(System.out::println);  
        System.out.println();  
  
        System.out.println("6.자바 수업 중에 Test가 들어있는 수업이 있는지 확인");  
//        javaClasses.stream().filter(s->s.getTitle().contains("Test"))  
//                        .forEach(s-> System.out.println(s.getTitle());  
        boolean test = javaClasses.stream().anyMatch(s -> s.getTitle().contains("Test"));  
        System.out.println(test);  
        System.out.println();  
  
        System.out.println("7.스프링 수업 중에 제목이 spring이 들어간 제목만 모아서 List로 만들기");  
        List<String> spring = springClasses.stream().filter(s -> s.getTitle().contains("spring"))  
                .map(OnlineClass::getTitle)  
                .collect(Collectors.toList());  
        spring.forEach(System.out::println);  
  
    }  
}
```


*map을 쓰면 Type이 변하므로, 그 다음 쓸 operator의 타입을 잘 생각해야 한다*
