## mysql slow query log


### 確認

```
mysql> use isucari
mysql> show variables like 'slow%';
+---------------------+---------------------+
| Variable_name       | Value               |
+---------------------+---------------------+
| slow_launch_time    | 2                   |
| slow_query_log      | ON                  |
| slow_query_log_file | /tmp/mysql-slow.log |
+---------------------+---------------------+
3 rows in set (0.00 sec)

mysql> show variables like 'long%';
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| long_query_time | 0.300000 |
+-----------------+----------+
1 row in set (0.00 sec)
```

### 設定

```
mysql> set global slow_query_log_file = '/tmp/mysql-slow.log';
mysql> set global long_query_time = 0.3;
mysql> set global slow_query_log = ON;
```



## mysql index

### 確認

```
mysql> show index from items;
+-------+------------+-----------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table | Non_unique | Key_name        | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------+------------+-----------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| items |          0 | PRIMARY         |            1 | id          | A         |         382 |     NULL | NULL   |      | BTREE
   |         |               |
| items |          1 | idx_category_id |            1 | category_id | A         |          32 |     NULL | NULL   |      | BTREE
   |         |               |
+-------+------------+-----------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
2 rows in set (0.00 sec)
``` 

### index を張る

```
mysql> CREATE INDEX index_items_created_on ON items(created_at);
mysql> CREATE INDEX index_items_buyer ON items(buyer_id);
mysql> CREATE INDEX index_items_buyer ON items(buyer_id);
```

```


