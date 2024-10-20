# Bellabeat: How Can a Wellness Technology Company Play It Smart? 

## A Google Data Analytics Professional Certificate Capstone Project

![Bellabeat](https://user-images.githubusercontent.com/81607668/127726632-fe6da755-6267-4227-8740-77d3275f446e.png)
![image](https://github.com/user-attachments/assets/e2a82ebd-6a2e-40ae-aa8f-792ed4829eba)

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
It contains 11 csv files containing FitBit user data. 

### Prepare the Data:
This Kaggle data set contains personal tness tracker from thirty FitBit users. Each eligible Fitbit user consented to the submission of personal tracker data, including minute-level output for physical activity, hea  rate, and sleep monitoring. It includes information about daily activity, steps, and heart rate that can be used to explor users’ habits. <br>
<b>ROCCC Analysis:</b> <br>
- Reliability: Sampling bias may apply as it is unclear how users were chosen to participate. Additionally, those willing to make their data public are likely to be heavier FitBit users which may skew the data. Finally, the data set does not specify gender. Bellabeat is a smart device company for women, so it would be helpful to be able to segment that group. <br>
- Original: low, the data was collected from a third party survey, Amazon Mechanical Turk.
- Comprehensiveness: 30 users consented to share their data, which is the minimum sample size you should use for analysis. Two data sets only had data for 11-14 users ("weight_log_info_merged.csv & minuteSleep_merged.csv) so I removed those 2 sets from my analysis. However, the data I used was comprehensive to allow me to answer the business objectives <br>
- Current: Data is from 2016, so it may be outdated as smart device technology has evolved. Additionally, the data is only 3 months from 3.12.2016-5.12.2016. Data would be more reliable if it was collected over a longer period of time of at least a year. 
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

`#Remove original data sets now that they are merged` <br>
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
n_unique(heartrate_seconds$Id) `#14 unique Ids--too few participants, so will drop this data set,(less than 30 not reliable)` <br>
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


