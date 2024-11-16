# Data Portfolio: Excel to Power BI

# Objectives
- What is the key pain point?


The Head of Marketing wants to find out who the top YouTubers are in 2024 to decide on which YouTubers would be best to run marketing campaigns throughout the rest of the year.

- What is the ideal solution?

To create a dashboard that provides insights into the top UK YouTubers in 2024 that includes their

- subscriber count
- total views
- total videos, and
- engagement metrics


This will help the marketing team make informed decisions about which YouTubers to collaborate with for their marketing campaigns.

# User story
I want to use a dashboard that analyses YouTube channel data in the UK .

This dashboard should allow me to identify the top performing channels based on metrics like subscriber base and average views.

With this information, I can make more informed decisions about which Youtubers are right to collaborate with, and therefore maximize how effective each marketing campaign is.
# Data Source
What data is needed to achieve our objective?
We need data on the top UK YouTubers in 2024 that includes their

- channel names

- total subscribers

- total views

- total videos uploaded

Where is the data coming from? The data is sourced from Kaggle (an Excel extract), see here to find it.
# Stages
- Design
- Developement
- Testing
- Analysis

# Design
## Dashboard components required

What should the dashboard contain based on the requirements provided?
To understand what it should contain, we need to figure out what questions we need the dashboard to answer:

1. Who are the top 10 YouTubers with the most subscribers?

2. Which 3 channels have uploaded the most videos?

3. Which 3 channels have the most views?

4. Which 3 channels have the highest average views per video?

5. Which 3 channels have the highest views per subscriber ratio?

6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

For now, these are some of the questions we need to answer, this may change as we progress down our analysis.


# Dashboard mockup
What should it look like?

Some of the data visuals that may be appropriate in answering our questions include:

- Table
- Treemap
- Scorecards
- Horizontal bar chart

  ## Tools 


| Tool | Purpose |
| --- | --- |
| Excel | Exploring the data |
| SQL Server | Cleaning, testing, and analyzing the data |
| Power BI | Visualizing the data via interactive dashboards |
| GitHub | Hosting the project documentation and version control |
| Mokkup AI | Designing the wireframe/mockup of the dashboard | 

# Development

## Pseudocode

- What's the general approach in creating this solution from start to finish?

1. Get the data
2. Explore the data in Excel
3. Load the data into SQL Server
4. Clean the data with SQL
5. Test the data with SQL
6. Visualize the data in Power BI
7. Generate the findings based on the insights
8. Write the documentation + commentary
9. Publish the data to GitHub Pages

# Data exploration notes

This is the stage where you have a scan of what's in the data, errors, inconcsistencies, bugs, weird and corrupted characters etc

1. What are your initial observations with this dataset? What's caught your attention so far?
2. There are at least 4 columns that contain the data we need for this analysis, which signals we have everything we need from the file without needing to contact the client for any more data.
3. The first column contains the channel ID with what appears to be channel IDS, which are separated by a @ symbol - we need to extract the channel names from this.
4. Some of the cells and header names are in a different language - we need to confirm if these columns are needed, and if so, we need to address them.
5. We have more data than we need, so some of these columns would need to be removed

# Data cleaning
What do we expect the clean data to look like? (What should it contain? What contraints should we apply to it?)

The aim is to refine our dataset to ensure it is structured and ready for analysis.

The cleaned data should meet the following criteria and constraints:

- Only relevant columns should be retained.
- All data types should be appropriate for the contents of each column.
- No column should contain null values, indicating complete data for all records.
- Below is a table outlining the constraints on our cleaned dataset:

| Property | Description |
| --- | --- |
| Number of Rows	| 100 |
| Number of Columns	| 4 |





And here is a tabular representation of the expected schema for the clean data:

| Column Name	| Data Type |	Nullable|
|--- | --- | --- |
| channel_name	| VARCHAR |	NO |
| total_subscribers	| INTEGER |	NO |
| total_views	| INTEGER |	NO |
| total_videos	| INTEGER |	NO |



## What steps are needed to clean and shape the data into the desired format?

- Remove unnecessary columns by only selecting the ones you need
- Extract Youtube channel names from the first column
- Rename columns using aliases

# Transform the data

## Data Cleaning Steps
```sql
/*
# 1. Remove the non-relevant columns
# 2. Extract the Youtube Channel name from the first column
# 3. Rename the column Name
*/

-- 1.

SELECT
      NOMBRE,
      total_subscribers,
      total_views,
      total_videos
FROM
      youtube_data_from_python

CREATE VIEW vieww_youtube_data_from_python AS

-- 2.
  
SELECT
      CAST(SUBSTRING (NOMBRE, 1, CHARINDEX('@', NOMBRE)-1) 
AS VARCHAR(100))

-- 3.

AS 
      Channel_name,
      Total_Subscribers,
      Total_Views,
      Total_Videos

FROM 
      youtube_data_from_python

```

# Create the SQL view
```sql
/*
# 1. Create a view to store the transformed data
# 2. Cast the extracted channel name as VARCHAR(100)
*/

SELECT CAST(SUBSTRING(NOMBRE, 1, CHARINDEX('@', NOMBRE )-1) AS VARCHAR(100)) AS 
      Channel_Name ,
      total_subscribers,
      total_views,
      total_videos


FROM view_youtube_data_from_python

```

# Testing
- What data quality and validation checks are you going to create?
  
Here are the data quality tests conducted:


## Row count check
```sql
/*
1. Row count check 
*/
SELECT
	COUNT(*) 
AS
no_of_rows
from view_youtube_data_from_python

```

# Column count check


```sql
/*
2. Column count check
*/

SELECT 
	COUNT(*) AS COLUMN_COUNT
FROM
	INFORMATION_SCHEMA.COLUMNS
WHERE 
	TABLE_NAME = view_youtube_data_from_python
```

# Data type
```sql
SELECT 
	column_name,
	data_type
FROM
	INFORMATION_SCHEMA.COLUMNS
WHERE 
	TABLE_NAME = view_youtube_data_from_python
```


# Visualization
## Results
- What does the dashboard look like?


This shows the Top UK Youtubers in 2024 so far.

## DAX Measures

### 1. Total Subscribers (M)
```sql
Total Subcribers(M) = 
VAR million = 1000000
VAR sumOfSubscribers = SUM(vieww_youtube_data_from_python[Total_subscribers])
VAR totalsubscribers = DIVIDE(sumofsubscribers, million)
RETURN TotalSubscribers
```
### 2. Total Views (B)
```sql
Total Views (B) = 
VAR Billion =1000000000
VAR sumOfTotalviews = SUM(vieww_youtube_data_from_python[Total_Views])
VAR Totalviews = DIVIDE(sumOfTotalviews, Billion)
RETURN TotalViews
```
### 3. Total Videos
```sql
Total Videos = 
VAR totalvideos = SUM(vieww_youtube_data_from_python[Total_videos])

RETURN totalvideos
````
### 4. Average views per Videos
```sql
Avg Views Per Videos(M) = 
VAR SumOfTotalViews = SUM(vieww_youtube_data_from_python[total_views])
VAR SUMOfTotalVideos = Sum(vieww_youtube_data_from_python[total_videos])
VAR avgViewsPerVideo = DIVIDE(SumOfTotalViews,SUMOfTotalVideos, BLANK())
VAR finalAvgViewsPerVideo = DIVIDE(avgViewsPerVideo,1000000,BLANK())

RETURN FinalAvgViewsPerVideo
```

### 5. Subscribers Engagement Rate
```sql
Subscriber Engagement Rate = 
VAR SumofTotalSubscribers = SUM(vieww_youtube_data_from_python[Total_Subscribers])
VAR SumofTotalVideos = SUM(vieww_youtube_data_from_python[Total_Videos])
VAR SubscriberEngRate = DIVIDE(SumofTotalSubscribers, SumofTotalVideos, BLANK())

RETURN SubscriberEngRate
```
### 6. Views Per Subscriber
```sql
Views Per Subscriber = 
VAR SumofTotalViews = SUM(vieww_youtube_data_from_python[Total_Views])
VAR SumofTotalSubscribers = SUM(vieww_youtube_data_from_python[Total_Subscribers])
VAR ViewsPerSubscribers =  DIVIDE(SumofTotalViews, SumofTotalSubscribers, BLANK())

RETURN ViewsPerSubscribers
```


# Analysis
## Findings
- What did we find?

For this analysis, we're going to focus on the questions below to get the information we need for our marketing client -

Here are the key questions we need to answer for our marketing client:

1. Who are the top 10 YouTubers with the most subscribers?

2. Which 3 channels have uploaded the most videos?

3. Which 3 channels have the most views?

4. Which 3 channels have the highest average views per video?

5. Which 3 channels have the highest views per subscriber ratio?

6. Which 3 channels have the highest subscriber engagement rate per video uploaded?


| Rank | Channel Name         | Subscribers (M) |
|------|----------------------|-----------------|
| 1    | NoCopyrightSounds    | 33.60           |
| 2    | DanTDM               | 28.60           |
| 3    | Dan Rhodes           | 26.50           |
| 4    | Miss Katy            | 24.50           |
| 5    | Mister Max           | 24.40           |
| 6    | KSI                  | 24.10           |
| 7    | Jelly                | 23.50           |
| 8    | Dua Lipa             | 23.30           |
| 9    | Sidemen              | 21.00           |
| 10   | Ali-A                | 18.90           |




### 2. Which 3 channels have uploaded the most videos?

| Rank | Channel Name    | Videos Uploaded |
|------|-----------------|-----------------|
| 1    | GRM Daily       | 14,696          |
| 2    | Manchester City | 8,248           |
| 3    | Yogscast        | 6,435           |



### 3. Which 3 channels have the most views?


| Rank | Channel Name | Total Views (B) |
|------|--------------|-----------------|
| 1    | DanTDM       | 19.78           |
| 2    | Dan Rhodes   | 18.56           |
| 3    | Mister Max   | 15.97           |


### 4. Which 3 channels have the highest average views per video?

| Channel Name | Averge Views per Video (M) |
|--------------|-----------------|
| Mark Ronson  | 32.27           |
| Jessie J     | 5.97            |
| Dua Lipa     | 5.76            |


### 5. Which 3 channels have the highest views per subscriber ratio?

| Rank | Channel Name       | Views per Subscriber        |
|------|-----------------   |---------------------------- |
| 1    | GRM Daily          | 1185.79                     |
| 2    | Nickelodeon        | 1061.04                     |
| 3    | Disney Junior UK   | 1031.97                     |



### 6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

| Rank | Channel Name    | Subscriber Engagement Rate  |
|------|-----------------|---------------------------- |
| 1    | Mark Ronson     | 343,000                     |
| 2    | Jessie J        | 110,416.67                  |
| 3    | Dua Lipa        | 104,954.95                  |


## Notes

For this analysis, we'll prioritize analysing the metrics that are important in generating the expected ROI for our marketing client, which are the YouTube channels wuth the most

- subscribers
- total views
- videos uploaded

# Validation
## 1. Youtubers with the most subscribers

Calculation breakdown
Campaign idea = product placement

a. NoCopyrightSounds

Average views per video = 6.92 million

Product cost = $5

Potential units sold per video = 6.92 million x 2% conversion rate = 138,400 units sold

Potential revenue per video = 138,400 x $5 = $692,000

Campaign cost (one-time fee) = $50,000

#### Net profit = $692,000 - $50,000 = $642,000

b. DanTDM

Average views per video = 5.34 million

Product cost = $5

Potential units sold per video = 5.34 million x 2% conversion rate = 106,800 units sold

Potential revenue per video = 106,800 x $5 = $534,000

Campaign cost (one-time fee) = $50,000

#### Net profit = $534,000 - $50,000 = $484,000

c. Dan Rhodes

Average views per video = 11.15 million

Product cost = $5

Potential units sold per video = 11.15 million x 2% conversion rate = 223,000 units sold

Potential revenue per video = 223,000 x $5 = $1,115,000

Campaign cost (one-time fee) = $50,000

#### Net profit = $1,115,000 - $50,000 = $1,065,000

Best option from category: Dan Rhodes

## SQL query
```sql
/*

1. Define the Variables
2. Create a CTE that rounds the average views per video
3. Select the tables that are required for the analysis
4. Filter the result with the Youtube channel with highest subscribers
5. 0rder by net_profit from highest to lowest 



*/
--STEP 1
DECLARE @ConversionRate FLOAT= 0.02;				--The Conversion rate at 2%
DECLARE @ProductCost MONEY= 5.0;					--The Product cost at $5
DECLARE @CampaignCost MONEY= 50000;					--The Campaign cost at $50000

--STEP 2
WITH ChannelData AS(
	SELECT
		Channel_name,
		Total_Views,
		Total_Videos,
		ROUND((CAST(Total_Views as FLOAT) / Total_Videos), -4) AS rounded_avg_views_per_video
	FROM youtube_db.dbo.vieww_youtube_data_from_python
)
--SELECT * FROM ChannelData

--STEP 3
SELECT 
	Channel_name,
	rounded_avg_views_per_video,
	(rounded_avg_views_per_video * @ConversionRate) AS Potential_unit_sold_per_video,
	(rounded_avg_views_per_video * @ConversionRate * @productCost) AS Potential_revenue_per_video,
	(rounded_avg_views_per_video * @ConversionRate * @productCost) - @CampaignCost AS Net_profit
FROM 
	ChannelData

--STEP 4
WHERE
	Channel_name IN ('NoCopyrightSounds', 'DanTDM', 'Dan Rhodes')

--STEP 5
ORDER BY
	Net_profit DESC
```

## 2. Youtubers with the most videos uploaded

Calculation breakdown

Campaign idea = sponsored video series

a. GRM Daily

Average views per video = 510,000

Product cost = $5

Potential units sold per video = 510,000 x 2% conversion rate = 10,200 units sold

Potential revenue per video = 10,200 x $5= $51,000

Campaign cost (11-videos @ $5,000 each) = $55,000

#### Net profit = $51,000 - $55,000 = -$4,000 (potential loss)

b. Manchester City

Average views per video = 240,000

Product cost = $5

Potential units sold per video = 240,000 x 2% conversion rate = 4,800 units sold

Potential revenue per video = 4,800 x $5= $24,000

Campaign cost (11-videos @ $5,000 each) = $55,000

#### Net profit = $24,000 - $55,000 = -$31,000 (potential loss)

b. Yogscast

Average views per video = 710,000

Product cost = $5

Potential units sold per video = 710,000 x 2% conversion rate = 14,200 units sold

Potential revenue per video = 14,200 x $5= $71,000

Campaign cost (11-videos @ $5,000 each) = $55,000

#### Net profit = $71,000 - $55,000 = $16,000 (profit)

Best option from category: Yogscast

## SQL query
```sql
/* 
# 1. Define variables
# 2. Create a CTE that rounds the average views per video
# 3. Select the columns you need and create calculated columns from existing ones
# 4. Filter results by YouTube channels
# 5. Sort results by net profits (from highest to lowest)
*/

-- 1.
DECLARE @conversionRate FLOAT = 0.02;           -- The conversion rate @ 2%
DECLARE @productCost FLOAT = 5.0;               -- The product cost @ $5
DECLARE @campaignCostPerVideo FLOAT = 5000.0;   -- The campaign cost per video @ $5,000
DECLARE @numberOfVideos INT = 11;               -- The number of videos (11)


-- 2.
WITH ChannelData AS (
    SELECT
        channel_name,
        total_views,
        total_videos,
        ROUND((CAST(total_views AS FLOAT) / total_videos), -4) AS rounded_avg_views_per_video
    FROM
        youtube_db.dbo.view_uk_youtubers_2024
)


-- 3.
SELECT
    channel_name,
    rounded_avg_views_per_video,
    (rounded_avg_views_per_video * @conversionRate) AS potential_units_sold_per_video,
    (rounded_avg_views_per_video * @conversionRate * @productCost) AS potential_revenue_per_video,
    ((rounded_avg_views_per_video * @conversionRate * @productCost) - (@campaignCostPerVideo * @numberOfVideos)) AS net_profit
FROM
    ChannelData


-- 4.
WHERE
    channel_name IN ('GRM Daily', 'Man City', 'YOGSCAST Lewis & Simon ')


-- 5.
ORDER BY
    net_profit DESC;
```


## 3. Youtubers with the most views

Calculation breakdown

Campaign idea = Influencer marketing

a. DanTDM

Average views per video = 5.34 million

Product cost = $5

Potential units sold per video = 5.34 million x 2% conversion rate = 106,800 units sold

Potential revenue per video = 106,800 x $5 = $534,000

Campaign cost (3-month contract) = $130,000

#### Net profit = $534,000 - $130,000 = $404,000

b. Dan Rhodes

Average views per video = 11.15 million

Product cost = $5

Potential units sold per video = 11.15 million x 2% conversion rate = 223,000 units sold

Potential revenue per video = 223,000 x $5 = $1,115,000

Campaign cost (3-month contract) = $130,000

#### Net profit = $1,115,000 - $130,000 = $985,000

c. Mister Max

Average views per video = 14.06 million

Product cost = $5

Potential units sold per video = 14.06 million x 2% conversion rate = 281,200 units sold

Potential revenue per video = 281,200 x $5 = $1,406,000

Campaign cost (3-month contract) = $130,000

#### Net profit = $1,406,000 - $130,000 = $1,276,000

Best option from category: Mister Max

## SQL query

```sql
/*
# 1. Define variables
# 2. Create a CTE that rounds the average views per video
# 3. Select the columns you need and create calculated columns from existing ones
# 4. Filter results by YouTube channels
# 5. Sort results by net profits (from highest to lowest)
*/



-- 1.
DECLARE @conversionRate FLOAT = 0.02;        -- The conversion rate @ 2%
DECLARE @productCost MONEY = 5.0;            -- The product cost @ $5
DECLARE @campaignCost MONEY = 130000.0;      -- The campaign cost @ $130,000



-- 2.
WITH ChannelData AS (
    SELECT
        channel_name,
        total_views,
        total_videos,
        ROUND(CAST(total_views AS FLOAT) / total_videos, -4) AS avg_views_per_video
    FROM
        youtube_db.dbo.view_uk_youtubers_2024
)


-- 3.
SELECT
    channel_name,
    avg_views_per_video,
    (avg_views_per_video * @conversionRate) AS potential_units_sold_per_video,
    (avg_views_per_video * @conversionRate * @productCost) AS potential_revenue_per_video,
    (avg_views_per_video * @conversionRate * @productCost) - @campaignCost AS net_profit
FROM
    ChannelData


-- 4.
WHERE
    channel_name IN ('Mister Max', 'DanTDM', 'Dan Rhodes')


-- 5.
ORDER BY
    net_profit DESC;
```
