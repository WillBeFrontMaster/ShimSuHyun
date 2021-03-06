# HTTP

### HTTP(HyperText Transfer Protocol):

WWW의 기초. 하이퍼텍스트 링크를 사용하여 웹 페이지를 로드하는 데 사용. 네트워크로 연결된 장치 간 정보를 전송하도록 설계된 애플리케이션 계층 (상태 비저장)프로토콜.

### HTTP 흐름 :

1. TCP 연결 열기
2. HTTP 메시지 보내기
3. 서버에서 보낸 응답 읽기
4. 추가 요청을 위해 연결을 다시 사용하거나 닫기

### HTTP 요청 :

인터넷 통신 플랫폼이 웹 사이트를 로드하는 데 필요한 정보를 요청하는 방식. <HTTP 버전 유형, URL, HTTP 메소드, HTTP 요청 헤더, (HTTP 요청 본문)> 포함

- HTTP 메소드: HTTP 요청이 쿼리된 서버에서 기대하는 작업. ( GET, POST, …)
- HTTP 요청 흠헤더: key-value 쌍에 저장된 텍스트 정보 포함. 모든 HTTP 요청 및 응답에 포함.
- HTTP 요청 본문: 웹 서버에 제출되는 모든 정보를 포함한 본문

### HTTP 응답 :

웹 클라이언트가 HTTP 요청에 대해 인터넷 서버로부터 수신하는 응답. <HTTP 상태 코드, HTTP 응답 헤더, 선택적 HTTP 본문>

- HTTP 상태 코드: HTTP 요청이 성공적으로 완료되었는 지 여부를 나타내는 데 사용되는 3자리 코드.
    1. 1nn : 정보 제공
    2. 2nn : 성공 (200)
    3. 3nn : 리디렉션
    4. 4nn : 클라이언트 오류
    5. 5nn  : 서버 오류
- HTTP 응답 헤더: 응답 본문에서 전송되는 데이터의 언어 및 형식과 같은 중요한 정보를 전달하는 헤더
- HTTP 응답 본문: 웹 브라우저가 웹 페이지로 변환하는 HTML