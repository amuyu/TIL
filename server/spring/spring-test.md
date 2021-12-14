

# Spring test context 이용
## dependency
```
testCompile('org.springframework.boot:spring-boot-starter-test')
```

## code
```
// #1
@SpringJUnitConfig(MovieBuddyFactory.class)
// #2
@ExtendWith(SpringExtension.class)
@ContextConfiguration(classes = MovieBuddyFactory.class)
// 
@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional
```

# example

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional
public class MemberServiceTest {

	@Autowired MemberService memberService;
	@Autowired MemberRepository memberRepository;

	@Test
	@Rollback(false)	// Insert query 와 데이터를 보고 싶을 때
	public void 회원가입() throws Exception {
		Member member = new Member();
		member.setName("kim");

		Long savedId = memberService.join(member);

		assertEquals(member, memberRepository.findONe(savedId));
	}

}

```