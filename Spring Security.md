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
  - `/user-service/(?<segment>.*)` 이러한 형태의 uri 가 들어오면 `/$\{segment} 이러한 경로로 바꿔주겠다는 의미`
  - 각 REST API의 routes 설정
    - ![image](https://user-images.githubusercontent.com/35948339/122784308-53cda400-d2ed-11eb-9dfe-2b552f11b354.png)
    - routes를 설정하고 나면 `userController` -> @RequestMapping에 `user-service` 없이 uri 호출 가능하다.

