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
`#Key findings: calories per min minimum 0, max 10, mean 1.12. Total intensity is min 0, max 3, mean .01.`  <br>
`#METs min 0, mean 10.36, max 92.`  <br>
`#StepTotal is min 0, mean 1819, max 111.`  <br>

daily_activity %>%   <br>
  select(TotalSteps,  <br>
         TotalDistance,  <br>
         VeryActiveDistance) %>%  <br>
  summary()  <br>
`#Key findings: max daily steps is 28497 and mean is 6547. For reference- CDC recommends step goal of 10k per day.`  <br>

#Make a viz to determine if there is a correlation between steps taken & calories burned
ggplot(data = daily_activity, aes(x = TotalSteps, y = Calories)) + 
  geom_point() + geom_smooth() + labs(title ="Total Steps vs. Calories")
#I see a positive correlation between total steps taken and the amount of calories burned.

#What day of week are users most active?
#First need to convert the date column from string to Date format in "daily_activity"data frame
daily_activity <- daily_activity %>%
  mutate(ActivityDate = mdy(ActivityDate),  # Convert to Date
         Weekday = weekdays(ActivityDate)) #add a new column for day of week


#Summarize steps per Weekday
summary_data <- daily_activity %>%
  group_by(Weekday) %>%
  summarize(StepSummary = mean(TotalSteps), .groups = 'drop')  # Summarize and drop grouping
# View the summarized data- this showed my data in a tibble
  print(summary_data)

#Here is the summarized step data in a tibble (this is not code, just for notes.) Most steps Saturday
  # A tibble: 7 × 2
  #This also saved a new dataframe called "summary_data"
  # A tibble: 7 × 2
  Weekday   StepSummary
  <chr>           <dbl>
    1 Friday          6738.
  2 Monday          7119.
  3 Saturday        7090.
  4 Sunday          6058.
  5 Thursday        6847.
  6 Tuesday         4915.
  7 Wednesday       7511.
  
#Now that we have gotten the steps taken for each day of the week. I will make a visualization to better understand the data.
ggplot(summary_data, aes(x = reorder(Weekday, StepSummary), y = StepSummary, fill = Weekday)) +
  geom_bar(stat = "identity") +
  labs(title = "Total Steps per Day of the Week",
       x = "Day of the Week",
       y = "Total Steps") +
  scale_y_continuous(labels = comma) +  # Format y-axis numbers with commas
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))  # Rotate x-axis labels for better readability

#Now will look at Hourly data- what time of day are people most active? Using hourly intensities. 
#First need to split ActivityHour column into date and hour
# Split the datetime column into date and hour
hourly_data <- hourly_data %>%
  mutate(ActivityHour = as.POSIXct(ActivityHour, format = "%m/%d/%Y %I:%M:%S %p"))
# Add a new column for the hour
hourly_data <- hourly_data %>%
  mutate(Hour = hour(ActivityHour))  # Extract the hour using lubridate

# Summarize average intensity per hour in new data frame "summary_hourly_intensities"
summary_hourly_intensities <- hourly_data %>%
  group_by(Hour) %>%
  summarize(TotalIntensity = mean(TotalIntensity), .groups = 'drop')

#Create a plot to summarize average intensities per hour
ggplot(summary_hourly_intensities, aes(x = Hour, y = TotalIntensity)) +
  geom_line(color = "blue", size = 1) +  # Line for average intensity
  geom_point(color = "blue", size = 2) +  # Points for each average
  labs(title = "Average Intensity by Hour",
       x = "Hour of the Day",
       y = "Average Intensity") +
  theme_minimal()
#This graph shows spikes around noon, 7PM and dips substantially. People likely working out at lunch hour and after work

#Taking a step further, is there a correlation between calories burned for hour?
summary_hourly_calories<- hourly_data %>%
  group_by(Hour) %>%
  summarize(Calories = mean(Calories), .groups = 'drop')
#Create a plot to summarize average calories burned per hour
ggplot(summary_hourly_calories, aes(x = Hour, y = Calories)) +
  geom_line(color = "red", size = 1) +  # Line for average calories burned 
  geom_point(color = "red", size = 2) +  # Points for each average
  labs(title = "Average Calories burned by Hour",
       x = "Hour of the Day",
       y = "Average Calories Burned") +
  theme_minimal()
#Same peaks and dips as average intensities per hour. This shows 


#TRYING SOMETHING
# Create a combined plot
ggplot() +
  geom_line(data = summary_hourly_calories, aes(x = Hour, y = Calories, color = "Calories"), size = 1) +
  geom_point(data = summary_hourly_calories, aes(x = Hour, y = Calories, color = "Calories"), size = 2) +
  geom_line(data = summary_hourly_intensities, aes(x = Hour, y = TotalIntensity, color = "Intensity"), size = 1) +
  geom_point(data = summary_hourly_intensities, aes(x = Hour, y = TotalIntensity, color = "Intensity"), size = 2) +
  labs(title = "Average Calories and Intensity by Hour",
       x = "Hour of the Day",
       y = "Values",
       color = "Legend") +
  theme_minimal() +
  scale_color_manual(values = c("Calories" = "red", "Intensity" = "blue")) +  # Custom colors
  scale_x_continuous(breaks = 0:23)  # Ensure all hours are shown on the x-axis


#Activity level by category
#Distribution of users based on daily step count. CDC recommendations chart. 
##TRYING SOMETHING NEW 
steps <- daily_activity %>% 
  group_by(Id) %>% 
  summarise(
    total_steps = mean(TotalSteps, na.rm = TRUE), 
    avg_daily_cal = mean(Calories, na.rm = TRUE)
  ) %>%
  mutate(user_type = case_when(
    total_steps < 5000 ~ "sedentary",
    total_steps >= 5000 & total_steps < 7499 ~ "lightly active",
    total_steps >= 7499 & total_steps < 9999 ~ "fairly active",
    total_steps >= 10000 ~ "very active"
  ))
# Display the first few rows of the resulting dataset
head(steps)
# A tibble: 6 × 4 showung results
Id total_steps avg_daily_cal user_type    
<dbl>       <dbl>         <dbl> <chr>        
  1 1503960366      11641.         1796. very active  
2 1624580081       4226.         1353. sedentary    
3 1644430081       9275.         2916. fairly active
4 1844505072       3641.         1616. sedentary    
5 1927972279       2181.         2254  sedentary    
6 2022484408      12175.         2475. very active  

user_type_sum <- steps %>%
  group_by(user_type) %>%
  summarise(total= n()) %>%
  mutate(total_percent = round(total * 100 / sum(total), 1)) 
print(user_type_sum)
#Here is a tibble showing summary of user types
# A tibble: 4 × 3
user_type      total total_percent
<chr>          <int>         <dbl>
  1 fairly active      9          25.7
2 lightly active     6          17.1
3 sedentary         14          40  
4 very active        6          17.1

#Pie chart with user type by activity
ggplot(data = user_type_sum, aes(x = "", y = total_percent, fill = user_type)) +
  geom_bar(stat = "identity", width = 1, color = "white") +
  coord_polar("y", start = 0) +
  scale_fill_brewer(palette = 'Blues') +
  theme_void() +  # Removes background, grid, numeric labels
  theme(plot.title = element_text(hjust = 0.5, size = 22, face = "bold")) +
  geom_text(aes(label = paste0(total_percent, "%")), position = position_stack(vjust = 0.5), color = "black") +  # Display rounded percentage
  labs(title = "User Type by Activity") + 
  guides(fill = guide_legend(title = "Activity Type"))

#majority of Bellabeat users are considered sedentary based on step count (under
#5k a day. Bellabeat could set step count goals for users and send out reminders
#when they need to get up and walk to hit their goals. 
![image](https://github.com/user-attachments/assets/9842954a-e84e-4200-9dfc-10a8ff9d72cf)
