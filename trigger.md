# 触发器

## 触发器简介

>   MySQL触发器（MySQL Triggers）是一种数据库对象，它是与表相关联的特殊类型的存储过程。触发器可以在表上执行某些操作，当表上发生特定事件时，MySQL会自动触发这些操作。触发器通常用于实现数据一致性、自动化任务和记录历史更改等需求。

>   触发器是与表有关的数据库对象，指在insert/update/delete之前(BEFORE)或之后(AFTER)，触发并执行触发器中定义的SQL语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性, 日志记录 , 数据校验等操作 。**使用别名OLD和NEW来引用触发器中发生变化的记录内容**，这与其他的数据库是相似的。现在触发器还只支持行级触发，不支持语句级触发。

### 概念

以下是MySQL触发器的一些关键概念和特点：

1.  **触发器事件（Trigger Event）**：触发器事件是触发器执行的触发条件。MySQL支持INSERT、UPDATE和DELETE事件。你可以为每个事件创建不同的触发器。
2.  **触发器类型（Trigger Type）**：MySQL支持BEFORE和AFTER两种触发器类型。BEFORE触发器在事件发生之前执行，而AFTER触发器在事件发生之后执行。
3.  **触发器主体（Trigger Body）**：触发器主体包含触发器的实际执行代码。这可以是SQL语句、存储过程或函数。
4.  **触发器条件（Trigger Condition）**：你可以为触发器定义一个条件，只有当条件满足时触发器才会执行。条件可以基于表的列值。
5.  **触发器事件表（Triggered Table）**：触发器与特定表相关联，它们在触发表上的事件时执行。触发器可以针对单个表或多个表。
6.  **触发器的触发顺序（Trigger Order）**：如果多个触发器与同一表的同一事件关联，你可以指定它们的执行顺序。

### 触发器类型

| 触发器类型    | NEW（新增数据）      | OLD（修改前的数据）  |
| ------------- | -------------------- | -------------------- |
| INSERT 触发器 | NEW 表示新增的数据   | 无                   |
| UPDATE 触发器 | NEW 表示修改后的数据 | OLD 表示修改前的数据 |
| DELETE 触发器 | 无                   | OLD 表示删除的数据   |

### 基本语法和命令

#### 创建

```sql
CREATE TRIGGER trigger_name
BEFORE/AFTER INSERT/UPDATE/DELETE
ON tbl_name FOR EACH ROW -- 行级触发器
BEGIN
	trigger_stmt ;
END;
```

#### 查看

```sql
SHOW TRIGGERS ; 
```

#### 删除

```sql
DROP TRIGGER [schema_name.]trigger_name ; 
-- 如果没有指定 schema_name，默认为当前数据库 。
```

### 示例

下面是一个简单的示例，说明了MySQL触发器的用法：

假设我们有一个名为 `orders` 的表，当在这个表中插入新订单时，我们希望自动在另一个表 `order_log` 中记录订单的创建时间。我们可以创建一个AFTER INSERT触发器来实现这个功能：

```sql
DELIMITER //
CREATE TRIGGER after_order_insert
AFTER INSERT ON orders
FOR EACH ROW
BEGIN
    INSERT INTO order_log (order_id, create_time)
    VALUES (NEW.id, NOW());
END;
//
DELIMITER ;
```

在上述示例中，`after_order_insert` 是触发器的名称，它在 `orders` 表的 INSERT 事件之后触发。每当插入新订单时，触发器会在 `order_log` 表中插入相应的记录，包括订单ID和创建时间。

MySQL触发器是一个强大的工具，可以帮助你自动化数据管理任务、维护数据一致性和记录数据变更历史。但要小心使用触发器，确保它们的执行逻辑合理，以避免不必要的性能开销。

## 触发器实践

### 需求

通过触发器记录 `user` 表的数据变更日志(`user_logs`)，包含增加、修改、删除操作。

### 准备：创建日志表

```sql 
create table user_logs(
  id int(11) not null auto_increment,
  operation varchar(20) not null comment '操作类型, insert/update/delete',
  operate_time datetime not null comment '操作时间',
  operate_id int(11) not null comment '操作的ID',
  operate_params varchar(500) comment '操作参数',
  primary key(`id`)
) engine=innodb default charset=utf8;
```

### 插入数据触发器

```SQL
create trigger tb_user_insert_trigger
    after insert on tb_user for each row
begin
    insert into user_logs(id, operation, operate_time, operate_id, operate_params) VALUES
    (null, 'insert', now(), new.id, concat('插入的数据内容为: id=',new.id,',name=',new.name, ', phone=', NEW.phone, ', email=', NEW.email, ', profession=', NEW.profession));
end;

insert into tb_user(
    id, name, phone, email, profession, age, gender, status, createtime
)	VALUES (
        26,'三皇子','18809091212','erhuangzi@163.com','软件工程',23,'1','1',now()
    );
```

### 修改数据触发器

```sql
create trigger tb_user_update_trigger
    after update on tb_user for each row
begin
    insert into user_logs(id, operation, operate_time, operate_id, operate_params) VALUES
    (null, 'update', now(), new.id,
        concat('更新之前的数据: id=',old.id,',name=',old.name, ', phone=', old.phone, ', email=', old.email, ', profession=', old.profession,
            ' | 更新之后的数据: id=',new.id,',name=',new.name, ', phone=', NEW.phone, ', email=', NEW.email, ', profession=', NEW.profession));
end;

-- 更新数据
update tb_user set profession = '会计' where id = 23;
update tb_user set profession = '会计' where id <= 5;
```

### 删除数据触发器

```sql
create trigger tb_user_delete_trigger
    after delete on tb_user for each row
begin
    insert into user_logs(id, operation, operate_time, operate_id, operate_params) VALUES
    (null, 'delete', now(), old.id,
        concat('删除之前的数据: id=',old.id,',name=',old.name, ', phone=', old.phone, ', email=', old.email, ', profession=', old.profession));
end;

delete from tb_user where id = 26;
```

这些SQL代码演示了如何创建和使用触发器来记录 `user` 表的数据变更日志。触发器可以在数据增加、修改和删除时自动执行，将相关信息记录到 `user_logs` 表中，以实现数据变更的跟踪和记录功能。