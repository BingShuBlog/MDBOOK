# Pg操作文档

### 一、创建用户

1.创建超级管理员用户 

  代码：

​    

```
CREATE USER xxx WITH   
	LOGIN
	SUPERUSER
	CREATEDB
	CREATEROLE
	INHERIT
	NOREPLICATION
	CONNECTION LIMIT -1
	PASSWORD 'xxxxxx';  //密码
COMMENT ON ROLE xxx IS 'xxx';
```



### 二、创建数据库

1.创建数据库

代码：

 

```
CREATE DATABASE project_solution_db
    WITH 
    OWNER = project_solution
    ENCODING = 'UTF8'
    CONNECTION LIMIT = -1;
```



### 三、创建扩展

1.创建uuid扩展

代码：

 

```
create extension  pgcrypto  //创建uuid扩展
select gen_random_uuid();   //查询uuid
```

