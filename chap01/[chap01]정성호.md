## 1장 사용자 수에 따른 규모 확장성

### - <span style="background-color:#fff5b1">_단일서버_</span>
> 모든 컴포넌트(앱, 웹, 데이터베이스, 캐시 등)가 단 한대의 서버에서 실행되는 간단한 시스템

![](https://velog.velcdn.com/images/kjeng7897/post/da3fda21-1382-4d1e-b4a5-c50fc1fc3c16/image.png)


>1. 사용자가 Domain name을 통해 웹사이트에 접속
2. DNS서버를 통해 Domain name 과 1대1 매칭되는 웹서버 ip주소 반환
3. 해당 ip 주소로 HTTP 요청 전송
4. 웹 서버가 HTML 페이지나 JSON 형태의 응답 반환

### - <span style="background-color:#fff5b1">_데이터베이스_</span>
> 사용자가 늘면 서버 하나로는 충분하지 않아 여러 서버를 두게 된다. 하나는 웹/모바일 트래픽 처리 용도, 다른 하나는 데이터베이스용으로 둔다.
>> 서버를 분리하면 각각을 독립적으로 확장해 나갈 수 있어 유리하다.

![](https://velog.velcdn.com/images/kjeng7897/post/0a0e4b3e-7d95-40f7-9c30-be801e320ff4/image.png)

> #### <span style='background-color:#f7ddbe'>**_어떤 데이터 베이스를 사용할 것인가?_**</span> **RDBMS vs NoSQL**
>> |--|RDBMS|NoSQL|
|------|---|---|
|특징|조인연산 지원|조인연산을 지원 X|
|용도|정형화된 데이터를 다룰 때|비정형 데이터를 다룰 때|
|예시|MySQL, PostgreSQL|CouchDB, Amazon DynamoDB|

>#### <span style='background-color:#f7ddbe'>**_수직적 규모 확장 vs 수평적 규모 확장_**</span>
>> 수직적 규모 확장(scale up) : 서버에 고사양 자원을 추가하는 행위
>> 수평적 규모 확장(scale out) : 서버를 추가헤 성능을 개선하는 행위

### - <span style="background-color:#fff5b1">_로드밸런서_</span>

> 부하 분산 집합(load balancing set)에 속한 웹 서버들에게 트래픽 부하를 고르게 분산하는 역할

![](https://velog.velcdn.com/images/kjeng7897/post/1f976379-88cf-4525-9c7a-1cdc3f42fdc0/image.png)
> 사용자는 웹서버에 직접 접속하지 않고 로드밸런서의 Public ip 주소를 통해서 서버에 접속하게 된다.

>#### <span style='background-color:#f7ddbe'>_**데이터베이스 다중화**_</span>
>> DB 서버를 다중화 하고 서버 사이에 주(master)-부(slave) 관계를 설정해 주 DB서버에서 쓰기연산을 수행하고 부 DB서버에서 읽기 연산 수행
* 주 서버가 다운되면 부 서버1개를 주 서버로 전환


![](https://velog.velcdn.com/images/kjeng7897/post/759a3d60-e60f-48fb-8e98-6b20292b45a1/image.png)
>#### <span style='background-color:#f7ddbe'>_**다중화로 인한 이점**_</span>
>> 1. 더 나은 성능(better functionality)
2. 안정성(reliability)
3. 가용성(availability)


 ### - <span style="background-color:#fff5b1">_캐시_</span>
 
> 값비싼 연산결과 또는 자주 참조되는 데이터를 메모리 안에 두고, 뒤 이은 요청이 이보다 빨리 저장될 수 있도록 하는 저장소

>#### <span style='background-color:#f7ddbe'>_**캐시계층**_</span>
>> 데이터가 잠시 보관되는 곳으로 데이터베이스보다 훨씬 빠르게 데이터에 접근 가능하다.
![](https://velog.velcdn.com/images/kjeng7897/post/a07f3076-69cb-4b23-a373-a1f7291e1a96/image.png)
읽기 주도형 전략(read-through caching startegy)
>>
1. 요청을 받으면 웹 서버는 캐시를 살펴보고 있으면 캐시에서 데이터를 읽어 클라이언트에게 반환한다.
>>
2-1. 캐시에 데이터가 없으면 DB에서 데이터를 읽어 캐시에 쓴다.
>>
2-2. 해당 데이터를 클라이언트에게 반환한다.

>#### <span style='background-color:#f7ddbe'>_**캐시 사용시 고려할 점**_</span>
>> - 캐시는 어떤 상황에 바람직한가?
>>- 어떤 데이터를 캐시에 두어야 하는가?
- 캐시에 보관된 데이터는 어떻게 만료되는가?
- 일관성(consistency)는 어떻게 유지되는가?
- 장애에는 어떻게 대처할 것인가?
- 캐시 메모리는 얼마나 크게 잡을 것인가?
- 데이터 방출(eviction) 정책은 무엇인가?

### - <span style="background-color:#fff5b1">_CDN_</span>

> <U>**정적 콘텐츠**</U>를 전송하는 데 쓰이는, 지리적으로 분산된 서버의 네트워크. 이미지, 비디오, CSS,  JavsScript 파일 등을 캐시할 수 있다.

![](https://velog.velcdn.com/images/kjeng7897/post/af9805c6-c023-461f-a9ef-f6153b968a9b/image.png)
> CDN 의 동작순서는 그림과 같다.
>#### <span style='background-color:#f7ddbe'>_**CDN 사용시 고려할 점**_</span>
>>- 비용 : CDN은 보통 3사업자에 의해 운영되므로 과금을 고려해야한다.
- 적절한 만료 시한 설정 : 만료 시점이 너무 길면 콘텐츠의 신선도가 떨어지고 너무 짧으면 원본 서버에 번번히 접속하게 될 것이다.
- CDN 장애에 대한 대처 방안
- 콘텐츠 무효화(invalidation) : 만료되지 않은 콘텐츠라도 무효화를 통해 CDN 에서 제거할 수 있다. 

>#### <span style='background-color:#f7ddbe'>_**CDN이 추가된 설계**_</span>
![](https://velog.velcdn.com/images/kjeng7897/post/76c23694-eb18-412c-a466-4e91d49c8e80/image.png)


### - <span style="background-color:#fff5b1">_무상태 웹 계층_</span>
> 상태 정보(사용자 세션 데이터와 같은)를 웹 계층에서 제거하여 웹 계층을 수평적으로 확장하는 방법 
>
>#### <span style='background-color:#f7ddbe'>_**사용 가능 전략**_</span>
상태 정보를 관계형DB나 NoSQL 같은 지속성 저장소에 보관하고, 필요할 때 가져오도록 한다.


>#### <span style='background-color:#f7ddbe'>_**상태 정보 의존적인 아키텍쳐 VS 무상태 아키텍쳐**_</span>
<p align="center" style="color:black">
  <!-- 마진은 위아래만 조절하는 것이 정신건강에 좋을 듯 하다. 이미지가 커지면 깨지는 경우가 있는 듯 하다.-->
  <img style="margin:0px 0px" src="https://velog.velcdn.com/images/kjeng7897/post/efa85856-d73c-45b9-9fa7-0b8aee789cbe/image.png" width=600/>
  상태 의존적인 아키텍쳐
</p>
<p align="center" style="color:gray">
  <!-- 마진은 위아래만 조절하는 것이 정신건강에 좋을 듯 하다. 이미지가 커지면 깨지는 경우가 있는 듯 하다.-->
  <img style="margin:0px 0px" src="https://velog.velcdn.com/images/kjeng7897/post/6b70111d-29a6-4384-9437-20f87b2b194a/image.png" width=400/>
  무상태 아키텍쳐
</p> 

>#### <span style='background-color:#f7ddbe'>_**상태 의존적인 아키텍쳐**_</span> 
- 상태 정보를 보관하는 서버는 클라이언트 정보, 즉 상태를 유지하여 요청들 사이에 공유되도록 한다.
- 사용자 A를 인증하기 위해서 반드시 서버 1로 HTTP 요청이 서버1로 전송되어야 하고 서버2로 전송되면 인증이 실패한다.
- 문제는 같은 클라이언트로부터의 요청은 항상 같은 서버로 전송되어야 한다는 것이다.
> 
> #### <span style='background-color:#f7ddbe'>_**무상태 아키텍쳐**_</span>
- 사용자로부터의 HTTP 요청은 어떤 웹 서버로도 전달될 수 있다.
- 웹 서버는 상태 정보가 필요할 경우 공유 저장소(shared storage)로부터 데이터를 가져온다.

>#### <span style='background-color:#f7ddbe'>_**무상태 웹계층을 갖는 설계**_</span>
![](https://velog.velcdn.com/images/kjeng7897/post/3b48b29b-d960-4a61-9ab7-3b4e31301862/image.png)
- 세션 데이터를 웹 계층에서 분리하고 지속성 데이터 보관소에 저장한다.
- 상태 정보가 웹 서버들로부터 제거되었으므로, 트래픽 양에 따라 웹 서버에 넣거나 빼기만 하면 자동으로 규모를 확장할 수 있다.

### - <span style="background-color:#fff5b1">_데이터 센터_</span>
>장애가 없는 상황에 사용자는 지리적 라우팅을 통해서 가까운 데이터 센터로 연결된다.
![](https://velog.velcdn.com/images/kjeng7897/post/7aaeab1d-1ba1-4edd-b0b9-750b4ea55026/image.png)

>#### <span style='background-color:#f7ddbe'>_**다중 데이터센터 아키텍쳐 설계 시 해결해야할 기술적 난제**_</span>
- 트래픽 우회 : 올바른 데이터 센터로 트래픽을 보내는 효과적인 방법의 고려
- 데이터 동기화 : 여러 데이터 센터간 데이터 동기화 빙법의 고려
- 테스트와 배포 

### - <span style="background-color:#fff5b1">_메시지 큐_</span>
> 메세지의 무손실(durability, 즉 메세지 큐에 일단 보관된 메세지는 소비자가 꺼낼 때까지 안전하게 보관한다는 특성)을 보장하는, 비동기 통신을 지원하는 컴포넌트, 
--> 메세지의 버퍼 역할

>#### <span style='background-color:#f7ddbe'>_**메세지 큐 구조**_</span>
![](https://velog.velcdn.com/images/kjeng7897/post/f3a96887-3fc5-4bc2-a762-fd16b38d1108/image.png)
- 서비스 또는 서버간 결합이 느슨해져서, 규모 확장성이 보장되어야 하는 안정적 어플리케이션을 구성하기 좋다.
- 생산자는 소비자 프로세스가 다운되어 있어도 메세지를 발행할 수 있고,
소비자는 생산자 서비스가 가용한 상태가 아니더라도 메시지를 수신할 수 있다.
- 큐의 크기가 커지면 더 많은 작업 프로세스를 추가해야 처리 시간을 줄일 수 있고,
하지만 큐가 거의 항상 비어 있는 상태라면, 작업 프로세스의 수는 줄일 수 있다.

### - <span style="background-color:#fff5b1">_로그, 메트릭 그리고 자동화_</span>
>#### <span style='background-color:#f7ddbe'>_**로그**_</span>
> 에러 로그를 통해서 시스템의 오류와 문제들을 쉽게 찾아낼 수 있다.

>#### <span style='background-color:#f7ddbe'>_**메트릭**_</span>
>메트릭을 잘 수행하면 사업 현황에 관한 유용한 정보를 얻을 수 있고, 시스템의 현재 상태를 손쉽게 파악할 수 있다.
>- 호스트 단위 메트릭 : CPU, 메모리, 디스크 I/O에 관한 메트릭
- 종합 메트릭 : 데이터베이스 계층의 성능, 캐시 계층의 성능 등에 관한 메트릭
- 핵심 비즈니스 메트릭 : 일별 능동 사용자, 수익, 재방문 등에 관한 메트릭

>#### <span style='background-color:#f7ddbe'>_**자동화**_</span>
시스템이 크고 복잡해지면 생산성을 높이기 위해 자동화 도구를 활용해야 한다.

>#### <span style='background-color:#f7ddbe'>_**메세지 큐, 로그, 메트릭, 자동화 등을 반영한 설계안**_</span>
![](https://velog.velcdn.com/images/kjeng7897/post/d0a3254d-f2f0-4c84-beb2-fbd56ef2660d/image.png)


### - <span style="background-color:#fff5b1">_데이터베이스 규모 확장_</span>
> 저장할 데이터가 많아지면 데이터베이스에 대한 부하가 증가하여 데이터베이스를 증설해야 한다.
> 
>>#### <span style='background-color:#f7ddbe'>_**수직적 확장(scale up)**_</span>
>> - 기존 서버에 더 많은, 또는 고성능의 차원(CPU, RAM, Disk 등)을 증설하는 방법
- <span style="color:red"> BUT </span> 데이터베이스 서버 하드웨어에 한계가 존재하기 때문에 무한대로 증설 불가
- <span style="color:red"> BUT </span> SPOF(Single Point of Failure)로 인한 위험성이 크다
- <span style="color:red"> BUT </span> 고성능 서버로 갈수록 가격이 올라가 비용이 많이 든다.
>>
>>#### <span style='background-color:#f7ddbe'>_**수평적 확장**_</span>
- 샤딩(sharding)이라고도 부르고 더 많은 서버를 추가하는 방법
- 샤딩은 대규모 데이터베이스를 샤드(shard)라고 부르는 작은 단위로 분할하는 기술을 일컫는다.
- 샤딩 전략을 어떻게 구현할 지에 대해서 고려해야한다.
  -  샤딩 키는 파티션 키라고도 부르고는데, 데이터가 어떻게 분산될지 정하는 하나 이상의 칼럼으로 구성된다.
  - 데이터를 고르게 분할 할 수 있도록 하는 게 중용하다.
>>>#### <span style='background-color:#f7ddbe'>_**샤딩 도입시 풀어야할 문제**_</span>
>>> - 데이터의 재 샤딩 : 
(1) 데이터가 너무 많아 하나의 샤드로는 더 이상 감당하기 힘들거나,
(2) 샤드 간 데이터 분포가 균등하지 못할때 
  재 샤딩을 해주어야 한다. 
>>> - 유명인사 문제 : 핫스팟 키(Hotspot key)문제라고도 부르고 특정 샤드에 질의가 집중되어 서버에 과부하가 걸리는 문제
>>> - 조인과 비정규화 : 하나의 데이터베이스를 여러 샤드 서버로 쪼개고 나면, 여러 사드에 걸친 데이터를 조인하기가 힘들어진다.
>
>
>![](https://velog.velcdn.com/images/kjeng7897/post/2eed58f0-ae6a-4b9f-a86d-66d0dcdc2a5a/image.png)
