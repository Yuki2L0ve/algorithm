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
