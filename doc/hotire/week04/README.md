# 4장 



## 배타적 제어

- 래치 : (microsecond 단위)로 아주 짧게 획득함, 데드락이 발생하지 않는다. (트랜잭션 단위가 아니라 오퍼레이션 단위)

- 스핀락 : lock이 반환될때가지 기다림, busy-wating이라고 하기도함.



### DB

- **Shared** : 공유락으로 읽기만 가능하다. 공유락이 어려개 걸릴 수 있다. 배타락을 걸수 없다.
- **Exclusive** :  배타락으로 읽기, 쓰기가 불가능하다. 공유락 조차 걸수 없다.

### MySQL의 데드락(Dead Lock) 처리

 **MySQL의 데드락 감지 기능에 의해서 데드락으로 감지되면 2개의 트랜잭션 중 데이터 변화가 적은 트랜잭션을 rollback 시켜 데드락을 해소시킵니다.** 



## 고정길이 가변길이

- 자바의 고정길이 8바이트 단위이다. 

~~~java
>>3 시프트 연산으로 한번에 이동이 가능하기 때문이다. 
~~~

고정 길이를 사용할 경우 논리적주소가 물리적 주소로 변환될떄 유리하다.

실제 논리적인 주소 값을 사용할때 1, 2, 3, ,4 

물리적인 주소 값은 배수로 확장이 가능하다.  Compressd OOP방식

https://blog.naver.com/gngh0101/221105919309



## 탐색알고리즘

### Index 

- InnoDB B-Tree 인덱스

MySQL의 InnoDB의 대표적인 인덱스는 B-Tree입니다. 데이터는 Primary Key 순으로 정렬되어 관리되고, Secondrary Key는 인덱스키+PK를 조합으로 정렬이 되어 있습니다.

즉, 특정 데이터를 찾기 위해서는 Secondrary Key에서 PK를 찾고, 그 PK를 통해 다시 원하는 데이터로 찾아가는 형태로 데이터가 처리 됩니다. 트리의 가장 큰 강점은 데이터 접근 퍼포먼스가 데이터 증가량에 따라서도 결코 선형적으로 증가하지 않다는 점에 있습니다.

참고로, PK 접근 시 데이터 접근에 소요되는 비용은 O(logN)이고,두번 트리에 접근하는 Secondrary Key에 소요되는 비용은 2 * O(logN)입니다.

### Hash

https://tech.kakao.com/2016/04/07/innodb-adaptive-hash-index/