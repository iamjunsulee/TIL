# Http Method Mapping

### Http Method 란?
Http(Hyper Text Transfer Protocol)는 클라이언트와 서버 사이에 이루어지는 요청/응답(request/response) 프로토콜이다. Http Method는 서버로 보내는 요청의 방법이라 할 수 있고, 종류에는 GET, POST, PUT, DELETE, PATCH 등이 있다.

### 응답 코드
클라이언트가 서버에 어떤 요청을 하면 서버는 세 자리 수로 된 응답 코드와 함께 결과를 응답한다.

1XX : 정보 교환  
2XX : 데이터 전송이 성공적으로 이루어졌거나, 이해되었거나, 수락됨
- 200 : OK (오류 없이 전송 성공)

3XX : 자료의 위치가 바뀜  
4XX : 클라이언트 측의 오류  
- 404 : Not Found(문서를 찾을 수 없음)
- 405 : Method Not Allowed(메서드 허용 안됨)
- 415 : Unsupported Media Type

5XX : 서버 측 오류
- 500 : Internal Server Error(서버 내부 오류)

### Http Method Mapping 하는 법
1. 클래스 레벨에서 선언하는 방법
```java
@Controller
@RequestMapping(value = "/hello", method = RequestMethod.GET)
public class HelloController {
}
```
위와 같이 컨트롤러 클래스 상단에 @RequestMapping 어노테이션을 사용해서 매핑할 수 있다. "/hello" 로 시작하는 요청을 처리하는 클래스가 된다.

2. 메소드 레벨에서 선언하는 방법
```java
@Controller
@RequestMapping(value = "/hello", method = RequestMethod.GET)
public class HelloController {
    @RequestMapping(value = "/list", method = {RequestMethod.GET, RequestMethod.PUT})
    @ResponseBody
    public String getList() {
        return "hello";
    }
}
```
위와 같이 메소드 레벨에서 선언하여 매핑할 수도 있다. method field 값에 RequestMethod enum을 활용하여 Http Method Type을 설정할 수 있고, 여러 개의 Http Method Type을 같이 설정해줄 수도 있다. 위 getList 메소드의 경우, "/hello/list" 로 들어오는 PUT, GET 요청을 처리한다.
```text
@RequestMapping(value = "/list", method = RequestMethod.GET)
```
위 어노테이션을 아래와 같이 간단하게 사용할 수 있다. Http Method에 따라 @PostMapping, @PutMapping, @DeleteMapping, @PatchMapping을 사용하면 된다.
```text
@GetMapping("/list")
```

### MediaType 지정
@RequestMapping 어노테이션을 사용해서 미디어 타입을 지정해줄 수 있다.  
consumes 필드에 지정한 값에 따라 들어오는 요청을 제한할 수 있다.
```java
@Controller
public class HelloController {
    @RequestMapping(value = "/list", consumes = MediaType.APPLCATION_JSON_VALUE)
    @ResponseBody
    public String getList() {
        return "hello";
    }
}
```
위와 같은 컨트롤러가 있다고 하자. consumes 필드를 통해 JSON 타입으로 미디어 타입을 지정했다. 
```java
@Test
public void getHttpMethodTest() throws Exception {
    this.mockMvc.perform(get("/list"))
    .andDo(print())
    .andExpect(status().isOk())
    .andExpect(content().string("hello"))
    .andExpect(handler().handlerType(HelloController.class));
}
```
테스트를 실행해보자. 위 테스트는 정상적으로 통과되지 않을 것이다.  
결과는 415 응답코드(Unsupported Media Type)가 나타난다.  
요청을 처리하는 컨트롤러의 메소드에서 미디어 타입으로 JSON의 요청만 처리한다고 지정했는데, 테스트 코드에서 미디어 타입을 지정해주지 않았기 때문에 MockHttpServletRequest Header에 아무런 정보가 없기 때문이다.
따라서 아래와 같이 테스트 코드를 수정해야한다.
```java
@Test
public void getHttpMethodTest() throws Exception {
    this.mockMvc.perform(get("/list").contentType(MediaType.APPLICATION_JSON))
    .andDo(print())
    .andExpect(status().isOk())
    .andExpect(content().string("hello"))
    .andExpect(handler().handlerType(HelloController.class));
}
```
Header에 Content-Type 정보를 넘겨주면서 정상적으로 요청을 처리할 수 있게 된다.  

아래와 같이 produces 필드에 지정한 값에 따라 응답 타입을 보장해줄 수 있다.
```java
@Controller
public class HelloController {
    @RequestMapping(value = "/list"
            , consumes = MediaType.APPLCATION_JSON_VALUE
            , produces = MediaType.APPLICATION_JSON_VALUE)
    @ResponseBody
    public String getList() {
        return "hello";
    }
}
```
응답 미디어 타입의 경우, 따로 accept로 지정해주지 않아도 정상적으로 테스트가 통과되었다. 응답 미디어 타입을 지정하지 않았으니까 요청 처리할 때 지정해준 미디어 타입으로 그대로 반환이 되서 그런가보다.
<br>

참고 : <https://ko.wikipedia.org/wiki/HTTP#%EC%9A%94%EC%B2%AD_%EB%A9%94%EC%8B%9C%EC%A7%80>
