# 🔧 리팩토링

💡 `가독성`, `재사용성`, `성능`을 고려하며 리팩토링

## 유지보수 편의성을 고려하여 MVC 패턴으로 코드 리팩토링

- Model : Car, Game
- Controller: GameController
- View: View
- MVC 리팩토링 체크 사항
  [X] Model은 Controller와 View에 의존하지 않음
  [X] View는 Model에만 의존하고, Controller에 의존해서는 안됨.
  [X] View가 Model로부터 데이터를 받을 때는, 사용자마다 다르게 보여줘야하는 데이터에 한함.
  [X] View가 Model로부터 데이터를 받을 때는 항상 Controller를 거쳐야 함.
  [X] Controller는 Model과 View에 의존해도 됨.

## 피드백 검토 및 반영 사항

### step1 질의 응답 기반 반영 사항

1. 비동기 입력 처리 코드
   [X] callback에서 async/await 으로 변경

2. 클래스 필요성
   [X] Controller, View 클래스 -> IIFE 모듈로 변경

3. 테스트
   [X] 테스트케이스 자동 생성 코드 제거

### step1 피드백 기반 반영 사항

1. 상수 처리
   [X] Model 내부로 상수 위치 변경
   [X] 상수가 클래스와 결합해야하면 `static` 키워드 사용
   [X] 클래스 내부 상태 은닉 필요하면 `private` 접근 제어자 사용

2. 요구사항 변경 가능지점 체크하여 확장성 고려해 코드 작성
   [X] 자동차 전진 조건이 변경되는 지점 자유
   [X] 자동차 전진 조건이 변경되는 시점 자유
   [X] 자동차 전진 조건 변경함에 따라 테스트코드 변경

3. 보다 객체 지향적으로 설계
   [X] 팩토리 패턴 도입

### 공통 피드백 기반 반영 사항

1.  가독성

- 코딩 컨벤션 변경
  [X] 클래스 내 변수/메소드 구현 순서 통일: 상수 -> 클래스 변수 -> 인스턴스 변수 -> 생성자 -> 메서드
  [X] 개행 방식 통일
  [X] import문 순서 규칙에 따라 통일
  [ ] eslint, prettier 환경 설정 도입
- 의미가 드러나도록 코드 작성하기
  [X] 최대한 주석 없애기
  [X] 의미가 드러날 수 있도록 변수/함수명 지정
  [X] 코드만 봐도 충분히 의미를 파악할 수 있게 단순 명료하게 작성
- 함수/메소드는 하나의 역할만 담당하도록 처리
  [X] 입력값 검증과 예외 생성 함수 분리
- 함수/메소드를 최대한 간단하게 작성
  [X] 함수의 인자 수는 3개 미만으로 유지. 그 이상으로 필요하다면 함수/메소드를 분리하거나 객체 형태로 인자 처리
  [X] 함수 depth 2 이상 지양하기. 그 이상으로 필요하다면 함수/메소드를 분리
  [X] early return 사용하기. 예외 케이스는 함수/메소드 앞부분 위치
  [X] boolean값만 리턴하는 함수는 조건문 없이 처리

2.  재사용성

- JS 빌트인 객체 내장메서드 최대한 활용
  [X] for, while 반복문 대신 Array 객체 내장 메서드 사용
- 명령형보다는 선언형 코드 작성

3.  테스트
    [X] 테스트를 위한 코드는 프로덕션 코드에서 분리
    [X] 단위 테스트 실패 이유는 하나!
    [X] 경계조건 테스트에 parameterized test 방식 사용

## step1 2차 코드리뷰 관련 반영 사항

1. 불필요한 지점 점검하기
   [X] 외부에서 쓰이지 않는 변수, 함수가 public 필드로 선언되어있지 않은지 점검
   [X] 클래스 내 불필요한 getter, setter 점검
   [X] 굳이 함수화하지 않기 -> 가독성을 낮추는 불필요한 함수화 지양

2. 예외 케이스 추가하기
   [X] Input 값이 의도치 않은 type인 경우 방어 코드 추가

3. 테스트 코드 명확하게 작성하기
   [X] 테스트 코드의 유효한 데이터, 유효하지 않은 데이터 이유로 테스트 문장 작성
   [X] 테스트코드를 이해하기 위한 불필요한 시간 들지 않게 처리하기
   [X] 너무 많은 환경 변수 사용 지양
   [X] 불필요한 변수 저장 하지 않기

4. 객체의 역할과 책임 구분하기
   [X] Game 객체를 분리하기 -> Cars 객체 추가

5. 함수를 예측 가능하게 만들기
   [X] 랜덤 값 생성 등 예측 불가능한 부분 분리해서 외부 인자로 주입받기
   [X] 예측 불가능한 부분을 외부로 분리해야 테스트 코드 쉽게 작성 가능

## 객체의 소리 듣기 이후 설계 변경

💡 `가독성`, `재사용성`, `성능`을 고려하며 리팩토링

1. 요구사항 기반으로 (1) 역할 (2) 객체 (3) 협력관계를 추출

- 이 때, 도메인 로직과 UI 로직을 분리
- 도메인 로직부터 진행하고, 이후 UI 로직 진행

2. 객체 추출하고, 객체 관계도 그림 그리기

- 이 때, 객체의 역할과 책임이 하나인지 확인하기

3. 객체 관계도로부터 전체 프로그램의 큰 흐름 파악하기

- 이 때, 외부에서 주입되어야 하는 조건이 무엇인지 파악

4. Controller 집어 넣기
5. 도메인 조직 내에서 각 객체 별 역할 및 관리해야하는 필드 뽑아내기

- 이 때, 역할이 여러 개이거나 관리해야하는 필드가 너무 많으면 객체 분리
- 객체 간 협력에 필요한 메소드는 public 필드로 설정

6. 사용 목적에 따른 설계 구현 방식 지정

- class vs IIFE 모듈

7. 구현

### step2 피드백 기반 반영 사항

1. 목적에 맞는 구현 방식 선택
   [X] IIFE -> 일반 함수 변경
   [X] 프로덕션 layer vs 테스트 layer 분리
   [X] Runtime 에러 객체 - Custom 에러 객체 상속 구조 도입
   [ ] 존재하는 함수가 있다면, 수레바퀴 재발명 지양 \*readline Promise 기반 API

2. 테스트 코드 가독성 향상
   [X] 상수 사용 지양
   [X] 굳이 저장할 필요 없다면 변수 사용 지양
   [X] 결과값도 하드코딩 값으로 변경
   [X] testcase 인식 가능하도록 name 변경
   [X] jest.fn, jest.spyOn 지양 -> 역할 분리?

3. 객체의 역할과 책임 분리
   [X] 테스트가 필요한 핵심적인 private 코드가 있다면 역할 분리 고민 -> generateNumber 분리
   [X] 객체는 메시지로 소통

### step2 피드백 기반 질문

[X] test layer와 production layer 분리 의미 = test 폴더 어디에 위치?
[X] test용 파일을 위한 test 코드 -> 배보다 배꼽이 커지는 건 아닌지?
[X] 테스트가 필요한 핵심적인 private 코드가 있다면 역할 분리 고민 -> generateNumber 분리 하지 않기로 결정 (캡슐화 << 추상 메소드 미구현 에러처리)
