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

`# install packages to clean & analyze:` <br>
install.packages("tidyverse") <br>
install.packages("lubridate") <br>
install.packages("dplyr") <br>
install.packages("tidyr") <br>
install.packages("ggplot2") <br>
install.packages("skimr") <br>
install.packages("janitor") <br>

`# install corresponding libraries:`
library(tidyverse) <br>
library(lubridate) <br>
library(dplyr) <br>
library(tidyr) <br>
library(ggplot2) <br>
library(readr) <br>
library(skimr) <br>
library(janitor) <br>

`# determine working directory to find file paths for .csv files to upload`
getwd() <br>

`#import files into R studio:`
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

#preview data frames
head(dailyActivity_merged) 
head(heartrate_seconds_merged)
head(hourlyCalories_merged)
head(hourlyIntensities_merged)
head(hourlySteps)
head(minuteCaloriesNarrow_merged)
head(minuteIntensitiesNarrow_merged)
head(minuteMETsNarrow_merged)
head(minuteSleep_merged)
head(minuteStepsNarrow_merged)
head(weightLogInfo_merged)

#View data frames
View(dailyActivity_merged) 
View(heartrate_seconds_merged)
View(weightLogInfo_merged)

#rename "dailyActivity_merged" data frame so it is consistent with other data frame names
daily_activity <- dailyActivity_merged
rm(dailyActivity_merged)  # Remove the old data frame since it's no longer needed

#rename "heartrate_seconds_merged" data frame so it is consistent with other data frame names
heartrate_seconds <- heartrate_seconds_merged
rm(heartrate_seconds_merged) # Remove the old data frame since it's no longer needed

#rename "weightLogInfo_merged" data frame so it is consistent with other data frame names
weight_log_info <- weightLogInfo_merged
rm(weightLogInfo_merged) # Remove the old data frame since it's no longer needed

#Hourly data becomes new data frame, "hourly_data". Merge data together using primary key "Id" & "ActivityHour"
View(hourlyCalories_merged)
View(hourlyIntensities_merged)
View(hourlySteps_merged)
# Full Join
hourly_data <- hourlyCalories_merged %>%
  full_join(hourlyIntensities_merged, by = c("Id", "ActivityHour")) %>%
  full_join(hourlySteps_merged, by = c("Id", "ActivityHour"))
print(hourly_data)
#Remove original data sets now that they are merged
rm(hourlyCalories_merged)
rm(hourlyIntensities_merged)
rm(hourlySteps_merged)

#minuteSleep_merged- rename column name "date" to "ActivityDate" to match other data sets
colnames(minuteSleep_merged)[colnames(minuteSleep_merged) == "date"] <- "ActivityMinute"
#Minute data becomes new dataframe, "minute_data". Merge data together using primary key "Id" & "ActivityHour"
View(minuteCaloriesNarrow_merged)
View(minuteIntensitiesNarrow_merged)
View(minuteMETsNarrow_merged)
View(minuteSleep_merged)
View(minuteStepsNarrow_merged)
# Full Join
minute_data <- minuteCaloriesNarrow_merged %>%
  full_join(minuteIntensitiesNarrow_merged, by = c("Id", "ActivityMinute")) %>%
  full_join(minuteMETsNarrow_merged, by = c("Id", "ActivityMinute")) %>% 
  full_join(minuteSleep_merged, by = c("Id", "ActivityMinute")) %>% 
  full_join(minuteStepsNarrow_merged, by = c("Id", "ActivityMinute"))
print(minute_data)

#Remove unneeded data frames after merging
rm(hourlyCalories_merged) 
rm(hourlyIntensities_merged)
rm(hourlySteps_merged)
rm(minuteCaloriesNarrow_merged)
rm(minuteIntensitiesNarrow_merged)
rm(minuteMETsNarrow_merged)
rm(minuteSleep_merged)
rm(minuteStepsNarrow_merged)

#verify number of unique users 
n_unique(daily_activity$Id) #35
n_unique(heartrate_seconds$Id) #14 too few participants, so will drop this data set,(less than 30 not reliable)
n_unique(hourly_data$Id) #34
n_unique(minute_data$Id) #34
n_unique(weight_log_info$Id) #11 too few participants,so will drop this data set (less than 30 not reliable)

rm(heartrate_seconds)
rm(weight_log_info)

#check for duplicates in data sets
sum(duplicated(daily_activity)) #0 duplicates
sum(duplicated(hourly_data)) #0 duplicates
sum(duplicated(minute_data)) #525 duplicates

#Remove all duplicates and missing data
daily_activity <- daily_activity %>%
  distinct() %>%
  drop_na()
hourly_data <- hourly_data %>%
  distinct() %>%
  drop_na()
minute_data <- minute_data %>%
  distinct() %>%
  drop_na()

#Ran this code to double check & there are now no duplicates
sum(duplicated(dailyActivity_merged)) #0 duplicates
sum(duplicated(hourly_data)) #0 duplicates
sum(duplicated(minute_data)) #0 duplicates

#Minute Sleep uses "Value" and "Logid" columns. I don't know what these mean so can't use them in my analysis.

#rename columns in minute_data to match hourly_data
# Rename the column B to "NewName"
colnames(minute_data)[colnames(minute_data) == "Intensity"] <- "TotalIntensity"
colnames(minute_data)[colnames(minute_data) == "value"] <- "Value"
colnames(minute_data)[colnames(minute_data) == "Steps"] <- "StepTotal
![image](https://github.com/user-attachments/assets/4f9ea222-c6f8-4ed8-99d9-f5440016d0f1)




