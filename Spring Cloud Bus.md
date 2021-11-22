- ### Configuration 값을 바꾸는 방법
  - #### 서버 재기동 - ❌ 번거롭다
  - #### Actuator refresh - ❌ 여러 마이크로 서비스를 운영 중이면, refresh 할 때, 일일이 다 작업을 해야 해서 번거로움
  - #### Spring cloud bus 사용
-----
- ## Spring cloud bus
  - #### 분산 시스템의 `노드(마이크로 서비스)`를 `경량 메시지 브로커(Rabbit MQ)`와 연결
  - #### 상태 및 구성에 대한 `변경 사항`을 연결된 `노드(마이크로 서비스)에게 전달`한다.
  -----
  - ### `AMQP (Advanced Messaged Queuing Protocol)` : 메시지 지향 미들웨어를 위한 개방형 표준 응용 계층 프로토콜
    - #### 메시지 지향, 큐잉, 라우팅 (P2P), 신뢰성, 보안
    - #### Erlang, RabbitMQ에서 사용
  - ### `Kafka 프로젝트`
    - #### Apache Softwre Foundation이 Scalar 언어로 개발한 오픈 소스 메시지 브로커 프로젝트
    - #### 분산형 스트리밍 플랫폼
    - #### 대용량의 데이터를 처리 가능한 메시징 시스템
  -------
  - ### RabiitMQ vs Kafka
    - #### RabitMQ
      - #### 메시지 브로커
      - #### 초당 20+ 메시지를 컨슈머에게 전달
      - #### 메시지 전달 보장, 시스템 간 메시지 전달
      - #### 브로커, 컨슈머 중심
    - #### Kafka
      - #### 초당 100k+ 이상의 이벤트 처리
      - #### Pub/Sub 구조, Topic에 메시지 전달
      - #### Ack를 기다리지 않고 전달 가능
      - #### 프로듀서 중심
-------
- ### Spring Cloud Bus 실습해보기
  - #### Erlang 설치 (23.2) <br><br> ![image](https://user-images.githubusercontent.com/35948339/142766633-50b5f06b-68ed-43a6-b867-6734ed4fc65c.png)
    - #### 환경 변수 설정 <br><br> ![image](https://user-images.githubusercontent.com/35948339/142766623-6f0435f8-c693-410d-b122-39472a3e9be3.png)
      - #### 시스템 변수 - Path - erl 경로 설정
  -----
  - #### Rabbit MQ 설치 (3.8.11) <br><br> ![image](https://user-images.githubusercontent.com/35948339/142766717-9ba5e0fb-6f87-41cd-8bab-fb04ec1fa35c.png)
    - #### 환경 변수 설정 <br><br> ![image](https://user-images.githubusercontent.com/35948339/142766737-ddadc0a7-8787-4619-bd6f-c1f832e4cf1b.png)
    - #### Management Plugin 설치 (권장, PowerShell로 설치) <br><br> ![image](https://user-images.githubusercontent.com/35948339/142766853-cfd430dd-a668-409d-b368-2627eec04617.png)
  -------
  - #### 설치 완료 확인 <br><br> ![image](https://user-images.githubusercontent.com/35948339/142766942-c1333085-593f-439f-ab73-c96435dba65b.png)
  - #### 15672로 접속 ID : guest, PW : guest <br><br> ![image](https://user-images.githubusercontent.com/35948339/142766982-402720cc-a794-431d-a4c6-c9ba07b951bd.png)
-------
  - ### config-service 세팅 하기
    - #### 1️⃣ 의존성을 3 가지 추가한다.
    ``` java
      implementation 'org.springframework.boot:spring-boot-starter-actuator'
      implementation 'org.springframework.cloud:spring-cloud-starter-bus-amqp'
      implementation 'org.springframework.cloud:spring-cloud-starter-bootstrap'
    ```
    - #### 2️⃣ rabbitMQ에 접속하기 위한 정보를 application.yml에 설정한다. <br><br> ![image](https://user-images.githubusercontent.com/35948339/142767711-753256eb-13db-4311-9fb7-edb50b0d869f.png)
    - #### 3️⃣ management도 등록한다. <br><br> ![image](https://user-images.githubusercontent.com/35948339/142767736-85142bc6-dc17-48e8-a021-f2125e303bed.png)
  --------
  - ### userservice 세팅 하기
    - #### 1️⃣ 의존성을 추가한다.
    ``` java
      implementation 'org.springframework.cloud:spring-cloud-starter-bus-amqp'
    ```
    - #### 2️⃣ rabbitMQ에 접속하기 위한 정보를 application.yml에 설정한다. <br><br> ![image](https://user-images.githubusercontent.com/35948339/142767776-5915c2ad-d872-4f78-b951-ef94a89ea1a6.png)
    - #### 3️⃣ management에 busrefresh를 추가한다. <br><br> ![image](https://user-images.githubusercontent.com/35948339/142767825-7f4e66bb-23ae-4d88-b3bb-59525ad762d6.png)
  ------
  - ### spring-cloud-gateway 세팅 하기
    - #### 1️⃣ 의존성을 추가한다.
    ``` java
      implementation 'org.springframework.cloud:spring-cloud-starter-bus-amqp'
    ```
    - #### 2️⃣ rabbitMQ에 접속하기 위한 정보를 application.yml에 설정한다. <br><br> ![image](https://user-images.githubusercontent.com/35948339/142767870-c680d6bf-2ce8-4d16-a370-9a6ad3b3aff8.png)
    - #### 3️⃣ management에 busrefresh를 추가한다. <br><br> ![image](https://user-images.githubusercontent.com/35948339/142767881-47a1fc29-12f4-45f0-930b-2a944542609a.png)
-------
  - #### 정상적으로 rabbitMQ를 설정한 서비스가 접속한 것을 확인 할 수 있다. <br><br> ![image](https://user-images.githubusercontent.com/35948339/142768267-156d5d28-b9e6-4dc1-963e-0679b08577cf.png)
-------
  - ### bootstrap.yml 파일 수정하기
    - #### `userservice`, `spring-cloud-gateway`에서 `bootstrap.yml`을 수정하기 <br><br> ![image](https://user-images.githubusercontent.com/35948339/142769431-80b099ca-0613-4be5-b35a-b511ff33762e.png)
      - #### `인증에 필요한 JWT 토큰정보`를 `config-service`를 거쳐서 읽어온다.
    -------
    - #### 테스트에 용이하도록 `config-service`는 `native(local)에 있는 application.yml파일`을 읽어온다. <br><br> ![image](https://user-images.githubusercontent.com/35948339/142880834-20f40e45-c363-4c8d-ba2e-d3cbefdae495.png)
    ------
    - #### `user-service`에서 계정 생성 후, jwt 토큰을 가지고 `health_check`메소드를 호출하면 정상적으로 값을 잘 가져온다. <br><br> ![image](https://user-images.githubusercontent.com/35948339/142881535-ad3f6007-93be-4960-abb2-fa44b3dfd7a6.png)
    ``` java
      It's Working in User Service on PORT, port(local.server.port)=11632, port(server.port)=0, token             
      secret=user_token_native_application, token expiration time=864000000
    ```
------
  - ### Spring Cloud Bus를 통해 토큰 값 Refresh 해보기
    - #### 1️⃣ config-service의 application.yml 파일을 수정한다.
    ```
      token:
        expiration_time: 864000000
        secret: user_token_native_application_changed_#1

      gateway:
        ip: 192.168.0.8
    ```
    - #### 2️⃣ user-service에 추가한 actuator/`busrefresh`를 통해 변경한 값을 갱신한다. <br><br> ![image](https://user-images.githubusercontent.com/35948339/142891505-9a5f15d8-d0c1-46be-a785-de21025056bc.png) <br><br> ![image](https://user-images.githubusercontent.com/35948339/142891142-a79a2b12-e30d-4b43-a8b6-896dc9fc1ebb.png)
      - #### `user-service`에는 refresh가 되었다는 로그가 생기고 204 메시지를 통해 갱신이 된 것을 알 수 있다.
    - #### 3️⃣ `user-service/health_check`를 통해 application.yml 정보를 확인하면 갱신 된 것을 확인할 수 있다.
    ``` 
      It's Working in User Service on PORT, port(local.server.port)=11632, port(server.port)=0, token 
      secret=user_token_native_application_changed_#1, token expiration time=864000000
    ```

    




  


