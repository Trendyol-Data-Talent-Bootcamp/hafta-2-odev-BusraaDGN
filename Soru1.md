# Soru 1) 1980’den itibaren spor grubu bazında en çok madalya alan 1. 3. 5. ülkeyi bulalım.
  
  ``` SQL 
  with answer1 as(
  select 
    Sport,Country,
    count(Medal) as Medal_cnt,row_number() over(partition by sport order by count(Medal) desc) as row_num
  from busra_dogan.summer_medals 
  where     Year >= 1980
  group by  Country,  Sport
  order by  Sport,  Medal_cnt DESC
                 )

select * from (
 select Sport,
      Country as First_country,Medal_cnt as First_country_cnt,
      lead(country,2) over(partition by sport order by Medal_cnt desc) as Third_country, 
      lead(Medal_cnt,2) over(partition by sport order by Medal_cnt desc) as Third_country_cnt,
      lead(country,4) over(partition by sport order by Medal_cnt desc) as Fifth_country,
      lead(Medal_cnt,4) over(partition by sport order by Medal_cnt desc) as Fifth_country_cnt,  row_num
   from answer1
   order by  Sport,  Medal_cnt DESC
            )
  where row_num =1;
  
 ```
