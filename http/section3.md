# HTTP 기본

## 모든 것이 HTTP

#### HTTP(HyperText Transfer Protocol)

-   HTTP 메시지에 모든 것을 전송
-   HTML, TEXT
-   IMAGE, 음성, 영상, 파일
-   JSON, XML(API)
-   거의 모든 형태의 데이터 전송 가능
-   서버 간에 데이터를 주고 받을 때 대부분 HTTP 사용

#### HTTP 역사

-   HTTP/0.9: 1991년 GET 메서드만 지원, HTTP 헤더 X
-   HTTP/1.0: 1996년 메서드, 헤더 추가
-   **HTTP/1.1: 1997년 가장 많이 사용, 가장 중요한 버전**
    -   RFC2068(1997) → RFC2616(1999) → RFC7230 ~ 7235(2014)
-   HTTP/2: 2015년 성능 개선
-   HTTP/3 진행 중: TCP 대신에 UDP 사용, 성능 개선

#### 기반 프로토콜

-   TCP: HTTP/1.1, HTTP/2
-   UDP: HTTP/3
-   크롬 브라우저 network 탭에서 protocol 항목으로 확인 가능
    -   HTTP/1.1 → http/1.1
    -   HTTP/2 → h2
    -   HTTP/3 → h3-29

#### HTTP 특징

-   클라이언트 서버 구조
-   무상태 프로토콜(Stateless), 비연결성
-   HTTP 메시지
-   단순함, 확장 가능
