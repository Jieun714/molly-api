**패션을 쉽게 MollyMol**
---


## **📽️ 시연 영상**
https://drive.google.com/file/d/13Jez_t-zlCY9VO-dskcfzjaCsl4pcMxr/view

<br>

## **🔖 프로젝트 개요**


- 의류 E-commerce
- Fashionista, anyone can be

<br>

## **🎯 목표**

- 대용량 트래픽에서 준수한 속도를 유지
- 다수의 트랜잭션에서 동시성 처리 및 일관성 보장

<br>

<br>

## 🧑‍🤝‍🧑 팀 역할

![image](https://github.com/user-attachments/assets/97fbfff2-ec90-4350-952c-7050890ae2a7)

<br>

## **📚 기술 스택**

![image](https://github.com/user-attachments/assets/e4fbaec5-dde3-48b4-9706-fba3f6fc3f36)

<br>

## **🌏 서버 아키텍쳐**

![image](https://github.com/user-attachments/assets/f4ff3c43-79f9-47f4-978a-34aab6a6b43f)

<br>

## 🗺️ ERD

![image](https://github.com/user-attachments/assets/d9ddd0fa-e303-4643-a723-b88219edd958)

<br>

<br>

## **🔗주요 기능**

**1️⃣ 회원가입/ 로그인**

- 이메일을 통해 회원 가입 및 로그인을 할 수 있습니다.
- 마이페이지에서 회원 정보를 관리할 수 있습니다.

**2️⃣ 상품 관리**

- 상품 검색 및 필터링 (필터: 카테고리, 색상, 사이즈, 가격 등/정렬: 최신 순, 조회 많은 순, 구매 많은 순  등)
- 상품 상세 페이지 - 이미지, 설명, 리뷰, 재고, 배송 정보 표시
- 상품 등록 -  상품 단건 등록, 상품 다건 (100건 이상부터 1,000,000건 이하) 엑셀로 등록 가능
- 상품 수정 및 삭제

3️⃣ **장바구니**

- 사용자는 상품을 장바구니에 담을 수 있습니다.
- 장바구니에서 옵션 및 수량 변경할 수 있습니다.
- 장바구니 전체 선택, 전체 삭제 할 수 있습니다.

4️⃣ **결제**

- 토스 API를 통해 사용자는 결제를 진행할 수 있습니다.
- 포인트를 사용하여 결제 할 수 있습니다.

**5️⃣ 주문 및 배송 관리**

- 자신의 기본 배송지 설정 및 추가, 변경, 삭제 할 수 있습니다.
- 마이페이지에서 사용자의 주문 내역을 조회할 수 있습니다.

**6️⃣ 리뷰**

- 사용자는 배송완료된 상품의 리뷰를 작성할 수 있습니다.
- 사용자는 리뷰 작성, 수정, 삭제 할 수 있습니다.
- 리뷰에 대한 좋아요 기능을 제공합니다.
- 최근 7일간 누적 좋아요가 많은 인기순위 Top 12를 조회할 수 있습니다.

<br>

## 📸 시연 이미지  
### 🏠 메인 페이지
![1](https://github.com/user-attachments/assets/beca93ca-68b6-4b55-95c5-9d8ed93d03e4)

<br>

### 🛒 상품 페이지
![2](https://github.com/user-attachments/assets/dd275067-a169-4f17-ac13-4fb1d682da18)

<br>

### 🔍 상품 상세 페이지(w.리뷰조회)
![3](https://github.com/user-attachments/assets/99d7aab4-bd02-4c7b-9c8a-62c0383dbf4c)

<br>

### 💳 상품 결제
![4](https://github.com/user-attachments/assets/6decaaf6-6081-41a2-94a8-6fbd8921ea62)

<br>

### 🛠️ 마이페이지(w. 관리자페이지)
![5](https://github.com/user-attachments/assets/ea82e9fd-2983-4470-8848-587fd1ee46f4)

<br>

## **🌈 개선 사항**
1️⃣ **장바구니 조회 성능 개선 - TPS 83.19% 향상(vuser 1000 기준)**

**문제 상황**

- 현재는 데이터 양이 많지 않지만 추후 데이터가 많아지게 될 경우 sort비용 증가, 다중 조회 문제가 발생하는 등 성능 저하가 우려됨

<br>

**문제 해결**

※ 장바구니 데이터는 1,000만개 활용

1. **복합인덱스 설정**
    - created_at 내림차순 정령에서 sort 비용이 증가하는 것으로 판단
    - user_id와 created_at desc의 복합인덱스 설정
2. **지연로딩(Lazy) 설정**
    - 상품에 해당하는 제품리스트를 조회하기 위해 findAllByProdcutId()를 날렸지만 즉시로딩(Eager)이 걸려있어, 각각의 ProductItem가 가진 Product를 모두 검색하게 되는 N+1 문제가 발생
3. **HashMap 적용**
    - 옵션 정보(사이즈, 컬러)가 다르다면 동일한 상품도 장바구니에 담을 수 있어, 상품 옵션 정보를 조회할 때 동일 상품에 대해 중복 조회하는 것을 확인
    - HashMap을 활용해 이미 조회한 상품의 옵션 정보라면 필터링 되도록 수정해 조회 성능 개선

<img width="1335" alt="장바구니 조회 성능표" src="https://github.com/user-attachments/assets/66a40f4a-d395-4875-a13d-84b35bffcccc" />


<br>
<br>

2️⃣ **리뷰 좋아요 성능 개선 - 응답속도 약 28.4% 향상, DB 부하 약 5배 감소**

**문제 상황**

- 다수의 사용자가 동시에 리뷰 좋아요를 누르는 경우 누적 좋아요 수가 누락되는 문제가 발생
- 현재 좋아요 로직의 경우, 하나의 API 안에서 리뷰 생성/삭제 요청과 누적 좋아요수 변경을 하기 때문에 강결합이 발생

<br>

**문제 해결**

**1차 개선 - 비관적락** 

- 좋아요 데이터와 리뷰 데이터 각각에 배타락을 걸어, 해당 데이터를 점유함으로써 동시성 문제를 해결하고자 함

**2차 개선 - Spring Event**

- 리뷰 좋아요 API는 생성과 삭제 두 가지 역할을 수행 중이기 때문에 API 책임 분리가 필요하다고 판단
- 누적 좋아요수를 업데이트하는 로직을 이벤트 리스너로 작성하여 비동기처리를 수행하도록 개선
- 정해진 개수의 스레드만 재사용함으로써 자원을 효율적으로  관리할 수 있도록, ThreadPoolTaskExceutor 설정

**3차 개선 - Redis 원자적 연산**

- 스레드가 많아질 수록 데드락 발생 가능성이 높아지고, DB부하가 증가할 것이라 판단
- 이를 해결하기 위해 리뷰 좋아요 수 증가/감소 처리를 DB 대신 Redis를 통해 원자적으로 수행하도록 변경
- 추가적인 개선 진행 중

<br>

**[응답속도 표]**

<img width="861" alt="리뷰 좋아요 성능표" src="https://github.com/user-attachments/assets/ab4b3682-b4de-441e-a001-03c7c3de1635" />

<br>

<br>

**[InnoDB Read-Write 그래프]**
<img width="761" alt="리뷰 좋아요 DB부하 감소" src="https://github.com/user-attachments/assets/8e39377a-40e3-494a-9046-48f0e9d4485f" />
