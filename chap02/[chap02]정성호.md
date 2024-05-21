## 2장 개략적인 규모 추정

### - <span style="background-color:#fff5b1">_2의 제곱수_</span>
> 최소 단위는 1바이트(8비트)이고, ASCII 문자 하나가 차지하는 메모리 크기가 1바이트이다.
> >#### <span style='background-color:#f7ddbe'>**_흔히 쓰이는 볼륨 단위_**</span>
![](https://velog.velcdn.com/images/kjeng7897/post/bea85176-581c-40dc-9bc6-2a5a894bbc72/image.png)

### - <span style="background-color:#fff5b1">_모든 프로그래머가 알아야하는 응답지연 값_</span>
>![](https://velog.velcdn.com/images/kjeng7897/post/f5ebf651-ba63-4668-b749-da77c40c4c8f/image.png)

>![](https://velog.velcdn.com/images/kjeng7897/post/2298ea22-6471-4b0b-80b6-ff394e8b24ec/image.png)

> - 메모리는 빠르지만 디스크는 아직도 느리다
- 디스크 탐색은 가능한 한 피하라
- 단순한 압축 알고리즘은 빠르다
- 데이터를 인터넷으로 전송하기 전에 가능하면 압축하라.
- 데이터 센터는 보통 여러 지역에 분산되어 있고, 센터들 간에 데이터를 주고받는 데는 시간이걸린다.

### - <span style="background-color:#fff5b1">_가용성에 관계된 수치들_</span>
> - 고가용성은 시스템이 오랜 시간 동안 지속적으로 중단 없이 운영될 수 있는 능력을 지칭하는 용어다. 대부분 99에서 100사이의 값으로 표현하고 100퍼센트는 시스템이 단 한 번도 중단된 적이 없었음을 의미한다. 
> - SLA(Service Level Agreement)는 서비스 사업자와 고객 사이에 맺어진 합의를 의미한다. 이 합의에는 서비스 사업자가 제공하는 서비스의 가용시간이 공식적으로 기술되어 있다. 


### - <span style="background-color:#fff5b1">_예제 : 트위터 QPS와 저장소 요구량 추정_</span>
> - 가정
	- 월간 능동 사용자는 3억명이다.
    - 50%의 사용자가 트위터를 매일 사용한다.
    - 평균적으로 각 사용자는 매일 2건의 트윗을 올린다.
    - 미디어를 포함하는 트윗은 10% 정도이다.
    - 데이터는 5년간 보관된다.
> - 추정
	- QPS(Quality Per Second) 추정치
    - 일간 능동 사용자= 3억 x 0.5 = 1.5억명
    - QPS = 1.5억 X 2트윗 / 24시간 /3600초 = 약 3500
    - 최대 QPS = 2 x QPS = 약 7000
>- 평균 트윗 크기
	- tweet_id 64바이트
    - 텍스트에 140바이트
    - 미디어에 1MB
>- 미디어 저장소 요구량 : 1.5억 X 2 X 10% X 1MB = 30TB/일
- 5년간 미디어를 보관하기 위한 저장소 요구량 = 30TB X 365 X 5 = 55PB

### - <span style="background-color:#fff5b1">_팁_</span>
>- 근차시를 활용해서 계산해라
- 가정을 적어두고 나중에 살펴보자
- 단위를 붙여서 계산해라

