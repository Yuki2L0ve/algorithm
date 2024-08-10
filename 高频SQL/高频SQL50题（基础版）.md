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

# LC577. 员工奖金
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

# LC1280. 学生们参加各科测试的次数
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

# LC 570. 至少有5名直接下属的经理
[传送门](https://leetcode.cn/problems/managers-with-at-least-5-direct-reports/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select
    m.name
from
    Employee m
join
    Employee e on e.managerId = m.id
group by
    e.managerId
having 
    count(e.id) >= 5
```


# LC1934. 确认率
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

# LC1731. 每位经理的下属员工数量
[传送门](https://leetcode.cn/problems/the-number-of-employees-which-report-to-each-employee/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select
    m.employee_id,
    m.name,
    count(e.employee_id) as reports_count,
    round(avg(e.age)) as average_age 
from
    Employees m
join
    Employees e on e.reports_to = m.employee_id
group by
    m.employee_id, m.name
order by
    m.employee_id
```

# LC1789. 员工的直属部门
[传送门](https://leetcode.cn/problems/primary-department-for-each-employee/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
with t as (
    select
      employee_id,
      department_id,
      primary_flag,
      count(*) over (partition by employee_id) as cnt
    from Employee 
)

select 
    employee_id,
    department_id
from
    t
where
    t.cnt = 1 or t.primary_flag = 'Y'
```

# LC610. 判断三角形
[传送门](https://leetcode.cn/problems/triangle-judgement/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select x, y, z,
case when
    x + y > z and x + z > y and y + z > x
    then 'Yes' else 'No'
    end as triangle 
from
    Triangle
```

# LC180. 连续出现的数字
[传送门](https://leetcode.cn/problems/consecutive-numbers/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
with t as (
    select
        id,
        num,
        id + 1 - row_number() over (partition by num order by id) as serialNum
    from
        Logs
)

select 
    distinct num as ConsecutiveNums 
from 
    t
group by
    t.num, t.serialNum
having 
    count(*) >= 3
```

# LC1164. 指定日期的产品价格
[传送门](https://leetcode.cn/problems/product-price-at-a-given-date/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
with p as(
    select distinct product_id from Products
)

select
    p.product_id,
    coalesce((select new_price from Products
        where product_id = p.product_id and change_date <= '2019-08-16'
        order by change_date desc
        limit 1), 10) as price
from
    p
```

# LC1204. 最后一个能进入巴士的人
[传送门](https://leetcode.cn/problems/last-person-to-fit-in-the-bus/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
with t as (
    select
        turn,
        person_name,
        sum(weight) over (order by turn) as sum_weight
    from
        Queue
)

select person_name from t
where sum_weight <= 1000
order by turn desc
limit 1
```

# LC1907. 按分类统计薪水
[传送门](https://leetcode.cn/problems/count-salary-categories/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
SELECT 
    'Low Salary' AS category,
    SUM(CASE WHEN income < 20000 THEN 1 ELSE 0 END) AS accounts_count
FROM 
    Accounts
    
UNION

SELECT  
    'Average Salary' category,
    SUM(CASE WHEN income >= 20000 AND income <= 50000 THEN 1 ELSE 0 END) 
    AS accounts_count
FROM 
    Accounts

UNION

SELECT 
    'High Salary' category,
    SUM(CASE WHEN income > 50000 THEN 1 ELSE 0 END) AS accounts_count
FROM 
    Accounts
```

# LC1978. 上级经理已离职的公司员工
[传送门](https://leetcode.cn/problems/employees-whose-manager-left-the-company/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select
	employee_id 
from
	Employees 
where
	salary < 30000
	and manager_id not in ( select employee_id from Employees ) 
order by
	employee_id;
```

# LC626. 换座位
[传送门](https://leetcode.cn/problems/exchange-seats/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select 
    case when id % 2 = 1 and id = (select max(id) from seat) then id
        when id % 2 = 1 then id + 1
        else id - 1
        end as id,
    student
from seat
order by id;
```

# LC1341. 电影评分
[传送门](https://leetcode.cn/problems/movie-rating/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
-- 使用两个查询并将结果合并在一起
with t as (
    -- 查询评论最多的用户名
    (
        select name as results from Users u
        join MovieRating mr on u.user_id = mr.user_id
        group by u.user_id
        order by count(distinct movie_id) desc, name asc
        limit 1
    )

    union all

    -- 查询在 February 2020 平均评分最高的电影名称
    (
        select title as results from Movies m
        join MovieRating mr on m.movie_id = mr.movie_id
        where year(created_at) = 2020 and month(created_at) = 2
        group by m.movie_id
        order by avg(rating) desc, title asc
        limit 1
    )
)

select results from t
```

# LC1321. 餐馆营业额变化增长
[传送门](https://leetcode.cn/problems/restaurant-growth/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
with t as (
    select 
        visited_on,
        sum(amount) over (order by visited_on range interval 6 day preceding) as total_amount
    from 
        Customer
)

select
    distinct visited_on,
    total_amount as amount,
    round(total_amount / 7, 2) as average_amount
from
    t
where
    datediff(visited_on, (select min(visited_on) from Customer)) >= 6;
```

# LC602. 好友申请 II ：谁有最多的好友
[传送门](https://leetcode.cn/problems/friend-requests-ii-who-has-the-most-friends/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
-- 创建一个包含所有好友关系的临时表
with FriendsCount as (
    select requester_id as id from RequestAccepted
    union all
    select accepter_id as id from RequestAccepted
),
-- 计算每个人的好友数
TotalFriends as (
    select id, count(*) as num from FriendsCount group by id
)

-- 选择好友数最多的人
select id, num from TotalFriends order by num desc limit 1
```

# LC585. 2016年的投资
[传送门](https://leetcode.cn/problems/investments-in-2016/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
with t as (
    select 
        tiv_2016,
        count(*) over (partition by tiv_2015) as cnt_2015,
        count(*) over (partition by lat, lon) as cnt_pos
    from
        Insurance 
)

select 
    round(sum(tiv_2016), 2) as tiv_2016
from 
    t
where
    cnt_2015 > 1 and cnt_pos = 1
```

# LC185. 部门工资前三高的所有员工
[传送门](https://leetcode.cn/problems/department-top-three-salaries/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
with e as (
    select 
        *,
        dense_rank() over (partition by departmentId order by salary desc) as rk
    from Employee
)

select
    d.name AS Department,
    e.name AS Employee,
    e.salary AS Salary
from
    e
JOIN
    Department d on e.departmentId = d.id
where
    e.rk <= 3
```

# LC1667. 修复表中的名字
[传送门](https://leetcode.cn/problems/fix-names-in-a-table/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select 
    user_id,
    concat(
        upper(substring(name, 1, 1)),
        lower(substring(name, 2))
    ) as name
from
    Users
order by user_id
```

# LC1527. 患某种疾病的患者
[传送门](https://leetcode.cn/problems/patients-with-a-condition/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select * from Patients
where conditions regexp '^DIAB1|\\sDIAB1'
```

# LC196. 删除重复的电子邮箱
[传送门](https://leetcode.cn/problems/delete-duplicate-emails/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
delete p1 from Person p1, Person p2
where p1.email = p2.email and p1.id > p2.id
```

# LC176. 第二高的薪水
[传送门](https://leetcode.cn/problems/second-highest-salary/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select coalesce ((select distinct salary from Employee
    order by salary desc limit 1 offset 1)) as SecondHighestSalary
```

# LC1484. 按日期分组销售产品
[传送门](https://leetcode.cn/problems/group-sold-products-by-the-date/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select
    sell_date,
    count(distinct product) as num_sold,
    group_concat(distinct product order by product separator ',') as products
from
    Activities 
group by 
    sell_date
order by
    sell_date
```

# LC1327. 列出指定时间段内所有的下单产品
[传送门](https://leetcode.cn/problems/list-the-products-ordered-in-a-period/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select 
    product_name,
    sum(unit) as unit
from 
    Products p
join 
    Orders o 
on 
    p.product_id = o.product_id
where 
    order_date like '2020-02%'
group by
    o.product_id
having 
    unit >= 100
```

# LC1517. 查找拥有有效邮箱的用户
[传送门](https://leetcode.cn/problems/find-users-with-valid-e-mails/description/)
```SQL
select * from Users
where mail regexp '^[a-zA-Z][a-zA-Z0-9_.-]*\\@leetcode\\.com$'
```
