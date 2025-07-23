# Spring filter
Spring filter는 웹 애플리케이션에서 HTTP 요청과 응답을 가로채서 처리할 수 있는 컴포넌트입니다.

서블릿 기반의 스프링 애플리케이션에서 동작하며, 스프링 자체보다는 Servlet 스펙의 일부입니다.

Filter는 DispatcherServlet보다 먼저 실행됩니다.

    Client → Filter → DispatcherServlet → Controller → DispatcherServlet → Filter → Client
즉, 요청이 DispatcherServlet까지 도달하기 전에 필터를 통해 먼저 가공하거나 검사할 수 있고, 응답을 보낼 때도 가공할 수 있습니다.

## Filter의 대표적인 활용 예
- 로그 출력 : 요청 시간, 요청 URI 등을 콘솔에 출력
- 인코딩 처리: 요청 파라미터 인코딩 강제 설정
- 보안 검사: 토큰 검사, IP 차단 등
- CORS 처리: 특정 도메인만 요청 허용
## 동작 흐름
1. 클라이언트가 HTTP 요청을 보낸다.
2. 서버는 필터 체인을 따라 등록된 필터들을 순서대로 실행한다.
3. 각 필터는 요청을 검사하거나 가공하고, 다음 단계로 넘긴다.
4. 모든 필터를 거치면 DispatcherServlet이 요청을 받아 처리한다.
5. 응답이 생성된 후, 필터들을 역순으로 다시 거쳐 클라이언트로 응답이 전달된다.