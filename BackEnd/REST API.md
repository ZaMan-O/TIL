[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&pause=1000&color=3AF7DC&center=true&vCenter=true&width=435&lines=REST+API%EC%97%90+%EB%8C%80%ED%95%B4%EC%84%9C)](https://git.io/typing-svg)
# REST
HTTP URI를 통해 자원을 명시하고, HTTP 메서드(GET, POST, PATCH, PUT, DELETE)를 통해 자원에 대한 CRUD(Create, Read, Update, DELETE)를 적용하는 것을 의미합니다.
## REST의 특징
### 서버-클라이언트 구조
- 자원이 있는 쪽이 서버, 자원을 요청하는 쪽이 클라이언트가 됩니다.
- REST 서버는 API를 제공하고 비즈니스 로직을 처리 및 저장을 책임이고, 클라이언트는 사용자 인증이나 세션, 로그인 정보 등을 직접 관리하고 책임집니다.
### Stateless (무상태)
- 클라이언트의 context를 서버에 저장하지 않습니다.
- 세션, 쿠키와 같은 context 정보를 신경쓰지 않아도 되므로 구현이 단순해지지만 요청마다 context 정보를 다시 보내야 할 수도 있습니다.
# REST API
REST를 기반으로 서비스 API를 구현한 것으로 백엔드와 프론트엔드, 서버와 클라이언트가 데이터를 주고받기 위해 사용하는 방식입니다.
## REST API의 특징
REST 기반으로 시스템을 분산해 확장성과 재사용성을 높여 유지보수 및 운용을 편리하게 할 수 있습니다.
## REST API 기본 설계 규칙
URI에 동사보다는 명사를, 대문자보다는 소문자를 사용합니다.
<br>그리고 URI에 HTTP 메서드가 들어가면 안됩니다.

올바른 예시) GET 메서드, http://localhost:8080/articles
<br>틀린 예시) GET 메서드, http://localhost:8080/articles/get

슬래시 구분자(/)는 계층 관계를 나타내는데 사용합니다.

**그러나 URI 경로의 마지막에는 슬래시(/)를 사용하지 않습니다.**

올바른 예시) http://localhost:8080/articles
<br>틀린 예시) http://localhost:8080/articles/

언더바(_) 대신 하이픈(-)은 URI 가독성을 높이는데 사용됩니다.

# RESTful API
RESTful API는 REST 원칙을 충실히 지켜서 만든 API를 말합니다.
<br>즉 RESTful은 "얼마나 REST답게 만들었는가"를 의미합니다.
## RESTful API의 특징
- URL만 보고 어떤 자원에 접근할 것인지, 메소드를 보고 어떤 행위를 할지 알 수 있기 때문에, 개발을 할때 용이합니다.
- 1개의 URI로 4개의 행위(CRUD)를 명시할 수 있기 때문에, 굉장히 효율적입니다.