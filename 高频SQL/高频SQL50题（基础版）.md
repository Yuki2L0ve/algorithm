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
