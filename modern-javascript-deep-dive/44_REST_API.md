# 44. REST API

- 2024.8.8

## 🏷 REST API란?

```
- REST (Representational State Transfer)
- HTTP를 기반으로 클라이언트가 서버의 리소서에 접근하는 방식을 규정한 아키텍처
- REST API: REST를 기반으로 서비스 API를 구현한 것
```

<br />

## 🏷 REST API의 구성

| 구성 요소            | 내용                           | 표현 방법        |
| -------------------- | ------------------------------ | ---------------- |
| 자원 resource        | 자원                           | URI(ENDPOINT)    |
| 행위 verb            | 자원에 대한 행위               | HTTP 요청 메서드 |
| 표현 representations | 자원에 대한 행위의 구체적 내용 | 페이로드         |

<br />

## 🏷 REST API 설계 원칙

```
- 1) URI: 리소스 표현에 집중
- 2) 행위에 대한 정의: HTTP 요청 메서드를 통해 하는 것
```

#### 1. URI는 리소스를 표현해야한다

```
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```

#### 2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현해야한다

```
- 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법
- GET, POST, PUT, PATCH, DELETE
```

| HTTP 요청 메서드 | 종류           | 목적                  | 페이로드 |
| ---------------- | -------------- | --------------------- | -------- |
| GET              | index/retrieve | 모든/특정 리소스 취득 | X        |
| POST             | create         | 리소스 생성           | O        |
| PUT              | replace        | 리소스의 전체 교체    | O        |
| PATCH            | modify         | 리소스의 일부 수정    | O        |
| DELETE           | delete         | 모든/특정 리소스 삭제 | X        |

<br />

## 🏷 JSON Server를 이용한 REST API 실습

### 3.1 JSON Server 설치

```
- JSON Server는 json 파일을 사용하여 가상 REST API 서버를 구축할 수 있는 툴
```

- 설치

```
$ mkdir json-server-exam && cd json-server-exam
$ npm init -y
$ npm install json-server --save-dev
```

### 3.2 db.json 파일 생성

```
- db.json 파일은 리소스를 제공하는 데이터베이스 역할
```

### 3.3 JSON Server 실행

```
- db.json 파일의 변경을 감지하려면 watch 옵션 추가
```

```
$ json-server --watch db.json

$ npm start
```

### 3.4 GET 요청

```
- 리소스에서 모든 데이터 취득
```

### 3.5 POST 요청

```
- 리소스에 새로운 데이터 생성
- 요청 시 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송
```

### 3.6 PUT 요청

```
- 특정 리소스 전체를 교체 시 사용
- 요쳥 시 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송
```

### 3.7 PATCH 요청

```
- 특정 리소스의 일부 수정 시 사용
- 요쳥 시 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송
```

### 3.8 DELETE 요청

```
- 리소스에서 특정 값을 사용하여 데이터 삭제
```
