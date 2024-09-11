```mysql
#//用户信息表
DROP TABLE IF EXISTS t_user ;
create table t_user(
u_id bigint(11) NOT NULL auto_increment,
u_name varchar(45) NOT NULL,
u_tel varchar(15) NOT NULL,
u_password varchar(45) NOT NULL,
u_headpath varchar(260) NOT NULL,
u_feeling varchar(260),
PRIMARY KEY ( u_id )
)ENGINE=InnoDB CHARSET=utf8;
#//朋友关系表
DROP TABLE IF EXISTS t_friend;
create table t_friend(
idA bigint(11) NOT NULL,
idB bigint(11) NOT NULL,
index(idA)
)ENGINE=InnoDB CHARSET=utf8;
#//群信息表
DROP TABLE IF EXISTS t_group;
create table t_group(
g_id bigint(11) NOT NULL auto_increment,
g_name varchar(45) NOT NULL,
g_headpath varchar(260) NOT NULL,
u_id bigint(11) NOT NULL,
PRIMARY KEY ( g_id )
)ENGINE=InnoDB CHARSET=utf8;
#//用户群关系表
DROP TABLE IF EXISTS t_user_group;
create table t_user_group(
u_id bigint(11) NOT NULL,
g_id bigint(11) NOT NULL,
index(u_id)
)ENGINE=InnoDB CHARSET=utf8;
#//群用户关系表
DROP TABLE IF EXISTS t_group_user;
create table t_group_user(
u_id bigint(11) NOT NULL,
g_id bigint(11) NOT NULL,
index(g_id)
)ENGINE=InnoDB CHARSET=utf8;
#//对方不在线未读消息
DROP TABLE if exists ChatContent;
create table ChatContent(
sender bigint(11) NOT NULL,
recver bigint(11) NOT NULL,
groupId bigint(11) default 0,
type varchar(20) NOT NULL,
c_time varchar(60) NOT NULL,
content varchar(8000) NOT NULL,
index(recver)
)ENGINE=InnoDB CHARSET=utf8;
#//添加用户群关系触发器
drop trigger if exists insert_user_group;
delimiter // 
create trigger insert_user_group
after insert 
on t_user_group 
for each row 
begin 
insert into t_group_user(u_id,g_id) values(new.u_id,new.g_id); 
end // 
delimiter ;
#//删除用户群关系触发器
drop trigger if exists delete_user_group;
delimiter // 
create trigger refMinusOne
after delete 
on t_user_group 
for each row 
begin 
delete from t_group_user where t_group_user.u_id = old.u_id;
end // 
delimiter ;
```

```mysql
//客户端数据库
create table ChatInfo(
friendId int ,
userId int,
sender int ,
time varchar(60) ,
content varchar(8000),
type varchar(20)
);
CREATE INDEX fd ON ChatInfo (friendId);//创建索引
```





