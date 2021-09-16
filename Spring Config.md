## Spring Config
  - ### yml 파일 등 각 서비스의 설정을 한 곳에 모아서 관리할 수 있게 하자
------
## Spring Config 프로젝트 생성
  - ### 1️⃣ Config Server 체크하고 생성 <br><br> <img src="https://user-images.githubusercontent.com/35948339/133637665-e3a77359-a70e-4934-84ed-1e3e12cd6656.png" width=700>
  - ### 2️⃣ Dependency 추가 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133638436-d17d91b9-3dd2-4399-bb81-8f84a6d44e03.png)
  - ### ⭐ 서비스가 있는 프로젝트 폴더가 아닌 새로운 폴더에 폴더 생성 후, yml 파일 생성 ➡ git init / add <br><br> ❌ 서비스가 있는 폴더에 생성하면, 마지막 커밋으로 롤백되는 현상 발생 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133638233-047a00e4-3400-4103-b475-83f5715306ef.png)
  - ### 3️⃣ application.yml 생성 후, 포트 설정 및 위에서 만든 ecommerce.yml의 경로를 정해주면 된다. <br><br> ![image](https://user-images.githubusercontent.com/35948339/133640975-09a510b0-a764-4cdb-bc34-94bccd3c01d0.png)

------
## user-service에서 Test ‼
  - ### 1️⃣ Dependency 추가 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133638436-d17d91b9-3dd2-4399-bb81-8f84a6d44e03.png)
  - ### 2️⃣ resource에 bootstrap.yml을 만들어서 위에서 만든 ecommerce.yml의 정보를 가져온다고 설정 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133639597-5bc991cc-3f61-45db-89f1-7f4abeb33d4c.png)
  - ### 3️⃣ application.yml에 설정해 둔 정보는 bootstrap이 잘 동작하는지 확인하기 위해 주석 처리 하자 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133639838-792df07c-7d2c-4a1c-a0f2-ff1c6fe181e8.png)
  - ### 4️⃣ 남는 API health_check를 사용하여 정보를 잘 받아오는지 확인 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133639977-d71983fe-5c5d-418d-abb9-45465c7060e0.png) <br><br> 잘 가져온다 👌 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133640507-8971e164-e3ca-4764-8d0a-8e853ef6c9e8.png)

-------
## 🚧 Config 정보를 변경하려면?
  - ### 1️⃣ 설정 변경 후, 서버 재기동 - ❌❌ 불편해서 사용할 의미가 없다 ❌❌
  - ### 2️⃣ Actuator refresh - 🔵 재부팅 하지 않아도 바뀐 값을 얻을 수 있다.
  - ### 3️⃣ Spring Cloud bus 사용 - ⭐ Actuator refersh 보다 효율적으로 가능

------
## user-service에서 Spring actuator 사용해보기
  - ### 1️⃣ Dependency 추가 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133644636-ccab38fe-3d0c-4607-9b7b-478a26f94135.png)
  - ### 2️⃣ actuator의 사용할 기능 추가 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133644807-a3371d66-5661-46b9-ac0b-f66ab4f17d12.png)
  - ### 3️⃣ WebSecurity에서 actuator request를 허용해준다 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133644922-2c89db9a-319a-4ada-a229-b464ce13d710.png)
  - ### (GET) health : actuator 서비스 상태 확인 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133645195-56674ff3-d9a1-4fae-9e1f-490562790806.png)
  - ### (GET) bean : user-service에 등록되어 있는 bean 확인 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133645450-8f160896-5eca-40b5-a2b2-f91fdda0126a.png)
  - ### (POST) refresh : ecommerce 변경 후, 재기동 없이 refresh로 바로 적용 가능 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133645741-9799bad4-6569-441a-87a9-b9b31b278c53.png) 👌 token.secret을 변경하고 refresh하면 정상적으로 변경되어 있다.







