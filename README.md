# 클라우드 네이티브 번역 웹 서비스

**개발 기간:** 2주 (2024.11)  
**기술 스택:** Python, AWS Lambda, API Gateway, DynamoDB, S3  

언어 감지 기능을 통해 **한국어 ↔ 영어 양방향 번역**이 가능한 웹 서비스를 구축했습니다.  
입력된 텍스트와 번역 결과는 DynamoDB에 저장되며, 번역 기록 조회 페이지에서 이를 확인할 수 있습니다.

---

## 주요 기능

- **언어 자동 감지 및 양방향 번역 (영 ↔ 한)**
- **번역 기록 저장 (DynamoDB)**
- **번역 기록 조회 기능**
- **정적 웹사이트 S3 호스팅**
- **서버리스 아키텍처: Lambda + API Gateway**

---

## 아키텍처

![cloud-translate-architecture](https://github.com/user-attachments/assets/58e6d9e8-99a1-40df-acc9-83eedb028c92)

---

## 기능 상세 설명

### 1. `index.html`: 언어 감지 기반 번역 기능 구현
<img width="476" alt="index" src="https://github.com/user-attachments/assets/9c4baa42-20f4-4235-9530-0a2e9ff01d5f" />

- 입력된 텍스트의 언어를 감지하여 자동으로 한국어 ↔ 영어 번역
- 번역 결과와 원본 텍스트는 DynamoDB에 저장
- UI는 S3에서 정적 웹 호스팅

---

### 2. `history.html`: 번역 기록 조회 기능 구현
<img width="469" alt="history" src="https://github.com/user-attachments/assets/c73ff346-f28a-4b7e-a8da-3ec59b63625b" />

- DynamoDB에 저장된 번역 이력을 조회
- 사용자가 번역 기록 조회 버튼 클릭 시 API 호출을 통해 브라우저에 출력

---

### 3. Amazon S3 – 정적 웹 호스팅
<img width="469" alt="s3" src="https://github.com/user-attachments/assets/fee4053c-7508-40bf-ad7e-61b71bb67d38" />

-  이 버킷은 S3를 통해 정적 웹페이지를 호스팅하기 위해 생성됨
- `index.html`, `history.html` 업로드
- 퍼블릭 읽기 권한(ACL) 설정을 통해 웹에서 접근 가능
- 정적 웹 호스팅 기능 활성화

---

### 4. Lambda & API Gateway – 서버리스 번역 처리

<img width="220" alt="lambda" src="https://github.com/user-attachments/assets/8c767f5e-91ca-4fa8-8d23-15dd5abb2a63">
<img width="200" alt="api" src="https://github.com/user-attachments/assets/4f860014-24dc-4ac0-9f4c-48f1a243bc6a">

#### ✅ Lambda for Translation
1. API Gateway에서 요청 수신
2. 입력 텍스트 감지 및 번역 (AWS Translate)
3. 결과를 DynamoDB에 저장
4. 결과를 브라우저로 반환

#### ✅ Lambda for History
<img width="243" alt="lambda2" src="https://github.com/user-attachments/assets/edd23a5c-e2a9-4275-bfa6-b195216d647c">
<img width="194" alt="api2" src="https://github.com/user-attachments/assets/76f33ab4-70d4-429b-baa1-f87f1d6bf77f">

1. API Gateway에서 요청 수신
2. DynamoDB 조회 (전체 기록)
3. 기록 데이터를 브라우저로 반환

---

### 5. DynamoDB – NoSQL 저장소
<img width="262" alt="ddb1" src="https://github.com/user-attachments/assets/f292bad9-484d-43b2-aaf1-cc29ffe3bede">
<img width="299" alt="ddb2" src="https://github.com/user-attachments/assets/ebaa11cf-7599-4b62-9956-1ac81fc8b566">

- 테이블 이름: `dynamodb_translate`
- 저장 데이터: `id`, `입력된 텍스트`, `번역 결과`
- Lambda에서 CRUD 처리

---

## 느낀점

> 처음에는 AWS가 어렵고 막연한 기술로 느껴졌지만, 이번 프로젝트를 통해 **AWS의 다양한 서비스가 유기적으로 연결되는 경험**을 할 수 있었습니다.

- **클라우드 기술에 대한 막연한 두려움을 극복**하게 되었고,
- 서버를 직접 구성하지 않아도 Lambda와 API Gateway로 **완전한 기능 구현이 가능**하다는 점이 인상 깊었습니다.
- 이제는 AWS를 활용한 프로젝트에 **도전하고 싶은 욕심**이 생겼고, 앞으로는 AI, 블록체인 같은 기술도 클라우드와 함께 접목시켜보고 싶습니다.

## End-Point
http://b-t-s.s3-website.ap-northeast-2.amazonaws.com
