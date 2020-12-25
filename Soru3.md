# Soru 3) "sample.pageview": tablosunda 1 gün içerisinde trendyol.com a gelen tüm ziyaretlerin logu var.

``` SQL

CREATE TABLE IF NOT EXISTS busra_dogan.answer3 (
  `view_period` TIMESTAMP,
  `active_user_count` INT64
);

DECLARE initial_time TIMESTAMP DEFAULT '2020-03-02 23:55:00';


WHILE initial_time < '2020-03-03 23:55:00' DO
  INSERT INTO busra_dogan.answer3 (view_period, active_user_count)
        SELECT timestamp_add(initial_time,INTERVAL 5 MINUTE) as view_period, HLL_COUNT.extract(HLL_COUNT.init(deviceid,24)) as active_user_count FROM busra_dogan.pageview
          WHERE view_ts BETWEEN initial_time AND timestamp_add(initial_time,INTERVAL 5 MINUTE);
  SET initial_time = timestamp_add(initial_time, INTERVAL 1 MINUTE);
END WHILE;

 SELECT * FROM busra_dogan.answer3 ORDER BY view_period




```
