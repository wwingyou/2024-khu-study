## 7장 분산 시스템을 위한 유일 ID 생성기 설계

### - <span style="background-color:#fff5b1">_효과적 면접을 위한 4단계 접근법_</span>
> #### <span style='background-color:#f7ddbe'>**_1단계 - 문제 이해 및 설계 범위 확정_**</span>
- 적절한 질문을 통해 모호함을 없애고 설계 방향 정하기
- ID는 어떤 특성을 갖나요?
- 새로운 레코드에 붙일 ID는 항상 1만큼 큰 값이어야 하나요?
- ID는 숫자로만 구성되나요?
- 시스템 규모는 어는정도 입니까?

> #### <span style='background-color:#f7ddbe'>**_2단계 - 개략적인 설계안 제시 및 동의 구하기_**</s pan>
- 유일성이 보장되는 ID를 만든는 여러 방법들

>> 다중 마스터 복제
![](https://velog.velcdn.com/images/kjeng7897/post/03bf9d30-11be-4fe7-a46f-b18eef548b88/image.png)
- 데이터베이스의 auto_increment 기능을 활용하는 것
- 다만 1만큼이 아닌 k만큼 증가시켜 얻는다.
<br>
-- 장단점 --
- 규모 확장성 문제를 어느정도 해결가능하다.
- <span style="color:red"> BUT </span> 여러 데이터 센터에 걸쳐 규모를 늘리기 어렵다.
- <span style="color:red"> BUT </span> ID의 유일성은 보장되겠지만 그 값이 시간 흐름에 맞추어 커지도록 보장할 수는 없다.
- <span style="color:red"> BUT </span> 서버를 추가하거나 삭제할 때도 잘 동작하도록 만들기 어렵다.

>> UUID
- 유일성이 보장되는 ID를 만드는 또 하나의 간단한 방법 
- 컴퓨터 시스템에 저장되는 정보를 유일하게 식별하기 위한 128비트짜리 수
- 서버간 조율 없이 독립적으로 생성가능하다.
![](https://velog.velcdn.com/images/kjeng7897/post/05801087-19a1-41e1-a054-359f2d9c1698/image.png)
-- 장단점 --
- UUID를 만드는 것은 단순하다. 서버 사이의 조율이 필요 없으므로 동기화 이슈도 없다.
- 각 서버가 자기가 쓸 ID를 알아서 만드는 구조이므로 규모 확장도 쉽다.
- <span style="color:red"> BUT </span> ID가 128 비트로 길다. 요구사항을 만족하지 못할 수 있다.
- <span style="color:red"> BUT </span> ID를 시간순으로 정렬할 수 없다.
- <span style="color:red"> BUT </span> ID에 숫자 아닌 값이 포함될 수 있다.

>> 티켓 서버
- auto_increment 기능을 갖춘 데이터베이스 서버, 즉 티켓 서버를 중앙 집중형으로 하나만 사용
![](https://velog.velcdn.com/images/kjeng7897/post/a3ff7fd4-4e2b-48f4-8992-06c78fd3badc/image.png)
-- 장단점 --
- 유일성이 보장되는 오직 숫자로만 구성된 ID를 쉽게 만들 수 있다.
- 구현하기 쉽고, 중소 규모 애플리케이션에 적합하다.
- <span style="color:red"> BUT </span> SPOF(Single Point of Failure) 문제가 발생할수 있다.

>> 트위터 스노플레이크 접근법
- 생성해야 하는 ID의 구조를 여러 절로 구분하는 방법
![](https://velog.velcdn.com/images/kjeng7897/post/f0bb4100-9d3b-4171-987c-1205701ebeb7/image.png)
- 사인 비트 : 음수와 양수 구별 등
- 타임스탬프 : 41비트를 할당하고 시간 기록
- 데이터 센터 ID : 저장된 데이터 센터의 고유ID
- 서버 ID : 저장된 서버의 고유 ID
- 일렬번호 : 서버에서 ID를 생성할 때마다 일렬번호 1증가(동일 밀리초에 생성시에만)


> #### <span style='background-color:#f7ddbe'>**_3단계 - 상세 설계_**</span>
- 트위터 스노플레이크 접근법을 사용한 상세한 설계
![](https://velog.velcdn.com/images/kjeng7897/post/15699274-02ea-4415-880d-82fa77c9e1b6/image.png)
>> 타임 스탬프 : 2^41-은 대략 2199023255551밀리초이고  이 값은 대략 69년에 해당한다.
따라서 69년이 지나면 기원 시각을 바꾸거나 ID 체계를 다른 것으로 이전해야한다.
![](https://velog.velcdn.com/images/kjeng7897/post/2434bff3-5415-45eb-ac70-86307dcbe1cf/image.png)

>> 일렬번호 
- 일렬번호는 12비트로이므로 2^12 = 4096의 값을 가질 수 있다. 
- 같은 밀리초동안 하나 이상의 ID를 만들어낸 경우에만 0보다 큰 값을 가진다.

> #### <span style='background-color:#f7ddbe'>**_4단계 - 마무리_**</span>
> 이 단계에선 면접관은 설계 결과물에 관련한 몇가지 후속 질문을 던질 수 도 있고, 아래 항목에 대해서 추가 논의를 진행하도록 할 수도 있다.
>> 1. 시계 동기화(clock synchronization) 
2. 각 절(section)의 길이 최적화
3. 고가용성(high availability)
