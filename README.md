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

#### map.jsp
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

            // 마커 이미지의 이미지 주소입니다
            var imageSrc = "https://t1.daumcdn.net/localimg/localimages/07/mapapidoc/markerStar.png";


            for (var i = 0; i < positions.length; i++) {
               // 마커 이미지의 이미지 크기 입니다
               var imageSize = new kakao.maps.Size(20, 27);

               // 마커 이미지를 생성합니다    
               var markerImage = new kakao.maps.MarkerImage(imageSrc,
                     imageSize);

               // 마커를 생성합니다
               marker = new kakao.maps.Marker({
                  map : map, // 마커를 표시할 지도
                  position : positions[i].latlng, // 마커를 표시할 위치
                  title : positions[i].title, // 마커의 타이틀, 마커에 마우스를 올리면 타이틀이 표시됩니다
                  image : markerImage
                  // 마커 이미지 z
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

                        // 마커와 인포윈도우를 표시합니다
                        displayMarker(locPosition, message);
                        // 병원 인포인도우를 표시합니다
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
            // 현재위치 마커를 생성합니다
            marker = new kakao.maps.Marker({
               map : map,
               position : locPosition
               
            });

            var iwContent = message; // 인포윈도우에 표시할 내용
            var iwRemoveable = true;

            // 인포윈도우를 생성합니다
            var infowindow = new kakao.maps.InfoWindow({
               content : iwContent,
               removable : true
            });

            // 인포윈도우를 마커위에 표시합니다 
            infowindow.open(map, marker);

            // 지도 중심 좌표를 접속 위치로 변경합니다
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
                  // 마커 위에 인포윈도우를 표시합니다
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

<br>

<details>
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
            time.sleep(2)  # 페이지 이동 후 충분한 대기 시간을 두어 페이지가 로드될 수 있도록 합니다.

    # 데이터프레임 생성
    df = pd.DataFrame(data)

    # 엑셀 파일로 저장
    df.to_excel('hospital_data.xlsx', index=False)

finally:
    driver.quit()

```
</details>

</details> 
    
</br>

## 6. 회고 / 느낀점
>프로젝트 개발 회고 글: https://zuminternet.github.io/ZUM-Pilot-integer/
</details> 
