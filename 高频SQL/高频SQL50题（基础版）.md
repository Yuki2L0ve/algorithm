高频SQL50题（基础版）

# LC1757. 可回收且低脂的产品
[传送门](https://leetcode.cn/problems/recyclable-and-low-fat-products/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT
	product_id 
FROM
	Products 
WHERE
	low_fats = 'Y' 
	AND recyclable = 'Y';
```

# LC584. 寻找用户推荐人
[传送门](https://leetcode.cn/problems/find-customer-referee/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT name 
FROM
	Customer 
WHERE
	referee_id != 2 
	OR referee_id IS NULL;
```

# LC595. 大的国家
[传送门](https://leetcode.cn/problems/big-countries/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT 
	name
	population,
	area 
FROM
	world 
WHERE
	area >= 3000000 
	OR population >= 25000000;
```

# LC1148. 文章浏览 I
[传送门](https://leetcode.cn/problems/article-views-i/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT DISTINCT
	author_id AS id 
FROM
	Views 
WHERE
	author_id = viewer_id 
ORDER BY
	author_id;
```

# LC1683. 无效的推文
[传送门](https://leetcode.cn/problems/invalid-tweets/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT
	tweet_id 
FROM
	Tweets 
WHERE
	char_length( content ) > 15;
```

# LC1378. 使用唯一标识码替换员工ID
[传送门](https://leetcode.cn/problems/replace-employee-id-with-the-unique-identifier/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT
	EmployeeUNI.unique_id,
	Employees.NAME 
FROM
	Employees
	LEFT JOIN EmployeeUNI ON Employees.id = EmployeeUNI.id;
```

# LC1068. 产品销售分析 I
[传送门](https://leetcode.cn/problems/product-sales-analysis-i/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT
	p.product_name,
	s.YEAR,
	s.price 
FROM
	Sales s
	INNER JOIN Product p ON s.product_id = p.product_id;
```

# LC1581. 进店却未进行过交易的顾客
[传送门](https://leetcode.cn/problems/customer-who-visited-but-did-not-make-any-transactions/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT
	customer_id,
	count( customer_id ) AS count_no_trans 
FROM
	Visits v
	LEFT JOIN Transactions t ON v.visit_id = t.visit_id 
WHERE
	t.transaction_id IS NULL 
GROUP BY
	customer_id;
```

# LC197. 上升的温度
[传送门](https://leetcode.cn/problems/rising-temperature/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
WITH func AS (
	SELECT
		id,
		recordDate,
		Temperature,
		lag( recordDate, 1 ) over ( ORDER BY recordDate ) AS last_date,
		lag( Temperature, 1 ) over ( ORDER BY recordDate ) AS last_temperature 
	FROM
		Weather 
	) SELECT
	id 
FROM
	func 
WHERE
	datediff( recordDate, last_date ) = 1 
	AND Temperature > last_temperature;
```

# LC1661. 每台机器的进程平均运行时间
[传送门](https://leetcode.cn/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT
	machine_id,
	ROUND( SUM( CASE WHEN activity_type = 'end' THEN TIMESTAMP ELSE -TIMESTAMP END ) / count( DISTINCT process_id ), 3 ) AS processing_time 
FROM
	Activity 
GROUP BY
	machine_id
```
