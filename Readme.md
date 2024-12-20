# 게시판 만들기

## 사용된 기술

- Spring Boot
- Spring MVC
- Spring JDBC
- MYSQL - SQL
- thymeleaf 탬플릿 엔진

### MySql 계정
- username : root
- userpassword : root1234

## 아키텍처


```
Spring Boot : 전체 프레임워크
Spring core : 전체 Bean 관리.
Spring MVC : 컨트롤러부터 서비스까지 관리.
Spring JDBC : 다오와 DB사이를 관리
MySQL : 사용하는 DB

브라우저 -> 요청 -> Controller -> Service -> DAO -> DB
DB -> DAO -> Service -> Controller -> 템플릿 -> 응답 -> 브라우저 

layer간에 데이터 전송은 DTO를 사용.
```

## 게시판 만드는 순서

1. Controller와 템플릿
2. Service - 비지니스 로직을 처리 (하나의 트랜잭션 단위)
3. Service는 비지니스 로직을 처리하기 위해 데이터를 CRUD 하기위해 DAO를 사용


## DB 테이블
/*
요구사항 

1. 회원가입
- 권한은 일반 사용자, 관리자가 있다.
- 회원 가입을 하면 기본적으로 일반 사용자 권한을 가지게 된다.
- 회원은 여러 권한을 가질 수 있다.
- 일반 사용자는 여러명 있을 수 있다.
- 관리자는 여러명 있을 수 있다.

2. 로그인
- id(또는 email), 암호를 입력해 로그인을 한다.
- id, 암호가 일치할 경우 로그인 처리를 한다.

3. 글쓰기
- 로그인을 한 사용자는 글을 작성할 수 있다.

4. 글 목록 보기
- 1페이지는 최대 10개씩 보여진다.
- 전체 페이지 목록이 보여진다. 
ex) 31건일 경우 4개의 페이지가 존재한다.
*/

DROP TABLE IF EXISTS role;

CREATE TABLE role(
	role_id INT primary key,
    name varchar(20)
);
INSERT INTO role(role_id, name) VALUES (1, 'ROLE_USER');
INSERT INTO role(role_id, name) VALUES (2, 'ROLE_ADMIN');

SELECT * FROM role;

DROP TABLE IF EXISTS user;

CREATE TABLE user (
	user_id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(255) NOT NULL,
    name VARCHAR(50) NOT NULL,
    password VARCHAR(500) NOT NULL,
    regdate TIMESTAMP DEFAULT NOW()
);

ALTER TABLE USER AUTO_INCREMENT = 0;

INSERT INTO user(email,name,password,regdate) VALUES('mystory@gmail.com','김성박','1234',now());
INSERT INTO user(email,name,password,regdate) VALUES('hong@gmail.com','홍길동','4321',now());
DELETE FROM user WHERE user_id = 4;


DROP TABLE IF EXISTS user_role;

CREATE TABLE user_role (
	user_id INT,
    role_id INT,
    PRIMARY KEY(user_id, role_id), -- 복합키
    FOREIGN KEY(user_id) REFERENCES user(user_id),
    FOREIGN KEY(role_id) REFERENCES role(role_id)
);

INSERT INTO user_role(user_id, role_id) values(1,1);
INSERT INTO user_role(user_id, role_id) values(1,2);
INSERT INTO user_role(user_id, role_id) values(2,1);

DROP TABLE IF EXISTS board;

CREATE TABLE board(
	board_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(100) NOT NULL,
    content TEXT NULL,
    user_id INT NOT NULL,
    regdate TIMESTAMP DEFAULT NOW(),
    view_cnt INT DEFAULT 0,
    FOREIGN KEY (user_id) REFERENCES user(user_id)
);







