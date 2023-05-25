# Evil Geniuses Social Media Analysis  

## 💡Scenario💡  
Evil Geniuses is a professional esports organization with teams competing in various video games. It is one of the most popular and successful organizations in the esports scene and is known for its dedicated fan base and top-tier talent. In this case, I will analyze the social media marketing data to guide marketing strategy for the organization to enhance its engagement rate from the audience. 		

I will be using the six step data analysis process: 
Ask, Prepare, Process, Analyze, Share, Act

## 1. Ask ❓ 

#### Main Goal:   
Analyze social media data to gain insight and help guide marketing strategy for Evil Geniuses to grow its engagement rate.
#### Task 1: 
What is the typical engagement rate EG can expect? What’s the likelihood that we can achieve a 15% engagement rate?
#### Task 2: 
Does day of the week and time of posting affect engagement rates?
#### Task 3:
How are EG game titles doing in terms of social performance? Is there a specific game EG should focus more on or less?
#### Task 4: 
What media type performs the best?
#### Task 5:
What is EG's best performing campaign?
#### Task 6:
Define out a posting strategy for the EG social channels based on discoveries.
#### Task 7:
What suggestions I would give to the social media team if they want to expand their presence (e.g. if our CSGO youtube channel is doing well should we expand to TikTok)?
## 2. Prepare ⚙️  
#### Data Source:   
3000+ Cases of Social Media Data from Jan 1st 2023 to Mar 31st 2023. social_data.xlsx

The dataset contains limitations:   
* There are 1485 Campaigns that are categorized as N/As and not rational to be included to the campign analysis process.  
* There are 9 media types that are categorized as "Carousel", 5 media types that are categorized as "Mixed", and 4 media types that are categorized as "Album" while the rest of the media types are in the counts of hundreds or thousands. It would be improper to evaluate these three media types' engagement rates as they have a low simple size in comparision to the other media types.


## 3. Process 💻    
### Explore and examine data
```diff
data = pd.read_excel("social_data.xlsx")
data
```
The start date & time was 01/01/2023 2:59 PM, end date & time was 03/31/2023 07:55 PM.


### Drop duplicate or NA values  
There were 47 duplicate rows, so I updated the data frame without the duplicates.  
```diff
data = data.drop_duplicates()
data
``` 
There were 1485 Campaigns that are categorized as N/As, so I created a new data frame without the N/As.  
```diff
data['Campaign Name'] = data['Campaign Name'].str.strip()
campaign_performance = data.groupby('Campaign Name').agg({'Total Engagements': 'sum', 'Total Impressions': 'sum'})
no_NA = campaign_performance.drop("N/A")
``` 

### Clean Data   
Group the data by day of the week.
```diff
week_order = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
data['day_of_week'] = pd.Categorical(data['day_of_week'], categories=week_order, ordered=True)
data = data.sort_values('day_of_week')
```
Group the data by the hour of the day.
```diff
engagement_by_hour = data.groupby('hour_of_day')
```
This sorts the dataframe by the day of weeks and hour of the day for easier aggregate analysis.

🔧 [Link to Jupyter Notebook](https://github.com/JingChengGu/EG-Social-Analysis/blob/main/social_analysis.ipynb)


## 4. Analyze 📊    

```diff
SELECT AVG(TotalMinutesAsleep) AS AvgMinutesAsleep, AVG(TotalMinutesAsleep / 60) AS AvgHoursAsleep
FROM sleep_log;
/* Finding the average total minutes asleep */
```
The average minutes asleep is 419.4673, also about 7 hours.

```diff
SELECT AVG(TotalSteps) AS AvgSteps, AVG(TotalDistance) AS AvgDistance, AVG(Calories) AS AvgCalories
FROM daily_activity;
/* Finding the average total steps, total distance, and calories */
```
The average total steps, total distance, and calories is 7637.9106, 5.4897, and 2303.609 calories respectively. 

```diff
SELECT DISTINCT Id
FROM daily_activity
WHERE Id NOT IN (
	SELECT d.Id
	FROM daily_activity AS d 
	JOIN sleep_log_unique AS s
	ON d.ActivityDate = s.SleepDay AND d.Id = s.Id
	JOIN weight_log AS w
	ON s.SleepDay = w.Date AND s.Id = w.Id);
 /* Finding the IDs that do not have records in either the sleep log or weight log */
 
 SELECT DISTINCT Id FROM sleep_log GROUP BY Id;
 /* Finding distinct IDs from sleep log */
 
 SELECT DISTINCT Id FROM weight_log GROUP BY Id;
/* Finding distinct IDs from weight log */
```
* There are 28 records that are either not in the sleep log or the weight log. 
* From sleep log, there were only 24 users who recorded their sleep. 
* From weight log, there were only 8 users who recorded their weight.		

### Frequency users record sleep and weight data		

![sleepdata](https://user-images.githubusercontent.com/97275273/211120242-d19a5f40-37ae-4522-8298-91b74b991abf.png)		

* Users were least likely to record sleep data during Sunday and Monday. 

![weightdata](https://user-images.githubusercontent.com/97275273/211120245-68194ea6-6cb2-464b-aeaa-85d32309273e.png)			

* Users were least likely to record weight data during Friday and Saturday. 


## 5. Share 📋   
Here, I created visualizations using Matplotlib to communicate my findings.   

🎨 [Link to Jupyter Notebook](https://github.com/codinglovespri/BellabeatCaseStudy/blob/89f5566c740876ef5a602267910f980a7c6295de/Bellabeat%20Analysis%20-%20Google%20Data%20Analytics%20Capstone.ipynb)  

![avgsteps](https://user-images.githubusercontent.com/97275273/211120742-243f087b-21f2-4e6f-b3e7-bcc65a60c9df.png)

![avgcalories (1)](https://user-images.githubusercontent.com/97275273/211120754-e0a163be-2670-494c-9c1d-6a501cb22d52.png)

![avgsedentaryminutes](https://user-images.githubusercontent.com/97275273/211120763-971f356b-1680-4907-a7d0-7150279e3436.png)

### Sleep 💤

![avgsleep](https://user-images.githubusercontent.com/97275273/211120866-21c29374-c4d9-464c-8fab-8dab9c3778ca.png)

![timetosleep](https://user-images.githubusercontent.com/97275273/211120858-a4f186fb-7eca-4fa5-9ded-24d355c555ca.png)

### Calories vs Sleep 🏃‍♂️

![CaloriesBurned](https://user-images.githubusercontent.com/97275273/211120386-6ad15d54-0d91-430f-83de-00e9d0a55884.png)

``` diff!
The more steps taken in a day, the more calories a user will burn.    
```
![CaloriesBurnedMedian](https://user-images.githubusercontent.com/97275273/211120426-4091f6a2-4ed6-4585-aa0b-5e7ac0e33621.png)

``` diff
The same graph, with the median steps and median calories burned included. 
```		
### Activity Level vs Sleep 🏋️‍♀️
 
I plotted the line of regression in black to identify the correlation easier. 
![sedentaryandsleep](https://user-images.githubusercontent.com/97275273/211120940-1f334f5e-74dd-415c-bcdd-664bd57bb862.png)

```diff
Correlation coefficient: -0.6010731396971011    
There is a pretty strong negative correlation between the two variables, showing that the more sedentary a user is, the less sleep they get.   
```
 
![lightlyactiveandsleep](https://user-images.githubusercontent.com/97275273/211120962-7ba8278e-7f95-4258-a70d-db4595f815cc.png)

```diff
Correlation coefficient: 0.027583356789564462
There is not a strong correlation between the two variables.
```

![fairlyactiveandsleep](https://user-images.githubusercontent.com/97275273/211120971-08cea67b-c669-465d-8a3a-9fabe7f535bf.png)

```diff
Correlation coefficient: -0.2492079302480945
There is not a strong correlation between the two variables.
```
![veryactiveandsleep](https://user-images.githubusercontent.com/97275273/211120974-2f6aafa7-dbd5-4e52-965e-7a0175208f6e.png)

```diff
Corrleation coefficient: -0.08812657953070487
There is not a strong correlation between the two variables.
```

![PercentageBreakdown](https://user-images.githubusercontent.com/97275273/211120986-ff54ab4c-92ff-472b-8852-4ddaed82ea8b.png)
```diff
A breakdown of the activity level amongst the participants.
```
****
### Key findings: 
* Users recorded weight data the least on Friday and Saturday (the beginning of the weekend). 
* Users recorded sleep data the least during Sunday and Monday (the end of the weekend). 
* Most people in the study were sedentary, 81%.
* Saturday is the most active day, people take the most steps and burn the most calories and have the least amount of sedentary activity as well. 
* People have the hardest time falling asleep on Sunday, however they also get the most amount of sleep on that day. 
* Users burned more calories the more steps they took.
* There is a strong correlation between amount of time spent being sedentary and amount of sleep → the more time people spent being sedentary, the less sleep they got. 

## 6. Act 👩🏻‍🏫
### Recommendations:
* I would encourage Bellabeat to market that their device is comfortable to wear throughout the day, especially during the night or when going out (due to the lack of data on weekends).
* I would encourage Bellabeat to promote body-positivity and inclusivity in their marketing campaigns as the data shows there are more missing weight data than sleep data. 
* I would suggest Bellabeat to create an award system that rewards users with a digital badge or trophy when they hit 10,000 steps, which is the recommended daily average to boost health. Any sort of motivation to encourage users to increase daily steps, as the average daily steps in the dataset was about 7,600, significantly lower than 10,00. 
