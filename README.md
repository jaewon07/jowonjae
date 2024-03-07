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

## 1. 사용 기술
#### `Back-end`
  - Spring
  - JQuery
  - JavaScript
  - Oracle
  - AWS

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
  - DB 스케줄러를 이용하여 주기적으로 삭제하려 했으나, 권한 부여의 어려움 발생

#### 해결 방안
  - 스프링 스케줄러를 활용, 만료된 토큰을 삭제하여 해결

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




