JDBC를 사용할 때
- 커넥션 조회, 동기화
- PreparedStatement 생성, 파라미터 바인딩
- 쿼리 실행
- 결과 바인딩
- 예외 발생시 예외 변환기 실행
- 리소스 종료
의 과정을 항상 반복해야 했다

스프링은 이 반복 문제를 해결하기 위해 `JdbcTemplate`를 제공한다

```
private final JdbcTemplate template;

public MemberRepositoryV5(DataSource dataSource) {
	template = new JdbcTemplate(dataSource);
}

@Override    
public Member findById(String memberId) {	
	String sql = "select * from member where member_id = ?";
	return template.queryForObject(sql, memberRowMapper(), memberId);

private RowMapper<Member> memberRowMapper() {
	 return (rs, rowNum) -> {
		Member member = new Member();
		member.setMemberId(rs.getString("member_id"));
		member.setMoney(rs.getInt("money"));
		return member;

}; }
}
```