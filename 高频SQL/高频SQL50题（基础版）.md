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
SELECT 
	name 
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

LC577. 员工奖金
[传送门](https://leetcode.cn/problems/employee-bonus/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT 
	name
	bonus 
FROM
	Employee e
	LEFT JOIN Bonus b ON e.empId = b.empId 
WHERE
	bonus < 1000 
	OR b.empId IS NULL;
```

LC1280. 学生们参加各科测试的次数
[传送门](https://leetcode.cn/problems/students-and-examinations/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT
	stu.student_id,
	stu.student_name,
	sub.subject_name,
	count( e.subject_name ) AS attended_exams 
FROM
	Students stu
	CROSS JOIN Subjects sub
	LEFT JOIN Examinations e ON stu.student_id = e.student_id 
	AND sub.subject_name = e.subject_name 
GROUP BY
	stu.student_id,
	sub.subject_name 
ORDER BY
	stu.student_id,
	sub.subject_name;
```

#LC 570. 至少有5名直接下属的经理
[传送门](https://leetcode.cn/problems/managers-with-at-least-5-direct-reports/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT 
	name 
FROM
	Employee 
WHERE
	id IN ( SELECT DISTINCT managerId FROM Employee GROUP BY managerId HAVING count( managerId ) >= 5 );
```


#LC1934. 确认率
[传送门](https://leetcode.cn/problems/confirmation-rate/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT
	s.user_id,
	round( ifnull( avg( c.action = 'confirmed' ), 0 ), 2 ) AS confirmation_rate 
FROM
	Signups s
	LEFT JOIN Confirmations c ON s.user_id = c.user_id 
GROUP BY
	s.user_id;
```

# LC1193. 每月交易 I
[传送门](https://leetcode.cn/problems/monthly-transactions-i/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT
	date_format( trans_date, '%Y-%m' ) AS month,
	country,
	count(*) AS trans_count,
	sum( if(state = 'approved', 1, 0) ) as approved_count,
	sum( amount ) AS trans_total_amount,
	sum( if (state = 'approved', amount, 0) ) as approved_total_amount
FROM
	Transactions 
GROUP BY
	month,
	country;
```

# LC1174. 即时食物配送 II
[传送门](https://leetcode.cn/problems/immediate-food-delivery-ii/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
with d as (
    select 
        customer_id,
        order_date,
        customer_pref_delivery_date,
        rank() over (partition by customer_id order by order_date) as rk
    from 
        Delivery
) 

select
    round(sum(if(d.order_date = d.customer_pref_delivery_date, 1, 0)) * 100 / count(d.customer_id), 2) AS immediate_percentage 
from
    d
where
    d.rk = 1
```

# LC550. 游戏玩法分析 IV
[传送门](https://leetcode.cn/problems/game-play-analysis-iv/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
with t as (
    select 
        player_id,
        timestampdiff(DAY, 
                        event_date, 
                        lead(event_date, 1) over (partition by player_id order by event_date))
        as day_diff,
        rank() over (partition by player_id order by event_date) as rk
    from Activity
)

select
    round(sum(if(day_diff = 1, 1, 0 )) / count(player_id), 2) as fraction
from 
    t
where
    t.rk = 1
```

# 2356. 每位教师所教授的科目种类的数量
[传送门](https://leetcode.cn/problems/number-of-unique-subjects-taught-by-each-teacher/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select
    teacher_id,
    count(distinct subject_id) as cnt
from
    Teacher
group by
    teacher_id;
```

# LC1141. 查询近30天活跃用户数
[传送门](https://leetcode.cn/problems/user-activity-for-the-past-30-days-i/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select 
    activity_date as day,
    count(distinct user_id) as active_users
from
    Activity 
where
    datediff('2019-07-27', activity_date) between 0 and 29
group by
    activity_date
```

# LC1084. 销售分析III
[传送门](https://leetcode.cn/problems/sales-analysis-iii/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT
	p.product_id,
	p.product_name 
FROM
	Product p
	LEFT JOIN Sales s ON p.product_id = s.product_id 
GROUP BY
	p.product_id 
HAVING
	sum( s.sale_date BETWEEN '2019-01-01' AND '2019-03-31' ) = count(*);
```

# LC596. 超过5名学生的课
[传送门](https://leetcode.cn/problems/classes-more-than-5-students/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT
	class 
FROM
	Courses 
GROUP BY
	class 
HAVING
	count( student ) >= 5;
```

# LC1729. 求关注者的数量
[传送门](https://leetcode.cn/problems/find-followers-count/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select
    user_id,
    count(follower_id) as followers_count
from
    Followers 
group by
    user_id 
order by
    user_id
```

# LC619. 只出现一次的最大数字
[传送门](https://leetcode.cn/problems/biggest-single-number/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
with t as (
    select num from MyNumbers group by num having count(num) = 1 
)

select max(num) as num from t
```

# LC1045. 买下所有产品的客户
[传送门](https://leetcode.cn/problems/customers-who-bought-all-products/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select 
    customer_id 
from 
    Customer
group by
    customer_id
having 
    count(distinct(product_key)) = (select count(*) from Product)
```
