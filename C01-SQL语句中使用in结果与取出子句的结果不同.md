## in使用子句与取出子句的结果不同

> 2019-08-15 衾袭
>
> mysql5.7 in子句中group by不生效

## 前世今生

>一天甲方爸爸大促活动，RDS性能遇到了瓶颈，通过追踪是获取优惠卷的一条SQL是主要原因，于是进行了SQL的优化，从1.8s优化到了0.2s。
>
>--甲方爸爸疑问，以下这条语句也可以有相同结果，是因为in的原因不如我们改写的吗？
>
>--我们尝试了一下发现结果都不同，于是开始了以下的路程。



## 甲方爸爸SQL

```sql

select`c_name`,
`c_type`,
c_money,
`c_condition_money`
from fx_coupon where c_id in(SELECT `c_id`
  FROM `fx_coupon`
 WHERE `c_end_time`>= '2019-08-14 23:59:59' GROUP BY c_name)
+--------------------+------------------+-------------------+-----------------------------+
| c_name             | c_type           | c_money           | c_condition_money           |
+--------------------+------------------+-------------------+-----------------------------+
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |
| 5周年庆100元优惠券 |                0 | 100.00            | 1000.00                     |


SELECT `c_id` FROM `fx_coupon` WHERE `c_end_time`>= '2019-08-14 23:59:59' GROUP BY c_name
+----------------+
| c_id           |
+----------------+
| 1060604        |
| 1062604        |
| 1063604        |
| 1065604        |
| 1054804        |
| 1053804        |
| 1055604        |
| 1067604        |
| 1068604        |
| 1070604        |
| 1071604        |
| 1072604        |
| 1177884        |
| 1077886        |
+----------------+
```

## in使用子句的结果

```sql
select c_id,`c_name`,
`c_type`,
c_money,
`c_condition_money`
from fx_coupon a where c_id in
(1060604,1062604,1063604,1065604,1054804,1053804,1055604,1067604,1068604,1070604,1071604,1072604,1177884,1077886)
+----------------+-----------------------------+------------------+-------------------+-----------------------------+
| c_id           | c_name                      | c_type           | c_money           | c_condition_money           |
+----------------+-----------------------------+------------------+-------------------+-----------------------------+
| 1053804        | 兑吧8/9月400元优惠券        |                0 | 400.00            | 3000.00                     |
| 1054804        | 8月家装季满5000减500        |                0 | 500.00            | 5000.00                     |
| 1055604        | 沃驰400元优惠券             |                0 | 400.00            | 3000.00                     |
| 1060604        | 5周年庆100元优惠券          |                0 | 100.00            | 1000.00                     |
| 1062604        | 5周年庆200元优惠券          |                0 | 200.00            | 2000.00                     |
| 1063604        | 5周年庆300元优惠券          |                0 | 300.00            | 3000.00                     |
| 1065604        | 5周年庆400元优惠券          |                0 | 400.00            | 4000.00                     |
| 1067604        | 贝壳专属老板电器100元优惠券 |                0 | 100.00            | 1000.00                     |
| 1068604        | 贝壳专属老板电器200元优惠券 |                0 | 200.00            | 2000.00                     |
| 1070604        | 贝壳专属老板电器300元优惠券 |                0 | 300.00            | 3000.00                     |
| 1071604        | 贝壳专属老板电器400元优惠券 |                0 | 400.00            | 0.00                        |
| 1072604        | 贝壳专属老板电器500元优惠券 |                0 | 500.00            | 5000.00                     |
| 1077886        | 贝壳房源专属老板电器优惠券  |                0 | 450.00            | 4000.00                     |
| 1177884        | 贝壳专属老板电器优惠券      |                0 | 450.00            | 4000.00                     |
+----------------+-----------------------------+------------------+-------------------+-----------------------------+
返回行数：[14]，耗时：7 ms.
```
## 尝试in改写为exits

```sql
mysql>select c_id,`c_name`,
`c_type`,
c_money,
`c_condition_money`
from fx_coupon where exists (SELECT `c_id`
  FROM `fx_coupon`
 WHERE `c_end_time`>= '2019-08-14 23:59:59' GROUP BY c_name)
+----------------+-----------------------------+------------------+-------------------+-----------------------------+
| c_id           | c_name                      | c_type           | c_money           | c_condition_money           |
+----------------+-----------------------------+------------------+-------------------+-----------------------------+
| 5948           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 5949           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 5952           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 5953           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 5961           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 5962           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 5975           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 5987           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 5989           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 6028           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 6040           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 6047           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 6049           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 6053           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 6063           | 满4000减400（会员日）(过期) |                0 | 400.00            | 4000.00                     |
| 6266           | 满6000减600（会员日）(过期) |                0 | 600.00            | 6000.00                     |
| 6278           | 满6000减600（会员日）(过期) |                0 | 600.00            | 6000.00                     |
| 7032           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7033           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7034           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7035           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7036           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7037           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7038           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7039           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7040           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7041           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7042           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7043           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7044           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7045           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7046           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7047           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7048           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7049           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7050           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7051           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7052           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7053           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7054           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7055           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7056           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7057           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7058           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7059           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7060           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7061           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7062           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7063           | O2O专用(过期)               |                0 | 100.00            | 0.00                        |
| 7570           | 新品66A1 200元(过期)        |                0 | 200.00            | 2000.00                     |
| 7590           | 新品66A1 200元(过期)        |                0 | 200.00            | 2000.00                     |
| 7594           | 新品66A1 200元(过期)        |                0 | 200.00            | 2000.00                     |
| 7622           | 新品66A1 200元(过期)        |                0 | 200.00            | 2000.00                     |
| 12706          | 神州专车200元(过期)         |                0 | 200.00            | 2000.00                     |
| 12759          | 神州专车200元(过期)         |                0 | 200.00            | 2000.00                     |
| 12812          | 神州专车200元(过期)         |                0 | 200.00            | 2000.00                     |
| 13207          | 神州专车200元(过期)         |                0 | 200.00            | 2000.00                     |
| 209467         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209476         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209483         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209500         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209512         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209515         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209525         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209534         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209547         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209575         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209580         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209598         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209604         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209615         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209648         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209662         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
| 209673         | 国庆7天购100元(过期)        |                0 | 100.00            | 3000.00                     |
```

## 尝试子句吐出其他不重复的列

```sql
mysql>select`c_name`,
`c_type`,
c_money,
`c_condition_money`
from fx_coupon where c_name in(SELECT `c_name`
  FROM `fx_coupon`
 WHERE `c_end_time`>= '2019-08-14 23:59:59' GROUP BY c_name)
+----------------------+------------------+-------------------+-----------------------------+
| c_name               | c_type           | c_money           | c_condition_money           |
+----------------------+------------------+-------------------+-----------------------------+
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00                     |
| 兑吧8/9月400元优惠券 |                0 | 400.00            | 3000.00     
```

> 通过观察结果发现没有进行group by，验证想法改写语句

## 真相大白

```sql
改写为：
mysql>select`c_name`,
`c_type`,
c_money,
`c_condition_money`
from fx_coupon where c_id in(SELECT `c_id`
  FROM `fx_coupon`
 WHERE `c_end_time`>= '2019-08-14 23:59:59')
  GROUP BY c_name
+-----------------------------+------------------+-------------------+-----------------------------+
| c_name                      | c_type           | c_money           | c_condition_money           |
+-----------------------------+------------------+-------------------+-----------------------------+
| 5周年庆100元优惠券          |                0 | 100.00            | 1000.00                     |
| 5周年庆200元优惠券          |                0 | 200.00            | 2000.00                     |
| 5周年庆300元优惠券          |                0 | 300.00            | 3000.00                     |
| 5周年庆400元优惠券          |                0 | 400.00            | 4000.00                     |
| 8月家装季满5000减500        |                0 | 500.00            | 5000.00                     |
| 兑吧8/9月400元优惠券        |                0 | 400.00            | 3000.00                     |
| 沃驰400元优惠券             |                0 | 400.00            | 3000.00                     |
| 贝壳专属老板电器100元优惠券 |                0 | 100.00            | 1000.00                     |
| 贝壳专属老板电器200元优惠券 |                0 | 200.00            | 2000.00                     |
| 贝壳专属老板电器300元优惠券 |                0 | 300.00            | 3000.00                     |
| 贝壳专属老板电器400元优惠券 |                0 | 400.00            | 0.00                        |
| 贝壳专属老板电器500元优惠券 |                0 | 500.00            | 5000.00                     |
| 贝壳专属老板电器优惠券      |                0 | 450.00            | 4000.00                     |
| 贝壳房源专属老板电器优惠券  |                0 | 450.00            | 4000.00                     |
+-----------------------------+------------------+-------------------+-----------------------------+
返回行数：[14]，耗时：471 ms.
```





 

