# HTTP 메소드

HTTP 는 다양한 메소드를 지원해줍니다.

- GET : 리소스 조회
- POST : 요청 데이터 처리
- PUT : 리소스를 대체, 해당 리소스가 없으면 생성
- PATCH : 리소스 부분 변경
- DELETE : 리소스 삭제
- HEAD : GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
- OPTIONS : 대상 리소스에 대한 통신 기능 옵션을 설명
- CONNECT : 대상 자원으로 식별되는 서버에 대한 터널을 설정
- TRACE : 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

<br>

#### 1) GET 메소드

GET 메소드는 리퀘스트 URI로 식별된 리소스를 가져올 수 있도록 요구하는 메소드입니다. 가져올 리소스 내용은 지정된 리소스를 서버가 해석한 결과입니다.

- 리소스 조회

- 서버에 전달하고 싶은 데이터는 query string을 통해서 전달

- 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많으므로 사용 X

  ( 최근 스펙에서는 바디에 데이터를 넣는 것을 허용했지만, 실무에서는 사용 X )

<br>

#### 2) POST 메소드

POST 메소드는 엔티티를 전송하고 엔티티 처리를 요구하는 메소드입니다. 

- 요청 데이터 처리
- 메시지 바디를 통해 서버로 요청 데이터 전달
- 서버는 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행
- 대상 리소스가 리소스의 고유한 의미 체계에 따라 요청에 포함된 표현을 처리하도록 요청 (정식스펙) 

<br>

#### 3) PUT 메소드

PUT 메소드는 리소스가 있다면 완전히 대체하고, 리소스가 없다면 새롭게 생성해주는 메소드로 파일을 전송하기 위해서 사용됩니다. 리퀘스트 중에 포함된 엔티티를 리퀘스트 URI로 지정된 곳에 보존하도록 요구합니다.

- 리소스가 있으면 대체 없으면 생성 ( 완전히 대체 )
- PUT은 클라이언트가 리소스 위치를 알고 URI 를 지정한다.

<br>

#### 4) PATCH 메소드

PATCH 메소드는 리소스의 일부분만 교체하는 메소드입니다.

- 리소스 부분 변경

<br>

#### 5) DELETE 메소드

DELETE 메소드는 파일을 삭제하기 위해 사용합니다. PUT 메소드와는 반대로 동작하며 리퀘스트 URI로 지정된 리소스의 삭제를 요구합니다.

- 리소스 제거

<br>

#### 6) HEAD 메소드

HEAD 메소드는 GET과 같은 기능이지만 메시지 바디는 돌려주지 않습니다.

- 리소스 조회

<br>

#### 7) OPTIONS 메소드

OPTIONS 메소드는 리퀘스트 URI로 지정한 리소스가 제공하고 있는 메소드를 조사하기 위해 사용합니다.

- 사용 가능한 메소드 조사

<br>

#### 8) TRACE 메소드

TRACE 메소드는 Web 서버에 접속해서 자신에게 통신을 되돌려 받는 루프백을 발생시킵니다.  리퀘스트를 보낼 때 "Max-Forwards" 라는 헤더 필드에 수치를 포함시켜 서버를 통과할 때마다 그 수치를 줄여갑니다. 클라이언트는 TRACE 메소드를 사용함으로써, 리퀘스트를 보낸 곳에 어떤 리퀘스트가 가공되어 있는지 등을 조사할 수 있습니다.

- 경로 추적

<br>

#### 9) CONNECT 메소드

CONNECT 메소드는 프록시에 터널 접속 확립을 요함으로써, TCP 통신을 터널링 시키기 위해서 사용됩니다.

- 프록시에의 터널링 요구

<br>

#### 10) HTTP 메소드 속성

- 안전 (Safe)

  호출해도 리소스를 변경하지 않는 메소드를 `안전`하다고 한다. 계속 호출해서 장애가 나는 경우는 고려하지않고, 해당 리소스가 변하는지만 고려한다.

- 멱등 (Idempotent)

  몇 번 호출하든 결과가 똑같은 메소드를 `멱등`하다고 한다.

  ( 따라서, POST는 멱등이 아니다. 결제하는 경우를 생각해보면 POST는 2번 호출하면 중복 결제가 된다. )

  서버가 정상 응답을 하지 못했을 때, 멱등한 메소드는 자동적으로 다시 호출하도록 복구 메커니즘을 만드는데 사용한다. 

- 캐시가능 (Cacheable)

  응답 결과 리소스를 캐시해서 사용해도 되는 메소드를 `캐시가능`하다고 한다.

![20210222_105441](https://user-images.githubusercontent.com/59816811/108647602-6d580e00-74fc-11eb-9de9-08b6b2428d27.png)

##### Q. PATCH가 멱등이 아닌 이유

​	PATCH 데이터 일부분을 수정할 때 사용하므로, 예를 들어 나이 필드 값을 +10 하는 경우에 이용할 수 있다. 호출될 때마다 나이가 10살씩 많아지므로 멱등하지 않다. 물론, 멱등하게 설계할 수도 있다. 하지만 기본적으로 모든걸 대체해버리는 PUT과 달리 멱등이 아니다.
