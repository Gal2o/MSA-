![image](https://user-images.githubusercontent.com/35948339/120202766-f13f3600-c261-11eb-843e-44967f184eed.png)
  - register-with-eureka는 eureka의 registry에 등록할지 여부이고 (11)
  - fetch-registry는 registry에 있는 정보를 가져올지 여부이다.(12~13)  
------

![image](https://user-images.githubusercontent.com/35948339/120201351-4e39ec80-c260-11eb-9737-9588f9e0edf7.png)
  - 서비스를 복사할 수 있다. 하지만 그대로 실행하게 되면 port가 겹치므로
  - ![image](https://user-images.githubusercontent.com/35948339/120201479-71fd3280-c260-11eb-9a0f-d44349b7ed26.png)
    - Vmoption 에서 Dserver.port=9002 로 port를 바꿔주고 실행한다.
------

- CLI에서 -Dserver.port=?? 로도 서비스를 실행 할 수 있다.
------

![image](https://user-images.githubusercontent.com/35948339/120205978-927bbb80-c265-11eb-9383-90eb8e73f631.png)
  - random port로 시작할 수 있다.
  - instance 옵션으로 eureka에 보여지는 instance들의 이름을 설정 할 수 있다. (9-10)

