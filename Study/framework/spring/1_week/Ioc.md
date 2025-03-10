- Inversion of Control(제어의 역전)
- 객체의 생명주기와 메소드 호출에 대한 제어
- 객체의 생성과 관리를 프레임워크가 담당하는 개념이다

~~~
public class B {}

public class A {
	private B b;
	
	public A(B b) { 
		this.b = b; 
	}
}

public static void main(String[] args) {
	B b = new B();
	A a = new A(b);
}
~~~

- 일반적인 자바에서의 클래스 호출 방식은 다음과 같지만 스프링에서는 다음과 같은 객체 생성 및 멤버 변수에 객체를 넣어주는 일들을 스프링 프레임워크가 해준다.
- 즉, 이것도 비즈니스 로직에 집중 할 수 있게 해준 것

### IoC 컨테이너의 작동 방식

1. **객체를 class로 정의합니다.**
2. **객체들 간의 연관성 지정**: Spring 설정 파일(Config) 또는 어노테이션(`@Component` , `@Configuration`, `@Autowired`, `@Bean` )을 통해 객체들이 어떻게 연결될지(**의존성 주입**) 지정해줍니.
3. IoC 컨테이너가 이 정보를 바탕으로 객체들을 **생성하고 필요한 곳에 주입합니다**.

### 빈(Bean)

간단하게 말해, **Spring 컨테이너가 관리하는 자바의 객체**를 의미

`ApplicationContext.getBean()` 함수를 호출했을 때 얻어질 수 있는 것이 Spring의 빈이다.

Spring은 빈(Bean)을 통해 객체를 인스턴스화한 후, 객체 간의 의존 관계를 관리한다.

객체가 의존관계를 등록할 때 Spring 컨테이너에서 해당하는 빈을 찾고, 그 빈과 의존성을 만들게 된다.