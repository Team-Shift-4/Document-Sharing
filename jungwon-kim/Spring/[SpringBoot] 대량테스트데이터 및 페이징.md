https://wikidocs.net/162028

``` java
// [파일명:/practice/src/test/java/com/example/practice/PracticeApplicationTests.java]
package com.example.practice;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import com.example.practice.question.QuestionService;

@SpringBootTest
class PracticeApplicationTests {

	@Autowired
	private QuestionService questionService;
	
	@Test
	void testJpa() {
		for(int i=1; i<=300; i++) {
			String subject = String.format("테스트 데이터입니다:[%03d]", i);
			String content = "내용무";
			this.questionService.create(subject, content);
		}
	}
}

```

# 페이징 구현

- org.springframework.data.domain.Page
- org.springframework.data.domain.PageRequest
- org.springframework.data.domain.Pageable

``` java
// QuestionRepository.java
// Page<Question> findAll(Pageable pageable); 추가

package com.example.practice.question;

import java.util.List;

import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;

public interface QuestionRepository extends JpaRepository<Question, Integer> {
	Question findBySubject(String subject);
	Question findBySubjectAndContent(String subject, String content);
	List<Question> findBySubjectLike(String subject);

	Page<Question> findAll(Pageable pageable);
}
```

``` java
// QuestionService.java

import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
(... 생략 ...)
public class QuestionService {

    (... 생략 ...)

    public Page<Question> getList(int page) {
        Pageable pageable = PageRequest.of(page, 10);
        return this.questionRepository.findAll(pageable);
    }

    (... 생략 ...)
}
```

``` java
// QuestionController.java

import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.data.domain.Page;
(... 생략 ...)
public class QuestionController {

    (... 생략 ...)

    @GetMapping("/list")
    public String list(Model model, @RequestParam(value="page", defaultValue="0") int page) {
        Page<Question> paging = this.questionService.getList(page);
        model.addAttribute("paging", paging);
        return "question_list";
    }

    (... 생략 ...)
}
```

