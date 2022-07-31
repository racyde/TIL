### 1. 프로젝트에서 ORM을 사용하셨나요? 사용하셨다면 Raw Query를 사용하지 않고 ORM을 활용한 이유는?

시퀄라이즈를 사용했다. raw 쿼리의 경우 데이터베이스에 따른 sql 문법등을 잘 알고 있어야 한다.
그런데 시퀄라이즈는 개발자에 친숙하게 되어 있어서 굳이 sql문법을 통달하지 않아도? 사용하기 쉬워서 접근성이 좋다

### 2. REST한 API에 대해서 자세히 설명?

REST란 http를 기반으로 자원(리소스)을 이름으로 구분하여 리소스의 형태를 공유하는 것을 말한다
API는 클라이언트와 서버 간의 관계 같이 서로 다른 소프트웨어 간에 요청과 응답을 주고 받을 수 있게 만든 체계로

종합하자면, restful api는 rest를 통해 확장성과 재사용성 등 업무 효율을 높히는 규칙을 적용하여 아키텍쳐를 구현하는
웹 서비스를 의미한다고 할 수 있습니다.

restful api의 메소드로는 get, post, put, patch, delete가 있다
get은 데이터 조회, post는 데이터를 서버에 전송, put은 데이터 수정(자원 전체를 갱신), patch는 put과 다르게 해당 자원의 일부만을 교체할때 사용,
delete는 삭제에 용이한 메소드라 할 수 있다

### 3.  CSS에서 position은 어떻게 사용?

position 속성은 html 요소를 배치하는 방법과 관련된 속성이다.
크게 static, relative, absolute, fixed,  sticky 의 값을 쓸 수 있다.

* static

```
static은 element(요소)에 포지션을 지정하지 않았을 때 기본값이며
top, right, z-index 등의 속성이 아무런 영향을 주지 못한다(이거 되게 하려면 position:relative 를 사용)
```

* relative

```
relative는 static과 같이 요소가 일반적인 흐름으로 배치되나
요소가 top, right 등과 같은 속성에 의해 상대적인(relative) 위치에 배치된다는 차이점이 존재
(단, relative로 지정한 요소는 다른 요소들의 위치에 영향을 미치진 않는다)
-> 즉, relative가 적용된 요소의 배치만 변경되고 다른 부분의 배치에는 영향을 미치지 않는다
```

* absolute

```
absolute는 요소가 문서의 일반적인 흐름으로 배치되지 않고, 가장 가까운 위치에 있는 부모 요소(element)에 대해
상대적 위치로 배치되는 속성이다
-> 조상 요소가 없으면 문서 본문(body)를 기준으로 삼고 페이지 스크롤에 따라 움직이게 된다
```

* fixed

```
fixed는 absolute와 마찬가지로 일반적인 흐름으로 배치되지 않고, 스크린의 '뷰포트'를 기준으로 한 위치에 배치된다.
-> 즉, 스크롤 되어도 움직이지 않는 고정된 위치에 존재(뷰포트: 웹페이지가 사용자에게 보여지는 영역)
```

* sticky

```
sticky는 요소가 일반적인 흐름으로 배치가 되며, top, left, bottom과 같은 속성의 값을 기준으로 해당 요소를 포함하는 컨테이닝 블록에
대한 상대적 위치에 배치됨
-> 언뜻 fixed와 비슷하나 sticky는 문서의 일반적인 흐름을 따르면서 컨테이닝 박스를 기준으로 상대적인 위치에 배치된다는 차이가 있다
(주로 fixed를 사용하면 요소들이 겹쳐보이는 상황이 나올 수 있는데, sticky는 그 상황을 예방할 수 있다)
```

### 4. 클로저란 무엇이며, 왜 이러한 패턴을 사용하는지?

간단하게 함수 안의 함수가 있는 패턴을 말한다. 먼저 선언된 함수를 외부함수라하고 이후에 정의된 함수를 내부함수라 칭하는데,
내부 함수는 자신이 선언되었을 때의 환경인 스코프를 기억하여 그 스코프 밖에서 호출 되었을 때도 그 환경에 접근할 수 있다.
즉, 자신이 생성될 때의 환경을 기억하는 함수라 할 수 있다.

이를 통해 

```
1) 전역 변수의 사용을 억제할 수 있고
2) 정보를 은닉할 수 있으며
3) 현재 상태를 기억하고 변경된 최신 상태를 유지할 수 있다
```

* 단점: 메모리 성능?
-> 이 환경을 기억하기 위해서는 메모리가 소모되므로, 클로저 사용이 끝나면 참조를 제거하는것이 좋다

### 5. http 메소드에서 get과 post 메소드의 차이?

get과 post는 http 메소드 중의 하나이다.
(http는 클라이언트와 서버 간 데이터를 주소받는데  사용되는 요청/응답 프로트콜)

* get

```
get 은 서버로부터 데이터를 조회하기 위해 사용되는 http 메소드
요청을 전송할 때 필요한 데이터를 body에 담지 않고 쿼리스트링을 통해 전송함
(쿼리스트링이란? : URL의 끝에 ?와 함께 이름과 값으로 쌍을 이루는 요청 파라미터, 요청 파라미터가 여러 개이면 &로 연결)
-> 불필요한 요청을 제한하기 위해 요청이 캐시될 수 있다
(js, css, 이미지 같은 정적 컨텐츠는 데이터 양 크고 변경될 일이 적어서 변경될 일이 적어서 
반복해서 동일한 요청을 보낼 필요가 없는데, 이 때문에 정적 컨텐츠를 요청하고 나면 브라우저에서는 요청을 캐시해두고, 
동일한 요청이 발생할 때 서버로 요청을 보내지 않고 캐시된 데이터를 사용하는 경우)
```

* post

```
post는 서버로 데이터를 보내기 위해 사용되는 http 메소드
get과 달리 전송 데이터를 body에 담아서 전송
대용량 데이터를 전송 가능
요청 헤더의 content-type에 요청 데이터의 타입을 정의해야 함
(데이터 타입을 알 수 없는 경우 application/octet-stream로 요청을 처리한다)
```

-> get과 post의 차이는 get의 경우 동일한 연산을 여러번 수행하더라도 동일한 결과가 나오게 설계 되었고
post는 그렇지 않게 설계 되었다는 점이다.(설계 원칙이)

그렇기 때문에, **GET은 주로 조회를 할 때 사용**되어야 하고, **post는 동일한 요청을 해도 다른 응답**을 받을 수 있으므로,
주로 서버의 상태가 **데이터를 변경시킬 때 사용**되어야 한다.


### 6. this에 대해서 설명?

this는 어디에서 사용되느냐에 따라서 가리키는 대상이 다르다

```
* 전역 범위에서 사용될 때는 this는 전역 객체
* 함수에서 사용될 때도 전역 객체
* 단, 객체에 속한 메서드에서 사용될 때는 그 메서드의 객체를 가르킴
* 다만, 객체에 속한 메서드의 내부 함수에서 사용될 때는 전역 객체를 가르킨다
* 생성자에서 사용될 때는 생성자로 인해 생성된 새로운 객체를 가르킨다.
```

#### 6-1. call, apply, bind 에 대해서 설명?

3가지 방법은 this를 바인딩하기 위한 방법이다.
this가 함수 호출식에 따라 객체를 가르켰다면, call, apply, bind는 함수가 직접 실행 문맥을 결정한다

* call, apply: 함수를 호출해서 실행

```
call, apply의 장점은  첫번째 인자가 없더라도 에러없이 실행이 가능하다는 장점(자동으로 전역객체를 지정하여 실행)
call은 this를 바인딩하면서 함수를 호출한다. 두번째 인자를 apply와 다르게 하나씩 넘기는 것
apply는 this를 바인딩하면서 함수를 호출하는 것, 두번째인자가 배열
```

* bind: this가 바인딩된 새로운 함수를 만들어줌(리턴)

```
bind는 함수를 호출하는 것이 아닌 this가 바인딩 된 새로운 함수를 리턴함.(bind는 지정한 객체의 함수를 만든다.)
```

=> 정리하자면 call,apply는 함수를 실행시켜주는 것이고 bind는 새로운 함수를 만들어 주는 것이다.


### 7. 브라우저 저장소에 대한 차이점?

크게, 브라우저 저장소는 쿠키, 로컬스토리지, 세션스토리지가 있다.
보통 로컬스토리지, 세션스토리지를 합쳐서 웹스토리지라고 부름
(웹스토리지란? 웹의 데이터를 클라이언트에 키/값 쌍으로 저장, 키를 기반으로 데이터를 조회할 수 있는 패턴의 자료구조를 말함)


* 쿠키

```
* 서버와 계속해서 통신하며, 웹 스토리지 보다 저장할 수 있는 용량이 적음(용량에 제한이 있음)
```

* 웹스토리지

```
* 용량에 제한이 없음, 단순 문자열을 넘어 객체 정보를 저장할 수 있다. 쿠키와 마찬가지로 도메인 단위로 접근이 제한됨
(a 도메인에서 저장한 데이터는 b 도메인에서 조회할 수 없다는 뜻... 당연한 거지만)
* 로컬 스토리지, 세션 스토리지 등이 이에 포함
```

* 로컬 스토리지

```
* 키-밸류 쌍으로 저장. 강제로 지우지 않는 한 정보는 계속해서 저장되어 있음
(브라우저를 닫아도 데이터는 저장되어 있음)
```

* 세션 스토리지

```
* 키-밸류 쌍으로 저장. 세션이 만료될 때 마다(브라우저 닫을 때 마다) 정보가 삭제 됨
(세션 스토리지의 경우 같은 도메인이라 할 지라도 브라우저가 다르면 서로 다른 영역이 됨. 브라우저 컨텍스트가 다르기 때문이다)
-> 도메인만 같으면 전역적으로 공유가능한 로컬 스토리지와의 차이점
```