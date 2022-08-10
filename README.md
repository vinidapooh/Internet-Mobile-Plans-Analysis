## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/vinidapooh/Internet-Mobile-Plans-Analysis---2020-2021/edit/main/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Mobile Internet Plan Analysis
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

select * from dbo.prices
select count(*) from Speed  where InternetPlans is not null
select count(*)from Users 

--Create View testj as
--select p.Name as Country,u.Region,u.Population,p.NO_OF_Internet_Plans as Internet Plans Count,u.Internet_users,concat('$',p.Average_price_of_1GB_USD) as AveragePrice,p.Cheapest_1GB_for_30_days_USD,p.Most_expensive_1GB_USD,p.Average_price_of_1GB_USD_at_the_start_of_2021 as AvgPrice_Start2021,p.Average_price_of_1GB_USD_at_start_of_2020 as AvgPrice_Start2020,round(s.Avg_Mbit_s_Ookla,2) as AvgSpeed
--from Prices as p
--inner join Users as u on u.Country_or_area = p.Name
--inner join Speed as s on s.Country = u.Country_or_area


create view Test2 as
select Country, Continent,sum(InternetPlans) as IntPlans from Prices
group by Country,Continent -- When you make a new column for grouping, make sure you keep an alias name, or the view doesn't get created


select * from Prices

-- No of Countries here
select count(Country) from Prices where internetplans is not null --242 countries in the dataset
select count(Country_or_area) from Users
select count(Country) from Speed
-- Top 10 Countries with the most no of internet plans

select count(Country)from Prices
where internetplans is not null and InternetPlans < 60
group by Country
order by sum(InternetPlans) DESC 
OFFSET 0 ROWS FETCH FIRST 20 ROWS ONLY --TO GET TOP 5 VALUES.


select Country, sum(InternetPlans) as IntPlans from Prices
group by Country
order by sum(InternetPlans)
OFFSET 0 ROWS FETCH FIRST 30 ROWS ONLY -- CONGO, ERITREA, MARSHALL ISLANDS, NORTH KOREA, CHRISTMAS ISLAND HAVE NO INTERNET PLANS

select TOP 20 Country, sum(InternetPlans) as IntPlans from Prices
where internetplans is not null 
group by Country
order by sum(InternetPlans)  -- ANOTHER WAY TO GET LOWEST VALUES


select * from Prices where InternetPlans is null -- 12 Countries have been shown as having no internet plans

-- Top 20 cheapest countries for 1GB

select top 20 country, Most_expensive_1GB_USD, InternetPlans from Prices 
where InternetPlans is not null
order by Most_expensive_1GB_USD desc

--Most Expensive Avg prices for 1GB
Create View OneGBAnalyze as
select top 20 country,continent, Average_price_of_1GB_USD,Most_expensive_1GB_USD,InternetPlans,(-Average_price_of_1GB_USD+Most_expensive_1GB_USD) as Variation from Prices 
where InternetPlans is not null
order by Average_price_of_1GB_USD desc -- Sub Saharan African countries topping the list for the highest avg price of 1gb


--Cheapest Countries for 1GB by Avg Price
select top 30 country,continent, Average_price_of_1GB_USD, InternetPlans from Prices 
where InternetPlans is not null
order by Average_price_of_1GB_USD -- Israel, Krgyzstan, Fiji and Sudan on the top 4 lowest. India is at 28th position

--Cheapest Countries for 1GB by Avg Price with Desc order of Internet plans
select top 30 country,continent, Average_price_of_1GB_USD, InternetPlans from Prices 
where InternetPlans is not null
order by InternetPlans DESC, Average_price_of_1GB_USD -- ~20 countries have 60 plans, India has 58

select * from prices

--highest price increase of 1GB Avg Price from 2021 to 2022 - top 10 countries

Create View PriceInc2120 as
select top 10 country, continent,internetplans, (AvgPrice2021-AvgPrice2020) as priceincrease from prices
where internetplans is not null AND (AvgPrice2021-AvgPrice2020) >= 0
order by priceincrease DESC --Saint Helena in Africa leds here with a $46 hike, and just 4 internet plans. Seen with small nations of sub saharan africa, Crribean and western Europe

drop view internetplananalaysis
create view internetplananalaysis as
select p.country_code as code, p.country as country, p.continent as continent,u.region as region, p.internetplans as internetplans,
p.average_price_of_1GB_USD as avgprice1gb,p.Cheapest_1GB_for_30_days_USD as cheapest1GBplan,p.Most_expensive_1GB_USD as priciest1GBplan, p.avgprice2021 as avgprice21,p.avgprice2020 as avgprice20,round(s.Avg_Mbit_s_Ookla,2) as speedMB,(AvgPrice2021-AvgPrice2020) as pricechange,
u.Internet_users as usercount,u.population as pop
from prices as p
join users as u on u.Country_or_area = p.country
join speed as s on s.country = p.country
where internetplans is not null


select * from internetplananalaysis

--, users and pop by continent, region

select SUM(CAST(pop AS BIGINT)) from internetplananalaysis where pop is not null

select continent,region, SUM(CAST(usercount AS BIGINT)) as users,SUM(CAST(pop AS BIGINT)) as population,sum(internetplans) as plans
from internetplananalaysis
group by continent,region
order by users desc

select region, SUM(CAST(usercount AS BIGINT)) as users,SUM(CAST(pop AS BIGINT)) as population,sum(internetplans) as plans
from internetplananalaysis
group by region
order by users desc

select * from internetplananalaysis


--Top 10 Countries with the highest MB speed as per Ookla

select top 10 country, avgprice1gb, speedMB from internetplananalaysis
order by speedMB desc --UAE, Norway, qatar, South Korea, Denmark, Saudi Arabia at top6, over 100MBps speeds
--as per avg price of 1gb, Denmark is the cheapest at $0.79 ,with UAE being the priciest at $7.62

select top 10 country, avgprice1gb, speedMB from internetplananalaysis
order by speedMB --War torn or impoverished countries with the lowest speeds

select  country, avgprice1gb, speedMB from internetplananalaysis
where speedMB between 30 and 80
order by speedMB desc

select  country, avgprice1gb, speedMB from internetplananalaysis
where country = 'India'

select * from internetplananalaysis

create view priceinc as(
select country, region, avgprice21, avgprice20,(avgprice21-avgprice20) as pricechange,speedMB,internetplans 
from internetplananalaysis
)

drop view priceinc

--countries which saw avg price increase from 2020 to 2021. 
--Only 22 Countries saw an increase in the price, with Malawi the highest, and Greece being next at $7
select * from priceinc where priceincrease> 0 
order by priceincrease desc

--countries which saw avg price decrease from 2020 to 2021. 
--
select * from priceinc where pricechange <= 0 
order by pricechange

select * from priceinc where pricechange< 0 
order by pricechange desc


select country, internetplans, pricechange, speedMB from internetplananalaysis
where pricechange <= 0
order by pricechange

select count(country) from internetplananalaysis 
where pricechange <=0


select count(country) from internetplananalaysis 
where pricechange >0

select country, continent, internetplans, pricechange
from internetplananalaysis
where pricechange <=0
order by pricechange 

select country, continent, internetplans, pricechange
from internetplananalaysis
where pricechange > 0
order by pricechange desc

create view internetplansonly as
select p.country_code as code, p.country as country, p.continent as continent,u.region as region, p.internetplans as internetplans,
p.average_price_of_1GB_USD as avgprice1gb,p.Cheapest_1GB_for_30_days_USD as cheapest1GBplan,p.Most_expensive_1GB_USD as priciest1GBplan, p.avgprice2021 as avgprice21,p.avgprice2020 as avgprice20,(AvgPrice2021-AvgPrice2020) as pricechange,
u.Internet_users as usercount,u.population as pop
from prices as p
join users as u on u.Country_or_area = p.country
where internetplans is not null 
--creating a view only for those countries on prices and users, where usercount is identified

select country, continent, internetplans, pricechange
from internetplansonly
where pricechange > 0
order by pricechange desc

select * from internetplansonly

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/vinidapooh/Internet-Mobile-Plans-Analysis---2020-2021/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
