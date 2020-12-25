# Soru 2) 1980’den itibaren herhangi bir spor grubunda üst üste 3 veya daha fazla madalya almış atletleri bulalım.

``` SQL
With answer2 AS(
SELECT Sport,Athlete,Year ,
COUNT(Year) OVER(PARTITION BY Sport,Athlete ORDER BY Year,Sport ROWS BETWEEN  unbounded preceding and unbounded following) as cnt,
first_value(year) OVER(PARTITION BY Sport,Athlete ORDER BY Year,Sport) as firstYear,
last_value (year) OVER(PARTITION BY Sport,Athlete ORDER BY  Year,Sport ROWS BETWEEN current row and 1 following) as secondYear,
last_value (year) OVER(PARTITION BY Sport,Athlete ORDER BY Year,Sport ROWS BETWEEN current row and 2 following) as thirdYear,

FROM busra_dogan.summer_medals
WHERE Year>=1980 and Medal IS NOT NULL
GROUP by Sport,Athlete,Year
ORDER by cnt DESC,Sport,Athlete,Year asc )

Select Sport,Athlete,cnt,firstYear,secondYear,thirdYear from answer2
WHERE thirdYear-firstYear = 8 and cnt >= 3 ORDER by Sport ASC,cnt DESC ;

```
