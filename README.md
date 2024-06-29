# :pushpin: normALearn
> AutoML 기반 앙상블 모델을 활용한 AL합금 열처리 정보제공 서비스

</br>

## 1. 제작 기간 & 참여 인원
- 2024년 5월 28일 ~ 6월 20일
- 5인 (박준범, 강우석, 주우건, 강아람, 박종현)

</br>

## 2. 사용 기술
#### `Back-end`
  - SpringBoot
  - Mybatis
  - Flask
  - python
  - JPA
    
#### `Front-end`
  - React
  - HTML
  - JS
  - CSS
  - Bootstrap
  - OpenAPI
    
#### `IDE`
  - STS4
  - Visual Studio Code
  - jupyter
    

</br>

## 3. ERD 설계
![image](https://github.com/clavis13/normALearn/assets/155136484/49f20c78-0cf8-4ddd-95cd-be1407459d8f)



## 4. 핵심 기능
이 서비스의 핵심 기능은 콘서트를 확인 후 버스현황을 파악하고 전세버스의 좌석현황 및 예약기능입니다.</br>
사용자는 콘서트를 예매하고 다른 지역으로 가고자 할 때 전세버스의 좌석현황을 보고 자신이 원하는 자리를 예약하면 됩니다.</br>
기능의 상세한 설명은 다음에 준비되어있습니다.</br>

<details>
<summary><b>핵심 기능 설명 펼치기</b></summary>
<div markdown="1">

### 4.1. 전체 흐름
![](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/blob/master/BooksProject/src/main/webapp/img/main.jpg)

### 4.2. Frontcontroller
![](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/blob/master/BooksProject/src/main/webapp/img/Frontcontroller.jpg)

- **페이지 이동** :pushpin: [코드 확인](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/blob/master/BooksProject/src/main/java/com/smhrd/frontcontroller/FrontController.java)
  - 사용자의 페이지 이동 요청이 들어올 때마다 Frontcontroller로 이동하게됩니다.
  - init기능을 사용하여 각 controller가 실행된 후 반환 값을 가지고 다시 Frontcontroller로 이동하게 되며, 반환값.jsp로 이동하는 것으로 사용자의 요청에 응답해줍니다.


### 4.3. Data

![](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/blob/master/BooksProject/src/main/webapp/img/Getdata.jpg)

- **eventList** :pushpin: [코드 확인](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/blob/master/BooksProject/src/main/java/com/smhrd/controller/eventList.java)
- **mainEventlist** :pushpin: [코드 확인](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/blob/master/BooksProject/src/main/java/com/smhrd/controller/mainEventlist.java)
- **progressBar** :pushpin: [코드 확인](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/blob/master/BooksProject/src/main/java/com/smhrd/controller/progressBar.java)
- **bookingList** :pushpin: [코드 확인](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/blob/master/BooksProject/src/main/java/com/smhrd/controller/bookingList.java)</br>

  - Frontcontroller의 요청을 받은 Controller에서는 DAO에 접근하여 mappers 안의 mysql쿼리문을 데이터베이스에서 실행시켜 얻은 데이터를 DAO로 반환받아 controller에게 넘겨준다.</br>
- **DAO** :pushpin: [코드 확인](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/blob/master/BooksProject/src/main/java/com/smhrd/database/DAO.java)
- **BooksMapper** :pushpin: [코드 확인](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/blob/master/BooksProject/src/main/java/com/smhrd/database/BooksMapper.xml)</br>
</div>
</details>


</br>

## 5. 핵심 트러블 슈팅
### 5.1. 메인페이지 접속 시 이벤트 목록을 불러오지 못하는 현상
#### #수정 전
![image](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/assets/155136608/95b50124-2f74-47fc-b1f2-dc5b338c3590)

- 메인페이지 접속 시 마감일 임박순으로 목록을 불러와야 하지만, 목록을 불러오지 못함
<details>
<summary><b>코드 추가</b></summary>
<div markdown="1">
	
~~~java
if ("gomain.do".equals(path)) {
	command eventListCommand = map.get("mainEventlist.do");
	if (eventListCommand != null) {
		eventListCommand.execute(request, response);
	}
}
~~~

</div>
</details>

- 해당 코드를 추가하여, 메인페이지 접속 시, 데이터베이스와 연결하여, 이벤트 목록을 불러오도록 구현
#### #수정 후
![image](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/assets/155136608/183cbd08-9235-48ed-88ff-1c8e330fe9b9)

### 5.2. 데이터 수정 시 제약조건 위배되어 데이터 수정이 불가능한 현상
#### ON DELETE CASECADE 사용
- 특정 행 삭제시, 참조 키를 참조하여, 해당되는 데이터 전부 삭제
![image](https://github.com/2024-SMHRD-KDT-BigData-23/BooksProject/assets/155136608/24498323-aa2b-461d-bd9c-a5c92664a554)

### 5.3. MySQL DataBase Trigger 사용
#### 특정 이벤트가 발생할 시, 관련된 테이블에 자동적으로 index 생성
- 크롤링한 콘서트 목록을 EVENT_INFO TABLE에 등록 될때마다 SHUTTLE_INFO,SEAT_INFO TABLE에 자동으로 많은 데이터들이 추가되어야하나 이를 elipse에서 처리하기엔 코드가 너무 길어져 더 간편한 방법을 찾게됨

<details>
<summary><b>Trigger 1.</b></summary>
<div markdown="1">
	
~~~
이벤트가 등록될 때 자동으로 셔틀정보 생성

DELIMITER $$

CREATE TRIGGER after_event_insert
AFTER INSERT ON EVENT_INFO
FOR EACH ROW
BEGIN
    INSERT INTO SHUTTLE_INFO (START_RG, SHTL_NUM, START_DATE, END_DATE, RIDING_DATE, SHTL_STATE, EVENT_ID)
    VALUES 
    ('서울', 28, NOW(), DATE_ADD(NOW(), INTERVAL 20 DAY), DATE_ADD(NOW(), INTERVAL 1 MONTH), 'Y', NEW.EVENT_ID),
    ('광주', 28, NOW(), DATE_ADD(NOW(), INTERVAL 20 DAY), DATE_ADD(NOW(), INTERVAL 1 MONTH), 'Y', NEW.EVENT_ID),
    ('부산', 28, NOW(), DATE_ADD(NOW(), INTERVAL 20 DAY), DATE_ADD(NOW(), INTERVAL 1 MONTH), 'Y', NEW.EVENT_ID),
    ('대구', 28, NOW(), DATE_ADD(NOW(), INTERVAL 20 DAY), DATE_ADD(NOW(), INTERVAL 1 MONTH), 'Y', NEW.EVENT_ID),
    ('인천', 28, NOW(), DATE_ADD(NOW(), INTERVAL 20 DAY), DATE_ADD(NOW(), INTERVAL 1 MONTH), 'Y', NEW.EVENT_ID),
    ('대전', 28, NOW(), DATE_ADD(NOW(), INTERVAL 20 DAY), DATE_ADD(NOW(), INTERVAL 1 MONTH), 'Y', NEW.EVENT_ID),
    ('울산', 28, NOW(), DATE_ADD(NOW(), INTERVAL 20 DAY), DATE_ADD(NOW(), INTERVAL 1 MONTH), 'Y', NEW.EVENT_ID);
END$$

DELIMITER ;

~~~

</div>
</details>

<details>
<summary><b>Trigger 2.</b></summary>
<div markdown="1">
	
~~~
셔틀 정보가 생성되면 자동으로 28인승 좌석 정보 생성

DELIMITER $$
CREATE TRIGGER TRG_SHUTTLE_SEAT
AFTER INSERT ON SHUTTLE_INFO 
FOR EACH ROW 
BEGIN 
   DECLARE x INT DEFAULT 1;
  
    WHILE x <= NEW.SHTL_NUM DO 
         INSERT INTO SEAT_INFO (SHTL_ID, SEAT_NUM) VALUES 
            (NEW.SHTL_ID, x);
            SET x = x + 1;
    END WHILE;
END$$

DELIMITER ;
~~~
</div>
</details>
<br>

## 6. 팀원 소개
|박준범|강우석|주우건|강아람|박종현|
|:---:|:---:|:---:|:---:|:---:|
|Product Manager, <br> AI|Front-End|Back-End|Front-End|Front-End|
|XGBoost,Pipeline<br>기반 ML모델 구축<br><br>ML배포용 Flask 구축<br><br>통합개발환경 구축|Front-end 아키텍처<br><br>Back-end 연동<br>및 데이터 처리<br><br>Git 형상관리<br><br>통합개발환경 구축|DB 설계 및 관리<br><br>Back-end 로직 구현<br><br>Front-end 연동<br>및 데이터 통신|UI/UX 작업<br><br>OpenAPI 구현|Chart API 작업|
