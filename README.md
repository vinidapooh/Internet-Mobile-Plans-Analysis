# Vinit's Portfolio


# [Project 1: Internet Mobile Plans and Prices Analysis - World](https://github.com/vinidapooh/Internet-Mobile-Plans-Analysis---2020-2021) 
* Took up this set of data which had prices, speed and user/country profiles.
* I used MS SQL Server to join the tables, and analyse the data. I used Power BI to build out the visualizations for the analysis.


![Uploading Internet Prices project_page-0001.jpg…]()

* The challenge to begin this one was the uneven no of countries that were there in some of the datasets. For eg: Some of the African countries were present in the Prices dataset, but were not present in the Speed dataset, and vice versa. For this reason the joins were done keeping the speed data out separately.

* So I created 2 views, one just for prices, and one for speeds.

* Here is the SQL code for your referral

`

`select * from Prices`
`select * from Speed`
`select * from Users`

select count(*) from Prices 
select count(*) from Speed 
select count(*)from Users  





`




*
