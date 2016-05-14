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
