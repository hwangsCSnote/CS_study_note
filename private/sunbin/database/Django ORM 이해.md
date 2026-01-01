# Django ORM 이해

### ORM 존재 이유

orm은 객체 지향 코드와 관계 데이터베이스간의 충돌을 줄여 
코드로 쿼리문을 구현할 수 있게 해주는 추상화 계층

### ORM 장점

orm은 객체 - 관계 간의 거리를 좁힘으로써 단순히 코드만으로도 쿼리문을 작성할 수 있다

코드로 미리 작성해두고 쿼리문이 실행되는 위치는 따로 조절 할 수 있다 - Lazy Evaluation

코드가 길어지는 것을 방지할 수 있다

쿼리문이 지속적으로 사용되는 것을 방지 할 수 있다.

### ORM 단점

복잡한 쿼리문에 있어서는 ORM을 사용하기 어렵다

N+1 문제가 발생할 수 있다.

내부 동작이 보이지 않는다

### ORM의 구조

orm의 구조는 model - queryset 구조를 가지고 있다

model이 우선적으로 선언되어야한다. 

django orm 이전에 기본으로 사용하고 있던 model.py를 보면 알 수 있다

```c
class VisitorCount(models.Model):
    id = models.IntegerField(default=1, primary_key=True)
    total_visitor = models.IntegerField(default=0, blank=True, null=True)
    today_visitor = models.IntegerField(default=0, blank=True, null=True)

    def __str__(self):
        return self.today_visitor
        

#필수 데이터 수집 - 메타 데이터
```

해당 구문을 작성할 당시 model은 DB의 테이블을 여기서 선언한다고 이해했었지만 ORM을 이해하고 나서 model.py의 역할을 명확히 알 수 있었다.

메타데이터 즉, 데이터를 설명해주는 데이터를 선언함으로써 ORM에서 VisitorCount의 id를 가져올때

객체 지향 코드와 같이 VisitorCount.id로 가져올 수 있다.

이 점이 ORM(객체 - 관계 충돌 방지)에 대한 정확한 설명이라고 생각한다

이 이후에 view.py에서 import문으로 model의 객체들을 가져오고 queryset으로 선언한 후에
코드를 사용함으로써 신호를 SQL에게 전달하는 것이 하나의 프로세스이다.

### N+1 문제 해석

ORM과 연관된 데이터를 조회할 경우 실제 조회할 횟수(n)보다 추가 조회(+1) 된다라는 의미이다.

문장 자체로는 이해가 잘 되지 않지만 N+1에 관련된 문제를 이렇게 풀어볼 수 있다

orm의 장점인 객체 - 관계 간의 거리를 좁힌다와 orm의 단점인 내부 구현 동작을 알 수 없다라는 부분이 연관되어있는 것으로 보인다.

객체 지향에서는 for문이 매우 효율적인 코드이다. 가독성이 좋고 한번의 로직으로 여러 코드를 소화할 수 있다는 것이 장점이다. 하지만 쿼리문은 for문이라는 개념이 존재하지 않고 단순히 +1되는 구조이다.

여기서 객체 지향과 관계간의 데이터를 바라보는 방식의 차이로 객체 지향에서는 효율적인 for문이
데이터로 넘어와서는 비효율적인 코드로 변환되는 모습을 확인할 수 있다. 
데이터에서는 for문을 사용하여 한 동작 실행 후 또 다른 쿼리문을 만드는 것보다는 전체를 한번에 쿼리문으로 가져오는 것이 데이터에 부화를 덜 걸 수 있는 방법이기 때문이다.