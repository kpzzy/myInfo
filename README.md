# Dev life

![image](https://user-images.githubusercontent.com/107670953/194734547-dec6aa4e-ef94-4a9a-80db-5ee83c23f8a1.png)

# 리팩토링 사례

1. 문제( what’s the matter? )

   1일 전까지의 게시물 중 좋아요 개수가 가장 많은 게시물을 조회하는데 기존 게시물 조회보다 더욱 오래 걸림  
   데이터가 많이 요청된 상황을 가정하기 위하여 아틸러리를 통하여 부하테스트를 해 본 결과  
   30초간 초당 50번의 요청을 보냈을 때 200ms 가까이 나타나는 문제 발생

   ![image](https://user-images.githubusercontent.com/107670953/194734586-e845bed7-9fae-4223-b9d7-3f62c16b7501.png)

2. 원인( cause )
   big O 그래프를 생각해 보았을 때 이중반복문은 big O의 n제곱으로 이같은 코드가 문제를 야기했다고 생각

![image](https://user-images.githubusercontent.com/107670953/194734605-76295fd9-a0bc-4e57-b27a-d212be843cd0.png)

3. 해결 ( how to resolve )  
   (https://github.com/QBChaining/QBChaining-BE/issues/331)  
   이같은 문제를 백엔드에서 코드리팩토링 기간 때 수정하기로 결정하였고 service 계층의 이중반복문을 repository 계층에서 먼저 날짜 순서로 정렬하여 가져온 후 좋아요를 확인하는 isLike 또한 반복문에서 if 문을 통하여 개선하였음  
   또한 프론트와 hot blog 부분에 몇개의 게시물을 나타낼지에 대하여의논한 후 그에 맞게 limit: 4 를 주어서 개선하였음

4. 결과 ( result )  
   p99 기준 기존코드(186.8ms) 대비 개선코드(117.9ms) 로써 약 36% 의 개선 효과를 얻을 수 있었음

# 나의 학습 방법

- 문제 ( what’s the matter? )  
  CRUD 기능구현을 완료한 후 EC2 로 배포를 하는데 git commit이 있을때마다 매번 git pull 후 pm2 restart 를 해주는 것이 너무 reosource가 많이 투자되고 있다는 생각이 들었음.

- 기술 선정 이유 ( reason )  
  자동배포라는 기술을 알게 된 후 그에 대한 자료들을 조사해 보았음.  
  많이 사용되는 것들은 Jenkins , Travis, Github actions 가 사용되고 있었음.  
  현재 ec2를 사용하고 있는데 Jenkins를 사용한다면 개별 인스턴스와 복잡한 세팅이 문제가 되었음.  
  Travis 는 만약 private repo에 적용을 하려면 비용적인 측면이 문제가 되었음.  
  따라서 복잡한 셋팅이 없으며 repo에서 바로 적용이 가능한 Github actions를 통하여 CI를 구현하고 AWS Code Deploy를 통하여 CD 기능을 구현 하였음.

- 과정 ( process )  
  CI과정은 금방하였지만 CD기능은 Code Deploy Documents를 참고 하였지만 75번의 시도끝에 성공하였음. 다른 조원에게 설명해주기 위하여 이를 github issue에 정리하여 문서화로 기록하였음.  
  https://github.com/QBChaining/QBChaining-BE/issues/95
