# MySql
```mysql
create table  tb_user01(
    id int comment 'ID,唯一表示',
    username varchar(20) comment '用户名',
    name varchar(10) comment '姓名',
    age int comment '年龄',
    gender char(1) comment '性别'
) comment '用户表';
```

```mysql
create table  tb_user01(
       id int primary key auto_increment comment 'ID,唯一表示',
       username  varchar(20) not null unique comment '用户名',
       name varchar(10) not null comment '姓名',
       age int comment '年龄',
       gender char(1) default '男' comment '性别'
) comment '用户表'
```
##### 数值类型： tinyint  smallint mediumint int bigint float(5,2) double(5,2) decimal(5,2)
   > age tinyint unsigned
   > score double(4,1)

#####  字符串： char varchar
> char(10) 最多只能存10个字符，不足10个字符，占用10个字符（定长）
> varchar(10):最多只能存10个字符，不足10个字符，按照长度储存（变长）
> phone char(11)
> username varchar(20)

##### 日期类型 date YY-MM-DD time HH:MM:SS datetime YYYY-MM-DD HH:MM:SS
> birthday date
> update_time datetime

#####  根据原型 确定字段 + 基础字段  ——> create_time 记录的是当前这条数据的插入时间 update_time 记录这条数据的最后更新时间
```mysql
    # 查看当前数据库的表
    show tables;
    # 查看表结构
    desc tb_emp;
    # 查看数据库的建表语句
    show create table tb_emp;
```
#### DDL（表操作）# 修改
- 添加字段：alter table 表名 add 字段名 类型（长度）[comment 注释] [约束]
- 修改字段类型 alter table 表名 modify 字段名 新数据类型(长度）
- 修改字段名和字段类型 alter table 表名 change 旧字段名 新字段名 类型（长度）[comment注释] [约束]
- 删除字段： alter table 表名 drop column 字段名
- 修改表名： rename table 表名 to 新表名
- 删除表：drop table [if exists]表名

```mysql
alter table tb_emp add qq varchar(11) comment 'QQ';

alter table tb_emp modify qq varchar(20) comment 'myqq';

alter table tb_emp change qq qq_num varchar(30) comment 'youqq' not null ;

alter table  tb_emp drop column qq_num;

rename table tb_emp to tb_emp01;

drop table if exists tb_user01;
```

#### DML Data Manipulation Language(数据操作语言）
- INSERT UPDATE DELETE
- insert into 表名（字段1，字段2） values(值1，值1）
- insert into 表名 values(值1，值2，....)
- insert into 表名（字段1，字段2） values(值1，值2），（值1， 值2）;
```mysql
insert  into  tb_emp(username,name,gender, create_time, update_time) values('lining', 'oo', 1, now(),now());

insert  into tb_emp(id, username, password, name, gender, image, job, entrydate, create_time, update_time, qq) values (null, 'lin', '123', 'ddd',2,'1.jpg',2,'2010-04-10',now(), now(), '00')

insert  into  tb_emp(username,name,gender, create_time, update_time) values('lining3', 'oo', 1, now(),now()),('lining2', 'oo', 1, now(),now());
```
#update 表名 set 字段名1 = 值1， 字段名2 = 值2，...[where 条件]

update tb_emp set name = '张三', update_time=now() where id= 1;
update tb_emp set entrydate = '2010-01-01', update_time=now();

# delete from 表名 [where 条件]
```mysql    
delete from tb_emp where id = 1;
delete from tb_emp;
```
#### DQL Date Query Language (数据查询语言）
# select 
![image](https://github.com/Qiluzz/MySql/assets/4120789/d397beca-a728-4955-9a0f-0161682214ac)

![image](https://github.com/Qiluzz/MySql/assets/4120789/26b8d7a3-994d-47f1-9128-6cdfbf124dfe)

![image](https://github.com/Qiluzz/MySql/assets/4120789/e3581da0-8f75-4f48-9a81-bd44dfaecb7d)

![image](https://github.com/Qiluzz/MySql/assets/4120789/e188a65a-e41f-42c2-8f98-00a17a772d4c)

![image](https://github.com/Qiluzz/MySql/assets/4120789/31b16098-639a-4419-bcde-867c084ff86d)

![image](https://github.com/Qiluzz/MySql/assets/4120789/e46e6780-8646-4be6-978d-62e0c8f56056)

![image](https://github.com/Qiluzz/MySql/assets/4120789/7ad5ce22-3221-451b-8664-cbadec1147c0)
```mysql
select * from tb_emp, tb_dept where tb_emp.dept_id = tb_dept.id;

-- ---------------内连接-------
-- A.查询员工的姓名，及所属部门名称
select tb_emp.name, tb_dept.name from tb_emp, tb_dept where tb_dept.id = tb_emp.dept_id;

-- 起别名
select e.name, d.name from tb_emp e, tb_dept d where e.dept_id = d.id;

select tb_emp.name, td.name from tb_emp inner join tb_dept td on tb_emp.dept_id = td.id;

-- 外连接

-- 左外连接
select tb_emp.name, td.name from tb_emp left outer join tb_dept td on tb_emp.dept_id = td.id;

select tb_emp.name, td.name from tb_emp right outer join tb_dept td on tb_emp.dept_id = td.id

-- 子查询

-- 标量子查询
select id from tb_dept where name = '教研部';

select * from tb_emp where dept_id = (select id from tb_dept where name = '教研部');

select entrydate from tb_emp where name = '方东白';

select *
from tb_emp
where entrydate > (select entrydate from tb_emp where name = '方东白');

-- 列子查询

select id from tb_dept where name='教研部' or name = '咨询部';

select * from tb_emp where dept_id in (select id from tb_dept where name='教研部' or name = '咨询部')

-- 行子查询

select entrydate, job from tb_emp where name = '韦一笑';

select * from tb_emp where entrydate = '2007-01-01' and job = 2;

select * from tb_emp where (entrydate , job) = (select entrydate, job from tb_emp where name = '韦一笑')

-- 表子查询
select * from tb_emp where entrydate > '2006-10-01'

select * from (select * from tb_emp where entrydate > '2006-10-01') e, tb_dept where e.dept_id = tb_dept.id;
```

```mysql
select *
from category
where id in (select category_id from dish where price < 10);

select dish.name, dish.price, category.name
from dish,
     category
where dish.price < 10
  and dish.category_id = category.id;

select dish.name, dish.price, c.name, dish.status
from dish
         left join category c on c.id = dish.category_id
where price between 10 and 50
  and dish.status = 1;

select max(price)
from dish;
select *
from dish,
     category
where dish.category_id = category.id
  and price = (select max(price) from dish);

select c.name, max(d.price)
from dish d,
     category c
where d.category_id = c.id
group by c.name;

select count(*), c.name
from dish d,
     category c
where d.category_id = c.id
  and d.status = 1
group by c.name
having count(*) >= 3;

select setmeal.name, setmeal.price, d.name, d.price, sd.copies
from setmeal
         left join setmeal_dish sd on setmeal.id = sd.setmeal_id
         left join dish d on sd.dish_id = d.id
where setmeal.name = '商务套餐A';

select s.name, s.price, d.name, d.price, sd.copies
from setmeal s,
     setmeal_dish sd,
     dish d
where s.id = sd.setmeal_id
  and sd.dish_id = d.id
  and s.name = '商务套餐A';

select name, price from dish where price < (select avg(price) from dish);
```


