# Group By

URL: https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-group-by/
담당자: 정인
상태: In progress

- `Group By`
    - `**[SELECT](https://www.notion.so/SELECT-3875af64730e4d7ea0ea4518ef2a2e41)`** 문을 그룹화 해주고 row별로 나누어 return 해줌
    - 각 그룹은 집계함수인 `SUM()` , `COUNT()` 를 사용할 수있다.
    
    ```sql
    # GROUP BY 기본 구조
    SELECT [column1, function(2)...] FROM [table_name] GROUP BY [column1, ...function(2)];
    ```
    
    - `SELECT`를 통해 그룹화 하려는 열과 집계함수를 선택
    - `GROUP BY` 절에서 그룹화할 열을 나열
    
    ![Untitled](Group%20By%201eb7b0037dab430488f52e9cfd82e9d8/Untitled.png)
    
    - 위 사진처럼 `GROUP BY`는 `FROM`과 [WHERE](https://www.notion.so/Where-30ca6e9019e2428bad80d187fc162c7f) 이후 적용되고 `HAVING`, `[SELECT](https://www.notion.so/SELECT-3875af64730e4d7ea0ea4518ef2a2e41)`, [DISTINCT](https://www.notion.so/SELECT-DISTINCT-7e62441eaf6f4ecfb8cda98aa587e931), [ORDER BY](https://www.notion.so/ORDER-BY-981d3202b2074763824f38c90cbd43cc), [LIMIT](https://www.notion.so/Limit-ec4182806d8f42ed9d6c757d68975821) 전에 적용됨
    

<aside>
❕ `GROUP BY` 예제

```sql
# 위 쿼리문에서 GROUP BY는 distinct처럼 사용 됨
SELECT customer_id FROM payment GROUP BY customer_id;

# 집계함수 SUM 사용
SELECT customer_id, SUM(amount) FROM payment GROUP BY customer_id;
```

- `[INNER JOIN](https://www.notion.so/Inner-Join-339ea52711ac4bc9870474fec514d22f)` 절에 `GROUP BY` 사용

```sql
SELECT
	first_name || ' ' || last_name full_name,
	SUM (amount) amount
FROM
	payment
INNER JOIN customer USING (customer_id)    	
GROUP BY
	full_name
ORDER BY amount DESC;
```

```sql
# COUNT() 사용
SELECT staff_id, COUNT(payment_id) FROM payment GROUP BY staff_id;
```

</aside>