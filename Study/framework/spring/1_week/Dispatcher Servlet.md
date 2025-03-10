### 서블릿(Servlet)이란?

웹 애플리케이션에서 클라이언트의 요청을 처리하고, 그에 대한 응답을 생성하는 중요한 구성 요소
즉, 자바에서 웹 애플리케이션을 만들 때 HTTP 요청을 처리하는 역할

![[스크린샷 2025-03-03 오후 10.21.07.png]]
1. 사용자(클라이언트)가 URL을 입력하면 HTTP Request가 Servlet Container로 전송합니다.
2. 요청을 전송받은 Servlet Container는 HttpServletRequest, HttpServletResponse 객체를 생성합니다.
3. web.xml을 기반으로 사용자가 요청한 URL이 어느 Servlet에 대한 요청인지 찾습니다.
4. 해당 Servlet에서 service() 메소드를 호출한 후, 클라이언트의 GET, POST 여부에 따라 doGet() 혹은 doPost()를 호출합니다.
5. doGet() 혹은 doPost() 메소드는 동적 페이지를 생성한 후, HttpServletResponse 객체에 응답을 보냅니다.
6. 응답이 끝나면 HttpServletRequest, HttpServletResponse 두 객체를 소멸 시킵니다.

## Servlet Container란?

Servlet을 관리해주는 컨테이너 역할
Servlet이 어떤 역할을 수행하는 **메뉴얼**이라면, Servlet 컨테이너는 해당 메뉴얼을 보고 직접 **핸들링**한다
서블릿 컨테이너는 클라이언트의 요청(Request)를 받아주고 응답(Response)할 수 있게, 웹 서버와 Socket으로 통신을 한다.
우리가 잘 아는 걸로는 아파치 톰캣이다
톰캣은 실제로 웹 서버와 통신하여 JSP(Java Server Page)와 Servlet이 작동하는 환경을 제공해줍니다.

![[스크린샷 2025-03-03 오후 10.21.07.png]]
- 앞서 클라이언트의 요청이 들어오면, 서블릿 컨테이너는 web.xml을 기반으로 사용자가 요청한 URL이 어느 서블릿에 대한 요청인지 찾습니다. 해당 서블릿이 메모리에 없을 경우 init()을 통해 생성하고, 서블릿이 변경되었을 경우 destroy() 후 init()을 통해 새로운 내용을 적재합니다.
- 서블릿이 있는 경우 service() 메소드를 통해 요청에 대한 응답이 doGet(), doPost() 등등으로 나뉘어 response가 생성됩니다.
- 컨테이너가 서블릿을 종료시킬 때에는 destroy()를 통해 종료됩니다. ⇒ 자바의 가비지 컬렉션을 통해 편의를 제공합니다.

Servlet 컨테이너는 이런 기능을 API로 제공하여 간편화했습니다.
결과적으로 우리는 Servlet에 구현해야 할 비즈니스 로직에 대해서만 집중할 수 있겠죠‼️‼️

### DispatcherServlet
- 스프링이 요청을 받고 처리하는 방법
- Dispatecher: 보낸다는 의미
- Servlet: 자바를 이용해 웹을 만드는 기술
- 이는 Front Controller 패턴을 구현한 서블릿으로, 모든 HTTP 요청을 받게 되고 요청을 적절한 Controller로 전달하고, 로직을 실행한 후 응답을 생성해준다.

@Controller
![[스크린샷 2025-03-03 오후 9.45.50.png]]
- Response Body를 사용 
- @RestController ![[스크린샷 2025-03-03 오후 9.46.27.png]]

1. 클라이언트에서 요청을 받는다.
2. Handler Mapping 으로 적절한 Controller를 찾는다.
3. Handler Adapter를 통해 요청을 Controller로 전달한다.(middleware)
4. 비즈니스 로직을 처리하고 view name 을 반환한다. (ex. index.html → “index” 반환, 원래 Spring framework에서 확장자는 .jsp를 사용합니다.)
5. ViewResolver를 이용하여 view name에 맞는 view를 찾는다. (ex. “index”를 이용하여 index.html 찾기)
6. 적절한 view 를 가져온다. (ex. index.html 가져오기)
7. 응답을 반환한다. (Dispatcher servlet이 응답을 반환한다.)

- @RestController는 View 대신에 DTO를 사용하여 데이터를 반환한다

```
@Controller
public class MyController {

    @GetMapping("/umc")
    public String hello(Model model) {
        model.addAttribute("message", "UMC Spring Fighting!");
        return "greeting";  
    }
}

```

1. **요청**: 클라이언트가 `/umc` URL을 통해 HTTP Request가 들어오면, 이 요청은 DispatcherServlet으로 전달됩니다.
2. **DispatcherServlet의 Handler Mapping**: DispatcherServlet은 해당 HTTP Request를 처리할 수 있는 적합한 Controller을 찾습니다.
3. Controller은 요청을 처리하기 위해 `hello()` 메서드를 호출하고, 맞는 비즈니스 로직을 실행한 후 결과를 반환합니다.
4. 반환된 결과는 `ViewResolver`을 통해 적절한 View로 렌더링해주어, HTTP Response로 HTML이나 JSON의 형태로써 전달됩니다.
