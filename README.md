# Bellabeat: How Can a Wellness Technology Company Play It Smart?
## A Google Data Analytics Professional Certificate Capstone Project
<p align="center">
  <img src="https://user-images.githubusercontent.com/81607668/127726632-fe6da755-6267-4227-8740-77d3275f446e.png" alt="Bellabeat" width="500" /> <br>
  <img src="https://github.com/user-attachments/assets/e2a82ebd-6a2e-40ae-aa8f-792ed4829eba" alt="image" width="300" />
</p>

### Background:
Urška Sršen and Sando Mur founded Bellabeat, a high-tech company that manufactures health-focused smart products. Sršen used her background as an artist to develop beautifully designed technology that informs and inspires women around the world. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their own health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women. They focus on 5 smart products: the Bellabeat app, Leaf, Time, Spring, and the Bellabeat membership. 

### Business Task:
Analyze smart device usage data in order to gain insight into how consumers use non-Bellabeat smart devices. These insights will help guide marketing strategy for the company. 

### Business Objectives:
1) What are some trends in smart device usage?
2) How could these trends apply to Bellabeat customers?
3) How could these trends help influence Bellabeat's marketing strategy? 

### Tools:
R for Data Cleaning, Data Transformation, Data Analysis, and Data Visualisation.

### The data set: 
The data set is publicly available on [Kaggle](https://www.kaggle.com/arashnic/fitbit).
It contains 11 csv files of FitBit user data. 

### Prepare the Data:
This Kaggle data set contains personal fitness tracker data from 30 FitBit users. Each eligible Fitbit user consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes information about daily activity, steps, and heart rate that can be used to explore users’ habits. <br>
<b>ROCCC Analysis:</b> <br>
- Reliability: Sampling bias may be present as it is unclear how users were chosen to participate. Furthermore, those willing to make their data public are likely to be more engaged FitBit users, which may skew the data. Finally, the data set does not specify gender. Since Bellabeat is a smart device company for women, it would be helpful to be able to segment that group. <br>
- Originality: low, the data was collected from a third party survey, Amazon Mechanical Turk.
- Comprehensiveness: 30 users consented to share their data, which is the minimum sample size you should use for analysis. Two data sets only had data for 11-14 users ("weight_log_info_merged.csv & minuteSleep_merged.csv) so I removed those sets from my analysis. However, the data I used was comprehensive enough to allow me to answer the business objectives <br>
- Current: Data is from 2016, so it may be outdated as smart device technology has evolved since then. Additionally, the data was collected during a short 3 month period, from 3.12.2016-5.12.2016. Data would be more reliable if it was collected over a longer period of time of at least a year. 
- Cited: Data source cited well. <br>

### Process: 
I used R due to its ability to handle large data sets, as well as the ability to process, analyze, and visualize in the same platform. The steps I took to clean & process the data are outlined below. <br>

`# install packages to clean & analyze` <br>
install.packages("tidyverse") <br>
install.packages("lubridate") <br>
install.packages("dplyr") <br>
install.packages("tidyr") <br>
install.packages("ggplot2") <br>
install.packages("skimr") <br>
install.packages("janitor") <br>

`# install corresponding libraries` <br>
library(tidyverse) <br>
library(lubridate) <br>
library(dplyr) <br>
library(tidyr) <br>
library(ggplot2) <br>
library(readr) <br>
library(skimr) <br>
library(janitor) <br>

`# determine working directory to find file paths for .csv files to upload` <br>
getwd() <br>

`#import files into R studio` <br>
dailyActivity_merged <- read.csv("dailyActivity_merged.csv") <br>
heartrate_seconds_merged <- read.csv("heartrate_seconds_merged.csv") <br>
hourlyCalories_merged <- read.csv("hourlyCalories_merged.csv") <br>
hourlyIntensities_merged <- read.csv("hourlyIntensities_merged.csv") <br>
hourlySteps <- read.csv("hourlySteps_merged.csv") <br>
minuteCaloriesNarrow_merged <- read.csv("minuteCaloriesNarrow_merged.csv") <br>
minuteIntensitiesNarrow_merged <- read.csv("minuteIntensitiesNarrow_merged.csv") <br>
minuteMETsNarrow_merged <- read.csv("minuteMETsNarrow_merged.csv") <br>
minuteSleep_merged <- read.csv("minuteSleep_merged.csv") <br>
minuteStepsNarrow_merged <- read.csv("minuteStepsNarrow_merged.csv") <br>
weightLogInfo_merged <- read.csv("weightLogInfo_merged.csv") <br>

`#preview data frames` <br>
head(dailyActivity_merged) <br>
head(heartrate_seconds_merged) <br>
head(hourlyCalories_merged) <br>
head(hourlyIntensities_merged) <br>
head(hourlySteps) <br>
head(minuteCaloriesNarrow_merged) <br>
head(minuteIntensitiesNarrow_merged) <br>
head(minuteMETsNarrow_merged) <br>
head(minuteSleep_merged) <br>
head(minuteStepsNarrow_merged) <br>
head(weightLogInfo_merged) <br>

`#View data frames` <br>
View(dailyActivity_merged) <br>
View(heartrate_seconds_merged) <br>
View(weightLogInfo_merged) <br>

`#rename "dailyActivity_merged" data frame so it is consistent with other data frame names` <br>
daily_activity <- dailyActivity_merged <br>
rm(dailyActivity_merged)  `# Remove the old data frame since it's no longer needed` <br>

`#rename "heartrate_seconds_merged" data frame so it is consistent with other data frame names` <br>
heartrate_seconds <- heartrate_seconds_merged <br>
rm(heartrate_seconds_merged) `# Remove the old data frame since it's no longer needed` <br>

`#rename "weightLogInfo_merged" data frame so it is consistent with other data frame names` <br>
weight_log_info <- weightLogInfo_merged <br>
rm(weightLogInfo_merged) # Remove the old data frame since it's no longer needed <br>

`#Hourly data becomes new data frame, "hourly_data". Merge data together using primary key "Id" & "ActivityHour"` <br>
View(hourlyCalories_merged) <br>
View(hourlyIntensities_merged) <br>
View(hourlySteps_merged)<br>

`# Full Join all data frames using hourly data` <br>
hourly_data <- hourlyCalories_merged %>% <br>
  full_join(hourlyIntensities_merged, by = c("Id", "ActivityHour")) %>% <br>
  full_join(hourlySteps_merged, by = c("Id", "ActivityHour")) <br>
print(hourly_data) <br>

`#Remove original data frames now that they are merged` <br>
rm(hourlyCalories_merged) <br>
rm(hourlyIntensities_merged) <br>
rm(hourlySteps_merged) <br>

`#minuteSleep_merged- rename column name "date" to "ActivityDate" to match other data sets` <br>
colnames(minuteSleep_merged)[colnames(minuteSleep_merged) == "date"] <- "ActivityMinute" <br>
`#Minute data becomes new dataframe, "minute_data". Merge data together using primary key "Id" & "ActivityHour"` <br>
View(minuteCaloriesNarrow_merged) <br>
View(minuteIntensitiesNarrow_merged) <br>
View(minuteMETsNarrow_merged) <br>
View(minuteSleep_merged) <br>
View(minuteStepsNarrow_merged) <br>

`# Full Join all data frames using minute data` <br>
minute_data <- minuteCaloriesNarrow_merged %>% <br>
  full_join(minuteIntensitiesNarrow_merged, by = c("Id", "ActivityMinute")) %>% <br>
  full_join(minuteMETsNarrow_merged, by = c("Id", "ActivityMinute")) %>% <br>
  full_join(minuteSleep_merged, by = c("Id", "ActivityMinute")) %>% <br>
  full_join(minuteStepsNarrow_merged, by = c("Id", "ActivityMinute")) <br>
print(minute_data) <br>

`#Remove unneeded data frames after merging` <br>
rm(hourlyCalories_merged) <br>
rm(hourlyIntensities_merged) <br>
rm(hourlySteps_merged) <br>
rm(minuteCaloriesNarrow_merged) <br>
rm(minuteIntensitiesNarrow_merged) <br>
rm(minuteMETsNarrow_merged) <br>
rm(minuteSleep_merged) <br>
rm(minuteStepsNarrow_merged) <br>

`#verify number of unique users` <br>
n_unique(daily_activity$Id) `#35 unique Ids` <br>
n_unique(heartrate_seconds$Id) `#14 unique Ids--too few participants, so will drop this data set (less than 30 not reliable)` <br>
n_unique(hourly_data$Id) `#34 unique Ids` <br>
n_unique(minute_data$Id) `#34 unique Ids` <br>
n_unique(weight_log_info$Id) `#11 unique Ids --- too few participants,so will drop this data set (less than 30 not reliable)` <br>

rm(heartrate_seconds) <br>
rm(weight_log_info) <br>

`#check for duplicates in data sets` <br>
sum(duplicated(daily_activity)) `#0 duplicates` <br>
sum(duplicated(hourly_data)) `#0 duplicates` <br>
sum(duplicated(minute_data)) `#525 duplicates` <br>

`#Remove all duplicates and missing data` <br>
daily_activity <- daily_activity %>% <br>
  distinct() %>% <br>
  drop_na() <br>
hourly_data <- hourly_data %>% <br>
  distinct() %>% <br>
  drop_na() <br>
minute_data <- minute_data %>% <br>
  distinct() %>% <br>
  drop_na() <br>

`#Run this code to double check & there are now no duplicates` <br>
sum(duplicated(dailyActivity_merged)) #0 duplicates <br>
sum(duplicated(hourly_data)) #0 duplicates <br>
sum(duplicated(minute_data)) #0 duplicates <br>

`#Minute Sleep uses "Value" and "Logid" columns. I don't know what these mean so can't use them in my analysis.` <br>

`#rename columns in minute_data to match hourly_data format` <br>
`# Rename the column B to "NewName"` <br>
colnames(minute_data)[colnames(minute_data) == "Intensity"] <- "TotalIntensity" <br>
colnames(minute_data)[colnames(minute_data) == "value"] <- "Value" <br>
colnames(minute_data)[colnames(minute_data) == "Steps"] <- "StepTotal <br>

### Analyze <br>
`#Summarize & explore each data set` <br>
hourly_data %>%  <br>
  select(Calories,  <br>
         TotalIntensity,  <br>
         AverageIntensity, StepTotal) %>%  <br>
  summary()  <br>
`#Key findings: min calories per hour is 42, max is 933, mean is 94. Total intensity is min 0, max 180, mean 10.83.`  <br>
`#AverageIntensity is min 0 max 3, mean .18. Step total min is 0, max is 10565, mean is 286.2.`  <br>

minute_data %>%   <br>
  select(ActivityMinute,  <br>
         Calories,  <br>
         TotalIntensity, METs, StepTotal) %>%  <br>
  summary()  <br>
`#Key findings: calories per minute is minimum 0, max 10, mean 1.12. Total intensity is min 0, max 3, mean .01.`  <br>
`#METs is min 0, mean 10.36, max 92.`  <br>
`#StepTotal is min 0, mean 1819, max 111.`  <br>

daily_activity %>%   <br>
  select(TotalSteps,  <br>
         TotalDistance,  <br>
         VeryActiveDistance) %>%  <br>
  summary()  <br>
`#Key findings: max daily steps is 28497 and mean is 6547. For reference- CDC recommends step goal of 10k per day.`  <br>

`#Answer question: what day of week are users most active?`  <br>
`#First need to convert the date column from string to Date format in "daily_activity"data frame`<br>
daily_activity <- daily_activity %>% <br>
  mutate(ActivityDate = mdy(ActivityDate),  `#Convert to Date` <br> 
         Weekday = weekdays(ActivityDate)) `#add a new column for day of week` <br>

`#Summarize steps per Weekday`  <br>
summary_data <- daily_activity %>%  <br>
  group_by(Weekday) %>%  <br>
  summarize(StepSummary = mean(TotalSteps), .groups = 'drop')  `# Summarize and drop grouping`  <br>
`#View the summarized data in a tibble`  <br>
  print(summary_data)  <br>

`#Here is the summarized step data in a tibble. Most steps were taken on Wednesaday.`<br>
`#A corresponding visualization is provided in the "share" section below.` <br>
`#This also saved a new dataframe called summary_data` <br> 
`#A  tibble: 7 × 2` <br>
  Weekday   StepSummary  <br>
  <chr>           <dbl>  <br>
    1 Friday          6738.  <br>
  2 Monday          7119.  <br>
  3 Saturday        7090. <br>
  4 Sunday          6058.  <br>
  5 Thursday        6847.  <br>
  6 Tuesday         4915.  <br>
  7 Wednesday       7511.  <br>

`#Now I will look at hourly data to determine what time of day are people most active. Using column hourly intensities.`  <br>
`#First need to split ActivityHour column into date and hour`  <br>
`#Split the datetime column into date and hour`  <br>
hourly_data <- hourly_data %>%  <br>
  mutate(ActivityHour = as.POSIXct(ActivityHour, format = "%m/%d/%Y %I:%M:%S %p"))  <br>
`#Add a new column for the hour`  <br>
hourly_data <- hourly_data %>%  <br>
  mutate(Hour = hour(ActivityHour))  `#Extract the hour using lubridate` <br>

`#Summarize average intensity per hour in new data frame called "summary_hourly_intensities"`  <br>
summary_hourly_intensities <- hourly_data %>%  <br>
  group_by(Hour) %>%  <br>
  summarize(TotalIntensity = mean(TotalIntensity), .groups = 'drop')  <br>
`#A corresponding visualization of this data is included in the "share" section below.` <br>

`#Taking it a step further, is there a correlation between calories burned and hour of day?`  <br>
`#A corresponding visualization is included in the "share" section below.` <br>
summary_hourly_calories<- hourly_data %>%  <br>
  group_by(Hour) %>%  <br>
  summarize(Calories = mean(Calories), .groups = 'drop')  <br>

`#Next, using the daily_activity data set, I will segment users into groups based on average daily step count.`
`#A corresponding visualization of this data is provided in the "share" section below` <br>
`#Based on CDC recommendations of step count per day, I will use the following step count chart to divide users into 5 categories`  <br>
![Picture1](https://github.com/user-attachments/assets/e1112b25-e993-41d5-a367-cb5a0747df19)  <br>

steps <- daily_activity %>%  <br>
  group_by(Id) %>%   <br>
  summarise(  <br>
    total_steps = mean(TotalSteps, na.rm = TRUE),   <br>
    avg_daily_cal = mean(Calories, na.rm = TRUE)  <br>
  ) %>%  <br>
  mutate(user_type = case_when(  <br>
    total_steps < 5000 ~ "sedentary",  <br>
    total_steps >= 5000 & total_steps < 7499 ~ "lightly active",  <br>
    total_steps >= 7499 & total_steps < 9999 ~ "fairly active",  <br>
    total_steps >= 10000 ~ "very active"  <br>
  ))  <br>
`#Display the first few rows of the resulting dataset`  <br>
head(steps)  <br>
`#A tibble: 6 × 4 showung results`  <br>
Id total_steps avg_daily_cal user_type    <br>  
  1 1503960366      11641.         1796. very active   <br> 
2 1624580081       4226.         1353. sedentary      <br>
3 1644430081       9275.         2916. fairly active  <br>
4 1844505072       3641.         1616. sedentary      <br>
5 1927972279       2181.         2254  sedentary    <br> 
6 2022484408      12175.         2475. very active   <br>

user_type_sum <- steps %>%  <br>
  group_by(user_type) %>%  <br>
  summarise(total= n()) %>%  <br>
  mutate(total_percent = round(total * 100 / sum(total), 1))  <br>
print(user_type_sum)  <br>
`#A tibble: 4 × 3 showing summary of user types`  <br>
user_type      total total_percent  <br>
<chr>          <int>         <dbl>  <br>
1 fairly active      9          25.7  <br>
2 lightly active     6          17.1  <br>
3 sedentary         14          40    <br>
4 very active        6          17.1  <br>
`#This analysis tells me that the majority of users are considered "sedentary" in relation to step count, followed by "fairly active", and finally "lightly active" and "fairly active" make up the lowest percentage of users.

### Share:
`#Make a visualization to determine if there is a correlation between steps taken & calories burned`  <br>
ggplot(data = daily_activity, aes(x = TotalSteps, y = Calories)) +  <br>
  geom_point() + geom_smooth() + labs(title ="Total Steps vs. Calories")  <br>
`#The visualization shows a positive correlation between total steps taken and the amount of calories burned.`  <br>
![Total Steps vs Calories 2](https://github.com/user-attachments/assets/9bb04dfc-0c1a-4c72-98d1-d7ea4db2e424)

`#Now that we have analyzed the data to determine the steps taken for each day of the week, I will make a visualization to better understand the data.`  <br>
ggplot(summary_data, aes(x = reorder(Weekday, StepSummary), y = StepSummary, fill = Weekday)) +  <br>
  geom_bar(stat = "identity") +  <br>
  labs(title = "Total Steps per Day of the Week",  <br>
       x = "Day of the Week",  <br>
       y = "Total Steps") +  <br>
  scale_y_continuous(labels = comma) +  # Format y-axis numbers with commas  <br>
  theme_minimal() +  <br>
  theme(axis.text.x = element_text(angle = 45, hjust = 1))  `#Rotate x-axis labels for better readability`  <br>
  `Tuesday & Sunday show the lowest step count, while Monday & Wednesday show the highest step count.` <br>
![Total Steps per Day Updated](https://github.com/user-attachments/assets/027865ce-593e-451b-b882-17274dc46c65)


`#Create a plot to summarize average intensities per hour. What hour of the day are participants most active on average?`  <br>
ggplot(summary_hourly_intensities, aes(x = Hour, y = TotalIntensity)) +  <br>
  geom_line(color = "blue", size = 1) +  # Line for average intensity  <br>
  geom_point(color = "blue", size = 2) +  # Points for each average  <br>
  labs(title = "Average Intensity by Hour",  <br>
       x = "Hour of the Day",  <br>
       y = "Average Intensity") +  <br>
  theme_minimal()  <br>
`#This graph shows that average intensities start to pick up just before 5AM and gradually increase until they spike around noon and 7PM before they dip substantially. I hypothesize that users likely wake up early to work out and start their day, and many people work out again during their lunch hour and after work.`  <br>
![Intensities by Hour](https://github.com/user-attachments/assets/304d0e62-65c6-491f-95ec-88a6c2901ad8)  <br>


`#Create a plot to summarize average calories burned per hour`  <br>
ggplot(summary_hourly_calories, aes(x = Hour, y = Calories)) +  <br>
  geom_line(color = "red", size = 1) +  `# Line for average calories burned`  <br>
  geom_point(color = "red", size = 2) +  `# Points for each average`  <br>
  labs(title = "Average Calories burned by Hour",  <br>
       x = "Hour of the Day",  <br>
       y = "Average Calories Burned") +  <br>
  theme_minimal()  <br>
`#Same peaks and dips as average intensities per hour. Since the graph his similar spikes, it suggests a relationship between average intensity and calories burned.`  <br>
![Calories Burned Per Hour](https://github.com/user-attachments/assets/e549f5a9-3df9-451e-ab08-8f7834198b24)  <br>


`# Create a pie chart to visualize user type by activity`  <br>
ggplot(data = user_type_sum, aes(x = "", y = total_percent, fill = user_type)) +  <br>
  geom_bar(stat = "identity", width = 1, color = "white") +  <br>
  coord_polar("y", start = 0) +  <br>
  scale_fill_brewer(palette = 'Blues') +  <br>
  theme_void() +  # Removes background, grid, numeric labels  <br>
  theme(plot.title = element_text(hjust = 0.5, size = 22, face = "bold")) +  <br>
  geom_text(aes(label = paste0(total_percent, "%")), position = position_stack(vjust = 0.5), color = "black") +  `# Display rounded percentage`  <br>
  labs(title = "User Type by Activity") +  <br>
  guides(fill = guide_legend(title = "Activity Type"))  <br>

`#A majority of Bellabeat users are considered sedentary based on step count (under 5k a day).`  <br>
![User Type by Activity](https://github.com/user-attachments/assets/6e175223-bbb7-45d4-a4a6-e6b02c57b3cd)

### Act <br>
<b>Key Trends & Findings:</b> <br> 
- Based on step count recommendations from the CDC, a majority of users are considered "sedentary" and do not get enough steps per day. <br>  
- Step count averages vary throughout the week. Days ordered from highest to lowest step count are: Wednesday, Monday, Saturday, Thursday, Friday, Sunday, and lastly Tuesday <br>
- There is a positive correlation between total steps taken and amount of calories burned. <br>
- Average intensities by hour start to pick up just before 5AM and gradually increase until they spike around noon and again at 7PM before they dip substantially. I hypothesize that users likely wake up early to exercise and start their day, and many people exercise again during their lunch hour and after work. <br>
- Average calories by hour show the same peaks and dips as average intensities per hour. Since the graphs have similar lines, it suggests a relationship between average intensity and calories burned. As a user has a higher intensity of exercise, they burn more calories. <br>
- Less than half of the participants had sleep and weight log data to share. <br> 

<b> Suggested next steps for Bellabeat marketing team:</b> <br>
- Personalized goal setting: Encourage users to hit their 10k step goal for the day by sending out reminders. <br>
- Gamification & engagement: Smart device users tend to be health conscious & goal oriented. Set daily and weekly goals for calories, step count, sleep, etc. and allow users to easily track data in their app. Bellabeat could gamefy the experience by setting up challenges and giving users awards if they meet their goals. Leaders in the category like Apple are using a simiilar strategy. <br>
- Use historical trends to suggest personalized fitness goals for users. <br>
- Determine why few participants use the sleep tracking feature by sending out a survey. Possible reasons: band is uncomfortable to sleep in (Bellabeat could create comfortable sleep band), users might want to charge device when they are sleeping, or finally users might not understand the importance of tracking sleep. Consider educational materials like consumer emails or blog posts. <br>
- Determine why few participants use the weight log feature by sending out a survey. Is it complicated or cumbersum to record weight? Does the company need to educate consumers with emails or blog posts? <br>
- Send out targeted marketing emails to each category of user (i.e. from active to sedentary) to help them optimize their smart device and get results. <br>
- Encourage new consumers to purchase Bellabeat devices. Consumers may have health goals they want to hit but don't know how to reach them. Bellabeat should emphasize that their devices are able to track comprehensive health data and send personalized goals & coaching along the way. Bellabeat is unique among fitness trackers in that it tracks reproductive health  <br>
