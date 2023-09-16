#  视图

## 介绍

视图(View)是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。通俗的讲，视图只保存了查询的SQL逻辑，不保存查询结果。所以我们在创建视图的时候，主要的工作就落在创建这条SQL查询语句上。

## 语法

### 创建

```SQL
CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [CASCADED | LOCAL ] CHECK OPTION ]
```

### 查询

```SQL
查看创建视图语句: SHOW CREATE VIEW 视图名称;
查看视图数据：	   SELECT * FROM 视图名称 ...... ;
```

### 修改

```sql
方式一：CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]
方式二：ALTER VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]
```

### 删除

```sql
DROP VIEW [IF EXISTS] 视图名称 [,视图名称] ...;
```

### 示例

```sql
-- 创建视图
create or replace view stu_v_1 as select id,name from student where id <= 10;
-- 查询视图
show create view stu_v_1;
select * from stu_v_1;
select * from stu_v_1 where id < 3;
-- 修改视图
create or replace view stu_v_1 as select id,name,no from student where id <= 10;
alter view stu_v_1 as select id,name from student where id <= 10;
-- 删除视图
drop view if exists stu_v_1;
```

## 检查选项

当使用WITH CHECK OPTION子句创建视图时，MySQL会通过视图检查正在更改的每个行，例如插入，更新，删除，以使其符合视图的定义。 MySQL允许基于另一个视图创建视图，它还会检查依赖视图中的规则以保持一致性。为了确定检查的范围，mysql提供了两个选项： CASCADED 和 LOCAL，默认值为 CASCADED 。

### CASCADED级联。

比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 cascaded，但是v1视图创建时未指定检查选项。 则在执行检查时，不仅会检查v2，还会级联检查v2的关联视图v1。

![image-20230916161408709](https://cdn.jsdelivr.net/gh/ZeirSor/picgo_img@main/202309161614946.png)

### LOCAL本地

比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 local ，但是v1视图创建时未指定检查选项。 则在执行检查时，知会检查v2，不会检查v2的关联视图v1。

![image-20230916161455909](https://cdn.jsdelivr.net/gh/ZeirSor/picgo_img@main/202309161614959.png)

### 示例测试

```sql
CREATE TABLE stu (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    gender VARCHAR(10),
    department VARCHAR(50)
);
-- 插入10条随机学生信息示例
INSERT INTO stu (name, age, gender, department)
SELECT
    CONCAT('Student', FLOOR(RAND() * 1000)) AS name,
    FLOOR(RAND() * 10 + 18) AS age,
    CASE WHEN RAND() < 0.5 THEN 'Male' ELSE 'Female' END AS gender,
    CASE
        WHEN RAND() < 0.3 THEN 'Computer Science'
        WHEN RAND() < 0.6 THEN 'Mathematics'
        ELSE 'History'
    END AS department
FROM
    (SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION
     SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9 UNION SELECT 10) AS numbers;

SELECT * FROM stu;

CREATE OR REPLACE VIEW stu_1 AS SELECT age, name from stu WHERE age > 19 WITH CASCADED CHECK OPTION;
INSERT INTO stu_1 VALUES(2, 'Tom'); 	-- 插入失败
INSERT INTO stu_1 VALUES(24, 'Tom');	-- 插入成功，原表会被修改

CREATE OR REPLACE VIEW stu_2 AS SELECT age, name from stu_1 WHERE age < 23 WITH CHECK OPTION;
INSERT INTO stu_2 VALUES(2, 'Tom');		-- 可以插入原表，但是视图2不会显示，因为视图2依赖于视图1
INSERT INTO stu_2 VALUES(24, 'Tom');	-- 插入失败

CREATE OR REPLACE VIEW stu_3 AS SELECT age, name from stu_2 WHERE age < 21 WITH LOCAL CHECK OPTION;
INSERT INTO stu_3 VALUES(2, 'Tom');		-- 可以插入原表，但是视图3不会显示，因为视图3依赖于视图1
INSERT INTO stu_3 VALUES(24, 'Tom');	-- 插入失败
INSERT INTO stu_3 VALUES(20, 'Tom');	-- 可以插入原表，且视图3会显示
```

## 视图的更新

要使视图可更新，视图中的行与基础表中的行之间必须存在一对一的关系。

>   如果视图包含以下任何一项，则该视图**不可更新**：
>
>   A. 聚合函数或窗口函数（SUM()、 MIN()、 MAX()、 COUNT()等）
>
>   B. DISTINCT
>
>   C. GROUP BY
>
>   D. HAVING
>
>   E. UNION 或者 UNION ALL

```sql
示例演示:
create view stu_v_count as select count(*) from student; 

上述的视图中，就只有一个单行单列的数据，如果我们对这个视图进行更新或插入的，将会报错。
insert into stu_v_count values(10); 
```

## **视图作用**

### 简单

视图不仅可以简化用户对数据的理解，也可以简化他们的操作。那些被经常使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件。

### 安全

数据库可以授权，但不能授权到数据库特定行和特定的列上。通过视图用户只能查询和修改他们所能见到的数据

### 数据独立

视图可帮助用户屏蔽真实表结构变化带来的影响。

## 案例

1.   **为了保证数据库表的安全性**，开发人员在操作tb_user表时，只能看到的用户的基本字段，屏蔽手机号和邮箱两个字段。

```sql
create view tb_user_view 
	as select id,name,profession,age,gender,status,createtime
from tb_user;
select * from tb_user_view;
```

2.   查询每个学生所选修的课程（三张表联查），这个功能在很多的业务中都有使用到，为了简化操作，定义一个视图。

```sql
create view tb_stu_course_view 
	as select s.name student_name , s.no student_no ,
			 c.name course_name 
	from student s, student_course sc , course c 
	where s.id = sc.studentid and sc.courseid = c.id;

select * from tb_stu_course_view;
```

