# API 가이드
<!--![sg-core api](http://)-->


## Overview
**SG-Core API**는 기존 자주 이용하는 API 리소스의 집합이다. 특별한 말이 없는 한 모든 API의 접두어는 **sg**가 들어간다. 앞으로 API 리소스 집합을 API Group 즉 **apig**라 하겠다.
해당 기능은 단순 api 형태이기 때문에 플랫폼에 종속적이지 않으며, 팀슬로그업 자체에서 해당 플랫폼을 각 프로토콜에 맞춰 개발해 놓고 레이어를 나눠 생산성을 높인다.

개발언어는 `Node.js`의 `Express framework`가 이용되고 있으며, RDBMS는 `Sequelize` ORM을 이용하고 표준 SQL문만 이용했기 때문에 DBMS에 종속적이지 않다. 

테스트를 위해 모든 리소스는 <http://yourdomain.com/api/tester> 에서 수행이 가능하다. 이때 tester앞에 있는 api 접두어는 프로젝트마다 변경이 가능하다.

해당 문서는 플랫폼에 종속적이지 않은 클라이언트용 api 가이드이다. 추가적으로 [**프로젝트가이드라인**](http://naver.com) 문서와 각 플랫폼별 [**iOS가이드라인**](), [**Android가이드라인**](), [**Web가이드라인**]()이 있다. 또한 서버의 app단 개발을 위해 [**Server가이드라인**]()이 존재한다. 


## 리소스 표기 형태
주요 apig로는 **accounts, etc, socials** 등이 있으며, 특별한 명사가 아니라면 모두 **복수형**으로 표기된다. 또한 해당 apig는 최종 리소스이름 앞단에 존재한다. 

예를 들면 accounts apig에 속해있는 users라는 리소스는 다음과 같은 리소스명을 형성한다. 

<http://yourdomain.com/api/accounts/users>

이때 users라는 리소스명은 리소스 단위 즉 **resource unit**라 하겠다.


## 구조
Node.js의 express apig는 `repository/core/server/apis` 위치에 존재한다. 따라서 코드 수정 혹은 추가는 해당 폴더 아래에서 이루어 진다. 해당 경로를 보면 `core` 가 존재하는데, 이는 core 외 추가적으로 app단 고유의 api를 추가할 수 있다는 의미가 된다.

크게 나누어 보면 `core`, `bridge`, `app`으로 이루어져 있다. 그중 현 문서에서 설명할 부분은 core에 해당하는 내용이다. app의 경우 core와 유사한 형태를 갖고 있으며, 이 두개의 api는 app이 더 높은 우선순위를 갖고 bridge에 의해서 mix된다. 즉 중복된 resource unit이 app과, core에 모두 존재한다면 app에 있는 resource unit이 작동하게 된다.

리소스유닛 명은 대부분 일반 명사의 복수형 형태로 되어 있으며 복합명사로 이루어질 경우 하이픈(-)을 이용한다. 만약 리소스유닛이 복수형이 아니라면 모든 get요청은 단일 오브젝트를 반환한다는 의미이다. 복수형일 경우의 CRUD 예시는 다음과 같다.

1. GET <http://yourdomain.com/api/accounts/users> 복수형 배열 반환.
2. GET <http://yourdomain.com/api/accounts/users/:id:> 해당 아이디에 해당하는 단일 객체 반환.
3. POST <http://yourdomain.com/api/accounts/users> 유저 생성.
4. PUT <http://yourdomain.com/api/accounts/users/:id:> 해당 아이디에 해당하는 유저 수정.
5. DELETE <http://yourdomain.com/api/accounts/users/:id:> 해당 아이디에 해당하는 유저 제거.

단수형일 경우의 CRUD 예시는 다음과 같다.

1. GET <http://yourdomain.com/api/socials/bbs> 게시판을 로드하기 위한 여러 데이터를 포함하는 객체 반환. 예를들면 현재 게시판의 Board값, Category, Article list 등.



## 활용 가이드
### Accounts
해당 apig는 유저계정관련 대부분의 요소를 제공한다. 특히 가입, 로그인, 정보수정 등의 주요 기능을 수행한다. 회원가입 즉 `Users` 유닛의 `post`메소드부터 차례대로 알아보도록 하겠다.
먼저 회원가입은 크게 3가지 형태로 나뉜다. apig앞의 prefix는 `api`를 쓰도록 하겠다. 즉 앞으로 모든 도메인의 형태는 <http://yourdomain.com/api/apig/unit> 형태가 되며 단순히 줄여서 `/api/apig/unit`의 형태를 이용하도록 하겠다.

1. 이메일 회원가입
2. 전화번호 회원가입
3. OAuth 회원가입

먼저 이메일 회원가입부터 알아보자.

#### 이메일 회원가입
이메일 회원가입은 인증관련 두가지 옵션이 있다. 즉 회원가입시 이메일인증이 필요한 경우와 그렇지 않은 경우이다. 인증의 필요 유무는 `repository/app/server/metadata/standards.js` 파일의 `flag.isAutoVerifiedEmail` 플래그를 토글하여 조절할 수 있다.








