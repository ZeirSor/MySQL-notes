# MySQL基础
<!-- TOC -->

- [1. 基本命令](#1-基本命令)
- [2. 图形化工具: DBeaver](#2-图形化工具-dbeaver)
- [3. SQL语言分类](#3-sql语言分类)
    - [3.1. DDL(Data Define Language) 数据定义](#31-ddldata-define-language-数据定义)
    - [3.2. DML(Data Manipulation) 数据操纵](#32-dmldata-manipulation-数据操纵)
    - [3.3. DCL(Data Control Language) 数据控制](#33-dcldata-control-language-数据控制)
    - [3.4. DQL(Data Query Language) 数据查询](#34-dqldata-query-language-数据查询)
- [4. SQL语法特征](#4-sql语法特征)

<!-- /TOC -->
## 1. 基本命令

-   打开命令行, 输入`mysql -uroot -p`, 回车输入密码
-   `show databases;` 查看有哪些数据库（记得分号！）
-   `use 数据库名` 使用某个数据库
-   `show tables` 查看数据库内有哪些表
-   `exit` 退出MySQL的命令行环境

## 2. 图形化工具: DBeaver

## 3. SQL语言分类

### 3.1. DDL(Data Define Language) 数据定义

-   库的创建删除、表的创建删除等

### 3.2. DML(Data Manipulation) 数据操纵

-   新增数据、删除数据、修改数据等

### 3.3. DCL(Data Control Language) 数据控制

-   新增用户、删除用户、密码修改、权限管理等

### 3.4. DQL(Data Query Language) 数据查询

-   基于需求查询和计算数据

## 4. SQL语法特征

-   大小写不敏感
-   可单行多行书写,以`;`结束
-   支持注释
    -   单行
        -   `-- 注释内容`（要有空格）
        -   `# 注释内容`
    -   多行
        -   `/*注释内容*/`



