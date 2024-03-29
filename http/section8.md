# HTTP 헤더2 - 캐시와 조건부 요청

## 캐시 기본 동작

#### 캐시가 없을 때

-   데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다.
-   인터넷 네트워크는 매우 느리고 비싸다.
-   브라우저 로딩 속도가 느리다.
-   느린 사용자 경험을 준다.

#### 캐시 적용

-   캐시 덕분에 캐시 가능 시간동안 네트워크를 사용하지 않아도 된다.
-   비싼 네트워크 사용량을 줄일 수 있다.
-   브라우저 로딩 속도가 빠르다.
-   빠른 사용자 경험을 준다.

#### 캐시 시간 초과

-   cache-control: max-age=60(초)
-   캐시 유효 시간이 초과하면, 서버를 통해 데이터를 다시 조회하고, 캐시를 갱신한다.
-   이때 다시 네트워크 다운로드가 발생한다.

## 검증 헤더와 조건부 요청1

#### 캐시 시간 초과

-   캐시 만료 후에도 서버에서 데이터를 변경하지 않았다면 데이터를 전송하는 대신에 저장해 두었던 캐시를 재사용할 수 있다.
-   단 클라이언트의 데이터와 서버의 데이터가 같다는 사실을 확인할 수 있는 방법이 필요하다.

#### 검증 헤더 추가

-   Last-Modified: 2024년 2월 12일 22:20:00
-   캐시가 만료되었을 때 최종 수정일 정보를 가지고 있는 경우 웹 브라우저에서 if-modified-since 헤더에 최종 수정일 정보를 추가해서 서버에 요청한다. 서버에서 데이터 수정 여부를 판단하고 수정이 안되었다면 304 Not Modified로 응답 코드를 리턴한다. 이때 HTTP BODY가 없다. 웹 브라우저가 304 응답 코드를 받으면 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신하고, 캐시에 저장되어 있는 데이터를 재활용한다.

## 검증 헤더와 조건부 요청2

#### 검증 헤더

-   캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
-   Last-Modified, ETag

#### 조건부 요청 헤더

-   검증 헤더로 조건에 따른 분기
-   If-Modified-Since: Last-Modified 사용
-   If-None-Match: ETag 사용
-   조건이 만족하면(데이터 변경 시) 200 OK
-   조건이 만족하지 않으면(데이터 변경 없을 시) 304 Not Modified

#### Last-Modified, If-Modified-Since 단점

-   초 단위 미만으로 캐시 조정이 불가능
-   날짜 기반의 로직 사용
-   데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우 - A → B → A 인 경우 데이터는 바뀌지 않았지만 수정한 날짜는 변경됨.
-   서버에서 별도의 캐시 로직을 관리하고 싶은 경우

#### ETag(Entity Tag), If-None-Match

-   캐시용 데이터에 임의의 고유한 버전 이름을 달아둠
-   데이터가 변경되면 해당 이름을 바꿈(Hash를 다시 생성)
-   ETag가 같으면 유지, 다르면 다시 받기 로직 사용
-   캐시 제어 로직을 서버에서 완전히 관리
-   클라이언트는 단순히 이 값을 서버에 제공(클라이언트는 서버의 캐시 메커니즘을 모름)
-   애플리케이션 배포 주기에 맞추어 ETag 모두 갱신

## 캐시와 조건부 요청 헤더

#### 캐시 제어 헤더

-   Cache-Control: 캐시 제어
-   Pragma: 캐시 제어(하위 호환)
-   Expires: 캐시 유효 기간(하위 호환)

#### Cache-Control

-   캐시 지시어(directives)
-   Cache-Control: max-age → 캐시 유효 시간, 초 단위
-   Cache-Control: no-cache → 데이터는 캐시해도 되지만, 항상 원(origin) 서버(중간 캐시 서버 X)에 검증하고 사용
-   Cache-Control: no-store → 데이터에 민감한 정보가 있으므로 저장하면 안 됨(메모리에서 사용하고 최대한 빨리 삭제)

#### Pragma

-   캐시 제어(하위 호환)
-   Pragma: no-cache
-   HTTP 1.0 하위 호환

#### Expires

-   캐시 만료일 지정(하위 호환)
-   expires: Mon, 12 Feb 2024 22:51:00 GMT
-   캐시 만료일을 정확한 날짜로 지정
-   HTTP 1.0 부터 사용
-   지금은 더 유연한 Cache-Control: max-age 권장
-   Cache-Control과 같이 사용 시 Expires는 무시

#### 검증 헤더(Validator)

-   ETag
-   Last-Modified

#### 조건부 요청 헤더

-   If-Match, If-None-Match: ETag 값 사용
-   If-Modified-Since, If-Unmodified-Sine: Last-Modified 값 사용

## 프록시 캐시

#### 프록시 캐시

-   프록시는 원(origin) 서버를 대리하여 통신하며 캐시, 로드밸런서 등 중계 역할을 하는 서버이다.
-   CDN(Content Delivery Network), 아마존의 CloudFront 등이 프록시 캐시 서버이다.

#### Cache-Control

-   Cache-Control: public → 응답이 public 캐시에 저장되어도 됨
-   Cache-Control: private → 응답이 해당 사용자만을 위한 것임. private 캐시에 저장해야 함(기본값)
-   Cache-Control: s-maxage → 프록시 캐시에만 적용되는 max-age
-   Age: 60(HTTP 헤더) → 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)

## 캐시 무효화

#### 확실한 캐시 무효화 응답

-   Cache-Control: no-cache, no-store, must-revalidate
    -   Cache-Control: no-cache → 데이터는 캐시해도 되지만, 항상 원 서버에 검증하고 사용
    -   Cache-Control: no-store → 데이터에 민감한 정보가 있으므로 저장하면 안 됨.(메모리에서 사용하고 최대한 빨리 삭제)
    -   Cache-Control: must-revalidate → 캐시 만료 후 최초 조회 시 원 서버에 검증해야 함. 원 서버 접근 실패 시 반드시 오류가 발생(504 Gateway Timeout). 캐시 유효 시간이라면 캐시를 사용함.
-   Pragma: no-cache
    -   HTTP 1.0 하위 호환

#### no-cache, must-revalidate 차이점

-   no-cache인 경우 원 서버에 접근할 수 없을 때 캐시 서버 설정에 따라서 캐시 데이터를 반환할 수 있음. → Error OR 200 OK
-   must-revalidate인 경우 원 서버에 접근할 수 없을 때 항상 오류가 발생함. 매우 중요한 데이터인 경우 사용. → 504 Gateway Timeout
