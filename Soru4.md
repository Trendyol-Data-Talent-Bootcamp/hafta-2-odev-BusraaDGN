# soru 4) Insert, Update and Delete  & Comparise
``` SQL

create or replace table busra_dogan.content_category as select * from sample.content_category;


create or replace table busra_dogan.content_category_20201222_00_59 as select * from dsmbootcamp.sample.content_category_20201222_00_59;

MERGE busra_dogan.content_category targett
USING busra_dogan.content_category_20201222_00_59 sourcee
ON targett.id = sourcee.id

WHEN NOT MATCHED BY TARGET THEN

  Insert (cdc_date, is_deleted,id,category)
  values (sourcee.cdc_date, sourcee.is_deleted, sourcee.id, sourcee.category)

WHEN MATCHED  THEN
  Update set
        targett.is_deleted = sourcee.is_deleted,
        targett.cdc_date = sourcee.cdc_date,
        targett.category = sourcee.category

WHEN NOT MATCHED BY SOURCE THEN
  Update set targett.is_deleted = true

-- ***   check    ***
SELECT count(*) as Is_equals
FROM busra_dogan.content_category as s
FULL OUTER JOIN
busra_dogan.content_category_target as t on s.id = t.id  
WHERE farm_fingerprint(to_json_string(s)) = farm_fingerprint(to_json_string(t));

```
