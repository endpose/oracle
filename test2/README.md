# oracle
第1步：以system登录到pdborcl，创建角色shen_res_view和用户shen，并授权和分配空间：

```sql
$ sqlplus system/123@pdborcl
SQL> CREATE ROLE shen_res_view;
Role created.
SQL> GRANT connect,resource,CREATE VIEW TO shen_res_view;
Grant succeeded.
SQL> CREATE USER shen IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;
User created.
SQL> ALTER USER shen QUOTA 50M ON users;
User altered.
SQL> GRANT shen_res_view TO shen;
Grant succeeded.
SQL> exit
```
![TUYI](https://github.com/endpose/oracle/blob/master/test2/1.jpg)

- 第2步：新用户shen连接到pdborcl，创建表mytable和视图myview，插入数据，最后将myview的SELECT对象权限授予hr用户。

```sql
$ sqlplus shen/123@pdborcl
SQL> show user;
USER is "SHEN"
SQL> CREATE TABLE mytable (id number,name varchar(50));
Table created.
SQL> INSERT INTO mytable(id,name)VALUES(1,'zhang');
1 row created.
SQL> INSERT INTO mytable(id,name)VALUES (2,'wang');
1 row created.
SQL> CREATE VIEW myview AS SELECT name FROM mytable;
View created.
SQL> SELECT * FROM myview;
NAME
--------------------------------------------------
zhang
wang
SQL> GRANT SELECT ON myview TO hr;
Grant succeeded.
SQL>exit
```
![TUYI](https://github.com/endpose/oracle/blob/master/test2/3.jpg)

- 第3步：用户hr连接到pdborcl，查询shen授予它的视图myview

```sql
$ sqlplus hr/123@pdborcl
SQL> SELECT * FROM shen.myview;
NAME
--------------------------------------------------
zhang
wang
SQL> exit
```
![TUYI](https://github.com/endpose/oracle/blob/master/test2/4.jpg)
