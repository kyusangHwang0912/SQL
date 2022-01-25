---
stitle:  "w3schools 기본문법 실습"

categories:
  - SQL_practice

tags:
  - [SQL, PostgreSQL, redash]

toc: true
toc_sticky: true
 
date: 2022-01-25
last_modified_at: 2022-01-25
---

# SQL 실습 - w3schools



### 문제 1

- Country 별로 ContactName이 ‘A’로 시작하는 Customer의 숫자를 세는 쿼리를 작성하세요.

```sql
-- 개인 풀이(정답과는 다름)
SELECT Country, 
	   count(CustomerID) as A_start_ContactName_Customer
  FROM Customers
 WHERE ContactName LIKE 'A%'
GROUP BY Country;


-- 정답
select Country, count(1) cnt
from Customers
where ContactName like 'A%'
group by Country;

-- count()를 어떻게 사용했냐가 다르다.
	-- count(column_name)은 null을 포함하지 않은 해당 row의 행의 개수를 세어준다.
    -- count(1)과 count(*)은 null을 포함한 전체 row의 개수를 세어준다. 
    -- 일반적으로는 일부 컬럼에 null 값이 있어도 전체적인 row 수를 파악하는 것이 더 중요한 경우가 많아 count(1)을 사용하는 경우가 더 많다.


```

![image](https://user-images.githubusercontent.com/80219821/150989025-f20feaa9-2d1d-4b94-9781-94249300e4ad.png)



---



### 문제2

- Customer 별로 Order한 Product의 총 Quantity를 세는 쿼리를 작성하세요.

```sql
-- 개인풀이(오답)
SELECT CustomerID, ProductID, sum(Quantity) as total_Quantity
  FROM OrderDetails a LEFT JOIN Orders b
    ON (a.OrderID = b.OrderID)
GROUP BY CustomerID
ORDER BY ProductID;

-- 정답
select a.CustomerID, sum(b.Quantity)
from Orders a 
left join OrderDetails b on a.OrderId = b.OrderId
group by a.CustomerID;



-- Customer 별로 group by를 했기 때문에 ProductID를 표시해 주는 것은 무의미하다.
	-- 한 Customer가 여러 개의 Product를 구매한 경우가 있을텐데 개인풀이처럼 쿼리를 작성하면 그 여러 개의 Product 중에 하나의 Product에 대한 ID만 나타나게 된다.
    -- 만약 고객별, 상품별로 Quantity를 세고 싶다면 group by를 고객별, 상품별로 해주면 된다.
-- join은 보통 제일 기준이 되는 테이블은 좌측에 두고 left join을 가장 많이 사용한다.
	-- inner join은 말 그대로 교집합을 검사하고 싶은 경우나, 중복을 제거하기 위한 기술적인 목적으로 사용한다.
	-- 이 문제에서는 inner join이나 left join 어느 것을 써도 상관없다.
	



```

- 개인풀이의 출력 결과

![image](https://user-images.githubusercontent.com/80219821/150990079-ac40a841-bb51-4197-a196-ff6b6d6de55d.png)

=> 각 customerID의 productID들 중(만약 여러 개 있다면) 하나의 productID에 대한 ID만 나타난 결과이다.

따라서 productID는 없어야 할 컬럼이다!

만약 고객별, 상품별로 Quantity를 세고 싶다면 [group by CustomerID, ProductID] 로 해주면 된다.



- 정답의 출력 결과

![image](https://user-images.githubusercontent.com/80219821/150989989-db6c9ee4-1389-4976-97ae-c7633a264c48.png)



---



### 문제3

- 년월별, Employee별로 Product를 몇 개씩 판매했는지를 표시하는 쿼리를 작성하세요.

```sql
-- 개인풀이(정답)
SELECT substr(OrderDate, 1, 7) as date, EmployeeID, sum(Quantity)
  FROM OrderDetails a LEFT JOIN Orders b
    ON (a.OrderID = b.OrderID)
GROUP BY substr(OrderDate, 1, 7), EmployeeID;

				
-- 정답
select substr(a.OrderDate,1,7) ym, a.EmployeeID, sum(b.Quantity) sumOfQuantity
from Orders a
	left join OrderDetails b on a.OrderID = b.OrderID
group by substr(a.OrderDate,1,7), a.EmployeeID;



-- MySQL에서는 datetime 변수를 다루기 위해 date_format() 등의 함수를 많이 쓴다.(하지만 w3schools에서는 작동 x)
	-- 이럴때는 좀 더 원시적(?)이지만 날짜를 문자열로 보고 substr() 함수를 사용하는 것도 가능하다.
	-- 이 경우처럼 MySQL, Postgres, Oracle 등등 DBMS 마다 쿼리 문법이 약간씩 다르고 사용 가능한 함수들도 조금 차이 있다. (구글링 때 환경도 같이 검색해줄 것)
```

![image](https://user-images.githubusercontent.com/80219821/150991138-6f14b497-f37c-4f61-80bb-8b7ce44f027a.png)




