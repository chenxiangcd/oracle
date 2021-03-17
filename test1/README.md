# 第一次实验
## 陈祥 18级 软件工程3班 201810414307

## <li>查询1
    set autotrace on
    SELECT d.department_name,count(e.job_id)as "部门总人数",
    avg(e.salary)as "平均工资"
    from hr.departments d,hr.employees e
    where d.department_id = e.department_id
    and d.department_name in ('IT','Sales')
    GROUP BY d.department_name;
### <li>查询结果
![image-20210317151136932](D:\oracle\test1\1.png)
### <li>执行计划
![image-20210317151325923](D:\oracle\test1\2.png)
### <li>优化指导
![image-20210317151348352](D:\oracle\test1\3.png)

## <li>查询2
    set autotrace on
    SELECT d.department_name,count(e.job_id)as "部门总人数",
    avg(e.salary)as "平均工资"
    FROM hr.departments d,hr.employees e
    WHERE d.department_id = e.department_id
    GROUP BY d.department_name
    HAVING d.department_name in ('IT','Sales');
### <li>查询结果
![image-20210317151430744](D:\oracle\test1\4.png)
### <li>执行计划
![image-20210317151444940](D:\oracle\test1\5.png)

## <li>代码分析
### <li>比较两个SQL语句，语句1的效率要比语句2的效率高一些：因为语句1是先进行条件的判断，然后进行筛选，最后得出结果；而语句2则进行了两次筛选，浪费资源。

## <li>代码优化
    set autotrace on
    SELECT c.department_name,count(e.job_id)as "部门总人数",
    avg(e.salary)as "平均工资"
    from (SELECT department_name,department_id
        from hr.departments
        WHERE department_name in('IT','Sales'))c ,hr.employees e
    WHERE c.department_id = e.department_id 
    GROUP BY c.department_name
### <li>查询结果
![image-20210317151556468](D:\oracle\test1\6.png)
## <li>分析
### 使用左外连接进行两个主键关联表的数据查询。原语句1在执行过程中产生了嵌套循环语句,使得每输出一行都要再查一次该表，所以这时可以采用多表外连接的方式查询