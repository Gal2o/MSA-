## Spring Config
  - ### yml 파일 등 각 서비스의 설정을 한 곳에 모아서 관리할 수 있게 하자

## Spring Config 프로젝트 생성
  - ### Config Server 체크하고 생성 <br><br> <img src="https://user-images.githubusercontent.com/35948339/133637665-e3a77359-a70e-4934-84ed-1e3e12cd6656.png" width=700>
  - ### ⭐ 서비스가 있는 프로젝트 폴더가 아닌 새로운 폴더에 폴더 생성 후, yml 파일 생성 ➡ git init / add <br><br> ❌ 서비스가 있는 폴더에 생성하면, 마지막 커밋으로 롤백되는 현상 발생 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133638233-047a00e4-3400-4103-b475-83f5715306ef.png)
  - ### Dependency 추가 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133638436-d17d91b9-3dd2-4399-bb81-8f84a6d44e03.png)

## ⚙ user-service에서 Test ‼
  - ### Dependency 추가 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133638436-d17d91b9-3dd2-4399-bb81-8f84a6d44e03.png)
  - ### resource에 bootstrap.yml을 만들어서 위에서 만든 ecommerce.yml의 정보를 가져온다고 설정 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133639597-5bc991cc-3f61-45db-89f1-7f4abeb33d4c.png)
  - ### application.yml에 설정해 둔 정보는 bootstrap이 잘 동작하는지 확인하기 위해 주석 처리 하자 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133639838-792df07c-7d2c-4a1c-a0f2-ff1c6fe181e8.png)
  - ### 남는 API health_check를 사용하여 정보를 잘 받아오는지 확인 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133639977-d71983fe-5c5d-418d-abb9-45465c7060e0.png) <br><br> 잘 가져온다 👌 <br><br> ![image](https://user-images.githubusercontent.com/35948339/133640507-8971e164-e3ca-4764-8d0a-8e853ef6c9e8.png)




