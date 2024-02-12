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

## 표현

#### 표현

-   Content-Type: 표현 데이터의 형식
-   Content-Encoding: 표현 데이터의 압축 방식
-   Content-Language: 표현 데이터의 자연 언어
-   Content-Length: 표현 데이터의 길이 -표현 헤더는 전송, 응답 둘 다 사용

#### Content-Type

-   표현 데이터의 형식 설명
-   미디어 타입, 문자 인코딩
-   예)text/html; charset=utf-8, application/json

#### Content-Encoding

-   표현 데이터 인코딩
-   표현 데이터를 압축하기 위해 사용
-   데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
-   데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
-   예)gzip, deflate, identity

#### Content-Language

-   표현 데이터의 자연 언어
-   예)ko, en, en-US

#### Content-Length

-   표현 데이터의 길이
-   바이트 단위
-   Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안 됨
    <br>
-   Transfer-Encoding(전송 코딩): 비교적 효율적으로 클라이언트에 데이터를 전송하기 위해 사용하는 인코딩 형식

## 콘텐츠 협상

#### 협상(콘텐츠 네고시에이션)

-   클라이언트가 선호하는 표현 요청
-   Accept: 클라이언트가 선호하는 미디어 타입 전달
-   Accept-Charset: 클라이언트가 선호하는 문자 인코딩
-   Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
-   Accept-Language: 클라이언트가 선호자는 자연 언어
-   협상 헤더는 요청 시에만 사용

#### 협상과 우선순위 - Quality Value(q)

-   Quality Values(q) 값 사용
-   0 ~ 1, 클수록 높은 우선 순위
-   생략하면 1
-   Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
    1. ko-KR;q=1(q생략)
    2. ko;q=0.9
    3. en-US;q=0.8
    4. en:q=0.7
-   구체적인 것을 우선한다.
-   Accept:text/\*, text/plain, text/plain;format=flowed. \*/\*
    1. text/plain;format=flowed
    2. text/plain
    3. text/\*
    4. \*/\*
-   구체적인 것을 기준으로 미디어 타입을 맞춘다.

## 전송 방식

#### 전송 방식

-   단순 전송
-   압축 전송
-   분할 전송
-   범위 전송

#### 단순 전송

-   응답을 줄 때 메시지 바디에 대한 Content Length를 지정하는 것
-   컨텐츠에 대한 길이를 알 수 있을 때 사용

#### 압축 전송

-   응답을 줄 때 gzip 같은 방식으로 압축해서 전송하는 것
-   어떤 방식으로 압축했는지 전달해야 하기 때문에 Content-Encoding을 추가로 넣어줘야 함.

#### 분할 전송

-   Chunk(덩어리)로 쪼개서 전송하는 것
-   전송을 마무리할 때는 0 바이트과 \r\n을 보냄
-   Content-Length를 예상할 수 없기 때문에 Content-Length를 사용하면 안 됨

#### 범위 전송

-   범위를 지정해서 요청을 하는 것
-   Content-Range

## 일반 정보

#### From

-   유저 에이전트의 이메일 정보
-   일반적으로 잘 사용되지 않음
-   검색 엔진 같은 곳에서, 주로 사용
-   요청에서 사용

#### Referer

-   이전 웹 페이지 주소
-   현재 요청된 페이지의 이전 웹 페이지 주소
-   A → B로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청
-   Referer를 사용하여 유입 경로 분석 가능
-   요청에서 사용
-   참고: Referer는 단어 Referrer의 오타

#### User-Agent

-   유저 에이전트 애플리케이션 정보(웹 브라우저 정보 등)
-   통계 정보
-   어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
-   요청에서 사용

#### Server

-   요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
-   응답에서 사용

#### Date

-   메시지가 발생한 날짜와 시간
-   응답에서 사용

## 특별한 정보

#### Host

-   요청한 호스트 정보(도메인)
-   요청에서 사용하며 필수임
-   하나의 서버가 여러 도메인을 처리해야 할 때
-   하나의 IP 주소에 여러 도메인이 적용되어 있을 때
-   가상 호스트(Virtual Host)를 통해 여러 도메인을 한 번에 처리할 수 있는 서버에 요청을 보낼 때 여러 애플리케이션 중 어떤 애플리케이션에서 요청을 처리할지 결정할 때 사용

#### Location

-   페이지 리다이렉션
-   웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)
-   응답 코드 201 Created의 경우 Location 값은 요청에 의해 생성된 리소스 URI
-   응답 코드 3xx: Location 값은 요청을 자동으로 리디렉션하기 위한 대상 리소스를 가리킴

#### Allow

-   허용 가능한 HTTP 메서드
-   405(Method Not Allowed)에서 응답에 포함해야 함

#### Retry-After

-   유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
-   응답 코드 503 Service Unavailable: 서비스가 언제까지 불능인지 알려줄 수 있음
-   날짜 표기와 초 단위 표기 가능

## 인증

#### Authorization

-   클라이언트 인증 정보를 서버에 전달

#### WWW-Authenticate

-   리소스 접근 시 필요한 인증 방법 정의
-   응답 코드 401 Unauthorized 응답과 함께 사용
