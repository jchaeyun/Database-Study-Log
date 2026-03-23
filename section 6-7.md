# section 6-7




### 1. MySQL 실행 및 접속 매뉴얼

윈도우에서 MySQL을 구동하기 위한 표준 절차는 다음과 같다.

* **서버 실행:** 관리자 권한 명령 프롬프트(CMD)에서 `net start mysql`을 입력한다.
* **클라이언트 접속:** CMD에서 `mysql -u root -p` 입력 후 비밀번호를 입력하여 접속한다.
* **환경 변수:** `mysql` 명령어가 인식되지 않을 경우, `C:\Program Files\MySQL\MySQL Server 8.0\bin` 경로를 시스템 변수 **Path**에 추가해야 한다.



### 2. 관계형 데이터베이스와 테이블 분리 

데이터가 방대해짐에 따라 발생하는 **중복(Redundancy)** 문제를 해결하기 위해 테이블을 분리한다.

* **필요성:** 저자 정보(이름, 프로필)가 모든 게시글마다 반복 저장되면 저장 공간이 낭비되고, 정보 수정 시 모든 행을 고쳐야 하는 데이터 불일치 위험이 발생한다.
* **구조 설계:** '글 정보'와 '저자 정보'를 별개의 테이블로 나눈다.
    * **topic 테이블:** `id`, `title`, `description`, `created`, `author_id` (저자의 ID만 저장)
    * **author 테이블:** `id`, `name`, `profile`



### 3. JOIN: 분리된 데이터 결합하기 

`JOIN`은 서로 다른 테이블을 특정 기준(고리)으로 합쳐서 하나의 표로 보여주는 기능이다.
관계형 데이터베이스의 정체성과 같은 기능이며, 이를 위해 테이블을 정교하게 나누는 과정을 '정규화'라고 한다.

#### **실전 예시 (LEFT JOIN)**
`topic` 테이블의 글 정보 옆에 `author` 테이블의 저자 상세 정보를 붙여서 조회하는 방법이다.

```sql
SELECT 
    topic.id, 
    topic.title, 
    author.name, 
    author.profile 
FROM topic 
LEFT JOIN author 
ON topic.author_id = author.id;
```

* **SELECT:** 출력할 컬럼을 지정한다. (테이블명.컬럼명 형식 권장)
* **FROM topic:** 기준이 되는 왼쪽 테이블을 지정한다.
* **LEFT JOIN author:** 오른쪽에 붙일 테이블을 지정한다.
* **ON:** 두 테이블을 연결할 조건이다. `topic`의 `author_id`와 `author`의 `id`가 같은 행끼리 매칭한다.




### 4. 인터넷과 클라이언트-서버 구조 

데이터베이스는 네트워크를 통해 서비스되는 구조를 가진다.

* **Database Server:** 실제 데이터가 저장되는 엔진(MySQL)이다.
* **Database Client:** 서버에 접속하여 명령을 전달하는 도구다. (CMD, Workbench 등)
* **작동 원리:** 클라이언트가 SQL 요청을 보내면 서버가 이를 처리하여 결과를 응답한다. 이 과정은 인터넷(IP, Port 3306)을 통해 원격으로도 가능하다.



### 5. MySQL Workbench 상세 사용법

MySQL Workbench는 마우스 클릭으로 데이터베이스를 관리할 수 있는 GUI(Graphic User Interface) 도구다.

#### **A. 접속 및 초기 설정**
1.  **MySQL Connections:** 홈 화면에서 생성된 인스턴스를 클릭하고 비밀번호를 입력한다.
2.  **Schema 선택:** 좌측 하단의 'Navigator' 창에서 **Schemas** 탭을 클릭한다. 작업할 데이터베이스(예: `opentutorials`)를 더블 클릭하여 활성화한다.

#### **B. 데이터 조회 및 편집 (SQL Editor)**
1.  **Query 창:** 상단의 SQL 아이콘을 눌러 새 탭을 연다.
2.  **실행:** 쿼리 작성 후 상단의 **번개 아이콘**을 클릭하거나 `Ctrl + Enter`를 누른다.
3.  **Result Grid:** 하단에 출력된 표에서 데이터를 직접 수정하고 **Apply**를 누르면 실제 DB에 반영된다.

#### **C. 테이블 생성 및 관리 (GUI 방식)**
1.  Navigator에서 'Tables' 우클릭 → **Create Table** 클릭.
2.  **Column 설정:**
    * **PK (Primary Key):** 중복되지 않는 고유 키.
    * **NN (Not Null):** 빈칸 허용 안 함.
    * **AI (Auto Increment):** 숫자가 자동으로 증가함.
3.  **Apply:** 우측 하단의 Apply 버튼을 누르면 Workbench가 SQL 문을 생성해 주며, 다시 한번 Apply를 누르면 최종 반영된다.





### 6. 핵심 요약

| 구분 | CLI (명령 프롬프트) | GUI (Workbench) |
| :--- | :--- | :--- |
| **특징** | 텍스트 기반, 빠르고 가벼움 | 시각적 기반, 복잡한 설계에 유리 |
| **권장 용도** | 기본 문법 연습, 서버 관리 | 실무 개발, 대량 데이터 조회 및 설계 |

 


