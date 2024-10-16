
<a href="https://github.com/KIMGUUNI/A_EyeF/">
        <img src="https://github.com/user-attachments/assets/cff814e1-a8ba-47e9-828c-72ded458920d" width = "80%">
 </a>
 
 #  NORMAL INDEX , REVERSE INDEX INSERT 속도 차 비교
> Jmeter를 활용한 NORMAL INDEX , REVERSE INDEX INSERT 속도 차 비교
> </br>
> </br>
> S/W : CUBRID, Jmeter

<details>

</br>

# right growing index란?

</br>

- 순차적으로 증가하는 값이 들어가는 컬럼을 인덱스로 만들면 입력되는 값이 순차적으로 증가하기 때문에 B*tree 인덱스에서 가장 오른쪽 리프 블록에만 데이터가 쌓이는 현상

</br>

- 이러한 인덱스는 동시 index가 심할 때 인덱스 블록 경합을 일으켜 초당 트랜잭션 처리량을 감소시킨다.
</br>

### 이러한 해결 방안으로 reverse index를 이용할 수 있다.
</br>

# reverse index란?

</br>

- 말 그대로 입력된 키 값을 거꾸로 변환해서 저장하는 인덱스

</br>
  
- 순차적으로 입력되는 값을 거꾸로 변환해서 저장하면 마치 데이터가 랜덤값처럼 변환이 되기 때문에 데이터를 고르게 저장할 수 있다. 따라서 리프 블록 맨 우측에만 집중되는 트랜잭션을 리프 블록 전체에 고르게 분산시키는 효과를 얻을 수 있다.

</br>

- Reverse Index에 대해서는 범위 조건(BETWEEN이나 >, <, >=, <= 같은)이 Access 조건(즉, 값을 실제로 찾을 수 있는 조건)으로 사용될 수 없다.

</br>

# 테스트 목적

- Right Growing Index가 초당 트랜잭션 처리량에 미치는 영향을 분석
- Reverse Index를 사용했을 때 데이터 삽입 성능이 어떻게 향상되는지 확인

</br>

# NORMAL INDEX 테스트 진행

**Number of Threads : 20**

**Ramp-up period : 1**

**Loop Count : 10000**

</br>

**테스트 쿼리**

<code>

```python
CREATE TABLE orders (
order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,  
order_id INT
);

CREATE INDEX idx_order_date ON orders(order_date);

INSERT INTO orders (order_date) VALUES (NOW());

```
</code>


소요 시간 : 05분04초

처리량 : 657.3/sec

<img src="https://github.com/user-attachments/assets/6a410a8a-1198-428a-9cf1-43785d32f749"/>

<img src="https://github.com/user-attachments/assets/972f2475-e1bc-4393-842d-33551cd730be"/>

# REVERSE INDEX 테스트 진행

**Number of Threads : 20**

**Ramp-up period : 1**

**Loop Count : 10000**

</br>

**테스트 쿼리**

<code>

```python
CREATE TABLE orders (
order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
order_id INT
);

CREATE INDEX idx_order_date ON orders(REVERSE(order_date));

INSERT INTO orders (order_date) VALUES (NOW());

```
</code>
<img src="https://github.com/user-attachments/assets/197b2fbf-1c7b-47fe-8798-0782c23968dd"/>

**소요 시간 : 04분43초**

**처리량 : 706.2/sec**

<img src="https://github.com/user-attachments/assets/75527d97-5a23-4ccb-958d-e5f427266be6"/>

#### NORMAL INDEX --> 05분04초 소요

#### REVERSE INDEX --> 04분43초 소요 ( 약 21초 빠르게 수행됨 , 6.91%향상 )

</br>

# 결론

**Right Growing Index**의 경우, 순차적으로 증가하는 값들이 가장 오른쪽 리프 블록에 집중되면서 인덱스 블록 경합이 심해지고 초당 트랜잭션 처리량이 감소하는 문제가 발생함. 이를 해결하기 위해 **Reverse Index**를 사용한 결과, 데이터 삽입 성능이 향상된 것을 확인할 수 있었음.

따라서, 순차적으로 증가하는 값을 인덱스하는 상황에서는 **Reverse Index**를 사용하는 것이 성능을 개선하는 데 유리할 수 있음.



</br>
</br>


</details>

</br>
</br>
<a href="https://github.com/KIMGUUNI/A_EyeF/">
        <img src="https://github.com/user-attachments/assets/b1dd13cd-eaf3-4639-86d7-9f0176001c80" width = "80%">
 </a>
 </br>
 </br>
 
 #  DB 서버 이중화
> DB 서버 이중화
> </br>
> </br>
> S/W : CUBRID

<details>

</br>

# 서버 이중화란?
- 서버와 네트워크, 프로그램 등의 정보 시스템이 지속적으로 정상 운영이 가능한 고가용성(HA, High Availability) 서버로 구성하는 것
- shared-nothing 구조 
</br>

# shared-nothing 구조란?
<img src="https://github.com/user-attachments/assets/a5cf8cbd-8115-4d29-aaba-ef755c8fed06"/>

- 복수의 서버가 독립적으로 동작하는 것

## 장점
- 확장성: 각 노드가 독립적이므로 새로운 노드를 추가하여 시스템 성능을 쉽게 확장할 수 있다.
- 고가용성: 한 노드가 실패하더라도 다른 노드에 영향을 미치지 않으므로 시스템의 가용성이 높아진다.
- 병렬 처리 효율성: 노드 간에 경합이 없으므로 병렬 처리가 효율적이다.
- 데이터 지역성: 각 노드가 독립적으로 데이터를 처리하기 때문에 데이터 지역성이 좋아, 데이터 접근 시간이 줄어든다.

## 단점
- 데이터 일관성: 각 노드가 독립적이므로 데이터 일관성을 유지하기가 어렵다.
- 복잡한 데이터 분배: 데이터를 각 노드에 분배하는 과정이 복잡할 수 있다.
</br>

# 이중화 순서

</br>

1. 마스터 노드와 슬레이브 노드가 동일하게 cubrid.conf, cubrid_ha.conf, databases.txt설정
2. 슬레이브 노드에 동일한 이름의 데이터베이스 디렉토리 생성
3. 마스터 노드 서비스 중지 후, 마스터 노드 백업
4. scp로 전송
5. 슬레이브 노드에서 데이터베이스 복구 (볼륨 경로가 반드시 같아야함)
6. 마스터, 슬레이브 노드 시작
7. ha 정상 작동 확인


# cubrid.conf (M,S 동일)

<img src="https://github.com/user-attachments/assets/fa51ba9e-de83-4987-b815-06f4e992badc"/>

</br>

# cubrid_ha.conf (M,S 동일)

<img src="https://github.com/user-attachments/assets/7888d9c1-d2aa-4377-a4cb-6b0fd14f2760"/>

</br>

# databases.txt (M,S동일)

<img src="https://github.com/user-attachments/assets/c67d2a09-f50a-4b33-bcd4-44568bac24b2"/>

</br>

# Master 노드의 DB 백업

<img src="https://github.com/user-attachments/assets/4a44cecf-c4d4-4119-9c3a-6a53bfb2c80a"/>

</br>

# scp 전송 

<img src="https://github.com/user-attachments/assets/e3488271-d9f6-4dfe-8b7f-83540234fc6e"/>

</br>

# Slave 노드에 복구

<img src="https://github.com/user-attachments/assets/b78b953c-2cf6-4450-9d33-0362940c0b77"/>
<img src="https://github.com/user-attachments/assets/0f43e362-6216-4553-81e6-4653e53fdc3b"/>

</br>

# 마스터, 슬레이브 시작 후 ha 정상 작동 확인

<img src="https://github.com/user-attachments/assets/d8514200-c3fc-4107-a4ef-28d9e34b941c"/>

</br>

# Master 노드에서 테이블 생성 후 데이터 insert

<img src="https://github.com/user-attachments/assets/4714e586-ffc8-459c-80df-5aa1fffe365a"/>

</br>


# Slave 노드에서 테이블 조회

<img src="https://github.com/user-attachments/assets/057b42d0-c646-4ff1-9244-a02b89349bef"/>

</br>

# Slave 노드 추가 전에 Master 노드에서 가지고 있던 데이터 복구 확인

<img src="https://github.com/user-attachments/assets/aea34b19-c836-4a03-a837-7ea680b4748b"/>

</br>

# applyinfo  유틸리티로 진행상황 확인

<img src="https://github.com/user-attachments/assets/f18f7a3c-2b61-4cab-bcc6-95d66852fce9"/>

</br>



# 느낀점

DB 서버 이중화에서 가장 핵심적인 요소는 마스터와 슬레이브 간의 데이터 동기화와 장애 발생 시의 failover가 잘 일어나는지가 가장 중요한 핵심이라고 생각한다. 특히, 데이터 동기화가 원활하게 이루어져야만 시스템의 일관성을 유지할 수 있으며, 마스터 노드에 문제가 생겼을 때 슬레이브 노드가 즉각적으로 그 역할을 대체하여 서비스를 지속하는 것이 매우 중요하다.

동기화 과정에서는 모든 데이터가 실시간으로 정확하게 슬레이브로 복제되어야 하며, 지연이나 손실이 발생하지 않도록 신경 써야 한다. 이중화 시스템이 제대로 구축되지 않으면, 마스터 노드에 장애가 발생했을 때 데이터 손실이 생기거나 시스템 가동이 멈추는 상황이 발생할 수 있기 때문이다.

따라서 이중화 구축 시에는 마스터-슬레이브 간의 실시간 데이터 동기화가 잘 이루어지고 있는지 지속적으로 모니터링하고, 장애 발생 시 슬레이브가 자동으로 마스터 역할을 수행하며 서비스 중단을 최소화할 수 있도록 failover 메커니즘을 철저히 검토해야 한다. 이 모든 과정을 통해 비즈니스 연속성과 데이터의 무결성을 보장하는 것이 DB 서버 이중화의 궁극적인 목표라 할 수 있다.





</details>



</details>

</details>

</br>
</br>
<a href="https://github.com/KIMGUUNI/A_EyeF/">
        <img src="https://github.com/Limmaji/hyeji/assets/118683437/dd5ab833-a1d1-4136-8821-d3df838b5cd2" width = "80%">
 </a>
 
 # 🖥 A-EYE 🖥

> #### 🏆 수상 : 우수상
> 
> ### Team name : A_eye
> 
> #### AI 기반 CCTV 분석을 통한 맞춤 광고 재공 서비스
> 
> #### 역할 : BackEnd 
>
> #### 기간 : 2024년 02월 01일 ~ 02월 27일 (27일 소요)


<details>

## 사용 기술
#### `Back-end`
  - Spring
  - JQuery
  - JavaScript
  - Oracle
  - AWS
  - Spring Security

#### `Front-end`
  - React
  - HTML
  - CSS
  - JavaScript
  - mui

</br>

</br>

<img src = "https://github.com/KIMGUUNI/A_EyeF/assets/142488051/d002a731-40eb-4bec-9337-c2773b836a6a" width="100%" height="100%">

</br>

### 결제페이지

![image](https://github.com/KIMGUUNI/A_EyeF/assets/118683437/8547053a-4efc-4f9e-9e1f-a770c049d704)


  - 신청한 광고의 현재 재생 횟수에 따른 결제 금액이 보여진다.
  - 결제를 원하는 광고를 선택하여 결제 가능하다.
  - 간편 결제가 가능하다. (카카오페이,삼성페이 등)

</br>

### JWT
![image](https://github.com/KIMGUUNI/A_EyeF/assets/118683437/236167f1-2452-4775-bf4b-b063490f5811)
- JWT를 통해 사용자 인증을 하고 권한을 부여한다.
- access token과 refresh token을 사용하였으며, RTR 기법을 적용하였다.

</br>

### 사용자 정보 암호화
![image](https://github.com/jaewon07/jowonjae/assets/133577206/0d7ddcde-0888-4f2a-a92b-468fab185ab1)

- 블로피시 기반 Bcrypt 암호화를 적용하였다.
- Bcrypt는 단방향 해시 알고리즘이기 때문에 복호화가 불가능했고, 때문에 matches 함수를 이용해 주어진 입력값과 저장된 해시를 비교하여 인증을 수행하였다.

</br>


## 마이페이지

![image](https://github.com/KIMGUUNI/A_EyeF/assets/118683437/20c8f44e-0cf9-45d6-a8dd-5c8a1dc61890)
  
  - 사용자가 신청한 광고 리스트를 볼 수 있다.
  - 사용자는 문의글 작성이 가능하다.
  - 관리자는 모든 문의 내용 열람, 답변 및 삭제가 가능하다.

</br>

## 광고 신청
![image](https://github.com/KIMGUUNI/A_EyeF/assets/118683437/a45c5e19-6983-4967-8d83-52d0f6b00479)



  - 사용자는 원하는 타겟의 성별과 연령을 선택한다.
  - 선택 후 광고 파일을 업로드한다.
  - 광고 신청 버튼을 클릭하면 S3에 정보가 저장된다.

</br>

## 트러블 슈팅
### 토큰 관리 문제

<details>
  
#### 문제 상황
  - 리프레시 토큰을 DB에 저장 시, 만료된 토큰이 DB에 계속 쌓이는 문제 발생

#### 해결 시도
  - DB 스케줄러를 이용하여 주기적으로 삭제하려 했으나, 권한 부여의 어려움 발생(서버의 권한을 받을 수 없었음)

#### 해결 방안
  - 스프링 스케줄러를 활용, 만료된 토큰을 삭제하여 해결
  - 개인적으로는 Redis와 같은 인메모리 데이터베이스를 사용하여 만료된 토큰을 자동으로 관리할 수 있었다면 더 효율적이었을 텐데, 이를 활용하지 못한 점이 아쉽다. Redis는 만료된 데이터를 자동으로 제거하는 기능(TTL)        을 기본적으로 제공하므로, 이러한 상황에서 훨씬 더 간편하고 효과적인 선택이 될 수 있기 때문이다.

~~~java

@Scheduled(fixeRate=604800000)

~~~


</details>

</br>

### CORS

<details>
  
#### 문제 상황
  - 웹 브라우저에서 보안상의 이유로 동일한 출처(origin)가 아닌 서버로부터 리소스를 요청할 때 발생하는 정책으로 React와 Spring boot가 API 통신을 할때 차단당하 는 상황 발생

#### 해결 방안
![image](https://github.com/jaewon07/jowonjae/assets/133577206/9155af82-d298-4a23-822c-73492793f3f9)

  - Spring Boot 애플리케이션에서 CORS 구성을 추가하고, 클라이언트 요청을 허용할 출처를 설정하여 해결



</details>


</details>

</br>

<a href="https://github.com/2023-SMHRD-IS-CLOUD-1/StrongRepo">
        <img src="https://github.com/Limmaji/hyeji/assets/118683437/36549b89-cf1d-493c-95db-2c7824672f35" width = "80%">
        
         
 </a>
 </br>
 </br>
 
 # 💊 Drug is Death 💊

> ### Team name : 강력 1팀
>
> #### openCV기반 10대 청소년 대상 마약 예방 교육 자료 및 체험 서비스
>
> #### 역할 : 대시보드 구현, 로그인 및 회원가입 기능 구현
>
> #### 기간 : 2023년 11월 22일 ~ 12월 07일 (21일 소요)
> 

<details>

## 1. 사용 기술
#### `Back-end`
  - Java 8
  - JQuery
  - JavaScript
  - Oracle
  - Chart.js
  - BootStrap
  - JSP

#### `Front-end`
  - HTML
  - CSS
  - JavaScript

</br>
        
<br>
        
<image src="https://github.com/jaewon07/jowonjae/assets/133577206/f038ced1-e7e8-4dcc-8580-4e2983c11a3b">

<br>

### 로그인/회원가입/회원수정/회원탈퇴
![제목을 입력해주세요_-001](https://github.com/2023-SMHRD-IS-CLOUD-1/StrongRepo/assets/142488105/1581b26c-2a14-4444-a211-cac71a304e8f)

회원가입
- 항목 중 하나라도 쓰지 않은 항목이 있다면 모든 정보를 입력하라는 문구가 표시된다
- 이메일 입력 시 사용 가능한 이메일인지 아닌지에 대한 문구가 표시된다

로그인/회원정보수정/회원탈퇴
- 아이디 또는 비밀번호가 맞지 않을 경우 아이디나 비밀번호가 잘못되었다는 문구가 표시 된다


<br>

### 게시판
![제목을 입력해주세요_-001 (1)](https://github.com/2023-SMHRD-IS-CLOUD-1/StrongRepo/assets/142488105/c09a3e48-7c8b-46d5-8774-cf5f09bba857)

게시글
- 로그인 되어 있지 않은 경우 : 등록 버튼이 비활성화 되어있고 글 내용 칸에 로그인 시 이용 가능하다는 문구가 표시된다. 
- 로그인 되어 있는 경우 : 등록 버튼이 활성화 되어있고 글 내용 칸에 내용 입력이라는 문구가 표시되며 글 제목 또는 내용이 없으면 모든 항목을 입력하라는 문구가 표시 된다
- 사용자 본인의 글인 경우 목록과 수정 버튼이 있다
- 사용자의 글이 아닌 경우 목록 버튼이 있다
- 수정하는 경우 제목과 내용 수정이 가능하며 하단에 수정, 삭제, 취소 버튼이 있다.

댓글
- 로그인 되어 있지 않은 경우 : 댓글 칸에 로그인 시 이용 가능하다는 문구가 표시되며 등록 버튼이 비활성화 되어 있다 
- 로그인 되어 있는 경우 : 댓글 칸에 댓글 입력이라는 문구가 표시되며 등록 버튼이 활성화되어 있다. 자신의 댓글이 등록된 경우 우측에 삭제 버튼이 생긴다

<br>

### 중독 치료 센터
![제목을 입력해주세요_-002 (2)](https://github.com/jaewon07/jowonjae/assets/133577206/732dc7aa-2fbf-4c4b-8dc7-7f224f1f9d42)

- 현재 접속한 위치와 주변에 있는 마약 중독 치료센터 위치를 지도에 표시한다
- 지도를 축소해서 전국에 있는 중독 치료센터 위치를 볼 수있다
- 마커를 클릭하면 중독 치료 센터의 이름과 주소를 볼 수있다

<br>

#### map.jsp 세부코드
<details>
        
```java
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>여러개 마커 표시하기</title>
<%@ page language="java" contentType="text/html; charset=UTF-8"
   pageEncoding="UTF-8"%>
<script src="assets/js/jquery-3.3.1.min.js"></script>
</head>
<body>
   <div id="map" style="width: 100%; height: 1000px;"></div>

   <script type="text/javascript"
      src="//dapi.kakao.com/v2/maps/sdk.js?appkey=0d0ff50962bd050bd1f6311e3d126443&libraries=services"></script>
   <script>
      var globalResult;
      var positions = [];
      var map;
      var marker;
      var markers = [];

      $(function() {
         $.ajax({
            url : "Map.do",
            method : 'GET',
            dataType : "json",
            contentType : "application/json; charset=utf-8",
            success : function(result) {
               globalResult = result;
               // 마커를 생성하고 표시하는 함수 호출(콜백함수) 
               displayMarkers();
            },
            error : function(error) {
               console.log("실패");
            }
         });

         // 마커를 생성하고 표시하는 함수
         function displayMarkers() {
            var mapContainer = document.getElementById('map');
            var mapOption = {
               center : new kakao.maps.LatLng(35.1305421, 126.858356),
               level : 5
            };
            map = new kakao.maps.Map(mapContainer, mapOption);

            for (var i = 0; i < globalResult.length; i++) {
               positions.push({
                  title : globalResult[i].M_TREAT,
                  latlng : new kakao.maps.LatLng(
                        globalResult[i].M_ADDRESS_Y,
                        globalResult[i].M_ADDRESS_X)
               });
            }

            // 마커 이미지의 이미지 주소
            var imageSrc = "https://t1.daumcdn.net/localimg/localimages/07/mapapidoc/markerStar.png";


            for (var i = 0; i < positions.length; i++) {
               // 마커 이미지의 이미지 크기
               var imageSize = new kakao.maps.Size(20, 27);

               // 마커 이미지를 생성    
               var markerImage = new kakao.maps.MarkerImage(imageSrc,
                     imageSize);

               // 마커를 생성합니다
               marker = new kakao.maps.Marker({
                  map : map, // 마커를 표시할 지도
                  position : positions[i].latlng, // 마커를 표시할 위치
                  title : positions[i].title, // 마커의 타이틀, 마커에 마우스를 올리면 타이틀이 표시
                  image : markerImage
                  // 마커 이미지
               });
               markers.push(marker); // 수정: markers 배열에 마커 추가
            }

            // 현재 위치 가져오기
            if (navigator.geolocation) {
               navigator.geolocation
                     .getCurrentPosition(function(position) {
                        var lat = position.coords.latitude; // 위도
                        var lon = position.coords.longitude; // 경도

                        var locPosition = new kakao.maps.LatLng(lat,
                              lon);
                        var message = '<div style="padding:5px;">내 위치</div>';

                        // 마커와 인포윈도우를 표시
                        displayMarker(locPosition, message);
                        // 병원 인포인도우를 표시
                        HinfoWindow();
                     });
            } else {
               // HTML5의 GeoLocation을 사용할 수 없을 때 기본 위치로 설정
               var locPosition = new kakao.maps.LatLng(37.566535,
                     126.9779692);
               var message = 'geolocation을 사용할 수 없어요..';

               displayMarker(locPosition, message);
            }
         }

         // 지도에 마커와 인포윈도우를 표시하는 함수
         function displayMarker(locPosition, message) {
            // 현재위치 마커를 생성
            marker = new kakao.maps.Marker({
               map : map,
               position : locPosition
               
            });

            var iwContent = message; // 인포윈도우에 표시할 내용
            var iwRemoveable = true;

            // 인포윈도우를 생성
            var infowindow = new kakao.maps.InfoWindow({
               content : iwContent,
               removable : true
            });

            // 인포윈도우를 마커위에 표시
            infowindow.open(map, marker);

            // 지도 중심 좌표를 접속 위치로 변경
            map.setCenter(locPosition);
         }

         // 병원 인포인도우
         function HinfoWindow() {
            // 수정: 배열에 있는 모든 마커에 이벤트 리스너 추가
            markers.forEach(function (marker, index) {
               console.log(globalResult[index].M_TREAT)
               var HiwContent = '<div style="padding:5px;">'+"병원: " + globalResult[index].M_TREAT + "..." + '<br/>'+ "주소: "+globalResult[index].M_ADDRESS +'</div>';

               var Hinfowindow = new kakao.maps.InfoWindow({
                  content: HiwContent,
                  removable: true
               });

               kakao.maps.event.addListener(marker, 'click', function () {
                  // 마커 위에 인포윈도우를 표시
                  Hinfowindow.open(map, marker);
               });
            });
         }
      });
   </script>
</body>
</html>


```  
</details>

#### 병원 주소 크롤링 코드
<details>
<code>

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium import webdriver
import time
import pandas as pd

driver = webdriver.Chrome()

try:
    driver.get("https://pcmap.place.naver.com/place/list?query=%EC%A4%91%EB%8F%85%EC%B9%98%EB%A3%8C%EC%84%BC%ED%84%B0&x=126.86638294606388&y=35.14658199999941&clientX=126.866383&clientY=35.146582&bounds=126.86109362988799%3B35.14109005710702%3B126.87188683896443%3B35.151898128441516&ts=1705409585097&mapUrl=https%3A%2F%2Fmap.naver.com%2Fp%2Fsearch%2F%EC%A4%91%EB%8F%85%EC%B9%98%EB%A3%8C%EC%84%BC%ED%84%B0")

    wait = WebDriverWait(driver, 10)
    WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.TAG_NAME, "body")))

    data = {'HospitalInfo': [], 'Address': []}

    for k in range(1, 5):
        for i in range(1, 7):
            scroll_container = driver.find_element(By.ID, '_pcmap_list_scroll_container')
            driver.execute_script("arguments[0].scrollTop = arguments[0].scrollHeight;", scroll_container)
            time.sleep(1)

        address_elements = driver.find_elements(By.CLASS_NAME, "uFxr1")
        hospital_elements = driver.find_elements(By.CLASS_NAME, "YwYLL")

        for address_element, hospital_element in zip(address_elements, hospital_elements):
            hospitals = hospital_element.text
            hospital_info = hospitals
            address_element.click()
            address_element = wait.until(EC.visibility_of_element_located((By.CLASS_NAME, "zZfO1")))
            address = address_element.text
            address = address[3:-2]

            data['HospitalInfo'].append(hospital_info)
            data['Address'].append(address)

        if k>=1 and k<=3:
            driver.find_element(By.CSS_SELECTOR, f"#app-root > div > div.XUrfU > div.zRM9F > a:nth-child({k+2})").click()
            time.sleep(2) 

    # 데이터프레임 생성
    df = pd.DataFrame(data)

    # 엑셀 파일로 저장
    df.to_excel('hospital_data.xlsx', index=False)

finally:
    driver.quit()

```
</code>

</details>

#### 트러블 슈팅

<details>
<summary>
  Ajax 문제
</summary>

![제목을 입력해주세요_-003 (3)](https://github.com/jaewon07/jowonjae/assets/133577206/dc0db44a-618f-4ded-8e8b-c586f25c30f1)


Ajax(Asynchronous JavaScript and XML)는 비동기적으로 서버와 데이터를 교환할 수 있는 기술이며. 페이지가 로드되고 나서 비동기적으로 데이터를 가져오기 때문에 $.ajax 메서드가 호출된 후에도 코드가 계속 진행되어, position 배열을 못찾는 현상 발생

해결방안

데이터가 도착하기 전에 다음 코드가 실행되는 문제를 해결하기 위해,
position 배열에 값을 넣는 부분과 마커를 생성하고 지도에 표시하는 부분을displayMarkers 라는 함수로 감싸고 ajax안에 콜백함수로 사용하여 문제 해결

![제목을 입력해주세요_-003 (3)](https://github.com/jaewon07/jowonjae/assets/133577206/8dc8f79e-49a0-4322-8d4f-53c68bafcf25)

</details>

    
</br>

</details> 

</br>
</br>
</br>




