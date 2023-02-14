`Controller -> Service -> Repository` 구조

## 순서

Controller : 주소매핑, ResponseEntity 메소드 생성

→ Service(interface) : ResponseEntity 생성

→ ServiceImpl : override 해서 코드 추가 (Optional), ResponseEntity 바디 추가

``` javascript
if (memberEntity==null){
            return new ResponseEntity("해당 아이디를 가진 회원이 존재하지 않습니다.", HttpStatus.OK);
}
```

→ ajax 통신 요청 

``` javascript
<script type="text/javascript">
        $(function () {
            $('.btn-submit').click((e) => {
                const id = $('#id').val();
                const password = $('#password').val();

                if (id == '') {
                    alert('아이디를 입력해주세요');
                    e.preventDefault();
                }
                if (password == '') {
                    alert('패스워드를 입력해주세요');
                    e.preventDefault();
                }

                if (id != '' && password !='') {
                    const path = 'http://localhost:9959/api/login';
                    const json = JSON.stringify({
                        'id': id,
                        'password': password,
                    });
                    $.ajax({
                        url: path,
                        type: 'POST',
                        contentType: 'application/json',
                        data: json,
                    }).done((response) => {
                        if (response == 'success') {
                            alert('로그인 성공')
                            localStorage.setItem('id', id);
                            location.href = "http://localhost:9959"
                        } else {
                            alert(response);
                        }
                    });
                }
            });
        });
    </script>
```





## ajax


1. ajax로 통신보냄 /api/signup 등

## DTO

2. 요청시 데이터 받아올 DTO 생성 (@Getter, @Setter)

## Repository

3. 인터페이스 생성 후 JpaRepository<Entity 클래스, PK 타입> 상속받기

``` java
import org.springframework.data.jpa.repository.JpaRepository;

public interface MemberRepository extends JpaRepository<Member, String> {
}
```

## Controller

4. Controller 에서 서비스 리턴

``` java
@RestController
@RequestMapping("/api")
@RequiredArgsConstructor
public class ApiController {

    private final MemberService memberService;
    
    @PostMapping("/signup")
    public ResponseEntity userSignup(@RequestBody SignUpFormDTO formDTO) {
        return memberService.signup(formDTO);
    }

}
```

## Service

5. 인터페이스 만들고 

``` java
public interface MemberService {
    ResponseEntity signup(SignUpFormDTO formDTO);
}
```

6. 구현 클래스 작성

``` java
@Service
@Transactional
@RequiredArgsConstructor
public class MemberServiceImpl implements MemberService {

    private final MemberRepository memberRepository;

    public ResponseEntity signup(SignUpFormDTO formDTO) {

        Optional<Member> member = memberRepository.findById(formDTO.getId());

        if (member.isEmpty()) {
            Member newMember = Member.builder()
                    .id(formDTO.getId())
                    .password(formDTO.getPassword())
                    .name(formDTO.getName())
                    .role(MemberRole.USER)
                    .build();

            memberRepository.save(newMember);

            return new ResponseEntity("success", HttpStatus.OK);
        } else {
            return new ResponseEntity("fail", HttpStatus.OK);
        }
    }
}
```







