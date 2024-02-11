# URI와 웹 브라우저 요청 흐름

## URI

#### URI? URL? URN?

-   URI: 리소스를 식별한다. → 자원이 어디 있는 지 자원 자체를 식별하는 방법이다.
-   URL: 리소스의 위치 → 리소스가 위치하고 있는 경로
-   URN: 리소스의 이름 → 리소스 그 자체
-   URI는 로케이터(Locator), 이름(Name) 또는 둘 다 추가로 분류될 수 있다.
-   URI(Resource Identifier)는 URL(Resource Locator), URN(Recourse Name)을 포함한다.

#### URI

-   Uniform: 리소스를 식별하는 통일된 방식
-   Resource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
-   Identifier: 다른 항목과 구분하는데 필요한 정보

#### URL, URN

-   Locator: 리소스가 있는 위치를 지정
-   Name: 리소스에 이름을 부여
-   위치는 변할 수 있지만, 이름은 변하지 않는다.
-   URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화되지 않았다.
-   일반적으로 URI = URL 이다.

#### URL

-   scheme://[userinfo@]host[:port][/path][?query][#fragment]
    <br>
-   scheme: 프로토콜 → http, https
-   userinfo: URL에 사용자 정보를 포함해서 인증, 거의 사용하지 않음
-   host: 호스트명 → www.example.com
-   port: 포트 번호 → 80, 443, 생략 가능
-   path: 리소스 경로, 계층적 구조 → /test
-   query: 쿼리 파라미터, key=value 형태, ?로 시작, &로 추가 가능 → word=hello-world&test=hi
-   fragment: html 내부 북마크 등에 사용, 서버에 전송하는 정보 아님 → #content

## 웹 브라우저 요청 흐름

#### 웹 브라우저 요청 흐름

1. 웹 브라우저에 https://www.example.com:443/search?q=hello 입력
2. www.example.com 도메인을 DNS 서버에 검색해서 서버 IP 주소를 찾음
3. 찾은 서버 IP 주소와 포트 정보 등을 조합하여 웹 브라우저가 HTTP 메시지 생성
4. SOCKET 라이브러리를 통해 TCP/IP 계층에 전달
5. 3-way-handshake로 서버와 연결
6. TCP/IP 패킷을 생성하여 서버로 전달. 이때 HTTP 메시지를 데이터에 포함
7. 서버는 패킷이 도착하면 내부 HTTP 메서드를 해석하여 정보에 맞는 처리 진행
8. 서버에서 HTTP 응답 메시지 생성
9. 웹 브라우저는 응답 메시지를 해석하여 동작
