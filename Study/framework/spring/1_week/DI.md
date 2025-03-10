- 의존성 주입
- 위의 코드를 보면 A는 B에 의존한다
- 여기서 주입의 의미는 B를 외부에서 받아서 사용한다는 것이다
- DI 원칙을 사용하면 코드가 더 깔끔해지고, 객체에 종속성이 제공되면 분리가 더 효과적이다
- 객체는 종속하는 객체의 위치나 클래스를 알지 못 하기 때문에 전혀 종속성에 대한 정보가 없다.
- 결과적으로 클래스를 테스트하기가 더 쉬워지며, 특히 종속성이 인터페이스나 추상 기본 클래스에 있는 경우 단위 테스트에서 스텁이나 모의 구현을 할 수 있게 된다.
- 의존성 주입을 하는 이유는 결합도를 낮추고 모듈의 변경용이성을 높이기 위함이다.
### DI 적용 순서
1. 개발자가 작성한 클래스를 스프링 빈(bean)에 등록시킨다.
	- @Bean, @Component등의 방법이 있다.
2. 해당 Annotation을 붙이면 스프링 앱 실행 시 표시된 클래스를 싱글톤 패턴으로 스프링 빈에 등록이 된다.
3. 이후에 필요한 의존관계를 가진 멤버 변수에 의존성 주입을 하며 IoC를 적용 시키면 된다.

~~~
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@Component// 객체를 스프링이 관리하도록 하는 설정
class B {
    public void doSomething() {
        System.out.println("B 클래스 실행");
    }
}

@Component
class A {
    private final B b;

    @Autowired// 생성자로 의존성 주입 
    public A(B b) {
        this.b = b;
    }

    public void execute() {
        b.doSomething();
    }
}

@SpringBootApplication
public class Main {
    public static void main(String[] args) {
        var context = SpringApplication.run(Main.class, args);
        // 다음과 같이 B에 대한 클래스를 정의하지 않고 의존성으로 주입했기에 A를 바로 선언이 가능
        A a = context.getBean(A.class);
        a.execute();
    }
}
~~~

### 주입방법

**1️. 생성자 주입(Constructor Injection) (⭐️)** 권장 방식

~~~
public class A {
    private final B b; // 반드시 초기화 필요

    public A(B b) {
        this.b = b; // 반드시 초기화됨 (null이 될 수 없음)
    }
}
~~~
- final 키워드를 사용해서 불변성 보장이 가능하다
- 필수적인 의존성이 없으면 오류가 발생해서 의존성 누락이 방지된다.
- `new MemberSignupService(mockRepository)`처럼 테스트 코드에서 명확하게 객체를 생성할 수 있음.

**2️.  setter 주입(Setter Injection)**
~~~
public class A {
    private B b; // 초기화되지 않음 (null 가능)

    public void setB(B b) {
        this.b = b; // 나중에 주입 (null 가능성 있음)
    }
}

~~~
- final 사용이 불가능
- 필드에 의존성 누락 위험이 있음

|방식|**값이 주입되는 시점**|
|---|---|
|**생성자 주입**|**객체가 생성될 때 값이 바로 주입됨** (즉시 할당)|
|**Setter 주입**|**객체가 생성된 후 나중에 값이 주입됨** (지연 할당)|

**3️. 필드 주입(Field Injection)**
~~~
@Service
public class MemberSignupService {
		@Autowired
		private MemberRepository memberRepository;
}
~~~
- 간결하게 작성가능
- 테스트 어려움