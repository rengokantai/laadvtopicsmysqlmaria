#### laadvtopicsmysqlmaria
#####1 How maria is diff
######How
Added XtraDB PBXT FederatedX storage engines  
install mariadb in ubuntu14.04
```
sudo apt-get install software-properties-common && sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db
sudo add-apt-repository 'deb [arch=amd64,i386] http://mirror.jmu.edu/pub/mariadb/repo/10.1/ubuntu trusty main' && sudo apt-get update -y && sudo apt-get install -y mariadb-server
```
######exploring
mariadb has built-in thread pooling since 5.5, mysql only available in enterprise edition, as plugin

```
thread_pool_size
thread_pool_max_threads
```
#######viewing user stats
```
SET GLOBAL userstat=1;
```
show
```
show
information_schema
```
keyword:  
user_statistics client_statistics, table, index  

demo:
```
select * from information_schema.user_statistics limit 5\G
show client_statistics;
select * from information_schema.table_statistics limit 5\G
```
```
create table test.index_me(id int unsigned not null primary key);
insert into test.index_me(id) values(1),(2);
```
then
```
select * from information_schema.index_statistics limit 5\G
```
######create and using virtual columns
only exist in mariadb.
```
create database examples;
create table virt_cols(email varchar(100) not null, unixtime int not null, rev_email varchar(100) as (reverse(email)) virtual,dt datetime as (from_unixtime(unixtime)) virtual);
insert into virt_cols(email,unixtime) values ('a@b.com',123),('c@d.com',456);
select * from virt_cols limit 5;
```
we dont bother if we choose virtual column
```
select email,dt from virt_cols limit 5;
```
######Using persistent virtual columns
```
select email from virt_cols where rev_email like 'moc.%';
```
no keys on virtual. Cannot alter table virtual.  
Not>252 chars , no dependencies on row data , no const
```
alter table virt_cols drop column rev_email, add column rev_email varchar(100) as (reverse(email)) persistent;
alter table virt_cols add index(rev_email);
select index_name,rows_read from information_schema.index_statistics where table_name='virt_cols'; --should return 0
select email from virt_cols where rev_email like 'moc.%';
select index_name,rows_read from information_schema.index_statistics where table_name='virt_cols'; --test again
```
######progress reporting and millisecond resolution
```
select * from information_schema.processlist\G
```
aria progress reporting:  
check table, repair table, analyze table,optimize table  
```
create table ms (milli time(3),micro datetime(6));
insert into ms(milli,micro) select now(),now();
select * from ms;
```
