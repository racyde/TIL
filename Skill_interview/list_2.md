### 1\. 브라우저에서 URL을 입력하고 요청한 페이지를 볼 때까지 어떤 일이 일어나는지 설명해주세요.

1.브라우저에 URL입력하면,  
2.URL을 IP 주소로 변환 (3계층)  
(URP로는 컴퓨터끼리 통신이 불가하기 때문에, 컴퓨터가 읽을수 있는 IP주소로 변경 필요.  
브라우저에서 자신의 로컬 hosts파일과 브라우저 캐시에 해당하는 URL이 존재하는지 확인,  
존재하지 않는다면 DNS서버에 요청하여 URL을 IP주소로 변경)  
3.해당 서버에 요청 (라우터를 통해 경로를 찾아가고, ARP를 통해 IP주소를 물리 주소인 MAC주소로 변경 )  
4.대상 서버와 TCP 통신을 통해 소켓을 염(https의 경우 handshakde추가)  
5.연결 완료되었으니 해당페이지에 요청 및 응답(https, http)  
6.그 이후 브라우저 렌더링을 하는 과정을 거침

#### 1-1. 브라우저의 렌더링 과정?

렌더링이란 HTML, CSS, javascript 등 개발자가 작성한 문서를 브라우저에서 출력하는 과정을 말한다

그 과정을 세부적으로 설명하면

```
1. 예를 들어 주소창에 google.com을 치면, 브라우저에서 자신의 로컬hosts 파일과 브라우저 캐시에 해당 url이 존재하는지 확인하여
존재하지 않는다면 dns 서버에 요청하여 url을 ip주소로 변환한다
2. 그리고 해당 서버에 요청을 하게됨(라우터를 통해 경로를 찾아가고 ARP(주소 결정  프로토콜)를 통해 ip를 mac주소로 변경)
3. 대상 서버와 tcp 통신을 통해 소켓을 열고
4. 연결이 완료되면 해당 페이지에 http 또는 https 프로토콜로 요청 및 응답을 통해 해당 파일들을 받습니다.
**(여기까지가 앞서 설명한 렌더링 이전 과정)**

**(여기서부터 렌더링 과정)**
5. 브라우저는 html 파싱을 시작
6. 해당 파일을 한줄씩 읽으면서 dom 트리를 구축합니다.
7. 중간에 link 태그를 만나면 잠시 멈춰서 css 파싱을 시작
8. css 파싱이 끝나면 다시 html 파싱을 하면서 dom 트리를 완성함
9. 그렇게 완성한 dom 트리와 cssom 트리를 합쳐서 렌더 트리를 만들고
10. 렌더 트리를 배치합니다(각 노드가 화면에 정확한 위치에 표시되도록 하는 과정)
11. 그리고 렌더 트리를 그리는 과정을 통해 브라우저가 서버에 요청한 내용들을 픽셀화 시키는 것이 바로 브라우저 렌더링입니다.

( dom 트리 구축을 위한 html 파싱 -> 렌더 트리 구축 -> 렌더 트리 배치 -> 렌더 트리 그리기(렌더링))
```

### 2\. HTTP와 HTTPS의 차이점은 무엇인지 설명해주세요.

http에 s인 보안(데이터 암호화)가 추가된 프로토콜  
네트워크 상에서 제3자가 정보를 볼 수 없도록 암호화를 지원한다  
대칭키, 비대칭키 암호화

HTTP는 암호화가 추가되지 않았기 때문에 보안에 취약한 반면,  
HTTPS는 안전하게 데이터를 주고받을 수 있다.  
하지만 HTTPS를 이용하면 암호화/복호화의 과정이 필요하기 때문에 HTTP보다 속도가 느리다

개인 정보와 같은 민감한 데이터를 주고 받아야 한다면 HTTPS를 이용해야 하지만,  
노출이 되어도 괜찮은 단순한 정보 조회 등 만을 처리하고 있다면 HTTP를 이용하면 된다.

### 3\. CORS란 무엇이고 목적은 어떻게 되는지 설명해주세요.

Cross-Origin Resource Sharing, CORS는 (추가 http 헤더를 사용해서)  
다른 출처의 리소스에 접근할 수 있도록 권한 설정을 하는 것이라 할 수 있다.

보통 same-site origin 정책에 의해서 cors 같은 상황이 발생하면 외부 서버에서 요청한 데이터를 브라우저에서  
보안 목적으로 차단하기에 정상적으로 데이터를 받을 수가 없다(cors 위반 cors에러?)

보통 이 경우에 cors 설정을 통해서 출처가 다른 경우에도 리소스 공유를 하게 할 수 있다  
nodeJS에서는 cors미들웨어 패키지를 설치해서 해당 모듈을 임포트해서 사용함으로, 간편하게 CORS 설정을 할 수 있다.

### 4\. CSR과 SSR의 개념적 차이와 동작 방식을 구분하여 설명해주세요.

-   클라이언트 사이드 렌더링: 브라우저가 렌더링을 진행하는 것

```
최초 로딩 시에 브라우저가 서버에서 html, js, css등의 각종 리소스를 받아서 분석하고 표현함
서버 부담이 적다
대신 html이 비어있어서 seo(검색최적화)에 불리
초기 로딩 속도는 느리지만 빠른 페이지 전환을 제공함
쿠키나 로컬 스토리지에 사용자 정보를 저장하기 때문에 xss 공격에 취약함
```

-   서버 사이드 렌더링: 서버가 렌더링을 진행하는 것

```
서버가 연산등을 다해서 마지막에 출력하는 결과만 브라우저에게 전달함
초기 로딩 속도는 빠른 편이지만 페이지 전환등이 느림(매번 그때마다 요청을 함)
서버 부담 증가
html에 정보가 포함되어 있어서 seo에 유리
```

### 5\. React 라이브러리에서 제공하는 기본 내장 API 함수에 대해서 설명해주세요.

useState, useEffect 등이 있다.

상태를 관리할 때 많이 쓰는게 useState고 useEffect의 경우, 리액트의 라이프 사이클과 관련이 깊은데,  
렌더링이 발생할 때마다, 매번 이펙트가 실행됨. 또는 두번째 파라미터를 이용해서 그거를 조정할 수도 있다.

### 6\. React를 사용하여 웹 프론트엔드를 구축할 때, 상태(state)로 관리해야 하는 데이터의 성질에 대해 설명해주세요.

상태로 관리해야하는 데이터는 서버에서 불러오는 데이터나 사용자가 이벤트를 실행했을 때  
생기는 데이터 등 사용에 따라 변화하는 데이터는 상태로 관리

```
1. 동적으로 변하는 데이터(변할 수 있는 값): 서버에서 불러오는 데이터나 사용자가 특정 이벤트를 실행했을때 생기는 데이터 등
사용에 따라 변화하는 데이터
2. 마찬가지로 컴포넌트 내에서 변경이 가능한 값    <- 쓸 수 있다
3. 외부에서부터 전달받은 데이터가 아니어야 함(props x)    <- 프롭스는 읽기 전용
4. 또한, props를 통해 계산이 되지 않는 데이터여야 함
```

이를 예로 들면,  
쇼핑몰에서 물건을 살 때 물건 리스트에서 선택된 값, 체크박스에서 체크된 값, 텍스트 박스의 텍스트 등이 될 수 있음