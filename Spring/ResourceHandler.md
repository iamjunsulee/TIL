# 정적 리소스 처리
servlet web application 에서 클라이언트의 모든 요청을 DispatcherServlet 이 받아 해당 요청을 처리할 컨트롤러에게 위임하게 되는데, 매핑되는 URL이 없는 경우, DefaultServletHttpRequestHandler를 통해 DefaultServlet에게 요청에 대한 처리를 위임하게 된다.   

tomcat의 conf 폴더에 있는 web.xml을 확인해보면 아래와 같다.

![tomcat servlet](/Spring/image/tomcatDefaultServlet.PNG)  
![tomcat servlet mapping](/Spring/image/tomcatServletMapping.PNG)  

url-pattern(/)에 대해 DefaultServlet이 처리하도록 매핑되어 있다. 

**url-pattern에 대해서 주의해야할 점에 대해 알아보자.** 
```java
public class WebApplication implements WebApplicationInitializer {
    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        AnnotationConfigWebApplicationContext webApplicationContext = new AnnotationConfigWebApplicationContext();
        webApplicationContext.register(WebConfig.class);
        webApplicationContext.setServletContext(servletContext);
        webApplicationContext.refresh();

        DispatcherServlet dispatcherServlet = new DispatcherServlet(webApplicationContext);
        ServletRegistration.Dynamic app = servletContext.addServlet("app", dispatcherServlet);
        app.addMapping("/*");
    }
}
```
dispatcher servlet의 url-pattern을 위와 같이 "/&#42;" 이라고 설정하는 경우, application을 실행시키면 index.jsp를 찾을 수 없어서 404 에러가 발생하게 된다.
_도대체 이유가 뭘까?_  

"/&#42;"이 의미하는 것은 모든 요청에 대해 DispatcherServlet이 처리하겠다는 뜻이다. "/"에 대한 요청을 처리할 컨트롤러를 찾지만 설정해둔게 없기 때문에 "org.springframework.web.servlet.DispatcherServlet.noHandlerFound No mapping for GET /" 와 함께 404 에러를 리턴하는 것이다.

_그렇다면 해결 방법은 뭘까?_  

바로 url-pattern을 "/"으로 바꾸는 것이다. 바꾸고 나면 문제없이 index.jsp가 호출될 것이다. 하지만 정적 리소스를 요청하면 404 에러가 날 것이다. 왜냐하면 tomcat의 web.xml에 정의된 default servlet url-patten과 동일하기 때문에 해당 요청을 dispatcher servlet이 처리하려고 하다보니 생기는 문제이다.
따라서 해당 요청을 default servlet이 처리하도록 설정해줘야 한다.
```java
@Configuration
@ComponentScan(includeFilters = @ComponentScan.Filter(Controller.class))
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        registry.jsp("/WEB-INF/", ".jsp");
    }

    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }
}
```
WebMvcConfigurer를 구현한 WebConfig 클래스에 configureDefaultServletHandling 메소드를 구현해준다.  
설정하고나면 http://localhost:8080/resources/fcb.jpg 이렇게 정적 리소스에 접근할 경우 아무런 문제없이 처리가 된다.


