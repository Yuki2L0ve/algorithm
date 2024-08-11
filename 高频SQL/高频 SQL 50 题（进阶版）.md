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
