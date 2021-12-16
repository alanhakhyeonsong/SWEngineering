# SWEngineering

## 소프트웨어공학(02) 팀프로젝트 6조

### 구성원

|                          조장                          |                     팀원                      |                      팀원                       |                       팀원                       |
| :----------------------------------------------------: | :-------------------------------------------: | :---------------------------------------------: | :----------------------------------------------: |
| 20174329 [송학현](https://github.com/alanhakhyeonsong) | 20174255 [김주영](https://github.com/KimJooY) | 20174308 [신재훈](https://github.com/newjaehun) | 20174281 [정윤찬](https://github.com/dbscks2140) |
|        전체 프로젝트 설계, 관리 및 BackEnd 구현        |             Android 설계 및 구현              |                  Android 구현                   |                   Android 구현                   |

### Used Stack

- Client: Android
- BackEnd: Spring Boot, Spring Data JPA, H2 Database(test), MySQL(배포용), Docker

Spring framework로 만든 RESTful API 서버와 이를 이용한 Android 앱의 구조입니다.

![system structure](https://user-images.githubusercontent.com/60968342/143846151-31c9fe60-f6ed-4d69-b1b9-63e60bda0099.png)

### 구현한 기능 설명

- 회원 가입(필수 요구 기능)

|                                                                회원가입 화면                                                                |                                                                회원가입 성공                                                                |
| :-----------------------------------------------------------------------------------------------------------------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/60968342/146349130-ce72e37e-d5e3-4e76-97fb-8058ba8356aa.png" width="200" height="400"/> | <img src="https://user-images.githubusercontent.com/60968342/146349171-6f9a80f8-a774-4f7b-a121-fe82b247b244.png" width="200" height="400"/> |

> - 초기 화면(메인화면)에서 로그인 버튼 클릭 후 나타나는 로그인 화면에서 회원 가입 버튼을 클릭하면 회원 가입 화면이 나타납니다.
> - 회원 가입 화면에서 양식에 맞게 입력하여 회원가입 버튼 클릭 시 정상적으로 회원이 등록됩니다.
> - 입력값 검사는 1차적으로 클라이언트에서의 정규표현식 검증과 2차적으로 서버에서의 `@Valid` 검증을 통해 구현하였으며 정상 응답, 에러 응답에 대해 각각의 Http Status Code로 Response 클라이언트에 반환하였습니다.
> - 각 입력 값들은 Dto 객체의 속성으로 저장되고 이 후 JSON으로 변환시켜 서버로 전송합니다. 서버에서 반환되는 결과는 마찬가지로 JSON 형식으로 돌아오게 됩니다.
> - 회원가입 및 로그인의 성공/실패 여부는 서버에서 응답 결과로 넘어온 Http Status Code를 통해 결정됩니다.

- 로그인(필수 요구 기능)

|                                                                 로그인 화면                                                                 |                                                                 로그인 성공                                                                 |
| :-----------------------------------------------------------------------------------------------------------------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/60968342/146349082-51ed59ef-5083-4432-9db4-69ca4e024400.png" width="200" height="400"/> | <img src="https://user-images.githubusercontent.com/60968342/146349198-e61a9991-b939-428c-bb02-101f1e2a9814.png" width="200" height="400"/> |

|                                                           로그인 성공 후 메인화면                                                           |
| :-----------------------------------------------------------------------------------------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/60968342/146349221-b2050c02-4b4d-4bb4-97f5-4a7d960683a1.png" width="200" height="400"/> |

> - 로그인 화면에서 email과 password를 입력하여 로그인을 하면 등록된 회원으로 로그인이 되고 메인화면으로 넘어가게 됩니다.
> - 그 결과로 클라이언트가 정상 응답(2xx)을 받게 되면 기존 메인화면의 좌측 상단에 위치한 로그인 버튼이 로그아웃으로 바뀌게 됩니다.
> - (초기 프로젝트 계획서에 있었던 "로그인 된 회원만 충전소 이용 후기 작성 기능"은 구현하지 않았습니다. 추 후 Http Cookie, Session 정보와 JPA의 테이블 연관관계 외래핑 매핑을 활용하여 구현할 예정입니다.)

- Google Maps를 이용한 메인 화면에서 전체 충전소 위치를 마커로 불러오기 및 충전소 정보 미리보기(추가 기능)

|                                                            메인 화면(초기 상태)                                                             |                                            마커 클릭 시 cardview로 불러오는 충전소 정보 미리보기                                            |
| :-----------------------------------------------------------------------------------------------------------------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/60968342/146352051-c1670d56-ceb2-48a6-b0e5-a9848d3f54ee.png" width="200" height="400"/> | <img src="https://user-images.githubusercontent.com/60968342/146354960-c43c441a-077a-458d-b8e9-0d162433ebce.png" width="200" height="400"/> |

> - 앱 실행 시 초기 화면은 Google Maps와 DB에 저장된 충전소들 각각의 좌표가 다중 마커로 나타납니다. 이 기능은 RESTful API 서버로 구현한 충전소 전체 조회 API를 사용하여 전체 충전소 정보를 불러와 마커로 표시하였습니다.
> - 메인 화면에 올라온 마커 하나를 클릭하면 해당 충전소의 미리보기 정보가 다음과 같이 Card View로 화면 하단에 나타납니다. 충전소마다 고유한 id값(Primary Key)이 지정되어 DB에 저장되어 있기 때문에 해당 id값을 URL query에 넣어 서버에 전송하면 충전소 단일 조회 API를 호출하여 만든 기능입니다.
> - 이 후 cardview를 클릭하면 충전소 상세정보 페이지로 넘어가 해당 충전소의 상세 정보가 나타납니다.

- 충전소 상세정보 보기(추가 기능)

|                                                            충전소 상세정보 화면                                                             |
| :-----------------------------------------------------------------------------------------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/60968342/146354944-2031a7c1-06b8-46cf-a85f-e896cd1c0cb6.png" width="200" height="400"/> |

- 도로명 주소를 입력하여 충전소 검색하기(추가 기능)

|                                                                  검색 화면                                                                  |                                                        도로명 주소 입력 후 검색 결과                                                        |
| :-----------------------------------------------------------------------------------------------------------------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/60968342/146349251-b4fad040-12bc-4c74-a945-dfb19fc1199e.png" width="200" height="400"/> | <img src="https://user-images.githubusercontent.com/60968342/146354929-c3d1ec89-25f5-43bd-8d8a-0429997044e8.png" width="200" height="400"/> |

|                                        검색 결과로 나타난 주소 중 하나를 클릭 시 나타나는 결과 화면                                         |
| :-----------------------------------------------------------------------------------------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/60968342/146354960-c43c441a-077a-458d-b8e9-0d162433ebce.png" width="200" height="400"/> |

> - 메인 화면에서 검색 버튼 클릭 시 검색 화면이 나타나고 도로명 주소(단어로도 검색 가능)를 입력하면 다음과 같이 해당 단어를 포함하는 전체 리스트 결과가 Recycler View로써 화면에 나타납니다.
> - 서버에서 구현한 도로명 주소로 충전소 검색 API를 사용한 결과로 화면에 렌더링을 하였습니다.
> - 이 API는 Spring Data JPA의 LIKE Query를 활용하여 만든 기능입니다.
> - 이 후 결과 리스트 중 하나를 클릭 하면 메인 화면으로 넘어가 지도상의 해당 충전소의 마커가 나타나고 하단에는 Card View로 미리보기 결과가 나타나게 됩니다.

### REST API docs

|           기능            | Method |        URL        |                                     Parameter                                      |
| :-----------------------: | :----: | :---------------: | :--------------------------------------------------------------------------------: |
|         회원가입          |  POST  |      /users       | Input Params - email(String), userPass(String), userName(String), phoneNum(String) |
|          로그인           |  POST  |   /users/login    |                   Input Params - email(String), userPass(String)                   |
|     충전소 단일 조회      |  GET   |  /chargers/{id}   |                               Path Param - id(Long)                                |
|     충전소 전체 조회      |  GET   |   /chargers/all   |                                                                                    |
| 도로명 주소로 충전소 검색 |  POST  | /chargers/address |                           Input Params - address(String)                           |

[응답 결과 Http Status Code](https://github.com/chosunMotherBird/SWEngineering/issues/10)

### 전체 프로젝트 설계 구조

![](https://images.velog.io/images/songs4805/post/6208aaa7-0d1f-45e2-8d36-3211069b50f6/dto%20flow.png)

> 프로젝트는 위와 같이 계층을 나누어 구현하였습니다. Client는 Android App에 해당되고, 나머지 계층들은 Spring으로 구현한 RESTful API 서버에 해당됩니다.  
> [DTO, Entity 분리 설계 이유 - 송학현 기술 블로그](https://velog.io/@songs4805/Vue.js-Spring-Boot-todo-app-%EB%A7%8C%EB%93%A4%EA%B8%B02-BackEnd-%EA%B0%9C%EB%B0%9C)

### GitHub 기록

- [commit과 pull request 기록](https://github.com/chosunMotherBird/SWEngineering/pulls?q=is%3Apr+is%3Aclosed)
- [Backend 구현 시 겪었던 issue와 해결 기록](https://github.com/chosunMotherBird/SWEngineering/issues)

## 개발과정에서 Repository를 사용하기 위한 Git/GitHub 설정 Guide

### git config 설정

1. 깃 설치 후 사용환경 설정

```bash
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

2. git clone

![스크린샷 2021-11-29 오후 1 46 20](https://user-images.githubusercontent.com/60968342/143810432-335add9c-efc3-466f-b113-5d5042c241e1.png)

clone 에서 링크 복사하면 됩니다.

```bash
git clone (복사한주소)

// 예시
git clone https://github.com/chosunMotherBird/SWEngineering.git
```

3. git remote 설정

로컬 환경에서 작업할 수 있도록 remote 저장소를 연결

- 원격 저장소 등록

```bash
git remote add (저장소 별칭) (저장소 url)
git remote add hakhyeon https://github.com/chosunMotherBird/SWEngineering.git
```

- git remote 확인 git remote 설정이 제대로 되었는지 확인하는 방법은 아래 명령어 입력

```bash
git remote -v
/*
origin  https://github.com/chosunMotherBird/SWEngineering.git (fetch)
origin  https://github.com/chosunMotherBird/SWEngineering.git (push)
*/
```

### commit & push

1. git 저장소 생성

```bash
git init
```

2. git branch 생성

```bash
git branch 브랜치이름
//예시
git branch hakhyeon
```

3. branch 이동

```bash
git chekcout 이동할브랜치이름
//예시
git checkout hakhyeon
```

4. git add

```bash
git add .
```

5. git commit

```bash
git commit -m "커밋 메시지"
```

6. git push : add된 로컬 파일들을 원격 저장소로 push

```bash
git push (저장소 별칭) (브랜치이름)
//예시
git push origin hakhyeon
```

7. pull : 현재 원격저장소의 커밋을 로컬로 가져옴

```bash
git pull (저장소 별칭) (브랜치이름)
/* 주로 아래 방법 처럼 하면 됩니다. 본인 origin으로 등록된 repository의 master브랜치 에서 pull 땡겨옴 */
git pull origin master
```

8. PR 보내기

push 작업까지 한 뒤 main 또는 master 브런치로 Pull Request를 보내려고 하는데, push까지 완료하면 자동으로 레포지토리 상에 알림이 뜨는데 PR 요청을 보내는 창을 보고 그대로 누르고 PR을 보내면 됩니다.

// 단 요청만 보내고 승인은 제가 하겠습니다.

## 📌 참고

본인 작업은 반드시 본인 branch 에서 작업하고 add, commit, push를 해야 함.

타인의 branch는 절대 손대지 말것!
