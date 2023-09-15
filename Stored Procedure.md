# 存储过程

## 存储过程简介

### 什么是存储过程

-   MySQL 5.0 版本开始支持存储过程。

-   简单的说，存储过程就是一组SQL语句集，功能强大，可以实现一些比较复杂的逻辑功能，类似于JAVA语言中的方法；

-   存储过就是数据库 SQL 语言层面的代码封装与重用。

### 有哪些特性

-   有输入输出参数，可以声明变量，有if/else, case,while等控制语句，通过编写存储过程，可以实现复杂的逻辑功能；

-   函数的普遍特性：模块化，封装，代码复用；

-   速度快，只有首次执行需经过编译和优化步骤，后续被调用可以直接执行，省去以上步骤；

## 存储过程格式

```sql
delimiter 自定义结束符号
create procedure 储存名([ in ,out ,inout ] 参数名 数据类形...)
begin
 	sql语句
end 自定义的结束符合
delimiter ;
```

## MySQL变量定义

### 局部变量

-   用户自定义，**在begin/end块中有效** 
-   语法： 声明变量 ` declare var_name type [default var_value]; `
-   举例：`declare nickname varchar(32);`

```sql
select col_name [...] into var_name[,...] 
from table_name wehre condition 

其中：
col_name 	参数表示查询的字段名称；
var_name 	参数是变量的名称；
table_name 	参数指表的名称；
condition 	参数指查询条件。
注意：当将查询结果赋值给变量时，该查询语句的返回结果只能是单行单列。

举例：
delimiter $$
create procedure proc03()
begin
 
  declare my_ename varchar(20) ;
  select ename into my_ename from emp where empno=1001;
  select my_ename;
end $$
delimiter ;
-- 调用存储过程
call proc03();

```

### 用户变量

-   用户自定义，**当前会话（连接）有效**。类比java的成员变量 
-   语法： `@var_name`不需要提前声明，使用即声明

```sql
delimiter $$
create procedure proc04()
begin
    set @var_name01  = 'ZS';
end $$
delimiter;
call proc04() ;
select @var_name01  ;  --可以看到结果
```

### 系统变量

-   系统变量又分为全局变量与会话变量
    -   全局变量在MYSQL启动的时候由服务器自动将它们初始化为默认值，这些默认值可以通过更改my.ini这个文件来更改。
    -   会话变量在每次建立一个新的连接的时候，由MYSQL来初始化。MYSQL会将当前所有全局变量的值复制一份。来做为会话变量。
-   也就是说，如果在建立会话以后，没有手动更改过会话变量与全局变量的值，那所有这些变量的值都是一样的。
-   全局变量与会话变量的区别就在于，**对全局变量的修改会影响到整个服务器，**但是对会话变量的修改，只会影响到当前的会话（也就是当前的数据库连接）。
-   有些系统变量的值是可以利用语句来动态进行更改的，但是有些系统变量的值却是只读的，对于那些可以更改的系统变量，我们可以利用set语句进行更改。

#### 全局变量

-   由系统提供，在整个数据库有效。 

-   语法：`@@global.var_name`

```sql
-- 查看全局变量 
show global variables; 
-- 查看某全局变量 
select @@global.auto_increment_increment; 
-- 修改全局变量的值 
set global sort_buffer_size = 40000; 
set @@global.sort_buffer_size = 40000;
```

#### 会话变量

-   由系统提供，当前会话（连接）有效 
-   语法：`@@session.var_name`

```sql
-- 查看会话变量
show session variables;
-- 查看某会话变量 
select @@session.auto_increment_increment;
-- 修改会话变量的值
set session sort_buffer_size = 50000; 
set @@session.sort_buffer_size = 50000 ;
```

## 存储过程-参数

>   in 输入参数，意思说你的参数要传到存过过程的过程里面去，在存储过程中修改该参数的值不能被返回
>
>   out 输出参数:该值可在存储过程内部被改变，并向外输出
>
>   inout 输入输出参数，既能输入一个值又能传出来一个值)

### 存储过程传参-in

-   in 表示传入的参数， 可以传入数值或者变量，即使传入变量，并不会更改变量的值，可以内部更改，仅仅作用在函数范围内。

```sql
-- 封装有参数的存储过程，传入员工编号，查找员工信息
delimiter $$
create procedure dec_param01(in param_empno varchar(20))
begin
        select * from emp where empno = param_empno;
end $$
 
delimiter ;
call dec_param01('1001');
```

```sql
-- 封装有参数的存储过程，可以通过传入部门名和薪资，查询指定部门，并且薪资大于指定值的员工信息
delimiter $$
create procedure dec_param0x(in dname varchar(50),in sal decimal(7,2),)
begin
        select * from dept a, emp b where b.sal > sal and a.dname = dname;
end $$
 
delimiter ;
call dec_param0x('学工部',20000);
```

### 存储过程传参-out

-   out 表示从存储过程内部传值给调用者

```sql
-- ---------传出参数：out---------------------------------
use mysql7_procedure;
-- 封装有参数的存储过程，传入员工编号，返回员工名字
delimiter $$
create procedure proc08(in empno int ,out out_ename varchar(50) )
begin
  select ename into out_ename from emp where emp.empno = empno;
end $$
 
delimiter ;
 
call proc08(1001, @o_ename);
select @o_ename;
```

```sql
-- 封装有参数的存储过程，传入员工编号，返回员工名字和薪资
delimiter $$
create procedure proc09(in empno int ,out out_ename varchar(50) ,out out_sal decimal(7,2))
begin
  select ename,sal into out_ename,out_sal from emp where emp.empno = empno;
end $$
 
delimiter ;
 
call proc09(1001, @o_dname,@o_sal);
select @o_dname;
select @o_sal;
```

### 存储过程传参-inout

-   inout 表示从外部传入的参数经过修改后可以返回的变量，既可以使用传入变量的值也可以修改变量的值（即使函数执行完）

```sql
-- 传入员工名，拼接部门号，传入薪资，求出年薪
delimiter $$
create procedure proc10(inout inout_ename varchar(50),inout inout_sal int)
begin
  select  concat(deptno,"_",inout_ename) into inout_ename from emp where ename = inout_ename;
  set inout_sal = inout_sal * 12;  
end $$
delimiter ;

set @inout_ename = '关羽';
set @inout_sal = 3000;
call proc10(@inout_ename, @inout_sal) ;
select @inout_ename ;
select @inout_sal ;
```

## 流程控制

### 判断

#### IF

```SQL
-- 语法
if search_condition_1 then statement_list_1
    [elseif search_condition_2 then statement_list_2] ...
    [else statement_list_n]
end if
```

```SQL
-- 输入员工的名字，判断工资的情况。
delimiter $$
create procedure proc12_if(in in_ename varchar(50))
begin
    declare result varchar(20);
    declare var_sal decimal(7,2);
        select sal into  var_sal from emp where ename = in_ename;
    if var_sal < 10000 
        then set result = '试用薪资';
    elseif var_sal < 30000
        then set result = '转正薪资';
    else 
        set result = '元老薪资';
    end if;
    select result;
end$$
delimiter ;
call proc12_if('庞统');
```

#### CASE

```SQL
-- 语法一（类比java的switch）：
case case_value
    when when_value then statement_list
    [when when_value then statement_list] ...
    [else statement_list]
end case
-- 语法二：
case
    when search_condition then statement_list
    [when search_condition then statement_list] ...
    [else statement_list]
end case
```

##### 语法一

```sql
-- 语法一
delimiter $$
create procedure proc14_case(in pay_type int)
begin
  case pay_type
        when  1 
          then 
              select '微信支付' ;
        when  2 then select '支付宝支付' ;
        when  3 then select '银行卡支付';
      else select '其他方式支付';
    end case ;
end $$
delimiter ;
 
call proc14_case(2);
call proc14_case(4);
```

##### 语法二

```sql
-- 语法二
delimiter  $$
create procedure proc_15_case(in score int)
begin
  case
  when score < 60 
      then
          select '不及格';
    when  score < 80
      then
          select '及格' ;
    when score >= 80 and score < 90
       then 
           select '良好';
  when score >= 90 and score <= 100
       then 
           select '优秀';
     else
       select '成绩错误';
  end case;
end $$
delimiter  ;
 
call proc_15_case(88);
```

### 循环

-   `leave` 类似于 break，跳出，结束当前所在的循环
-   `iterate`类似于 continue，继续，结束本次循环，继续下一次

#### WHILE

```SQL
[LABEL:] while 循环条件 do
    循环体;
end while [LABEL];
```

```sql
-- -------存储过程-while + leave
truncate table user;
delimiter $$
create procedure proc16_while2(in insertcount int)
begin
    declare i int default 1;
    label:while i<=insertcount do
        insert into user(uid,username,`password`) values(i,concat('user-',i),'123456');
        if i=5 then leave label;
        end if;
        set i=i+1;
    end while label;
end $$
delimiter ;
 
call proc16_while2(10);
```

```SQL
-- -------存储过程-while+iterate
truncate table user;
delimiter $$
create procedure proc16_while3(in insertcount int)
begin
    declare i int default 1;
    label:while i<=insertcount do
        set i=i+1;
        if i=5 then iterate label;
        end if;
        insert into user(uid,username,`password`) values(i,concat('user-',i),'123456');
    end while label;
end $$
delimiter ;
call proc16_while3(10);

```

#### REPEAT

```SQL
[LABEL:] repeat 
	循环体;
until 条件表达式
end repeat [LABEL];
```

```SQL
CREATE procedure repeat_test(in n int)
BEGIN
	declare total int default 0;
	
	REPEAT
		set total := total + n;
		set n := n - 1;
	UNTIL n <= 0
	END REPEAT;
	
	select total;
end;

call repeat_test(100);
```

#### LOOP

```SQL
[LABEL:] loop
	循环体;
 	if 条件表达式 then 
    	leave [LABEL]; 
	end if;
end loop;
```

```SQL
create procedure loop_1(in n int)
begin
	declare total int default 0;
	
	sum: loop
		if n <= 0 then leave sum; end if;
		
		set total := total + n;
		set n := n - 1;
	end loop sum;
	
	select total;

end;

call loop_1(100);
```

```SQL
create procedure loop_2(in n int)
begin 
	declare total int default 0;
	
	sum: loop
		if n <= 0 then leave sum; end if;
		
		if n % 2 = 1 then 
			set n := n - 1;
			iterate sum;
		end if;
		
		set total := total + n;
		set n := n - 1;
	end loop sum;
	
	select total;
	
end;

call loop_2(100);
```

## 游标cursor

-   游标(cursor)是用来存储查询结果集的数据类型 , 在存储过程和函数中可以使用光标对结果集进行循环的处理。光标的使用包括光标的声明、OPEN、FETCH 和 CLOSE.

```sql
-- 声明语法
declare cursor_name cursor for select_statement
-- 打开语法
open cursor_name
-- 取值语法
fetch cursor_name into var_name [, var_name] ...
-- 关闭语法
close cursor_name
```

```sql
use mysql7_procedure;
delimiter $$
create procedure proc20_cursor(in in_dname varchar(50))
begin
 -- 定义局部变量
 declare var_empno varchar(50);
 declare var_ename varchar(50);
 declare var_sal  decimal(7,2);
 
 -- 声明游标
 declare my_cursor cursor for
  select empno , ename, sal 
    from  dept a ,emp b
    where a.deptno = b.deptno and a.dname = in_dname;
    
    -- 打开游标
  open my_cursor;
  -- 通过游标获取每一行数据
  label:loop
        fetch my_cursor into var_empno, var_ename, var_sal;
        select var_empno, var_ename, var_sal;
    end loop label;
    
    -- 关闭游标
    close my_cursor;
end
 
 -- 调用存储过程
 call proc20_cursor('销售部');
```

## 异常处理-HANDLER句柄

-   MySql存储过程也提供了对异常处理的功能：通过定义**HANDLER**来完成异常声明的实现

-   官方文档：https://dev.mysql.com/doc/refman/5.7/en/declare-handler.html

```sql
DECLARE handler_action HANDLER
    FOR condition_value [, condition_value] ...
    statement
 
handler_action: {
    CONTINUE
  | EXIT
  | UNDO
}
 
condition_value: {
    mysql_error_code
  | condition_name
  | SQLWARNING
  | NOT FOUND
  | SQLEXCEPTION
```

### 标准模板

```sql
create procedure cursor_test(in age int)
begin
-- 	先声明普通变量
	declare s_name varchar(10);
	declare s_addr varchar(100);
-- 	再声明游标
	declare s_cursor cursor for select Sname, address from student where Sage > age;
-- 	最后声明条件处理程序 handler
	declare exit handler for not found close s_cursor;
	
-- 	先判断是否存在表，若存在，先删除
	drop table if exists name_addr_student;
-- 清空临时表
--  TRUNCATE TABLE name_addr_student;
-- 	若不存在，则创建
	create table if not exists name_addr_student(
		Sname varchar(10),
		address varchar(100)
	);
	
-- 	打开游标
	open s_cursor;
	while true do
-- 		用变量获取游标记录
		fetch s_cursor into s_name, s_addr;
		insert into name_addr_student values(s_name, s_addr);
	end while;
-- 	关闭游标，在这里没必要
-- 	close s_cursor;
	
end;

call cursor_test(18);
```

### 题目示例

```sql
mysql> desc course;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| Cno   | varchar(20) | NO   | PRI | NULL    |       |
| Cname | varchar(20) | NO   |     | NULL    |       |
| hours | varchar(20) | NO   |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+

mysql> desc sc;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| Sno   | varchar(20) | NO   | PRI | NULL    |       |
| Cno   | varchar(20) | NO   | PRI | NULL    |       |
| grade | int         | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+

--  请统计计算 每门课的成绩分布；
	-- 输出格式：
			-- 课程编号，课程名称，不及格人数；70--80分人数；80--90分人数；大于等于90分人数；
	-- 提示：
	--     1 可以创建一个临时表，来存放上述结构；
	--     2 然后分别统计每门课的数据分数分布情况数据，插入到上面的表；
```

存储过程实现代码：

```sql
create procedure count_grade_distribution()
begin
	-- 	先声明普通变量
	declare course_id varchar(20);
	declare course_name varchar(20);

    declare fail_count int default 0;
    declare seventy_to_eighty_count int default 0;
    declare eighty_to_ninety_count int default 0;
    declare ninety_and_above_count int default 0;

	declare done int default 0;
	-- 	再声明游标
	declare course_cur cursor for
		select Cno, Cname from course;

	-- 	最后声明条件处理程序 handler
	declare continue handler
		for not found
		set done := 1;

	-- 	先判断是否存在表，若存在，先删除
	drop table if exists score_distribution;
	-- 	若不存在，则创建
	create table if not exists score_distribution (
		Cno 				varchar(20),
		Cname 				varchar(20),
		不及格人数 			int default 0,
		`70--80分人数` 		int default 0,
		`80--90分人数` 		int default 0,
		`大于等于90分人数` 	int default 0
	);

	-- 	打开游标
	open course_cur;

	count: while done = 0 do
		-- 用声明的变量获取游标记录
		fetch course_cur into course_id, course_name;

		if done = 1 then
		    leave count;
		end if;

		-- 计算每门课程的成绩分布
		select
			sum(case when sc.grade < 60 then 1 else 0 end),
			sum(case when sc.grade >= 70 and sc.grade < 80 then 1 else 0 end),
			sum(case when sc.grade >= 80 and sc.grade < 90 then 1 else 0 end),
			sum(case when sc.grade >= 90 then 1 else 0 end)
		into
			fail_count,
			seventy_to_eighty_count,
			eighty_to_ninety_count,
			ninety_and_above_count
		from sc
		where sc.Cno = course_id;

		-- 插入结果到临时表
		insert into score_distribution values(
			course_id,
			course_name,
			fail_count,
			seventy_to_eighty_count,
			eighty_to_ninety_count,
			ninety_and_above_count
		);
	end while;

	close course_cur;

	-- 使用 COALESCE 函数替换 NULL 为 0
	UPDATE score_distribution
	SET
		不及格人数 = COALESCE(不及格人数, 0),
		`70--80分人数` = COALESCE(`70--80分人数`, 0),
		`80--90分人数` = COALESCE(`80--90分人数`, 0),
		`大于等于90分人数` = COALESCE(`大于等于90分人数`, 0);
	select * from score_distribution;

end;

call count_grade_distribution();
```

### **★★★注意：游标和handler都要放在创建表的前面**

## 存储函数

-   MySQL存储函数（自定义函数），函数一般用于计算和返回一个值，可以将经常需要使用的计算或功能写成一个函数。

-   存储函数和存储过程一样，都是在数据库中定义一些 SQL 语句的集合。

-   区别

    1.   存储函数有且只有一个返回值，而存储过程可以有多个返回值，也可以没有返回值。

    2.   存储函数只能有输入参数，而且不能带in, 而存储过程可以有多个in,out,inout参数。
    3.   **存储过程中的语句功能更强大**，存储过程可以实现很复杂的业务逻辑，而函数有很多限制，如不能在函数中使用insert,update,delete,create等语句；
    4.   存储函数只完成查询的工作，可接受输入参数并返回一个结果，也就是函数实现的功能针对性比较强。
    5.   存储过程可以调用存储函数。但函数不能调用存储过程。
    6.   存储过程一般是作为一个独立的部分来执行(call调用)。而函数可以作为查询语句的一个部分来调用.

```sql
create function func_name ([param_name type[,...]])
returns type
[characteristic ...] 
begin
	routine_body
end;

参数说明：
（1）func_name ：存储函数的名称。
（2）param_name type：可选项，指定存储函数的参数。type参数用于指定存储函数的参数类型，该类型可以是MySQL数据库中所有支持的类型。
（3）RETURNS type：指定返回值的类型。
（4）characteristic：可选项，指定存储函数的特性。
（5）routine_body：SQL代码内容。
```

```sql
create database mydb9_function;
-- 导入测试数据
use mydb9_function;
set global log_bin_trust_function_creators=TRUE; -- 信任子程序的创建者
 
-- 创建存储函数-没有输输入参数
drop function if exists myfunc1_emp;
 
delimiter $$
create function myfunc1_emp() returns int
begin
  declare cnt int default 0;
    select count(*) into  cnt from emp;
  return cnt;
end $$
delimiter ;
-- 调用存储函数
select myfunc1_emp();
```

```sql
-- 创建存储过程-有输入参数

drop function if exists myfunc2_emp;
delimiter $$
create function myfunc2_emp(in_empno int) returns varchar(50)
begin
    declare out_name varchar(50);
    select ename into out_name from emp where  empno = in_empno;
    return out_name;
end $$
delimiter ;
 
 
select myfunc2_emp(1008);
```

