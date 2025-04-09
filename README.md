# sql_Beverages_project

``` sql create database project_foodandbev```
``` sql use project_foodandbev```


--Primary Insights (Sample Sections / Questions)

--1. Demographic Insights (examples)

--a. Who prefers energy drink more? (male/female/non-binary?)
<pre> ``` sql select gender,count(*) as 'count' from dim_repondents group by gender order by 'count' desc```</pre>
--Ans = males drinks more

--b. Which age group prefers energy drinks more?
select age,count(*) as 'count'
from dim_repondents
group by age
order by 'count' desc
--Ans)= 19-30 age group drinks more 

--c. Which type of marketing reaches the most Youth (15-30)?
select age,marketing_channels,count(*) 'count'
from fact_survey_responses
join dim_repondents on dim_repondents.Respondent_ID=fact_survey_responses.Respondent_ID
where age in ('15-18','19-30')
group by Marketing_channels,age
order by Marketing_channels 
-- Ans)= online ads has most reach among customers

--2. Consumer Preferences:

--a. What are the preferred ingredients of energy drinks among respondents?
select ingredients_expected,count(*) 'count'
from fact_survey_responses
group by Ingredients_expected 
order by 'count' desc
--Ans)= caffeine is preffered followed by vitamins and sugar

--b. What packaging preferences do respondents have for energy drinks?
select Packaging_preference,count(*) 'count'
from fact_survey_responses
group by Packaging_preference 
order by 'count' desc
--Ans)= Compact and portable cans, innovative bottle design is the preference of customer for the packaging

--3. Competition Analysis:

--a. Who are the current market leaders?
select Current_brands,count(*) 'count'
from fact_survey_responses
group by Current_brands 
order by 'count' desc
--Ans)= Cola-Coka is the current market leader

--b. What are the primary reasons consumers prefer those brands over ours?
select Reasons_for_choosing_brands,count(*) 'count'
from fact_survey_responses
where Current_brands!='codex'
group by Reasons_for_choosing_brands 
order by 'count' desc

select Reasons_for_choosing_brands,count(*) 'count'
from fact_survey_responses
where Current_brands='codex'
group by Reasons_for_choosing_brands 
order by 'count' desc
--Ans)= Brand reputation,taste/flavour preference seems to be reason consumers prefer those brands over ours

--4. Marketing Channels and Brand Awareness:

--a. Which marketing channel can be used to reach more customers?
select Marketing_channels,count(*) 'count'
from fact_survey_responses
group by Marketing_channels 
order by 'count' desc
--Ans)= online ads and tv commercial can be used to reach more customers

--b. How effective are different marketing strategies and channels in reaching our customers?
select Marketing_channels,COUNT(*) 'count',concat(round(100.0 * COUNT(*) / SUM(COUNT(*)) OVER (),2),'%') AS 'percentage'
from fact_survey_responses
where Current_brands='codex'
group by Marketing_channels
order by 'count' desc
--Ans)- online ads has 41.9% reach,tv commercial has 26.6% reach,outdoor billboards has 12.1% reach,other has 11.8% reach,print media has 7.5% reach among consumers

--5. Brand Penetration:

--a. What do people think about our brand? (overall rating)
select Brand_perception,COUNT(Brand_perception) 'count',concat(100*count(*)/sum(count(*)) over(),'%') as percentage
from fact_survey_responses
where Current_brands='codex'
group by Brand_perception
order by 'count' desc
--Ans)= Out of 10000 survey response our market share is around 9.8% in which 60% is neutral,22% is positive,17% is negative


--b. Which cities do we need to focus more on?
select City,COUNT(city) 'count' from fact_survey_responses
join dim_repondents on dim_repondents.Respondent_ID=fact_survey_responses.Respondent_ID
join dim_cities on dim_cities.City_ID=dim_repondents.City_ID
group by City,Current_brands
having Current_brands='codex'
order by 'count' 

--Ans)= cities with lower sales can be targeted for increasing sales example-lucknow,jaipur,delhi,ahmedabad

--6. Purchase Behavior:

--a. Where do respondents prefer to purchase energy drinks?
select Purchase_location,count(*) 'count',concat(100*count(*)/sum(count(*)) over(),'%') as percentage
from fact_survey_responses
group by Purchase_location 
order by 'count' desc
--Ans)= 44% people prefer Supermarket as purchase location

--b. What are the typical consumption situations for energy drinks among respondents?
select Typical_consumption_situations,count(*) 'count',concat(100*count(*)/sum(count(*)) over(),'%') as percentage
from fact_survey_responses
group by Typical_consumption_situations 
order by 'count' desc
--Ans)= 44.94% consume energy drinks while playing and excercising

--c. What factors influence respondents purchase decisions, such as price range and limited edition packaging?
select Price_range,count(*) 'count',concat(100*count(*)/sum(count(*)) over(),'%') as percentage
from fact_survey_responses
group by Price_range 
order by 'count' desc

select Limited_edition_packaging,count(*) 'count',concat(100*count(*)/sum(count(*)) over(),'%') as percentage
from fact_survey_responses
group by Limited_edition_packaging 
order by 'count' desc
--Ans)= price range between 50-150 draws most sale.No clear majority in limited edition packaging

--7. Product Development

--a. Which area of business should we focus more on our product development?
--(Branding/taste/availability)
select Reasons_for_choosing_brands,COUNT(*) as 'count',concat(100*count(*)/sum(count(*)) over(),'%') as percentage
from fact_survey_responses
where Current_brands='codex'
group by Reasons_for_choosing_brands
order by percentage desc
--Ans)= brand reputation is already good for codex brand,availability and taste can be improved.
