# Vinit's Portfolio


# [Project 1: Internet Mobile Plans and Prices Analysis - World](https://github.com/vinidapooh/Internet-Mobile-Plans-Analysis---2020-2021) 
* Took up this set of data which had prices, speed and user/country profiles.
* I used MS SQL Server to join the tables, and analyse the data. I used Power BI to build out the visualizations for the analysis.


![Viz]![Internet Prices project_page-0001](https://user-images.githubusercontent.com/25292577/184003742-600d6b71-dbac-4a78-8734-e3efc3620355.jpg)


* The challenge to begin this one was the uneven no of countries that were there in some of the datasets. For eg: Some of the African countries were present in the Prices dataset, but were not present in the Speed dataset, and vice versa. For this reason the joins were done keeping the speed data out separately.

* So I created 2 views, one just for prices, and one for speeds.

* Here is the SQL code:

```

select * from Prices
select * from Speed
select * from Users

select count(*) from Prices 
select count(*) from Speed 
select count(*)from Users  


select Country from Prices where InternetPlans is null - 12 countries with no internet plans


select TOP 20 Country, sum(InternetPlans) as IntPlans from Prices
where internetplans is not null 
group by Country
order by sum(InternetPlans)  - Gives you the countries with single digit number of plans

```

--Most Expensive 1GB Mobile Internet Plan

```
select top 20 country, Most_expensive_1GB_USD, InternetPlans from Prices 
where InternetPlans is not null
order by Most_expensive_1GB_USD desc


-- Sub Saharan African countries topping the list for the highest avg price of 1gb Equatorial Guinea ,São Tomé and Príncipe ,Malawi ,Chad,Namibia, Saint Helena

select top 20 country,continent, Average_price_of_1GB_USD,Most_expensive_1GB_USD,InternetPlans,(-Average_price_of_1GB_USD+Most_expensive_1GB_USD) as Variation from Prices 
where InternetPlans is not null
order by Average_price_of_1GB_USD desc



--Cheapest Countries for 1GB by Avg Price
select top 30 country,continent, Average_price_of_1GB_USD, InternetPlans from Prices 
where InternetPlans is not null
order by Average_price_of_1GB_USD

-- Top 30 Cheapest Countries for 1GB by Avg Price with Desc order of Internet plans
select top 30 country,continent, Average_price_of_1GB_USD, InternetPlans from Prices 
where InternetPlans is not null
order by InternetPlans DESC, Average_price_of_1GB_USD


-- Joining countries which have usercount numbers in

create view internetplansonly as
select p.country_code as code, p.country as country, p.continent as continent,u.region as region, p.internetplans as internetplans,
p.average_price_of_1GB_USD as avgprice1gb,p.Cheapest_1GB_for_30_days_USD as cheapest1GBplan,p.Most_expensive_1GB_USD as priciest1GBplan, p.avgprice2021 as avgprice21,p.avgprice2020 as avgprice20,(AvgPrice2021-AvgPrice2020) as pricechange,
u.Internet_users as usercount,u.population as pop
from prices as p
join users as u on u.Country_or_area = p.country
where internetplans is not null 


Countries in descending order of price change from 2020 to 2021

select country, continent, internetplans, pricechange
from internetplansonly
where pricechange > 0
order by pricechange desc

-- 22 Countries saw an increase in mobile internet prices from 2020 to 2021


select count(country) from internetplananalaysis 
where pricechange >0

-- Descending order of countries which saw the sharpest price drop

select country, continent, internetplans, pricechange
from internetplananalaysis
where pricechange <=0
order by pricechange 


-- to incorporate data for speeds
create view internetplananalaysis as
select p.country_code as code, p.country as country, p.continent as continent,u.region as region, p.internetplans as internetplans,
p.average_price_of_1GB_USD as avgprice1gb,p.Cheapest_1GB_for_30_days_USD as cheapest1GBplan,p.Most_expensive_1GB_USD as priciest1GBplan, p.avgprice2021 as avgprice21,p.avgprice2020 as avgprice20,round(s.Avg_Mbit_s_Ookla,2) as speedMB,(AvgPrice2021-AvgPrice2020) as pricechange,
u.Internet_users as usercount,u.population as pop
from prices as p
join users as u on u.Country_or_area = p.country
join speed as s on s.country = p.country
where internetplans is not null


usercount, population and no of internet plans by region

select region, SUM(CAST(usercount AS BIGINT)) as users,SUM(CAST(pop AS BIGINT)) as population,sum(internetplans) as plans
from internetplananalaysis
group by region
order by users desc


 --War torn or impoverished countries with the lowest speeds
 
select top 10 country, avgprice1gb, speedMB from internetplananalaysis
order by speedMB 



```





