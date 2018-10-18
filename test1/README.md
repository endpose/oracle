# oracle
两种查询分别为如下所示：
- 查询一
```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
```
 - 查询二
 ```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
```
分析：两个查询语句都是为了查询部门总人数和平均工资，查询语句1是首先查询到部门再查询到对应ID的人，再计算平均工资。查询语句2是直接查询出每个部门的员工，再直接计算对应的平均工资。查询截图分别如下所示：
![查询1](https://github.com/endpose/oracle/blob/master/test1/1.jpg)
查询2截图：
![查询2](https://github.com/endpose/oracle/blob/master/test1/2.jpg)





通过对比两种查询所需时间




-查询时间1  
![查询时间1](https://github.com/endpose/oracle/blob/master/test1/shijian1.jpg)

-查询时间2：



![查询2](https://github.com/endpose/oracle/blob/master/test1/time2.jpg)



可以轻易得到第二种查询语句的查询时间明显短于第一种查询语句，故可以得出结论，第二种查询语句相比第一种更优。


通过Oracle自带的优化系统优化可得如下结果：


![优化](https://github.com/endpose/oracle/blob/master/test1/优化指导2.jpg)

 - 自己写的查询语句如下，可以查询所有经理手下员工的最低工资以及经理ID
 ```SQL
Select manager_id,min(salary) 
from employees 
group by manager_id 
having min(salary)>=6000
and manager_id is not null;
```

![优化](https://github.com/endpose/oracle/blob/master/test1/查询.jpg)
