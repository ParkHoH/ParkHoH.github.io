---
title: "Web Server와 WAS의 개념과 차이점"
excerpt: ""

categories: WEB
tags: [Web server, WAS, WEB, 웹 서비스 아키텍쳐]

date: 2022-03-26
last_modified_at: 2022-03-26
---

> 최근에 자바 스프링 부트에 대해 공부를 시작하면서, 웹 서버와 WAS에 대해 공부할 필요성을 느끼게 됐다. 웹에 대한 기본 개념인만큼 꼼꼼히 정리해보려고 한다.

## 웹 서버(Web Server)란?
> 웹 브라우저 클라이언트로부터 http 요청을 받아 정적인 , CSS, JavaScript, jpeg 등)를 직접 제공하고 동적인 컨텐츠에 대해 WAS에 요청을 보내는 프로그램

웹 서버는 하드웨어와 소프트웨어로 구분된다.  
- **스프트웨어**: 가장 위에서 정의한 개념
- **하드웨어**: 웹 서버가 설치되어 있는 컴퓨터를 의미


### <U>웹 서버의 역할</U>
- 정적인 컨텐츠를 제공할 때는 WAS를 거치지 않음
- 클라이언트가 동적인 컨텐츠에 대한 요청을 보내면 웹 서버는 이를 WAS에 보내고, WAS가 처리한 결과를 웹 서버가 받은 다음 클라이언트에 응답함.

정적/동적 컨텐츠를 웹 서버가 동시에 처리하지 않는 이유가 가장 궁금했는데, 이 부분은 '웹 서버와 WAS를 분리하는 이유'에서 설명하도록 하겠다.

### <U>웹 서버의 예시</U>
Apache, Nginx, IIS(Windows 전용 웹서버) 등



## WAS(Web Application Server)란?
> Http 프로토콜을 통해 사용자의 컴퓨터나 장치에 어플리케이션을 수행하는 미들웨어(소프트웨어 엔진)로서, 동적 컨텐츠를 제공하며 주로 DB 서버와 같이 동작함

DB 조회나 다양한 로직에 대한 처리를 요구하는 동적 컨텐츠를 제공하기 위해 만들어진 서버가 WAS이다.


웹 컨테이너 or 서블린(servlet) 컨테이너라고도 불린다. 여기서 컨테이너란 JSP, Servlet을 실행시킬 수 있는 소프트웨어이다. 참고로 자바에서는 웹 어플리케이션을 컨테이너라고 부른다.


### <U>WAS의 기능</U>
- 웹 서버 기능들을 구조적으로 분리하여 처리하고자 하는 목적
    - 웹 서버: 정적 기능 처리
    - WAS: 동적 기능 처리 / 분산 트랜잭션, 보안, 메시징, 쓰레드 처리 등의 기능을 처리하는 분산 환경에서 사용
- 프로그램 실행 환경과 DB 접속 기능 제공
- 비즈니스 로직 수행
- WAS 자체가 웹 서버를 가지고 있을 수 있는데, 여기서 정적인 컨텐츠를 처리하는데 성능상 큰 차이는 없다고 한다. 예를 들어, Apache Tomcat의 경우 대표적인 WAS인데, 정적 컨텐츠를 제공할 수 있는 기능을 5.5 버전부터 이용할 수 있고, 실제 성능도 괜찮다고 한다.


### <U>WAS의 예시</U>
Apache Tomcat, JBoss, Jeus, Web Sphere 등


## 웹 서버와 WAS를 분리하는 이유
웹과 여러 프로그램들의 개념, 기능들에 대해 공부하다보면 간혹 이해가 안 가는 것들이 있었는데, 이 이유에 대해 깊게 파고들다 보면 대체로 여러 효율성과 편의성을 위해서인 경우가 많았다.

웹 서버와 WAS를 분리하는 이유도 결국 자원 이용의 **효율성**과 장애 극복, 배포 및 유지 보수의 **편의성**을 위해서이다. 아래는 자세한 이유들이다.

- 웹 서버에서는 정적인 컨텐츠를 WAS까지 보내지 않고 빠르게 전송 가능
- WAS에서는 데이터를 DB에서 가져와 비즈니스 로직에 맞게 결과를 제공
- 이에 따라, 클라이언트의 요청에 맞는 데이터를 제공함으로써 서버의 부담을 줄여 자원을 효율적으로 사용할 수 있음. 또한 
- 여러 대의 WAS를 연결함으로써 웹 서버에서 적절한 Load Balancing(클라이언트의 요청을 여러 WAS에 적절히 분배하는 방법)이 가능함
- 웹 서버가 fail back(작동 중지된 WAS를 재시작), fail over(작동 중지된 WAS를 대신해 다른 WAS를 사용)에 유리해 대규모 서비스에서도 무중단 운영이 가능하다.
- 여러 웹 어플리케이션 서비스 동시 사용 가능: PHP 어플리케이션, Java 어플리케이션 동시에 사용 가능


그럼 WAS가 웹 서버의 기능을 모두 수행하면 어떻게 될까?  


실제로 사용자가 적을 경우에는 효과적일 수 있지만, 추후에는 위에 나열한 효율성과 편의성을 위해 웹 서버와 WAS를 분리하는 것이 필요하다.  
물론 웹 서버와 여러 WAS를 분리하는 것은 설계 단계와 유지보수 단계가 복잡하기 때문에 서비스의 규모와 효율성에 맞게 선택해야 한다.


## 웹 서비스 아키텍쳐
웹 서비스 시 서버가 클라이언트의 요청을 받아 처리하는 방법은 아래와 같다.

1. 웹 서버는 웹 브라우저 클라이언트로부터 Http request을 받아 WAS에 보낸다.
2. WAS는 관련된 Servlet을 메모리에 올린다.
3. WAS는 web.xml을 참조하여 해당 Servlet에 대한 Thread를 생성한다.(Thread Pool 이용)
4. HttpServletRequest와 HttpServletResponse 객체를 생성하여 Servlet에 전달한다.
5. 생성된 Thread는 Servlet의 service() 메서드를 호출한 후, service() 메서드는 요청에 맞게 doGet(), doPost() 등의 메서드를 호출한다.
6. 적절한 동적 페이지를 response 객체에 담아 WAS에 전달한다.
7. WAS는 response 객체를 HttpResponse 형태로 바꾸어 웹 서버에 전달한다.
8. 생성된 Thread를 종료하고, HttpServletRequest와 HttpServletResponse 객체를 제거한다.


## 정리하면서 느낀 점 & 추후 공부할 부분
하나씩 공부하는 재미가 있기는 하지만, 아직 어렴풋이 아는 것이 많은 것 같다.

웹 서버와 WAS에 대해 공부하면서도 추가로 궁금한 점들이 많이 생겼다.
- 'WAS 내에 있는 웹 서버가 어떻게 동작하는가?'
- 웹 서버의 Load Balancing Alogorithm(라운드 로빈, 가중 라운드 로빈 등)

공부를 하면 할 수록 궁금한 것들이 더 쌓이는 느낌이다.
혹 떼러 왔다가 혹 붙이는 느낌이다..
하나씩 지워가야지..


## 참고 자료
- [https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)
- [https://codechasseur.tistory.com/25](https://codechasseur.tistory.com/25)
- [https://gyoogle.dev/blog/web-knowledge/Web%20Server%EC%99%80%20WAS%EC%9D%98%20%EC%B0%A8%EC%9D%B4.html](https://gyoogle.dev/blog/web-knowledge/Web%20Server%EC%99%80%20WAS%EC%9D%98%20%EC%B0%A8%EC%9D%B4.html)
- [https://velog.io/@bky373/Web-%EC%9B%B9-%EC%84%9C%EB%B2%84%EC%99%80-WAS](https://velog.io/@bky373/Web-%EC%9B%B9-%EC%84%9C%EB%B2%84%EC%99%80-WAS)
- [https://helloworld-88.tistory.com/71](https://helloworld-88.tistory.com/71)