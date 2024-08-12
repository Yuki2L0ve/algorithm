# LC1821. 寻找今年具有正收入的客户
[传送门](https://leetcode.cn/problems/find-customers-with-positive-revenue-this-year/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    customer_id  
from
    Customers
where
    year = 2021 and revenue > 0
```

# LC183. 从不订购的客户
[传送门](https://leetcode.cn/problems/customers-who-never-order/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    name as Customers 
from
    Customers 
where
    id not in
(select distinct customerId from Orders)
```

# LC1873. 计算特殊奖金
[传送门](https://leetcode.cn/problems/calculate-special-bonus/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    employee_id,
    if (mod(employee_id, 2) = 1 and name not like 'M%', salary, 0) as bonus
from
    Employees 
order by
    employee_id
```

# LC1398. 购买了产品 A 和产品 B 却没有购买产品 C 的顾客
[传送门](https://leetcode.cn/problems/customers-who-bought-products-a-and-b-but-not-c/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    c.customer_id,
    c.customer_name 
from
    Customers c
join
    Orders o
on
    c.customer_id = o.customer_id
group by
    o.customer_id
having
    sum(if(o.product_name = 'A', 1, 0)) > 0 and
    sum(if(o.product_name = 'B', 1, 0)) > 0 and 
    sum(if(o.product_name = 'C', 1, 0)) = 0
```

# LC1112. 每位学生的最高成绩
[传送门](https://leetcode.cn/problems/highest-grade-for-each-student/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select
        *,
        rank() over (partition by student_id order by grade desc, course_id asc) as rk
    from
       Enrollments  
)

select
    student_id,
    course_id,
    grade
from
    t
where
    t.rk = 1
order by
    student_id
```

# LC175. 组合两个表
[传送门](https://leetcode.cn/problems/combine-two-tables/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    firstName,
    lastName,
    city,
    state
from
    Person p
left join
    Address a
on
    p.personId = a.personId
```

# LC1607. 没有卖出的卖家
[传送门](https://leetcode.cn/problems/sellers-with-no-sales/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    seller_name
from
    Seller s
left join
    Orders o
on
    s.seller_id = o.seller_id and year(sale_date) = 2020
where
    o.order_id is null
order by
    seller_name
```

# LC1407. 排名靠前的旅行者
[传送门](https://leetcode.cn/problems/top-travellers/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    name,
    coalesce(sum(distance), 0) as travelled_distance 
from
    Users u
left join
    Rides r
on
    u.id = r.user_id
group by
    user_id
order by 
    travelled_distance desc, name asc
```

# LC607. 销售员
[传送门](https://leetcode.cn/problems/sales-person/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select 
    s.name
from
    SalesPerson s
where
    s.sales_id not in
(
    select
        o.sales_id
    from
        Orders o
    left join
        Company c
    on
        o.com_id = c.com_id
    where
        c.name = 'RED'
)
```

# LC1440. 计算布尔表达式的值
[传送门](https://leetcode.cn/problems/evaluate-boolean-expression/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    e.left_operand,
    e.operator,
    e.right_operand,
    case
        when e.operator = '>' and v1.value > v2.value then 'true'
        when e.operator = '<' and v1.value < v2.value then 'true'
        when e.operator = '=' and v1.value = v2.value then 'true'
        else 'false'
    end as value
from
   Expressions e
join
    Variables v1 on e.left_operand = v1.name
join
    Variables v2 on e.right_operand = v2.name
```

# LC1212. 查询球队积分
[传送门](https://leetcode.cn/problems/team-scores-in-football-tournament/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with d as (
    select
        host_team as team_id,
        case
            when host_goals > guest_goals then 3
            when host_goals = guest_goals then 1
            else 0
        end as num_points
    from
        Matches
    
    union all

    select
        guest_team as team_id,
        case    
            when guest_goals > host_goals then 3
            when guest_goals = host_goals then 1
            else 0
        end as num_points
    from    
        Matches
)

select
    t.team_id,
    t.team_name,
    coalesce(sum(d.num_points), 0) as num_points
from
    Teams t
left join
    d
on
    t.team_id = d.team_id
group by 
    t.team_id
order by
    num_points desc, t.team_id asc
```

# LC1890. 2020年最后一次登录
[传送门](https://leetcode.cn/problems/the-latest-login-in-2020/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    user_id,
    max(time_stamp) as last_stamp
from
    Logins
where
    year(time_stamp) = 2020
group by
    user_id
```

# LC511. 游戏玩法分析 I
[传送门](https://leetcode.cn/problems/game-play-analysis-i/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    player_id,
    min(event_date) as first_login 
from
    Activity
group by
    player_id
```

# LC1571. 仓库经理
[传送门](https://leetcode.cn/problems/warehouse-manager/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select
        product_id,
        Width * Length * Height as vol
    from
        Products
)

select
    w.name as warehouse_name,
    sum(w.units * t.vol) as volume
from
    Warehouse w
left join
    t
on
    w.product_id = t.product_id
group by
    w.name
```

# LC586. 订单最多的客户
[传送门](https://leetcode.cn/problems/customer-placing-the-largest-number-of-orders/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    customer_number
from
    Orders
group by
    customer_number
order by
    count(*) desc
limit 1
```

# LC1741. 查找每个员工花费的总时间
[传送门](https://leetcode.cn/problems/find-total-time-spent-by-each-employee/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    event_day as day,
    emp_id,
    sum(out_time - in_time) as total_time
from
   Employees 
group by
    event_day, emp_id
```

# LC1173. 即时食物配送 I
[传送门](https://leetcode.cn/problems/immediate-food-delivery-i/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    round(sum(if(order_date = customer_pref_delivery_date, 1, 0)) / (select count(*) from Delivery) * 100, 2) as immediate_percentage 
from
    Delivery
```

# LC1445. 苹果和桔子
[传送门](https://leetcode.cn/problems/apples-oranges/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    sale_date,
    sum(if(fruit = 'apples', sold_num, -sold_num)) as diff
from
    Sales
group by
    sale_date
order by
    sale_date
```

# LC1699. 两人之间的通话次数
[传送门](https://leetcode.cn/problems/number-of-calls-between-two-persons/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    from_id as person1,
    to_id as person2,
    count(*) as call_count,
    sum(duration) as total_duration
from
    calls
group by
    least(from_id, to_id), greatest(from_id, to_id)
```

# LC1587. 银行账户概要 II
[传送门](https://leetcode.cn/problems/bank-account-summary-ii/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    name,
    sum(amount) as balance
from
    Users u
left join
    Transactions t
on
    u.account = t.account
group by
    u.account
having
    sum(amount) > 10000
```

# LC182. 查找重复的电子邮箱
[传送门](https://leetcode.cn/problems/duplicate-emails/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    email
from
    Person
group by
    email
having
    count(email) > 1
```

# LC1050. 合作过至少三次的演员和导演
[传送门](https://leetcode.cn/problems/actors-and-directors-who-cooperated-at-least-three-times/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    actor_id, director_id
from
    ActorDirector 
group by
    actor_id, director_id
having
    count(*) >= 3
```

# LC1511. 消费者下单频率
[传送门](https://leetcode.cn/problems/customer-order-frequency/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    c.customer_id,
    c.name
from
    Customers c
join
    Orders o
on
    c.customer_id = o.customer_id
join
    Product p
on
    o.product_id = p.product_id
group by
    c.customer_id, c.name
having
    sum(if(left(o.order_date, 7) = '2020-06', p.price * o.quantity, 0)) >= 100
    and
    sum(if(left(o.order_date, 7) = '2020-07', p.price * o.quantity, 0)) >= 100
```

# LC1693. 每天的领导和合伙人
[传送门](https://leetcode.cn/problems/daily-leads-and-partners/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    date_id,
    make_name,
    count(distinct lead_id) as unique_leads,
    count(distinct partner_id) as unique_partners
from
    DailySales d
group by
    date_id, make_name
```

# LC1495. 上月播放的儿童适宜电影
[传送门](https://leetcode.cn/problems/friendly-movies-streamed-last-month/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    distinct title
from
    TVProgram t
left join
    Content c
on
    t.content_id = c.content_id
where
    left(program_date, 7) = '2020-06' and Kids_content = 'Y'
    and content_type = 'Movies'
```

# LC1501. 可以放心投资的国家
[传送门](https://leetcode.cn/problems/countries-you-can-safely-invest-in/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with CountryAverage as (
    select
        c.name as country,
        avg(ca.duration) as country_avg_duration
    from
        Person p
    join
        Country c
    on
        c.country_code = left(p.phone_number, 3)
    join
        Calls ca
    on
        p.id = ca.caller_id or p.id = ca.callee_id
    group by
        c.name
),
GlobalAverage as (
    select
        avg(duration) as global_avg_duration
    from
        Calls
)

select
    ca.country
from
    CountryAverage ca, GlobalAverage ga
where
    ca.country_avg_duration > ga.global_avg_duration
```

# LC603. 连续空余座位
[传送门](https://leetcode.cn/problems/consecutive-available-seats/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select
        seat_id,
        free,
        lag(free) over (order by seat_id) as prev_free,
        lead(free) over (order by seat_id) as next_free
    from
        Cinema
)

select
    seat_id
from
    t
where
    free = 1 and (prev_free = 1 or next_free = 1)
order by
    seat_id
```

# LC1795. 每个产品在不同商店的价格
[传送门](https://leetcode.cn/problems/rearrange-products-table/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select product_id, 'store1' as store, store1 as price
from Products where store1 is not null

union all

select product_id, 'store2' as store, store2 as price
from Products where store2 is not null

union all

select product_id, 'store3' as store, store3 as price
from Products where store3 is not null
```

# LC613. 直线上的最近距离
[传送门](https://leetcode.cn/problems/shortest-distance-in-a-line/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    min(abs(p1.x - p2.x)) as shortest
from
    Point p1
join 
    Point p2
on
    p1.x != p2.x
```

# LC1965. 丢失信息的雇员
[传送门](https://leetcode.cn/problems/employees-with-missing-information/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select employee_id from Employees 
    union all
    select employee_id from Salaries
) 

select
    employee_id
from
    t
group by
    employee_id
having
    count(*) = 1
order by
    employee_id
```

# LC1264. 页面推荐
[传送门](https://leetcode.cn/problems/page-recommendations/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    distinct page_id as recommended_page
from
    Likes
where
    user_id in (
        select user1_id as user_id from Friendship where user2_id = 1
        union all
        select user2_id as user_id from Friendship where user1_id = 1
    )
    and
    page_id not in (
        select page_id from Likes where user_id = 1
    )
```

# LC608. 树节点
[传送门](https://leetcode.cn/problems/tree-node/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    id,
    case
        when Tree.id = (select t.id from Tree t where t.p_id is null) then 'Root'
        when Tree.id in (select t.p_id from Tree t) then 'Inner'
        else 'Leaf'
    end as type
from
    Tree
order by
    id
```

# LC534. 游戏玩法分析 III
[传送门](https://leetcode.cn/problems/game-play-analysis-iii/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    player_id,
    event_date,
    sum(games_played) over (partition by player_id order by event_date) as games_played_so_far
from
    Activity
```

# LC1783. 大满贯数量
[传送门](https://leetcode.cn/problems/grand-slam-titles/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select wimbledon as player_id from Championships
    union all
    select fr_open as player_id from Championships
    union all 
    select us_open as player_id from Championships
    union all
    select au_open as player_id from Championships
)

select
    p.player_id,
    p.player_name,
    count(*) as grand_slams_count
from
    Players p
join
    t
on
    p.player_id = t.player_id
group by
    p.player_id
```

# LC1747. 应该被禁止的 Leetflex 账户
[传送门](https://leetcode.cn/problems/leetflex-banned-accounts/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    distinct b.account_id
from
    LogInfo a
join
    LogInfo b
on
    a.account_id = b.account_id
where
    a.ip_address != b.ip_address and
    b.login between a.login and a.logout
```

# LC1350. 院系无效的学生
[传送门](https://leetcode.cn/problems/students-with-invalid-departments/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    id,
    name
from
    Students 
where
    department_id not in (
        select distinct id from Departments
    )
```

# LC1303. 求团队人数
[传送门](https://leetcode.cn/problems/find-the-team-size/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
select
    employee_id,
    count(*) over (partition by team_id) as team_size
from
    Employee
```

# LC512. 游戏玩法分析 II
[传送门](https://leetcode.cn/problems/game-play-analysis-ii/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select
        player_id,
        device_id,
        rank() over (partition by player_id order by event_date) as rk
    from
        Activity
)

select
    player_id,
    device_id
from
    t
where
    t.rk = 1
```

# LC184. 部门工资最高的员工
[传送门](https://leetcode.cn/problems/department-highest-salary/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select
        name,
        salary,
        departmentId,
        dense_rank() over (partition by departmentId order by salary desc) as rk
    from
        Employee
)

select
    d.name as Department,
    t.name as Employee,
    t.salary as Salary
from
    t
left join
    Department d
on
    t.departmentId = d.id
where
    t.rk = 1
```

# LC1549. 每件商品的最新订单
[传送门](https://leetcode.cn/problems/the-most-recent-orders-for-each-product/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select
       product_name,
       p.product_id as product_id,
       order_id,
       order_date,
       rank() over (partition by p.product_id order by order_date desc) as rk 
    from
        Orders o
    left join
        Products p
    on
        o.product_id = p.product_id
)

select  
    product_name,
    product_id,
    order_id,
    order_date
from
    t
where
    t.rk = 1
order by
    product_name, product_id, order_id
```

# LC1532. 最近的三笔订单
[传送门](https://leetcode.cn/problems/the-most-recent-three-orders/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select
        name as customer_name,
        c.customer_id,
        order_id,
        order_date,
        dense_rank() over (partition by c.customer_id order by order_date desc) as rk
    from
        Customers c
    join
        Orders o
    on
        c.customer_id = o.customer_id
)

select
    customer_name,
    customer_id,
    order_id,
    order_date
from
    t
where
    t.rk <= 3
order by
    customer_name, customer_id, order_date desc
```

# LC1831. 每天的最大交易
[传送门](https://leetcode.cn/problems/maximum-transaction-each-day/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select
        *,
        rank() over (partition by day order by amount desc) as rk
    from
        Transactions    
)

select
    transaction_id
from
    t
where
    t.rk = 1
order by    
    transaction_id
```

# LC1077. 项目员工 III
[传送门](https://leetcode.cn/problems/project-employees-iii/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select  
       project_id,
       e.employee_id,
       rank() over (partition by project_id order by experience_years desc) as rk 
    from
       Project p
    left join
        Employee e
    on
        p.employee_id = e.employee_id 
)

select  
    project_id,
    employee_id
from
    t
where
    t.rk = 1
```

# LC1285. 找到连续区间的开始和结束数字
[传送门](https://leetcode.cn/problems/find-the-start-and-end-number-of-continuous-ranges/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select
        log_id,
        log_id + 1 - row_number() over (order by log_id) as diff
    from
        Logs
)

select
    min(log_id) as start_id,
    max(log_id) as end_id
from
    t
group by
    diff
```

# LC1596. 每位顾客最经常订购的商品
[传送门](https://leetcode.cn/problems/the-most-frequently-ordered-products-for-each-customer/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select
        customer_id,
        product_id,
        rank() over (partition by customer_id order by count(product_id) desc) as rk
    from
        Orders
    group by
        customer_id, product_id
)

select
    t.customer_id,
    t.product_id,
    p.product_name
from
    t
join
    Products p
on
    t.product_id = p.product_id
where 
    t.rk = 1
```

# LC1709. 访问日期之间最大的空档期
[传送门](https://leetcode.cn/problems/biggest-window-between-visits/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select
        user_id,
        visit_date,
        coalesce(
            datediff(lead(visit_date) over (partition by user_id order by visit_date), visit_date),
            datediff('2021-1-1', visit_date)
        ) as diff
    from
        UserVisits 
)

select
    user_id,
    max(diff) as biggest_window
from
    t
group by
    user_id
order by
    user_id
```

# LC1270. 向公司 CEO 汇报工作的所有人
[传送门](https://leetcode.cn/problems/all-people-report-to-the-given-manager/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select employee_id from Employees where manager_id = 1
    union all
    select employee_id from Employees where manager_id in (
        select employee_id from Employees where manager_id = 1
    )
    union all
    select employee_id from Employees where manager_id in (
        select employee_id from Employees where manager_id in (
            select employee_id from Employees where manager_id = 1
        )
    )
)


select
    distinct employee_id
from
    t
where
    employee_id != 1
```

# LC1412. 查找成绩处于中游的学生
[传送门](https://leetcode.cn/problems/find-the-quiet-students-in-all-exams/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with t as (
    select
        exam_id,
        student_id,
        score,
        rank() over(partition by exam_id order by score desc) high_rk,
        rank() over(partition by exam_id order by score) low_rk
    from
        Exam
)

select
    t.student_id,
    student_name
from
    t
join
    Student s
on
    t.student_id = s.student_id
group by
    t.student_id
having
    min(t.high_rk) != 1 and min(t.low_rk) != 1
order by
    t.student_id
```

# LC1767. 寻找没有被执行的任务对
[传送门](https://leetcode.cn/problems/find-the-subtasks-that-did-not-execute/description/?envType=study-plan-v2&envId=sql-premium-50)
```SQL
with recursive t as (
    select task_id, subtasks_count as sub_id from Tasks
    union all
    select task_id, sub_id - 1 from t where sub_id > 1
)

select
    t.task_id,
    t.sub_id as subtask_id
from
    t
left join
    Executed e
on
    t.task_id = e.task_id and t.sub_id = e.subtask_id
where
    e.subtask_id is null
```

# LC1225. 报告系统状态的连续日期
[传送门](https://leetcode.cn/problems/report-contiguous-dates/description/)
```SQL
with CombinedDates as (
    select 'failed' as type, fail_date as date from Failed
    union all
    select 'succeeded' as type, success_date as date from Succeeded
),
DatediffSeries as (
    select
        type,
        date,
        SUBDATE(date, ROW_NUMBER() over (partition by type order by date)) as diff
    from 
        CombinedDates
    where 
        date between '2019-01-01' and '2019-12-31'
)


select
    type as period_state,
    MIN(date) as start_date,
    MAX(date) as end_date
from 
    DatediffSeries
GROUP BY 
    type, diff
order by 
    start_date;

```
