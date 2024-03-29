# HTTP 메서드

## HTTP API를 만들어보자

#### 회원 정보 관리 API

1. URI 설계 - 리소스 식별
    - 행위는 리소스가 아니다.(등록, 조회, 수정, 삭제 등)
    - 회원이라는 개념 자체가 리소스다.
    - 계층 구조상 상위를 컬렉션으로 보고 복수 단어 사용 권장
      <br>
    - 회원 목록 조회: /members
    - 회원 조회, 등록, 수정, 삭제: /members/{id} → 행위 구분 필요
2. 리소스와 행위를 분리
    - URI는 리소스만 식별
    - 리소스와 해당 리소스를 대상으로 하는 행위를 분리
    - 리소스는 명사, 행위는 동사

## HTTP 메서드 - GET, POST

#### HTTP 메서드 종류

-   GET: 리소스 조회
-   POST: 요청 데이터 처리, 주로 등록에 사용
-   PUT: 리소스를 대체, 해당 리소스가 없으면 생성
-   PATCH: 리소스 부분 변경
-   DELETE: 리소스 삭제
-   HEAD: GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
-   OPTIONS: 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)
-   CONNECT: 대상 자원에서 식별되는 서버에 대한 터널을 설정
-   TRACE: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

#### GET

-   /members/100
-   리소스 조회
-   서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달
-   메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지 않음

#### POST

-   /members
-   요청 데이터 처리
-   메시지 바디를 통해 서버로 요청 데이터 전달
-   서버는 요청 데이터 처리
    -   메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능 수행
-   주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리(컨트롤 URI)에 사용
-   다른 메서드로 처리하기 애매한 경우에도 사용
-   이 리로스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야 함 → 정해진 것이 없음.

## HTTP 메서드 - PUT, PATCH, DELETE

#### PUT

-   /members/100
-   리소스를 대체
    -   리소스가 있으면 대체 → 부분 업데이트가 아닌 전체 업데이트
    -   로소스가 없으면 생성
    -   덮어쓰기
-   클라이언트가 리소스를 식별
    -   클라이언트가 리소스 위치를 알고 URI 지정
    -   POST와 차이점

#### PATCH

-   /members/100
-   리소스 부분 변경

#### DELETE

-   /members/100
-   리로스 제거

## HTTP 메서드의 속성

#### 안전(Safe)

-   호출해도 리소스를 변경하지 않는다.
-   계속 호출해서 로그 부분에서 장애가 발생한 경우는 고려하지 않는다. 안전은 해당 리소스만 고려한다.
-   GET, HEAD, OPTIONS, TRACE

#### 멱동(Idempotent)

-   f(f(x)) = f(x)
-   한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 똑같다.
-   멱등 메서드
    -   GET: 한 번 조회하든, 두 번 조회하든 같은 결과가 조회된다.
    -   PUT: 결과를 대체한다. 따라서 같은 요청을 여러 번 해도 최종 결과는 같다.
    -   DELETE: 결과를 삭제한다. 같은 요청을 여러 번 해도 삭제된 결과는 똑같다.
    -   POST: 멱등이 아니다. 두 번 호춣하면 같은 결제가 중복해서 발생할 수 있다.
-   활용
    -   자동 복구 메커니즘
    -   서버가 TIMEOUT 등으로 정상 응답을 못 주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가?
-   멱등은 외부 요인(재요청 중간에 다른 곳에서 리소스를 변경하는 경우)으로 중간에 리소스가 변경되는 것까지는 고려하지 않는다.

#### 캐시 가능(Cacheable)

-   응답 결과 리소스를 캐시해서 사용해도 되는가?
-   GET, HEAD, POST, PATCH 캐시 가능
-   실제로는 GET, HEAD 정도만 캐시로 사용
    -   POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하기 때문에 구현이 쉽지 않음.
