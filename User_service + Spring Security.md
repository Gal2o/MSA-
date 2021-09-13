# 로그인 기능 추가

## AuthenticationFilter.java
  - Spring Security를 이용한 로그인 요청 발생 시, 작업을 처리해 주는 Custom Filter 클래스
  - ![image](https://user-images.githubusercontent.com/35948339/122778597-19add380-d2e8-11eb-9b1c-4c73048b90f2.png)
    - __`UsernamePasswordAuthenticationFilter`__ (org.springframework.security.web.authentication)을 상속 받아서
    - __`UsernamePasswordAuthenticationToken`__ 를 사용하여 ID, PW를 이용하여 토큰을 발생 시킨다.
    - __`getAuthenticationManager`__ 에 만들어진 Token을 전달한 후, ID와 PW를 비교하여 인증 처리를 한다.

## WebSecurity.java
  - ![image](https://user-images.githubusercontent.com/35948339/122779531-e7e93c80-d2e8-11eb-9ba8-cf88a1a4d66d.png)
    - 모든 요청에 대해 permit 하지 않는 필터 설정
    - __`.hasIpAddress`__ 해당 아이피만 접근 허용
    - and() : 작업 추가
    - __`.addFilter(getAuthenticationFilter());`__ getAuthenticationFilter()라는 필터를 적용한다.
-----  
  - ![image](https://user-images.githubusercontent.com/35948339/122780413-ca68a280-d2e9-11eb-8228-7f0062e05fc8.png)
    - __위에 만들어 둔 AuthenticationFilter 상속__ 받아서 __`.addFilter에 return`__
-----  
  - ![image](https://user-images.githubusercontent.com/35948339/122781262-96da4800-d2ea-11eb-886e-510ff0cdd064.png)
  - __인증 처리를 위한 Configure__ 만들기 위하여 AuthenticationManagerBuilder를 상속받은 메소드 사용
-----
  - ![image](https://user-images.githubusercontent.com/35948339/122781483-d3a63f00-d2ea-11eb-8457-c63bd1bf697f.png)
  - __userDetailsService() 가 쿼리문 역할을 한다.__ 
  - `DB에 있는 PW는 인코딩이 되어 있으므로 매개변수로 사용할 PW도 인코딩 하여 비교하게 만든다.`
-----
  ### 에러 발생 잡기
  - ![image](https://user-images.githubusercontent.com/35948339/122782698-e705da00-d2eb-11eb-9f2c-229d2f041586.png)
  - userService 에 userDetailsService 상속시키기.
-----
  - ![image](https://user-images.githubusercontent.com/35948339/122782865-07ce2f80-d2ec-11eb-926d-0ba2169e6970.png)
  - 쿼리 작동 위해 `loadUserByUsername` 메소드 구현 하면 WebSecurity 완성
  - (65 ~ 68) 비교해서 올바른 ID, PW면 그대로 return 해준다.
-----

## API-Gateway Routes 변경
  - ![image](https://user-images.githubusercontent.com/35948339/122783888-ede11c80-d2ec-11eb-92aa-76ce6deaab0e.png)
  - #### `/user-service/(?<segment>.*)` 이러한 형태의 uri 가 들어오면 `/$\{segment} 이러한 경로로 바꿔주겠다는 의미`
  --------
  - ### 각 REST API의 routes 설정
    - ![image](https://user-images.githubusercontent.com/35948339/122784308-53cda400-d2ed-11eb-9dfe-2b552f11b354.png)
    - ### routes를 설정하고 나면 `userController` -> @RequestMapping에 `user-service` 없이 uri 호출 가능하다.
    - ![image](https://user-images.githubusercontent.com/35948339/132234713-d573c5db-99f8-4c0a-89d6-a0f79af0ac95.png)
    - ### /user-service/ 필요없이 127.0.0.1:`user-service port`/`routes 설정한 API 주소` 처럼 바로 호출이 가능하다
-------
## User-service 로그인 처리 과정 (WebSecurity - AuthenticationFilter class)
  - ### 1️⃣ AuthenticationFilter 클래스가 로그인 시도 시, 가장 먼저 호출된다 <br><br> (user-service에 login API가 없어도 spring security가 알아서 실행)
  - ### 2️⃣ 로그인 시도 때, 입력 받은 email, password (InputStream) -> RequestLogin 객체로 매핑 <br><br> <img src="https://user-images.githubusercontent.com/35948339/133109339-002091bb-1cb2-4914-8f05-e493b4aeac7a.png" width=600>

  - ### 3️⃣ loadUserByUsername에서 email, password 을 확인해서 가져오면
  - ### 4️⃣ successfulAuthentication에서 `Jwt builder`로 JWT토큰 생성 후 클라이언트에게 반납
-------
## User-service 로그인 성공 처리 (AuthenticationFilter class)
  - ### 1️⃣ 인증된 객체 반환 <br><br> <img src="https://user-images.githubusercontent.com/35948339/133106657-3332e128-ba6e-4ca2-b694-1e2d67add519.png" width=600>
  - ### 2️⃣ 인증된 객체를 받아서 JWT token 생성하여 <br><br> responseHeader에 token값과 user_id를 넣어서 클라이언트에 반환 <br><br> <img src="https://user-images.githubusercontent.com/35948339/133108026-a360b3d3-9cc0-4bc3-b16c-d3d83a3761f7.png" width=700>
  - ### 💾 클라이언트에서 받는 값 <br><br> <img src="https://user-images.githubusercontent.com/35948339/133102446-a5dead85-6650-4c5f-a82c-ba2a6b3c18df.png" width=600>
-------
## API Gateway 에서 Spring Security와 JWT Token 사용하도록 기능 추가

