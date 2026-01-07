# Django ORM

### **ORM**(Object Relational Mapping, 객체-관계 매핑)
ORM이란, 객체 지향 프로그래밍(OOP) 언어에서의 **객체**(Object)와 관계형 데이터베이스(RDB)의 **테이블**을 매핑시켜  
개발자에게 익숙한 OOP 언어로 RDB의 데이터를 관리할 수 있게 해주는 기술이다.  

**관계형 데이터베이스**(RDB)는 2차원 테이블에 데이터를 저장하고, 테이블 간 관계를 통해 데이터를 연결하는   
**관계 데이터 모델**(Relation Data Model)을 따른다.

하지만, 관계 데이터 모델에는 상속, 다형성, 레퍼런스와 같은 객체 지향적 특성이 없어  
Java, Python 과 같은 객체 지향 언어로 RDB 테이블에 접근하는 것이 쉽지 않다.

따라서 많은 데이터베이스 쿼리를 수행해야 하는 **객체 지향 프로그램을 개발할 때**, ORM을 활용하면  
추상적으로 쿼리문을 다룰 수 있어, 개발 속도를 높이고 코드를 더 쉽게 유지 보수 할 수 있다.  


### ORM의 장점
- 개발자에게 가장 익숙한 OOP 언어로 복잡한 데이터베이스를 처리 할 수 있어 **개발 편의성**이 높다.
- ORM을 통해 비교적 복잡한 쿼리문을 추상화하고, 단일 언어로 작업할 수 있기 때문에 **더 쉽게 이해하고 유지 보수** 할 수 있다.
- 자주 조회되는 데이터를 애플리케이션과 더 가까운 위치에 두고 사용함으로써 더 빠르게 접근 가능하다.  

### ORM의 단점
- 객체 지향 언어를 사용하지만 ORM 문법을 따로 학습해야 한다.
- 추상화 정도가 높아, 비교적 복잡하고 정교한 데이터 접근이 필요한 경우 SQL을 활용해야 한다.
- 대규모 데이터베이스를 처리하거나 쿼리의 복잡도가 높을 경우 **성능 문제**가 발생할 수 있다.  

<br>
<br>

### Django ORM 특징
Djanog ORM은 기본적으로 **쿼리셋(QuerySet) 단위**로 데이터베이스의 정보를 가져오며,  
쿼리셋을 이용하여 데이터를 읽거나 필터링, 정렬 등의 작업을 수행할 수 있다.  


1. **지연 로딩**(Lazy Loading)
> Django ORM은 코드를 바로 SQL로 치환하여 동작하지 않고, **실제 데이터가 필요한 시점에 필요한 
> 만큼의 데이터를 가져오도록 동작**한다.

```python
users = User.object.filter(major = "소프트웨어") # 해당 줄에서 데이터를 가져오지 않고

for user in users:	# 정말 필요한 시점인 해당 줄에서 데이터를 가져와서 처리
	print(user.name)
```  

※ 여러 개의 쿼리셋이 한꺼번에 합쳐서 실행/호출되는 경우, 매우 느리게 동작할 수 있다.  
  
---  
Django는 기본적으로 **지연 로딩 방식**을 사용하여 쿼리셋을 통해 쿼리문을 실행한다.  
때문에 발생할 수 있는 문제들이 있는데 대표적으로 **N+1 Problem** 이 있다.

<br>

**N+1 Problem**  
Django의 **지연 로딩**(Lazy Loading) 특성에 의해 발생하는 문제로  
한 번의 접근으로 가져올 수 있는 데이터를 불필요하게 DB에 여러 번 접근하여 성능 저하를 일으키는 문제.  

예시)  
```python
class Author(models.Model):
    name = models.CharField(max_length=100)

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.ForeignKey(Author, on_delete=models.CASCADE)
```  

```python
books = Book.objects.all()

for book in books:				# SELECT * FROM book; (1번 실행)
    print(book.author.name)		
								# SELECT name FROM author WHERE id = 1;
								# SELECT name FROM author WHERE id = 2;
								# SELECT name FROM author WHERE id = 3;
								# ... (book 의 갯수만큼 N번 실행)
```  

실제 예시 구현)  
<img width="1549" height="908" alt="스크린샷 2026-01-07 12 34 11" src="https://github.com/user-attachments/assets/9dce6c19-45c6-430a-95ef-6d3af2c48816" />



<br>
<br>

2. **캐싱**(Caching)
> Django ORM은 **호출한 SQL의 결과를 캐싱**(Caching)하여, 불필요한 중복 호출을 방지할 수 있다.  

```python
users = User.object.filter(major = "소프트웨어")

print(list(users))	# 해당 줄에서 호출한 QuerySet 데이터를 캐싱(Caching)하여 저장하고,
print(users[0])	# 해당 줄에서 재사용한다.
```  

※ 위 코드에서 두 출력문의 순서를 변경한다면, 캐싱되지 않고 유사한 쿼리가 2번 호출되니 순서에 유의해야한다.  

<br>
<br>

3. **즉시 로딩**(Eager Loading)
> 즉시 로딩(Eager Loading)은 지연 로딩(Lazy Loading)의 반대 개념으로,   
> 필요한 데이터만 가져오지 않고 한번의 SQL로 필요할 수 있는 모든 데이터를 가져오는 방식이다.   
> Django에서는 즉시 로딩을 위해 **select-related( )**, **prefetch_related( )** 메서드를 제공한다.  

- **selected-related( )** 는 **Join**을 통해 데이터를 즉시 로딩 하는 방식  
- **prefetch_related( )** 는 **추가 쿼리를 실행**하여 데이터를 즉시 로딩 하는 방식  

※ **values( )**, **values_list( )** 를 사용하면 즉시 로딩(Eager Loading) 옵션이 무시된다.  
→ values( ), values_list( ) 는 DB Row 단위로 데이터를 반환하기 때문에, 객체(Object)와 관계(Relational) 간 매핑이 일어나지 않는다.  

<br>
<br>

References
- [Django ORM \(QuerySet\)구조와 원리 그리고 최적화전략 - 김성렬 - PyCon Korea 2020](https://www.youtube.com/watch?v=EZgLfDrUlrk)
- [객체 관계형 매핑\(ORM\)이란 무엇인가요? - 객체 관계형 매핑 설명됨 - AWS](https://aws.amazon.com/what-is/object-relational-mapping/)
