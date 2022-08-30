참고 : https://www.youtube.com/watch?v=92NizoBL4uA

1. Redis 캐시로 사용하기

2. Redis 데이터 타입 야무지게 활용하기

3. Redis에서 데이터를 영구 저장하려면? ( RDB vs AOF )

4. Redis 아키텍쳐 선택 노하우(Replication vs Sentinel vs Cluster)

5. Redis 운영꿀팁 + 장애포인트

---

Redis 캐시로 사용하기

레디스는 전 세계에서 가장 유명한 caching 솔루션 ( 대부분 caching용도로 사용 )

Caching이란 ? => 데이터의 원래 소스보다 더 빠르고 효율적으로 액세스할 수 있는 임시데이터 저장소

대부분의 어플리케이션에서 속도향상을 위해 caching을 사용하고있음.

즉, 데이터의 재사용 횟수가 한번 이상이어야 효율적.

단순하게 key value형태로 저장되기 때문에, 어떤 데이터라도 쉽게 저장할 수있다.

![Visual Studio Code](/img/redis.png)

---

**redis는 어떤상황에서 쓰면 좋을까?**

![Visual Studio Code](/img/redis2.png)

대표적으로 카운팅에서 쓸 수가 있다.

- String

예시를 보면 score의 키값 a를 10으로 설정해놓고

INCR (increment) 함수를 쓰면 11로 카운팅되는 것이 확인가능

INCRBY 를 통해 4라고 특정해주면 +4가 증가되어서 15로 카운팅 되는 것 확인가능

---

![Visual Studio Code](/img/redis3.png)

- Bits

예를 들어 우리 서비스에 오늘 접속한 유저 수를 세고 싶다할 때,

20210817이라는 날짜 key하나를 만들어놓고, 유저 ID에 해당하는 bit를 1로 올려주는 것

한개의 bit가 한 명을 의미하므로, 만약 천만 명의 유저는 천만개의 bit로 표현할 수 있고, 이는 곧 1.2 메가 바이트 정도 밖에 차지하지 않는다.

예제에서처럼 SETBIT를 통해 bit를 설정할 수 있고, BITCOUNT를 통해 1로 설정된 값을 모두 카운팅 할 수 있다.

하지만 이 방법을 사용하려면 모든 데이터를 정수로 표현할 수 있어야한다.

즉 user id 와 같은 값이 0 이상의 정수값일 때만 카운팅이 가능하며, 그런 시퀀셜한 값이 없을 때는 이 방법을 사용할 수 없다.

---

![Visual Studio Code](/img/redis4.png)

-HyperLogLogs

set과 비슷하나 대량의 카운트를 할 때 set보다 적절

모든 값이 12KB로 고정되어 저장되기 때문임

저장되는 값이 몇백만, 몇천만 건이던 상관없이 모두 12KB이다.

대신 한번 저장된 값은 다시 불러올 수 없는데, 경우에 따라 데이터를 보호하기 위한 목적으로도 적절하게 사용할 수 있다.

PFADD 커맨드로 데이터를 저장하고

PFCOUNT 커맨드로 유니크하게 저장된 값을 조회할 수 있다.

만약 나는 일별로 데이터를 저장했는데, 일주일치를 취합해서 보고 싶다면, PFMERGE 커맨드로 key들을 머지해서 확인 할 수 있다.

---

09:55
