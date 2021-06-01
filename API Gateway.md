# Netflix Zuul (API Gateway)
  - __`서비스의 진입점을 설정해서 각 마이크로 서비스에 자동으로 라우팅`__
  - 인증 및 권한 부여
  - 서비스 검색 통합
  - 응답 캐싱
  - 정책, 회로 차단기, Qos 다시 시도
  - 속도 제한
  - 부하 분산
  - 로깅, 추적, 상관관계
  - 헤더, 쿼리 문자열 및 청구 변환
  - IP 허용 목록에 추가

- Spring cloud에서의 MSA간 통신
  - RestTemplate
  - Feign Client

# Netflix Ribbon (Client side Load Balancer)
  - 비동기화 처리가 잘 되지 않아서 최근에는 잘 사용 안함
  - `서비스 이름으로 호출 (ip:port 필요 X)`
  - Health Check (Heartbeat)

## Zuul - application.yml
- ![image](https://user-images.githubusercontent.com/35948339/120265941-ec1ecd00-c2db-11eb-9684-bdde90cbbfc9.png)
  - @EnableZuulProxy 어노테이션 사용
-------
- ![image](https://user-images.githubusercontent.com/35948339/120265900-de694780-c2db-11eb-9ce4-910910171569.png)
  - 클라이언트들 등록
-------
- ![image](https://user-images.githubusercontent.com/35948339/120265995-022c8d80-c2dc-11eb-8a34-6246ede4b9e2.png)
  - ZuulFilter 상속하여 필터를 오버라이드 할 수 있고,
  - @Slf4j + log 메소드를 사용해서 로그 기능을 사용할 수 있다.

## Spring Cloud Gateway (비동기 처리가 가능하다) 
  - application.yml
    - ![image](https://user-images.githubusercontent.com/35948339/120267053-238e7900-c2de-11eb-80b0-7dcd253c73c8.png)
      - predicate : => client가 first-service, second-service 요청이 있다면 해당 uri로 접근하겠다.
-------
  - ![image](https://user-images.githubusercontent.com/35948339/120281424-e2a15f00-c2f3-11eb-993b-c38bb4dda7ef.png)
    - Tomcat이 아닌 Netty서버가 작동한다 (비동기 가능)
-------
  - application.yml 파일이 아닌 class로 filter 적용하기
    - ![image](https://user-images.githubusercontent.com/35948339/120311947-1770de80-c313-11eb-82e1-0710aa939d3a.png)
      - @Configuration, @Bean 등록 후,
      - 메소드 체이닝, Lamda 함수로 route, filter를 적용할 수 있다.
      
    - ![image](https://user-images.githubusercontent.com/35948339/120314021-bdbde380-c315-11eb-993b-08d5e8d09783.png)
      - yml에서도 동일하게 설정해 줄 수 있다.
-------

## Spring Cloud Gateway - Custom filter
  - ![image](https://user-images.githubusercontent.com/35948339/120331007-50677e00-c328-11eb-96a9-82678eac0c99.png)
    - filter 클래스를 만들어서 Webflux 객체인 ServeHttpRequest, Response를 통해 log를 남긴다.
  - ![image](https://user-images.githubusercontent.com/35948339/120331497-cb309900-c328-11eb-85bd-3b24ea31170d.png)
    - yml 파일에서 CustomFilter를 적용 시키면 작동완료!

## Spring Cloud Gateway - Global filter (공통 필터)
  - ![image](https://user-images.githubusercontent.com/35948339/120343828-fff61d80-c333-11eb-9e02-3070a0450781.png)
    - Custom Filter와 비슷하게 만들면 된다.
    - @Data로 Global Filter의 args를 따로 만들어 줄 수 있다.
    - boolean 으로 선언된 args는 isPreLogger()와 같이 lombok에서 자동으로 만들어 준다.
-------
  - ![image](https://user-images.githubusercontent.com/35948339/120343739-ed7be400-c333-11eb-8dc1-d2206d3f1ced.png)
    - default-filters를 선언하여 설정을 해주면 된다.

## Spring Cloud Gateway - Logging filter
  - ![image](https://user-images.githubusercontent.com/35948339/120348694-5d8c6900-c338-11eb-89d1-5610c29f6823.png)
    - 비슷한 방법으로 Logging filter를 만들 수 있다.
-------
  - ![image](https://user-images.githubusercontent.com/35948339/120348512-3766c900-c338-11eb-8f25-d1ec1b10e231.png)
    - 한 서비스에 Custom Filter를 한 개 이상 쓰려면 -name 으로 구분해 줘야 한다.

## Eureka Server 등록하기
  - ![image](https://user-images.githubusercontent.com/35948339/120353044-52d3d300-c33c-11eb-8448-cacf5584bf68.png)
    - 서버 등록 설정을 모두 true로 해주고, Eureka 서버의 위치를 등록 시켜 주면 된다.
-------

## Spring Cloud Gateway - Load Balancer
  - ![image](https://user-images.githubusercontent.com/35948339/120357463-713bcd80-c340-11eb-807d-12afe2663bd1.png)
    - service 하나를 random port로 설정 후,
-------
  - ![image](https://user-images.githubusercontent.com/35948339/120353197-77c84600-c33c-11eb-8692-9f87dd061050.png)
    - Eureka 페이지에 instance 들을 보면 Gateway 제외하고 서비스들 이름을 볼 수 있는데
      - ![image](https://user-images.githubusercontent.com/35948339/120353392-9a5a5f00-c33c-11eb-96e3-00f3f07ace46.png)
      - uri를 lb://instance_name 으로 변경을 한다.
-------
  - ![image](https://user-images.githubusercontent.com/35948339/120357650-a5af8980-c340-11eb-8df2-9847f89518f5.png)
    - 이전과는 다르게 port 설정 없이 multi service 를 실행 시키게 되면,
-------
  - ![image](https://user-images.githubusercontent.com/35948339/120357837-d394ce00-c340-11eb-8748-4618c68e3e7c.png)
    - 랜덤으로 service name이 등록 되고,
-------
  - ![image](https://user-images.githubusercontent.com/35948339/120357924-ea3b2500-c340-11eb-9975-89dcd9008faa.png)
    - port 를 출력하게 되면, 
      - ![image](https://user-images.githubusercontent.com/35948339/120358016-063ec680-c341-11eb-867a-b8e288a3d0d3.png)
      - ![image](https://user-images.githubusercontent.com/35948339/120358053-0f2f9800-c341-11eb-8fa4-5a57c1792354.png)
      - Round Robin 방식으로 번갈아서 port 번호를 출력해 준다.




