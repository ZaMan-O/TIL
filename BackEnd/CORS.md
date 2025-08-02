# CORS란?
Cross-Origin Resource Sharing(교차 출처 리소스 공유)의 약자로
<br>CORS는 쉽게 말하면 브라우저가 보안상 다른 **출처**의 리소스를 요청하지 못하게 막는 정책입니다.

여기서 출처(origin)는 다음 세 가지를 기준으로 판단됩니다.
- 프로토콜 (HTTP / HTTPS)
- 도메인 (예시: example.com)
- 포트 (예시: **:1234, :8080**)

이 세 가지 중 하나라도 다르면 "다른 출처"로 인식합니다.

## CORS는 왜 발생할까?
예시로 http://localhost:3000에서 프론트엔드를 개발 중일때
<br>http://localhost:8080로 열린 Spring Boot 백엔드 서버에 프론트가 데이터를 요청하면 아래와 같은 에러가 뜹니다.

    Access to fetch at 'http://localhost:8080/api/user' from origin 'http://localhost:3000' has been blocked by CORS policy.
이 에러가 **브라우저가 CORS 정책에 따라 요청이 차단된 것입니다.**
브라우저는 보안상, 자바스크립트가 다른 출처의 서버에 요청을 보내는 걸 막습니다.

서버가 이 요청을 허용하지 않으면, 브라우저가 요청 자체를 막아버립니다.

# CORS 작동 원리
CORS에는 두 가지 종류가 있습니다.

## Simple Request
- GET, POST, HEAD 메서드
- Content-Type이 text/plain, application/x-www-form-urlencoded, multipart/form-data
- 커스텀 헤더 없음

이 경우, 브라우저는 바로 서버에 요청을 보냅니다.
요청 헤더에는 다음이 포함됩니다

    Origin: http://localhost:3000
서버는 응답 시 다음과 같은 헤더를 반환해야 합니다.

    Access-Control-Allow-Origin: http://localhost:3000
이 응답이 없으면 브라우저는 데이터를 차단하고 에러를 발생시킵니다.

## Preflight Request (사전 요청)
요청이 다음 중 하나라도 해당되면, 브라우저는 먼저 OPTIONS 메서드로 Preflight 요청을 보냅니다.
- 메서드가 PUT, DELETE, PATCH 등일 경우
- 헤더에 Authorization, X-Requested-With 같은 커스텀 헤더가 포함될 경우
- Content 타입이 application/json일 경우

### 요청 예시
    OPTIONS /user HTTP/1.1
    Origin: http://localhost:3000
    Access-Control-Request-Method: PUT
    Access-Control-Request-Headers: Authorization
서버는 위 Preflight 요청에 다음과 같이 응답해야 합니다.

    HTTP/1.1 200 OK
    Access-Control-Allow-Origin: http://localhost:3000
    Access-Control-Allow-Methods: GET, POST, PUT, DELETE
    Access-Control-Allow-Headers: Authorization
    Access-Control-Allow-Credentials: true
이 응답이 만족스럽지 않으면 브라우저는 다음 요청을 보내지 않습니다.

요청이 통과하면 브라우저는 실제 요청을 전송합니다.

    PUT /user HTTP/1.1
    Origin: http://localhost:3000
    Authorization: Bearer abc.def.ghi
이 요청은 서버의 비즈니스 로직에 따라 처리되고, 서버는 응답 시 다시 다음과 같은 CORS 헤더를 반환해야 합니다.

    Access-Control-Allow-Origin: http://localhost:3000
    Access-Control-Allow-Credentials: true

브라우저는 응답을 수신한 뒤, CORS 정책을 다시 확인합니다.

응답에 Access-Control-Allow-Origin이 내 origin과 일치하는가 등을 확인하고 이 조건이 만족되지 않으면, 브라우저는 응답 본문을 차단하고 CORS 에러를 띄웁니다.

## 흐름 요약
    [클라이언트 요청]
           ↓
    브라우저: 출처 비교 → CORS 판단
           ↓
    [Simple Request]          [Preflight Request]
           ↓                           ↓
        서버 응답                OPTIONS 사전 요청
           ↓                           ↓
     CORS 통과 확인      ←      서버가 허용 헤더 응답
            ↓
     실제 요청 전송
            ↓
     서버 응답 + CORS 헤더
            ↓
     브라우저 최종 판단 → 응답 허용 or 차단

# Spring Boot에서 CORS 해결 방법
## 전역 설정 (모든 API에 CORS 허용)
    @Configuration
    public class WebConfig implements WebMvcConfigurer {
        @Override
        public void addCorsMappings(CorsRegistry registry) {
            registry.addMapping("/**")  // 모든 경로
                .allowedOrigins("http://localhost:3000") // 프론트 주소
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                .allowedHeaders("*")
                .allowCredentials(true);
        }
    }
## 특정 컨트롤러에서만 허용
    @CrossOrigin(origins = "http://localhost:3000")
    @RestController
    public class MyController {
        컨트롤러 내용
    }