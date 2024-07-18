高频SQL50题（基础版）

# LC1757. 可回收且低脂的产品
[传送门](https://leetcode.cn/problems/recyclable-and-low-fat-products/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select product_id from Products 
where low_fats = 'Y' and recyclable = 'Y';
```

# LC584. 寻找用户推荐人
[传送门](https://leetcode.cn/problems/find-customer-referee/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select name from Customer 
where referee_id != 2 or referee_id is null;
```

# LC595. 大的国家
[传送门](https://leetcode.cn/problems/big-countries/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select name, population, area from world
where area >= 3000000 or population >= 25000000;
```

# LC1148. 文章浏览 I
[传送门](https://leetcode.cn/problems/article-views-i/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select distinct author_id as id from Views
where author_id = viewer_id order by author_id;
```

# LC1683. 无效的推文
[传送门](https://leetcode.cn/problems/invalid-tweets/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select tweet_id from Tweets where char_length(content) > 15;
```

# LC1378. 使用唯一标识码替换员工ID
[传送门](https://leetcode.cn/problems/replace-employee-id-with-the-unique-identifier/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select EmployeeUNI.unique_id, Employees.name from 
Employees left join EmployeeUNI on
Employees.id = EmployeeUNI.id;
```

# LC1068. 产品销售分析 I
[传送门](https://leetcode.cn/problems/product-sales-analysis-i/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select p.product_name, s.year, s.price from Sales s
inner join Product p on s.product_id = p.product_id;
```

# LC1581. 进店却未进行过交易的顾客
[传送门](https://leetcode.cn/problems/customer-who-visited-but-did-not-make-any-transactions/description/?envType=study-plan-v2&envId=sql-free-50)
```SQL
select customer_id, count(customer_id) as count_no_trans 
from Visits v left join Transactions t on
v.visit_id = t.visit_id where t.transaction_id is null
group by customer_id;
```
