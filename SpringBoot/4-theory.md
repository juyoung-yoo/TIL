4.SpringBoot 원리 - 컨테이너와 서버 포트

참고 : https://docs.spring.io/spring-boot/docs/current/reference/html/howto-embedded-web-servers.html
Tomcat 외 다른 서블릿 컨테이너 사용하고 싶을 때, tomcat을 제거하고 원하는 container 추가한다. 
~~~
configurations {
        // exclude Tomcat
        compile.exclude module: 'spring-boot-starter-tomcat'
}

dependencies {
        compile 'org.springframework.boot:spring-boot-starter-webflux'
        // Use Undertow instead
        compile 'org.springframework.boot:spring-boot-starter-undertow'
        // ...
}
~~~
78.2 Disabling the Web Server(웹 서버 사용 안하는 설정)
	1. application.properties 에 설정
~~~
spring.main.web-application-type=none   
~~~
	1. application.properties 
	2. Application run 전에 webServer 가 아님을 명시한다. 
~~~
package com.juyoung.springboottomcat;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.WebApplicationType;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringBootTomcatApplication {


    public static void main(String[] args) {


//        SpringApplication.run(SpringBootTomcatApplication.class, args);
        
        SpringApplication springApplication = new SpringApplication(SpringBootApplication.class);
        springApplication.setWebApplicationType(WebApplicationType.NONE);
        springApplication.run(args);
    }


}
~~~
78.3 Change the HTTP Port ( 포트변경)
application.properties
~~~
server.port=8000
// 랜덤 포트 
Server.port=0
~~~
78.5 Discover the HTTP Port at Runtime 
~~~
best : @Bean of typeApplicationListener<ServletWebServerInitializedEvent> and pull the container out of the event when it is published.

import org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext;
import org.springframework.boot.web.servlet.context.ServletWebServerInitializedEvent;
import org.springframework.context.ApplicationListener;
import org.springframework.stereotype.Component;


@Component
public class PortListener implements ApplicationListener<ServletWebServerInitializedEvent> {
    @Override
    public void onApplicationEvent(ServletWebServerInitializedEvent event) {
        ServletWebServerApplicationContext context = event.getApplicationContext();
        System.out.println(context.getWebServer().getPort());
    }
}
~~~
Test 
~~~
@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(webEnvironment=WebEnvironment.RANDOM_PORT)
public class MyWebIntegrationTests {

        @Autowired
        ServletWebServerApplicationContext server;

        @LocalServerPort
        int port;

        // ..
}
~~~
78.6 Enable HTTP Response Compression (압축)
HTTP response compression is supported by Jetty, Tomcat, and Undertow. It can be enabled in application.properties, as follows:
~~~
server.compression.enabled=true
~~~
