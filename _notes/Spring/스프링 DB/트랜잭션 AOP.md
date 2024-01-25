[[트랜잭션 매니저]]나 [[트랜잭션 템플릿]]을 사용하면 기존의 방식보다는 코드가 간결해지기는 하지만,
서비스 계층에 비즈니스 로직**만** 남길 수는 없었다

이럴 때 스프링 AOP를 통해 프록시를 이용하면 해결 가능하다


```
	@Transactional
     public void accountTransfer(String fromId, String toId, int money) throws
		 SQLException 
	 {
         bizLogic(fromId, toId, money);
     }
```
트랜잭션을 적용하고자 하는 메소드/클래스에 `@Transactional`을 붙여주기만 하면 된다
*클래스에 붙이는 경우, public한 메소드가 대상이 된다*

`@Transactional`을 붙이면, 스프링이 해당 클래스를 상속받은 프록시 객체를 만들게 되고
프록시 객체 내에서 커넥션 관리를 알아서 다 하게 된다

***
*\*테스트의 경우
`@Transactional`은 스프링 컨테이너 위에서만 동작하므로 `@SpringBootTest`를 붙여주자
`@Autowired`로 빈들을 사용할 수 있다

*이 안에서 `@TestConfiguration`을 붙인 내부 설정 클래스를 생성해 빈을 직접 등록할 수 있다*

```
Assertions.assertThat(AopUtils.isAopProxy(memberService)).isTrue();
```
*위의 코드로 해당 객체가 프록시인지 확인할 수 있다*

***

트랜잭션 AOP는 트랜잭션 매니저를 사용하기 때문에 `DataSourceTransactionManager`를 빈으로 등록해야 한다

-> **이를 스프링 부트가 대신 자동 등록해준다**

DataSource -> **dataSource**
	`application.properties` 에 있는 속성들을 이용해 생성하고 빈에 등록한다
	`spring.datasource.url` 속성이 없으면 내장 데이터베이스(메모리 DB)를 생성하려고 시도한다.

PlatformTransactionManager -> **transactionManager**
	어떤 트랜잭션 매니저를 사용할지는 현재 등록된 라이브러리를 보고 판단한다
	JDBC 사용시 -> `DataSourceTransactionManager`
	JPA 사용시 -> `JpaTransactionManager`
	둘 다 사용시 -> `JpaTransactionManager`
	\* `JpaTransactionManager`는 `DataSourceTransactionManager`의 기능을 대부분 지원한다

데이터 소스와 트랜잭션 매니저를 직접 등록하는 경우, 스프링 빈은 자동생성하지 않는다







