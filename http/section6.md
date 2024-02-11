# HTTP 상태 코드

## HTTP 상태 코드 소개

#### 상태 코드

-   클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능
-   1xx(Informational): 요청이 수신되어 처리 중
-   2xx(Successful): 요청 정상 처리
-   3xx(Redirection): 요청을 완료하려면 추가 행동이 필요
-   4xx(Client Error): 클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
-   5xx(Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못함

#### 만약 모르는 상태 코드가 나타나면?

-   클라이언트가 인식할 수 없는 상태 코드를 서버가 반환하는 경우 상위 상태 코드로 해석해서 처리하면 됨.