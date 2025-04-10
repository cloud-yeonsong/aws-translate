# 클라우드 네이티브 토이 프로젝트
## 실습 해설
1. index.html : 언어감지를 통한 번역기능 구현
   <img width="476" alt="image" src="https://github.com/user-attachments/assets/9c4baa42-20f4-4235-9530-0a2e9ff01d5f" />
수업 시간에 실습한 번역 프로그램은 영어에서 한국어로만 번역이 가능해서 아쉬움이 있었으며,
이를 보완하기 위해 언어 감지 기능을 구현하여 영어는 한국어로, 한국어는 영어로 번역할 수 있도록 개선했다. 
또한, 입력된 텍스트와 번역 결과는 DynamoDB에 저장된다.

2. history.html : 번역 기록 조회 기능 구현
<img width="469" alt="image" src="https://github.com/user-attachments/assets/c73ff346-f28a-4b7e-a8da-3ec59b63625b" />
번역 기록 조회 버튼을 클릭하면, DynamoDB에 저장된 입력된 텍스트(원본 텍스트)와 번역된 텍스트를 조회 할 수 있다.

3. Amazon S3
   <img width="469" alt="image" src="https://github.com/user-attachments/assets/fee4053c-7508-40bf-ad7e-61b71bb67d38" />
b-t-s(bucket-translate-service) 버킷은 index.html(번역 서비스)과 history.html(기록 조회) 객체가 있다.
이 버킷은 S3를 통해 정적 웹페이지를 호스팅하기 위해 생성되었으며, 객체 소유권 설정에서 ACL을 활성화했다.
또한, 버킷의 퍼블릭 설정을 해제 했으며, index.html과 history.html은 ACL에 퍼블릭 읽기 액세스 권한을 부여하였다.

4. Lambda, API Gateway – 번역 기능
   <img width="220" alt="image" src="https://github.com/user-attachments/assets/8c767f5e-91ca-4fa8-8d23-15dd5abb2a63" /><img width="200" alt="image" src="https://github.com/user-attachments/assets/4f860014-24dc-4ac0-9f4c-48f1a243bc6a" />
Lambda_for_translate_service
- translate을 호출하여 번역하는Lambda함수
- translate을 사용하기 위해translate의 모든권한과s3에 접근하여 객체를생성하는권한을연결함
1. 입력된텍스트를submit하면 활성화된다.
2. Gateway API로부터값을받아번역한다.
3. 번역한텍스트를DynamoDB의 dynamodb_translate에 저장한후번역한값을반환한다.
4. 반환된데이터는API Gateway를 통해브라우저로전달된다.
5. 결과는index.html에 출력되어사용자가번역결과를 확인할수있다.

<img width="243" alt="image" src="https://github.com/user-attachments/assets/edd23a5c-e2a9-4275-bfa6-b195216d647c" /><img width="194" alt="image" src="https://github.com/user-attachments/assets/76f33ab4-70d4-429b-baa1-f87f1d6bf77f" />
Dynamodb_data
- DynamoDB에서 데이터읽어오는 Lambda함수
1. 번역기록조회버튼을클릭하면 활성화된다.
2. Gateway API로부터요청을받아DynamoDB의 dynamodb_translate 테이블에 저장된데이터를조회하고, 결과로id, 입력된 텍스트(원본), 번역된텍스트를반환한다.
3. 반환된데이터는API Gateway를 통해브라우저로전달된다.
4. 결과는history.html에 출력되어 사용자가번역기록을확인할 수있다.

6. DynamoDB
<img width="262" alt="image" src="https://github.com/user-attachments/assets/f292bad9-484d-43b2-aaf1-cc29ffe3bede" />
<img width="299" alt="image" src="https://github.com/user-attachments/assets/ebaa11cf-7599-4b62-9956-1ac81fc8b566" />
Dynamodb_translate
- Id, 입력된텍스트, 번역된텍스트를저장하는 NoSQL 기반의데이터베이스
1. 번역결과와원본텍스트가 DynamoDB의 dynamodb_translate 테이블에저장된다.
2. 사용자가history.html에서 번역기록조회버튼을클릭하면, 클릭이벤트가 API Gateway를 통해
Dynamodb_data Lambda함수로 요청을전달하고, 람다함수가DynamoDB의 dynamo_translate 테이블에서
모든데이터를조회한다.
3. 조회된데이터가API Gateway를 통해브라우저로전달되고, history.html에 출력한다.


## 아키텍처
![image](https://github.com/user-attachments/assets/58e6d9e8-99a1-40df-acc9-83eedb028c92)

## 느낀점
1, 2학년 때 AWS를 처음 접했을 때는 어렵고 비용이 많이 드는 무서운 기술로 느껴졌다. 또한, 작년에 다른 프로젝트를 할 때, AWS를 활용한 팀을 보며 흥미를 느꼈지만, 여전히 막연한 두려움이 있었다. 그래서 이번 클라우드 수업을 듣는 것도 망설였는데, 그래도 졸업하기 전에 AWS에 대한 막연한 두려움을 극복하고자 이번 수업을 듣게 되었다. 막상 수업을 들으니, AWS는 생각보다 간단하고 흥미로운 기술이며, 클라우드서비스는 저장공간만이 아니라 다양한 기능을 제공한다는 점도 알게 되었다. 수업 때 진행한 실습도 정말 재미있고 흥미를 느끼게 했으며, 기말과제를 하기 전에는 약간의 두려움이 있었지만 천천히 실습하다 보니 더 다양한 기능을 구현해 보고 싶었고, 여러 가지 AWS의 기술에 관해 관심이 생겼다. 이번 수업은 클라우드 기술에 대한 두려움을 극복하고 흥미를 키울 수 있는 너무 좋은 계기였으며, 앞으로 AI나 블록체인 기술을 클라우드와 결합해 더 복잡한 프로젝트를 구현해 보고 싶다.
