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

schemaにはる

```
CREATE TABLE `items` (
  `id` bigint NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `seller_id` bigint NOT NULL,
  `buyer_id` bigint NOT NULL DEFAULT 0,
  `status` enum('on_sale', 'trading', 'sold_out', 'stop', 'cancel') NOT NULL,
  `name` varchar(191) NOT NULL,
  `price` int unsigned NOT NULL,
  `description` text NOT NULL,
  `image_name` varchar(191) NOT NULL,
  `category_id` int unsigned NOT NULL,
  `created_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  INDEX idx_category_id (`category_id`),
  INDEX idx_seller(`seller_id`),
  INDEX idx_buyer(`buyer_id`),
  INDEX idx_created_at(`created_at`)
) ENGINE=InnoDB DEFAULT CHARACTER SET utf8mb4;
```


## pt query digest
以下のように実行
```sh
pt-query-digest /path/to/slow-query.log 
```

上部に以下のような統計情報、平均時間や、遅いqueryのランキングが記載
```
# 250ms user time, 0 system time, 38.25M rss, 48.06M vsz
# Current date: Tue Jul 19 00:47:49 2022
# Hostname: power
# Files: ../webapp/mysql/mysql-slow_4.log
# Overall: 2.79k total, 10 unique, 46.47 QPS, 0.18x concurrency __________
# Time range: 2022-07-18T15:46:22 to 2022-07-18T15:47:22
# Attribute          total     min     max     avg     95%  stddev  median
# ============     ======= ======= ======= ======= ======= ======= =======
# Exec time            11s     1ms    75ms     4ms     8ms     4ms     3ms
# Lock time            5ms       0    45us     1us     3us     2us     1us
# Rows sent         15.64k       0      21    5.74   13.83    5.20    5.75
# Rows examine      14.86M       0   9.98k   5.46k   9.80k   4.84k   9.33k
# Query size       275.06k      30     441  101.03  212.52   64.41  112.70

# Profile
# Rank Query ID                        Response time Calls R/Call V/M   It
# ==== =============================== ============= ===== ====== ===== ==
#    1 0xE83DA93257C7B787C67B1B05D2...  4.7442 44.0%   772 0.0061  0.00 SELECT posts
#    2 0xC9383ACA6FF14C29E819735F00...  2.3277 21.6%   770 0.0030  0.00 SELECT posts
#    3 0x19759A5557089FD5B718D440CB...  1.4289 13.2%   428 0.0033  0.03 SELECT posts
#    4 0x9F2038550F51B0A3AB05CA526E...  0.8840  8.2%   298 0.0030  0.00 INSERT comments
#    5 0x26489ECBE26887E480CA8067F9...  0.8019  7.4%   271 0.0030  0.00 INSERT users
#    6 0x009A61E5EFBD5A5E4097914B4D...  0.5346  5.0%   204 0.0026  0.00 INSERT posts
# MISC 0xMISC                           0.0684  0.6%    45 0.0015   0.0 <4 ITEMS>
```

それ以下に各queryの詳細が
(実行時間のヒストグラムや、代表的なqueryが記載)

```
# Query 1: 12.87 QPS, 0.08x concurrency, ID 0xE83DA93257C7B787C67B1B05D2469241 at byte 68794
# Scores: V/M = 0.00
# Time range: 2022-07-18T15:46:22 to 2022-07-18T15:47:22
# Attribute    pct   total     min     max     avg     95%  stddev  median
# ============ === ======= ======= ======= ======= ======= ======= =======
# Count         27     772
# Exec time     43      5s     3ms    16ms     6ms     9ms     2ms     6ms
# Lock time     24     1ms       0    13us     1us     2us     1us     1us
# Rows sent     48   7.57k       2      21   10.04   14.52    2.93    9.83
# Rows examine  50   7.44M   9.77k   9.98k   9.88k   9.80k  107.50   9.80k
# Query size    31  85.88k     113     114  113.91  112.70       0  112.70
# String:
# Databases    isuconp
# Hosts        webapp_app_1.webapp_default
# Users        root
# Query_time distribution
#   1us
#  10us
# 100us
#   1ms  ################################################################
#  10ms  #
# 100ms
#    1s
#  10s+
# Tables
#    SHOW TABLE STATUS FROM `isuconp` LIKE 'posts'\G
#    SHOW CREATE TABLE `isuconp`.`posts`\G
# EXPLAIN /*!50100 PARTITIONS*/
SELECT `id`, `user_id`, `body`, `mime`, `created_at` FROM `posts` WHERE `user_id` = 88 ORDER BY `created_at` DESC\G


```



