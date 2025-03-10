
[1] `@Component` → `@Autowired` : **묵시적 빈 정의**

- 클래스에 어노테이션을 추가하고, Autowired로 다른 클래스에서 해당 Bean을 끌어온다.

[2] `@Configuration` → `@Bean` : **명시적 빈 정의**

- Spring 설정 파일에 Configuration 어노테이션을 추가하고, Bean 어노테이션을 붙여 명시적으로 빈을 지정한다. 
 
#### ==1. **스프링 부트 관련 애너테이션**==

| 애너테이션                      | 설명                                              |
| -------------------------- | ----------------------------------------------- |
| `@SpringBootApplication`   | 스프링 부트 애플리케이션의 진입점(자동 설정, 컴포넌트 스캔, 설정 포함)       |
| `@EnableAutoConfiguration` | 자동 설정 활성화 (보통 `@SpringBootApplication` 내부에 포함됨) |
| `@ComponentScan`           | 지정한 패키지에서 스프링 빈을 자동으로 검색                        |
| `@Configuration`           | 스프링 설정 클래스 정의                                   |
| `@Bean`                    | 수동으로 스프링 빈 등록                                   |

---
#### ==2. **의존성 주입 (DI, Dependency Injection)**==

| 애너테이션                   | 설명                                                     |
| ----------------------- | ------------------------------------------------------ |
| `@Autowired`            | 생성자, 필드, 메서드에 의존성 자동 주입                                |
| `@Qualifier("이름")`      | 같은 타입의 빈이 여러 개 있을 때 특정 빈 선택                            |
| `@Primary`              | 동일한 타입의 빈이 여러 개 있을 때 기본적으로 선택할 빈 지정                    |
| `@Lazy`                 | 해당 빈을 처음 사용할 때까지 초기화를 지연                               |
| `@Value("${property}")` | `application.properties` 또는 `application.yml`에서 값을 가져옴 |

---

#### 3. **컴포넌트 관련 (빈 등록)**

| 애너테이션             | 설명                                              |
| ----------------- | ----------------------------------------------- |
| `@Component`      | 일반적인 스프링 빈 등록                                   |
| `@Service`        | 비즈니스 로직을 수행하는 서비스 계층 빈 등록                       |
| `@Repository`     | 데이터 접근 계층(Dao) 빈 등록 (예: JPA, MyBatis)           |
| `@Controller`     | 스프링 MVC 컨트롤러 클래스 지정                             |
| `@RestController` | `@Controller` + `@ResponseBody` (REST API 컨트롤러) |

---

#### 4. **스프링 MVC 및 REST API**

| 애너테이션                    | 설명                       |
| ------------------------ | ------------------------ |
| `@RequestMapping("/경로")` | HTTP 요청을 특정 컨트롤러 메서드에 매핑 |
| `@GetMapping("/경로")`     | HTTP `GET` 요청을 처리        |
| `@PostMapping("/경로")`    | HTTP `POST` 요청을 처리       |
| `@PutMapping("/경로")`     | HTTP `PUT` 요청을 처리        |
| `@DeleteMapping("/경로")`  | HTTP `DELETE` 요청을 처리     |
| `@PatchMapping("/경로")`   | HTTP `PATCH` 요청을 처리      |
| `@ResponseBody`          | 메서드 반환값을 JSON 형태로 응답     |
| `@RequestParam`          | URL의 쿼리 파라미터 값 매핑        |
| `@PathVariable`          | URL 경로 변수 값 매핑           |
| `@RequestBody`           | 요청 본문을 객체로 변환            |
| `@ModelAttribute`        | 폼 데이터를 객체로 바인딩           |

---

#### 5. **데이터 검증 및 유효성 검사**

| 애너테이션               | 설명              |
| ------------------- | --------------- |
| `@Valid`            | 객체의 유효성을 검사     |
| `@NotNull`          | 필드가 `null`이면 안됨 |
| `@NotEmpty`         | 필드가 빈 문자열이면 안됨  |
| `@NotBlank`         | 공백 문자열 허용 안 됨   |
| `@Size(min=, max=)` | 문자열 길이 제한       |
| `@Pattern(regex=)`  | 정규식을 이용한 패턴 검사  |
| `@Email`            | 이메일 형식인지 검사     |

---

#### 6. **AOP(Aspect-Oriented Programming)**

| 애너테이션                         | 설명                 |
| ----------------------------- | ------------------ |
| `@Aspect`                     | AOP 기능을 가진 클래스 지정  |
| `@Before("execution(메서드)")`   | 특정 메서드 실행 전에 실행    |
| `@After("execution(메서드)")`    | 특정 메서드 실행 후 실행     |
| `@Around("execution(메서드)")`   | 특정 메서드 실행 전후 모두 실행 |
| `@Pointcut("execution(메서드)")` | AOP 적용 지점을 정의      |

---

#### 7. **트랜잭션 관리**

| 애너테이션            | 설명                   |
| ---------------- | -------------------- |
| `@Transactional` | 메서드 또는 클래스에서 트랜잭션 적용 |
| `@Rollback`      | 테스트에서 트랜잭션 롤백 지정     |
| `@Propagation`   | 트랜잭션 전파 속성 설정        |

---

#### 8. **스프링 시큐리티**

| 애너테이션                                                         | 설명                         |
| ------------------------------------------------------------- | -------------------------- |
| `@Secured("ROLE_이름")`                                         | 특정 역할(Role)이 있는 사용자만 접근 가능 |
| `@PreAuthorize("hasRole('ROLE_이름')")`                         | 실행 전 권한 체크                 |
| `@PostAuthorize("returnObject.owner == authentication.name")` | 실행 후 권한 체크                 |
| `@EnableWebSecurity`                                          | 스프링 시큐리티 설정 활성화            |

---

#### 9. **캐시 및 성능 최적화**

| 애너테이션               | 설명         |
| ------------------- | ---------- |
| `@Cacheable("이름")`  | 메서드 결과를 캐싱 |
| `@CachePut("이름")`   | 캐시를 업데이트   |
| `@CacheEvict("이름")` | 캐시를 제거     |

---

#### 10. **테스트 관련**

| 애너테이션             | 설명                       |
| ----------------- | ------------------------ |
| `@SpringBootTest` | 스프링 부트 통합 테스트            |
| `@WebMvcTest`     | MVC 컨트롤러 테스트             |
| `@MockBean`       | 테스트에서 특정 빈을 가짜(Mock)로 교체 |
| `@BeforeEach`     | 테스트 실행 전 실행              |
| `@AfterEach`      | 테스트 실행 후 실행              |

---

#### 11. **이벤트 처리**

| 애너테이션            | 설명             |
| ---------------- | -------------- |
| `@EventListener` | 특정 이벤트 발생 시 실행 |
| `@Async`         | 비동기적으로 실행      |

---

#### 헷갈리는 개념

~~~
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

class B {
    public void doSomething() {
        System.out.println("B 클래스 실행");
    }
}

@Configuration
class AppConfig {
    @Bean
    public B b() {
        return new B();
    }
}
~~~
- 다음과 같이 수동으로 직접 빈을 등록할 수 있다.

| 구분    | 자동 등록 (Component Scan)                  | 수동 등록 (Bean 등록)                     |
| ----- | --------------------------------------- | ----------------------------------- |
| 등록 방식 | `@Component`, `@Service`, `@Repository` | `@Bean` 또는 `BeanDefinitionRegistry` |
| 객체 관리 | 스프링이 자동 관리                              | 개발자가 직접 관리                          |
| 유연성   | 기본적으로 많이 사용됨                            | 복잡한 의존성 관리가 필요할 때 사용                |
| 적용 예시 | 간단한 서비스, 컨트롤러                           | 외부 라이브러리 사용, 동적 빈 추가                |
- 와닿지 않아서 실제 사용해봐야 알 것 같다.


~~~
import org.springframework.stereotype.Component;

interface Printer {
    void print(String message);
}

@Component
class ConsolePrinter implements Printer {
    public void print(String message) {
        System.out.println("콘솔 출력: " + message);
    }
}

@Component
class FilePrinter implements Printer {
    public void print(String message) {
        System.out.println("파일에 저장: " + message);
    }
}
~~~

~~~
import org.springframework.stereotype.Service;

@Service
class MessageService {
    private final Printer printer;

    // @Autowired 사용 시 모호성 발생
    public MessageService(Printer printer) {
        this.printer = printer;
    }

    public void sendMessage(String message) {
        printer.print(message);
    }
}
~~~
- 다음의 경우에는 같은 printer라는 interface를 구현체로 받아서 같은 타입을 주입 받고 있는데 이러한 경우에 오류가 발생한다. 