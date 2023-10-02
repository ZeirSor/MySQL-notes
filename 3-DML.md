# DML

### `ALTER`的用法

`ALTER` 是 SQL 中的关键字，用于修改数据库表的结构和属性。它可以用来添加、修改或删除表的列、索引、约束等。以下是一些常见的 `ALTER` 语句用法：

#### **添加列：** 
你可以使用 `ALTER TABLE` 语句来向现有的表中添加新列。例如，在 MySQL 中:

```sql
ALTER TABLE table_name
ADD column_name datatype;
```

这将在表 `table_name` 中添加一个名为 `column_name` 的列，其数据类型为 `datatype`。

#### **修改列：** 
你可以使用 `ALTER TABLE` 语句修改现有列的属性，如数据类型、默认值等。例如，在 PostgreSQL 中：

```sql
ALTER TABLE table_name
ALTER COLUMN column_name SET DATA TYPE new_datatype;
```

这将修改表 `table_name` 中的 `column_name` 列的数据类型为 `new_datatype`。

#### **删除列：** 
你可以使用 `ALTER TABLE` 语句删除现有列。例如，在 SQL Server 中：

```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```

这将从表 `table_name` 中删除 `column_name` 列。

#### **添加索引：** 
你可以使用 `ALTER TABLE` 语句添加索引以加速查询。例如，在 Oracle 中：

```sql
ALTER TABLE table_name
ADD INDEX index_name (column1, column2, ...);
```

这将在表 `table_name` 上创建一个名为 `index_name` 的索引，该索引包含指定的列。

#### **修改约束：** 
你可以使用 `ALTER TABLE` 语句修改表的约束条件，如主键、外键、唯一约束等。例如，在 SQLite 中：

```sql
ALTER TABLE table_name
ADD CONSTRAINT constraint_name UNIQUE (column1, column2, ...);
```

这将在表 `table_name` 上添加一个名为 `constraint_name` 的唯一约束。

#### **重命名表：** 
你可以使用 `ALTER TABLE` 语句来重命名表。例如，在 MySQL 中：

```sql
ALTER TABLE old_table_name
RENAME TO new_table_name;
```

这将把表 `old_table_name` 重命名为 `new_table_name`。

注意，具体的 `ALTER` 语句用法和支持的功能会因数据库管理系统的不同而有所不同。在使用 `ALTER` 语句时，应仔细查阅所使用数据库系统的文档以了解详细信息，并确保谨慎操作，因为修改表结构可能会影响数据完整性和应用程序的正常运行。

### `DELETE`的用法

`DELETE` 命令用于从数据库表中删除一行或多行数据，通常需要指定要删除的条件。

基本语法如下：

```sql
DELETE FROM 表名
WHERE 条件;
```

-   `表名` 是要从中删除数据的表格名称。
-   `WHERE` 子句是可选的，用于指定要删除哪些行的条件。如果没有指定条件，将删除表中的所有数据。

以下是一些示例：
####  **删除单行数据：** 
删除表中满足特定条件的一行数据。

```sql
DELETE FROM employees
WHERE employee_id = 101;
```

上述示例删除了 "employees" 表中员工编号为 101 的数据行。
####  **删除多行数据：** 
删除表中满足条件的多行数据。

```sql
DELETE FROM orders
WHERE order_date < '2023-01-01';
```

上述示例删除 "orders" 表中订单日期早于指定日期的数据行。
####  **删除所有数据：** 
删除表中的所有数据。

```sql
DELETE FROM students;
```

上述示例将 "students" 表中的所有数据行删除，但保留表的结构。

### `UPDATE`的用法

`UPDATE` 命令用于更新数据库表中的现有数据，通常需要指定要更新的行和新值。

基本语法如下：

```sql
UPDATE 表名
SET 列1 = 新值1, 列2 = 新值2, ...
WHERE 条件;
```

-   `表名` 是要更新数据的表格名称。
-   `SET` 子句用于指定要更新的列和新值。
-   `WHERE` 子句用于筛选要更新哪些行的条件。如果没有指定条件，将更新表中的所有行。

以下是一些示例：
####  **更新单行数据：** 
更新表中满足特定条件的一行数据。

```sql
UPDATE employees
SET salary = 60000
WHERE employee_id = 101;
```

上述示例将 "employees" 表中员工编号为 101 的员工的工资更新为 60000。
####  **更新多行数据：** 
更新表中满足条件的多行数据。

```sql
UPDATE products
SET price = price * 0.9
WHERE category = 'Electronics';
```

上述示例将 "products" 表中类别为 'Electronics' 的产品价格降低 10%。
####  **更新所有数据：** 
更新表中的所有数据。

```sql
UPDATE students
SET department = 'Computer Science';
```

上述示例将 "students" 表中所有学生的系别更新为 'Computer Science'。

>   请注意，对于 `DELETE` 和 `UPDATE` 命令，**务必小心使用，特别是在没有备份的情况下**。这些命令可以永久性地删除或更改数据。在执行这些命令之前，请确保你知道自己在做什么，并根据需要备份数据。

### `GROUP BY`用法

>   GROUP BY 子句用于在 SQL 查询中对结果集按一个或多个列进行分组。它通常与聚合函数一起使用，如 SUM、COUNT、AVG、MAX 和 MIN，以对每个分组应用聚合函数。GROUP BY 子句的基本语法如下：

```sql
SELECT 列1, 列2, 聚合函数(列) AS 别名
FROM 表名
GROUP BY 列1, 列2;
```

以下是 GROUP BY 的用法和示例：
####  **基本用法：** 
按照一个列分组并使用聚合函数。

```sql
SELECT dept, AVG(age) AS 平均年龄
FROM student
GROUP BY dept;
```

上述示例将学生表按系别（dept）分组，并计算每个系别的平均年龄。
####   **多列分组：** 
可以按多个列进行分组。

```sql
SELECT dept, sex, COUNT(*) AS 人数
FROM student
GROUP BY dept, sex;
```

上述示例将学生表按系别和性别分组，并计算每个组的人数。
####   **HAVING 子句：** 
可以结合 HAVING 子句来筛选分组后的结果。

```sql
SELECT dept, AVG(age) AS 平均年龄
FROM student
GROUP BY dept
HAVING AVG(age) > 20;
```

上述示例只选择平均年龄大于20岁的系别。
####   **使用聚合函数：** 
GROUP BY 通常与聚合函数一起使用，以计算每个分组的聚合值。

```sql
SELECT dept, MAX(age) AS 最大年龄, MIN(age) AS 最小年龄
FROM student
GROUP BY dept;
```

上述示例计算每个系别的最大年龄和最小年龄。
####   **使用表达式：** 
你可以使用表达式进行分组。

```sql
SELECT YEAR(date) AS 年份, COUNT(*) AS 数量
FROM orders
GROUP BY YEAR(date);
```

上述示例按订单日期的年份分组，并计算每年的订单数量。

总之，GROUP BY 子句是 SQL 中用于分组数据并应用聚合函数的强大工具。它允许你对数据进行更细粒度的分析和汇总。

### 实现降序排列

>   要在 SQL 查询中实现降序排列，你可以使用 `ORDER BY` 子句，后跟列名，然后使用 `DESC` 关键字表示降序排列。以下是示例：

```sql
SELECT 列1, 列2
FROM 表名
ORDER BY 列1 DESC;
```

在上面的示例中，`ORDER BY` 子句将结果集按列1进行降序排列。

以下是一些示例，演示如何在 SQL 查询中实现降序排列：
####   **基本降序排列：** 
按某列的值降序排列。

```sql
SELECT Sno, Sname, age
FROM student
ORDER BY age DESC;
```

上述示例将学生表按年龄降序排列。
####   **多列降序排列：** 
可以按多个列进行降序排列。

```sql
SELECT Cno, Cname, hours
FROM course
ORDER BY hours DESC, Cname DESC;
```

上述示例将课程表首先按照课程时长 (hours) 降序排列，然后再按课程名称 (Cname) 降序排列。
####   **使用表达式：** 
你可以使用表达式进行降序排列。

```sql
SELECT Sname, age
FROM student
ORDER BY age * -1;
```

上述示例将学生表按年龄的相反数进行降序排列。
####   **与 GROUP BY 结合使用：** 
你可以在 GROUP BY 子句后面使用 ORDER BY 子句来对分组结果进行降序排列。

```sql
SELECT dept, AVG(age) AS 平均年龄
FROM student
GROUP BY dept
ORDER BY 平均年龄 DESC;
```

上述示例将学生表按系别分组，并按平均年龄降序排列。

请注意，降序排列使用 `DESC` 关键字，而升序排列使用 `ASC`（默认值）。如果不指定升序或降序，默认情况下，`ORDER BY` 将升序排列。

### `INNER JOIN`的用法

>   `INNER JOIN` 是 SQL 中用于连接两个表的一种方式，它返回符合连接条件的行。`INNER JOIN` 只返回两个表中有匹配关联的行，不包括没有匹配的行。

基本语法如下：

```sql
SELECT 列1, 列2, ...
FROM 表1
INNER JOIN 表2
ON 表1.列 = 表2.列;
```

以下是 `INNER JOIN` 的一些常见用法和示例：
####  **基本 INNER JOIN：** 
连接两个表并选择特定列。

```sql
SELECT student.Sno, student.Sname, sc.grade
FROM student
INNER JOIN sc
ON student.Sno = sc.Sno;
```

上述示例连接学生表（student）和成绩表（sc），并选择学号（Sno）、学生姓名（Sname）以及成绩（grade）列。
####   **多表 INNER JOIN：** 
你可以连接多个表。

```sql
SELECT student.Sno, student.Sname, sc.grade, course.Cname
FROM student
INNER JOIN sc
ON student.Sno = sc.Sno
INNER JOIN course
ON sc.Cno = course.Cno;
```

上述示例连接学生表、成绩表和课程表，同时选择学号、学生姓名、成绩和课程名。
####   **INNER JOIN 与 WHERE 子句结合：** 
你可以在 `INNER JOIN` 中使用 `WHERE` 子句来进一步筛选结果。

```sql
SELECT student.Sno, student.Sname, sc.grade
FROM student
INNER JOIN sc
ON student.Sno = sc.Sno
WHERE sc.grade >= 80;
```

上述示例连接学生表和成绩表，然后筛选出成绩大于等于80分的学生。
####   **INNER JOIN 与表达式结合：** 
你可以在连接条件中使用表达式。

```sql
SELECT student.Sno, student.Sname, sc.grade
FROM student
INNER JOIN sc
ON student.Sno = sc.Sno AND sc.Cno = 'C01';
```

上述示例连接学生表和成绩表，条件是学号匹配并且课程号为'C01'。
####   **INNER JOIN 与别名：** 
使用别名可以简化 SQL 查询。

```sql
SELECT s.Sno, s.Sname, sg.grade
FROM student AS s
INNER JOIN sc AS sg
ON s.Sno = sg.Sno;
```

上述示例使用别名（s 和 sg）连接学生表和成绩表。

总之，`INNER JOIN` 是用于连接两个或多个表以检索相关数据的强大工具。它允许你根据共享列的值在不同表之间建立关系，并从这些表中获取合并的结果。

### `LEFT JOIN`的用法

>   `LEFT JOIN` 是 SQL 中用于连接两个表的一种方式，它返回左表（第一个表）中的所有行，以及与右表（第二个表）中匹配的行。如果右表中没有匹配的行，则返回 NULL 值。

基本语法如下：

```sql
SELECT 列1, 列2, ...
FROM 表1
LEFT JOIN 表2
ON 表1.列 = 表2.列;
```

以下是 `LEFT JOIN` 的一些常见用法和示例：
####  **基本 LEFT JOIN：** 
连接两个表并选择特定列，如果没有匹配行，右表的列将显示为 NULL。

```sql
SELECT student.Sno, student.Sname, sc.grade
FROM student
LEFT JOIN sc
ON student.Sno = sc.Sno;
```

上述示例连接学生表（student）和成绩表（sc），并选择学号（Sno）、学生姓名（Sname）以及成绩（grade）列。如果没有匹配的成绩记录，成绩列将显示为 NULL。
####   **多表 LEFT JOIN：** 
你可以连接多个表。

```sql
SELECT student.Sno, student.Sname, sc.grade, course.Cname
FROM student
LEFT JOIN sc
ON student.Sno = sc.Sno
LEFT JOIN course
ON sc.Cno = course.Cno;
```

上述示例连接学生表、成绩表和课程表，同时选择学号、学生姓名、成绩和课程名。如果没有匹配的成绩或课程记录，相应列将显示为 NULL。
####   **LEFT JOIN 与 WHERE 子句结合：** 
你可以在 `LEFT JOIN` 中使用 `WHERE` 子句来进一步筛选结果。

```sql
SELECT student.Sno, student.Sname, sc.grade
FROM student
LEFT JOIN sc
ON student.Sno = sc.Sno
WHERE sc.grade IS NULL OR sc.grade < 60;
```

上述示例连接学生表和成绩表，然后筛选出没有成绩记录或成绩小于60分的学生。
####   **LEFT JOIN 与别名：** 
使用别名可以简化 SQL 查询。

```sql
SELECT s.Sno, s.Sname, sg.grade
FROM student AS s
LEFT JOIN sc AS sg
ON s.Sno = sg.Sno;
```

上述示例使用别名（s 和 sg）连接学生表和成绩表。

总之，`LEFT JOIN` 是用于连接两个或多个表以检索相关数据的强大工具，它返回左表中的所有行，而右表中没有匹配的行将显示为 NULL。这种连接类型非常有用，特别是当你希望保留左表中所有行时，而右表中的匹配是可选的时候。

## 关键字

### `as`的用法

`AS` 是 SQL 中用于为查询结果中的列或表达式指定别名的关键字。别名是一个可选的标识符，用于更改列或表达式的名称，使其在查询结果中更易于理解或使用。别名通常用于以下情况：
####  **重命名列：** 
可以将查询中的列重命名为更有意义或更简洁的名称。

```sql
SELECT first_name AS "First Name", last_name AS "Last Name"
FROM employees;
```

上述示例将查询结果中的 "first_name" 列重命名为 "First Name"，将 "last_name" 列重命名为 "Last Name"。在结果中，这些列的名称将显示为指定的别名。
####   **使用别名进行计算：** 
可以在查询中使用别名来表示计算的结果。

```sql
SELECT salary * 12 AS "Annual Salary"
FROM employees;
```

上述示例将员工的月薪乘以 12，并将结果列的别名设置为 "Annual Salary"。这样，在结果中，将显示每个员工的年薪。
####   **简化复杂表达式：** 
可以使用别名来简化查询中的复杂表达式。

```sql
SELECT (quantity * price) AS "Total Cost"
FROM orders;
```

上述示例计算每个订单的总成本，并使用别名 "Total Cost" 表示结果。这可以让查询更容易理解。
####   **为子查询或临时表设置别名：** 
别名也可以用于为子查询或临时表设置更简单的名称。

```sql
SELECT *
FROM (SELECT employee_id, first_name, last_name FROM employees) AS emp;
```

上述示例创建了一个子查询，并将其设置为 "emp" 别名，以便在外部查询中引用它。
####   `CREATE TABLE ... AS SELECT` :

`CREATE TABLE ... AS SELECT` 语句是用于创建一个新的表并将其初始化为一个查询的结果集。具体来说，它将执行一个 `SELECT` 查询，并将查询结果插入到新创建的表中。

语法如下：

```sql
CREATE TABLE 新表名 AS
SELECT 列1, 列2, ...
FROM 旧表名
WHERE 条件;
```

其中：

-   `新表名` 是你要创建的新表的名称。
-   `SELECT` 查询用于指定要从哪个表中选择数据以及如何选择数据。
-   `WHERE` 子句是可选的，用于筛选要插入到新表中的数据行。

以下是一个示例，演示如何使用 `CREATE TABLE ... AS SELECT` 语句：

```sql
CREATE TABLE new_employees AS
SELECT employee_id, first_name, last_name
FROM employees
WHERE department = 'IT';
```

​		上述示例创建了一个名为 "new_employees" 的新表，然后从 "employees" 表中选择部门为 "IT" 的员工的 "employee_id"、"first_name" 和 "last_name" 列，并将这些数据插入到新表中。

​		这种语法非常有用，因为它允许你在不手动创建表结构的情况下，将查询结果存储在新表中，以便以后使用。请注意，不同的数据库管理系统可能对此语法有一些细微的差异，因此在具体的数据库系统中使用时，请查阅相关文档以确保语法正确。

总之，`AS` 关键字用于为查询结果中的列、计算结果、子查询或临时表指定别名，以提高查询的可读性和可理解性。别名是可选的，但在复杂的查询中，它们通常非常有用。请注意，SQL 对大小写敏感，所以如果使用带空格或特殊字符的别名，需要将别名用双引号或方括号括起来。不同的数据库管理系统可能对别名语法有一些细微的差异。

### `INSERT`的用法

>   在 SQL 中，`INSERT` 语句用于将新的行插入到数据库表中。`INSERT INTO` 是 `INSERT` 语句的具体形式，用于指定要插入数据的表以及要插入的数据值。以下是 `INSERT INTO` 的基本语法：

```sql
INSERT INTO 表名 (列1, 列2, 列3, ...)
VALUES (值1, 值2, 值3, ...);
```

其中：

-   `表名` 是要插入数据的目标表的名称。
-   `列1, 列2, 列3, ...` 是要插入数据的列名，可以省略，但如果省略了列名，则必须提供与表中列的顺序相匹配的值。
-   `VALUES` 关键字后面的 `(值1, 值2, 值3, ...)` 是要插入到表中的数据值。

以下是一些示例，演示了 `INSERT INTO` 的用法：
####  **插入单行数据：** 
插入一行数据到表中，同时指定列名和值。

```sql
INSERT INTO employees (employee_id, first_name, last_name, hire_date)
VALUES (101, 'John', 'Doe', '2023-09-13');
```

上述示例将一名新员工的信息插入到 "employees" 表中。
####   **插入多行数据：** 
插入多行数据到表中，可以使用多个 `VALUES` 子句，每个子句表示一行数据。

```sql
INSERT INTO orders (order_id, customer_id, order_date)
VALUES (1, 'Cust001', '2023-09-13'),
       (2, 'Cust002', '2023-09-14'),
       (3, 'Cust003', '2023-09-15');
```

上述示例将多个订单信息插入到 "orders" 表中。
####   **省略列名：** 
如果省略了列名，必须提供与表中列的顺序相匹配的值。

```sql
INSERT INTO students
VALUES ('S101', 'Alice', 'Smith', '2023-01-15', 'CS');
```

上述示例将一名学生的信息插入到 "students" 表中，但需要确保值的顺序与表中的列顺序匹配。
####   **插入查询结果：** 
你还可以使用 `INSERT INTO` 来将查询的结果插入到表中。这通常用于将一个表的数据复制到另一个表。

```sql
INSERT INTO new_employees (employee_id, first_name, last_name)
SELECT employee_id, first_name, last_name
FROM old_employees
WHERE hire_date >= '2023-01-01';
```

上述示例将 "old_employees" 表中在指定日期之后入职的员工信息插入到 "new_employees" 表中。

请注意，具体的 SQL 语法和支持的功能可能因所使用的数据库管理系统而有所不同，因此在实际使用中，需要查阅相关数据库系统的文档以确保正确使用语法和功能。

### `DEFAULT`用法

`DEFAULT` 是 SQL 中的关键字，用于为表的列设置默认值。默认值是在插入新行或在没有指定值的情况下更新行时使用的值。以下是 `DEFAULT` 的常见用法：
####  **在创建表时设置默认值：**

~~~sql
在创建表时，可以使用 `DEFAULT` 关键字来指定列的默认值。如果在插入新行时未提供该列的值，将使用默认值。

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    hire_date DATE DEFAULT '2023-01-01'
);
```

在上述示例中，如果在插入新员工时未提供 `hire_date` 的值，将使用默认值 `'2023-01-01'`。
~~~
####  **在修改表时添加默认值：**

~~~sql
你还可以使用 `ALTER TABLE` 语句来添加列的默认值，这可以在表已经存在的情况下进行。

```sql
ALTER TABLE employees
ADD COLUMN middle_name VARCHAR(50) DEFAULT 'N/A';
```

在上述示例中，添加了一个名为 `middle_name` 的新列，并为它设置了默认值 `'N/A'`。
~~~
####  **在插入数据时使用默认值：**

~~~sql
当插入新行时，如果没有为某个列指定值，将使用该列的默认值。

```sql
INSERT INTO employees (employee_id, first_name, last_name)
VALUES (101, 'John', 'Doe');
```

在上述示例中，虽然没有提供 `hire_date` 的值，但由于表定义中设置了默认值，因此将使用默认值。
~~~
####  **在更新数据时使用默认值：**

~~~sql
如果更新行时未指定某个列的新值，并且该列有默认值，将使用默认值。

```sql
UPDATE employees
SET last_name = 'Smith'
WHERE employee_id = 101;
```

在上述示例中，虽然没有更新 `hire_date` 的值，但由于列定义中设置了默认值，因此将使用默认值。
~~~

总之，`DEFAULT` 关键字用于指定列的默认值，以确保在插入新行或更新行时，如果未提供值，将使用预定义的默认值。这有助于确保数据的一致性和完整性。不同的数据库管理系统对 `DEFAULT` 的语法和支持有一些差异，因此在具体的数据库系统中查阅相关文档以了解更多细节是很重要的。
####   `DEFAULT()` 函数

`DEFAULT()` 是一个函数，通常用于生成默认值。与 `DEFAULT` 关键字不同，`DEFAULT()` 是一个函数，可以用于返回一个默认值，该值可以用于插入或更新表的列。

基本语法如下：

```sql
DEFAULT(expression)
```

-   `expression` 是一个可以计算出默认值的表达式。这个表达式可以是任何有效的 SQL 表达式。

示例用法：
#####  **在插入新行时使用 `DEFAULT()` 函数**：

~~~sql
```sql
INSERT INTO employees (employee_id, first_name, last_name, hire_date)
VALUES (101, 'John', 'Doe', DEFAULT());
```
在上述示例中，`DEFAULT()` 函数用于为 `hire_date` 列生成默认日期值。这可以用于确保在插入新行时，`hire_date` 列将被设置为默认的日期值。
~~~
#####  **在更新行时使用 `DEFAULT()` 函数**：

~~~sql
```sql
UPDATE employees
SET hire_date = DEFAULT()
WHERE employee_id = 101;
```
在上述示例中，`DEFAULT()` 函数用于将 `hire_date` 列设置为默认日期值。这可以用于确保在更新行时，`hire_date` 列将被重置为默认日期值。
~~~

需要注意的是，`DEFAULT()` 函数的具体用法可能会因不同的数据库管理系统而有所不同。有些数据库系统可能支持使用 `DEFAULT()` 函数来生成默认值，而其他系统可能会有不同的方式来处理默认值的生成。因此，在使用 `DEFAULT()` 函数时，最好查阅特定数据库系统的文档以了解其用法和支持情况。

### `CASE`表达式

`CASE` 表达式的一般格式如下：

```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ...
    [ELSE else_result]
END
```

-   `CASE` 是 `CASE` 表达式的开始。
-   `WHEN condition1 THEN result1` 表示第一个条件和其对应的结果。如果 `condition1` 为真，那么表达式将返回 `result1`。
-   `WHEN condition2 THEN result2` 表示第二个条件和其对应的结果。如果 `condition2` 为真，那么表达式将返回 `result2`。
-   可以继续添加多个 `WHEN` 子句，每个子句包含一个条件和相应的结果。
-   `ELSE else_result` 是可选的。如果所有的 `WHEN` 子句都不满足条件，将返回 `else_result`。`ELSE` 子句通常是可选的，但如果没有提供 `ELSE` 子句且没有任何条件满足时，表达式将返回 `NULL`。

以下是一个示例 `CASE` 表达式：

```sql
CASE
    WHEN score >= 90 THEN '优秀'
    WHEN score >= 80 THEN '良好'
    WHEN score >= 70 THEN '中等'
    WHEN score >= 60 THEN '及格'
    ELSE '不及格'
END
```

在上述示例中，`CASE` 表达式根据分数 (`score`) 的不同范围返回不同的评级。如果分数大于等于 90，返回 '优秀'；如果分数大于等于 80，返回 '良好'；以此类推。如果没有条件满足，将返回 '不及格'。

`CASE` 表达式通常用于 `SELECT` 查询、`UPDATE` 和 `INSERT` 语句等情况，以便根据条件生成不同的结果或进行条件计算。

`CASE` 表达式在 `SELECT` 查询中的应用非常常见，它可以用于根据特定条件生成不同的查询结果或进行条件计算。以下是一些示例，展示了 `CASE` 表达式在 `SELECT` 查询中的不同用法：

**示例 1：基本的条件选择**

在这个示例中，`CASE` 表达式用于根据学生的成绩等级生成不同的结果。

```sql
SELECT StudentName, Score,
    CASE
        WHEN Score >= 90 THEN '优秀'
        WHEN Score >= 80 THEN '良好'
        WHEN Score >= 70 THEN '中等'
        WHEN Score >= 60 THEN '及格'
        ELSE '不及格'
    END AS Grade
FROM StudentScores;
```

上述查询将根据成绩生成学生的成绩等级，然后将结果列命名为 "Grade"。

**示例 2：条件计算**

在这个示例中，`CASE` 表达式用于根据学生的性别计算男女比例。

```sql
SELECT Department,
    SUM(CASE WHEN Gender = '男' THEN 1 ELSE 0 END) AS 男生人数,
    SUM(CASE WHEN Gender = '女' THEN 1 ELSE 0 END) AS 女生人数
FROM Students
GROUP BY Department;
```

上述查询将根据性别计算每个系的男女学生人数，并使用 `SUM` 函数进行求和。

**示例 3：多条件选择**

在这个示例中，`CASE` 表达式用于处理多个条件，并生成不同的结果。

```sql
SELECT ProductName, UnitsInStock,
    CASE
        WHEN UnitsInStock > 100 THEN '充足'
        WHEN UnitsInStock >= 50 THEN '一般'
        ELSE '不足'
    END AS StockStatus
FROM Products;
```

上述查询将根据库存数量生成不同的库存状态，并将结果列命名为 "StockStatus"。

这些示例说明了如何在 `SELECT` 查询中使用 `CASE` 表达式来根据条件生成不同的查询结果或进行条件计算。`CASE` 表达式可以根据具体需求进行灵活的定制，用于解决各种条件相关的查询问题。

### 反引号`包围特殊字符

在 SQL 中，如果你想为新创建的表使用带有特殊字符的名称（如横杠 `-`），你需要使用反引号 ` 包围表名。你的 SQL 查询语句中的表名 `'new-sc'` 有特殊字符，因此需要像下面这样使用反引号：

```sql
CREATE TABLE `new-sc` AS
SELECT s.Sname, c.Cname, sc.grade
FROM student s
INNER JOIN sc ON s.Sno = sc.Sno
INNER JOIN course c ON sc.Cno = c.Cno;
```

使用反引号 ` 包围表名是 MySQL 中的一种常见做法，以处理包含特殊字符或与保留关键字冲突的表名。这样可以确保 MySQL 正确解释表名。请注意，不同的数据库管理系统可能有不同的方法来处理特殊字符的表名，所以在具体的数据库系统中，你可能需要查阅相关文档以了解正确的语法。

## 复杂题目

### `所有`

```sql
mysql> desc course
    -> ;
+--------------+-------------+------+-----+---------+-------+
| Field        | Type        | Null | Key | Default | Extra |
+--------------+-------------+------+-----+---------+-------+
| Cno          | varchar(20) | NO   | PRI | NULL    |       |
| Cname        | varchar(20) | NO   |     | NULL    |       |
| Chours       | varchar(20) | NO   |     | NULL    |       |
| Credit       | float       | YES  |     | NULL    |       |
| Tno          | varchar(15) | YES  |     | NULL    |       |
| StudentCount | int         | YES  |     | NULL    |       |
+--------------+-------------+------+-----+---------+-------+
6 rows in set (0.04 sec)

mysql> desc sc;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| Sno   | varchar(20) | NO   | PRI | NULL    |       |
| Cno   | varchar(20) | NO   | PRI | NULL    |       |
| Score | int         | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
3 rows in set (0.03 sec)

mysql> desc student;
+---------+--------------+------+-----+---------+-------+
| Field   | Type         | Null | Key | Default | Extra |
+---------+--------------+------+-----+---------+-------+
| Sno     | varchar(20)  | NO   | PRI | NULL    |       |
| Sname   | varchar(20)  | NO   |     | NULL    |       |
| Ssex    | varchar(20)  | NO   |     | NULL    |       |
| Sage    | int          | NO   |     | NULL    |       |
| Dno     | varchar(20)  | NO   |     | NULL    |       |
| Sclass  | varchar(20)  | YES  |     | NULL    |       |
| address | varchar(255) | YES  |     | NULL    |       |
+---------+--------------+------+-----+---------+-------+
7 rows in set (0.03 sec)
```

#### 检索学过001号教师主讲的所有课程的所有同学的姓名

如果要检索那些选完001号教师主讲的所有课程的学生姓名，你需要使用子查询来实现。以下是一个SQL查询，可以找到这些学生的姓名：

```sql
SELECT DISTINCT Sname
FROM student
WHERE Sno IN (
    SELECT DISTINCT s.Sno
    FROM student s
    INNER JOIN sc ON s.Sno = sc.Sno
    INNER JOIN course c ON sc.Cno = c.Cno
    WHERE c.Tno = '001'
    GROUP BY s.Sno
    HAVING COUNT(DISTINCT c.Cno) = (
        SELECT COUNT(*)
        FROM course
        WHERE Tno = '001'
    )
);
```

这个查询执行以下操作：

1. 内部子查询首先找到选修了001号教师主讲的课程的学生（在 `sc` 表中有记录的学生）。
2. 子查询使用 `GROUP BY` 学生编号，并使用 `HAVING` 子句来确保学生选修了001号教师主讲的所有课程。
3. 外部查询选择符合子查询条件的学生的姓名。

这将返回选修了001号教师主讲的所有课程的学生的姓名列表。

