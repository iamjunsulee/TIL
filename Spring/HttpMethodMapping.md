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
<br>

참고 : <https://ko.wikipedia.org/wiki/HTTP#%EC%9A%94%EC%B2%AD_%EB%A9%94%EC%8B%9C%EC%A7%80>
