# HTTP 헤더1 - 일반 헤더

## HTTP 헤더 개요

#### HTTP 헤더

-   header-filed = field-name":"OWS field-value OWS
-   OWS: 띄어쓰기 허용
-   field-name은 대소문자 구분 없음

#### HTTP 헤더 용도

-   HTTP 전송에 필요한 모든 부가 정보
-   메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보 등
-   표준 헤더가 많음
-   필요 시 임의의 헤더 추가 가능

#### HTTP 헤더(RFC2616) 분류 - 과거

-   General 헤더: 메시지 전체에 적용되는 정보 - Connection: close
-   Request 헤더: 요청 정보 - User-Agent: Mozilla/5.0
-   Response 헤더: 응답 정보 - Server: Apache
-   Entity 헤더: 엔티티 바디 정보 - Content-Type: text/html

#### HTTP BODY, message body(RFC2616) - 과거

-   메시지 본문(message body)은 엔티티 본문(entity body)을 전달하는데 사용
-   엔티티 본문은 요청이나 응답에서 전달할 실제 데이터
-   엔티티 헤더는 엔티티 본문의 데이터를 해석할 수 있는 정보 제공
    -   데이터 유형(html, json), 데이터 길이, 압축 정보 등

#### HTTP 표준

-   1999년 RFC2616 폐기
-   2014년 RFC7230~7235 등장

#### RFC723x 변화

-   엔티티(Entity) → 표현(Representation)
-   Representation = representation Metadata + Representation Data
-   표현 = 표현 메타데이터 + 표현 데이터

#### HTTP BODY, message body(RFC7230) - 최신

-   메시지 본문(message body)을 통해 표현 데이터 전달
-   메시지 본문 = 페이로드(payload)
-   표현은 요청이나 응답에서 전달할 실제 데이터
-   표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공
    -   데이터 유형(html, json), 데이터 길이, 압축 정보 등
-   참고: 표현 헤더는 표현 메타 데이터와 페이로드 메시지를 구분해야 하지만, 너무 복잡하기 때문에 생략함.
