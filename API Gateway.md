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
  - ZuulFilter 상속 & @Slf4j 어노테이션 사용해서 필터를 사용할 수 있다.

## Spring Cloud Gateway (비동기 처리가 가능하다) - application.yml
  - ![image](https://user-images.githubusercontent.com/35948339/120267053-238e7900-c2de-11eb-80b0-7dcd253c73c8.png)
    - predicate : => client가 first-service, second-service 요청이 있다면 해당 uri로 접근하겠다.
  - ![image](https://user-images.githubusercontent.com/35948339/120281424-e2a15f00-c2f3-11eb-993b-c38bb4dda7ef.png)
    - Tomcat이 아닌 Netty서버가 작동한다 (비동기 가능)
