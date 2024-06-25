# THE CYCLISTIC CASE STUDY REPORT

The case study consists of 6 phases: Ask, Prepare, Process, Analyse, Share and Act.

## Ask

### 1.Introduction

Cyclistic is a bike-sharing company based in Chicago, Illinois. They possess more than 5800 bikes (and also cargo-bikes, hand-tricycles and reclining bikes) and 600 docked stations throughout the city.

### 2.Business task

Analysing historical trip data from Cyclistic to identify trends and connections between casual riders and annual members.

### 3.Key Stakeholders

**Lily Moreno**: She is the manager and the director of marketing. She is handling the development of campaigns and the ways to promote the bike sharing program.

**Cyclistic Executive team**: A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy.

## Prepare

The data has been shared publicly by Motivate International inc under strict licence. The dataset includes previous 24 months of Cyclistic trip data from May, 2022 through May, 2024. Each row of data is a unique ride with defined start and end data including date, time, station name and ID, lat/long coordinates, the type of bike and whether or not the user was a casual rider or an annual member. It’s also available publicly on Kaggle, thus showing strong integrity.

### Does the data ROCCC?

A good data source is ROCCC which stands for Reliable, Original, Comprehensive, Current, and Cited.
•	Reliable - High - It has millions of rides.  
•	Original - High - It is first-party data.
•	Comprehensive - Med - It matches most of Cyclistic's product parameters.
•	Current - High - The data is from 2022 to 2024.
•	Cited - High- It is first-party data.

## Process 

I used RStudio to conduct my analysis due to the efficiency and accessibility of the program, huge amount of data and being able to create the visualisations.

### 1.Installing packages and library 

```{r}

install.packages("tidyverse")

install.packages(“dplyr”)

install.packages("ggplot2")

install.packages("lubridate")

install.packages("janitor")

install.packages("ggpubr")

install.packages("skimr")

install.packages("here")

install.packages("ggrepel")

install.packages(“readr”)

install.packages(“scales”)


```

```{r}

library(tidyverse)

library(dplyr)

library(ggplot2)

library(lubridate)

library(janitor)

library(ggpubr)

library(skimr)

library(here)

library(ggrepel)

library(readr)

library(scales)

```

I chose these packages to help me with my analysis.

### 2.Importing and preparing the data

```{r}

202205-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202205-divvy-tripdata.csv"),

202206-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202206-divvy-tripdata.csv"),

202207-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202207-divvy-tripdata.csv"),

202208-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202208-divvy-tripdata.csv"),

202209-divvy-publictripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202209-divvy-publictripdata.csv"),

202210-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202210-divvy-tripdata.csv"),

202211-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202211-divvy-tripdata.csv"),

202212-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202212-divvy-tripdata.csv"),

202301-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202301-divvy-tripdata.csv"),

202302-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202302-divvy-tripdata.csv"),

202303-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202303-divvy-tripdata.csv"),

202304-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202304-divvy-tripdata.csv")

202305-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202305-divvy-tripdata.csv"),

202306-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202306-divvy-tripdata.csv"),

202307-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202307-divvy-tripdata.csv"),

202308-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202308-divvy-tripdata.csv"),

202309-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202309-divvy-tripdata.csv"),

202310-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202310-divvy-tripdata.csv"),

202311-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202311-divvy-tripdata.csv"),

202312-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202312-divvy-tripdata.csv"),

202401-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202401-divvy-tripdata.csv"),

202402-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202402-divvy-tripdata.csv"),

202403-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202403-divvy-tripdata.csv"),

202404-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202404-divvy-tripdata.csv"),

202405-divvy-tripdata` <- read.csv("C:/Users/Computer/Desktop/cyclistic.csv/202405-divvy-tripdata.csv")

```

To maintain the consistency of data formats of the columns of each dataset, I have deleted 
The last column ‘X’ consisting of null values of the dataset ‘202405-divvy-tripdata’ using the code:

```{r}

`202405-divvy-tripdata`<-select(`202405-divvy-tripdata`,-X)

```

### 3.Data cleaning 

-I checked the type of each bike offered using unique() function for all tables.

```{r}

unique(`202405-divvy-tripdata`$rideable_type)

```

It was observed that up to Aug 2023, 3 types of bikes were in service ie. classic_bike, docked_bike and electric_bike. Whereas from Sep 2023 docked_bikes were stopped as the users increasingly preferred dock less, e-assist bikes over traditional docked bikes according to various sources.

Then I checked the member status of every table using unique()

```{r}

unique(`202405-divvy-tripdata`$member_casual)

```

for all tables, there are 2 values ie. casual and member. Hence, it’s consistent.

-I checked for duplicated rides in every table.

```{r}

sum(duplicated(`202405-divvy-tripdata`))

```

it’s 0 in every case, therefore no duplicates found.

-I removed negative time of every table, using codes,

```{r}

`202205-divvy-tripdata` %>% filter(ended_at < started_at) %>% count()

`202205-divvy-tripdata` <- `202205-divvy-tripdata` %>% filter(ended_at > started_at)

```

-I removed rows with NA values in every table.

```{r}

`202205-divvy-tripdata`<-`202205-divvy-tripdata` %>% drop_na()

```

-I cleaned and formatted the column names of every table to make sure of its accuracy and consistency.

```{r}

clean_names(`202405-divvy-tripdata`)

`202405-divvy-tripdata` <- rename_with(`202405-divvy-tripdata`, tolower)

```

-I formatted the datetime column from “chr” to “datetime” format in every table.

```{r}

`202205-divvy-tripdata` <- `202205-divvy-tripdata` %>% mutate(started_at = ymd_hms(started_at), ended_at = ymd_hms(ended_at))

```

-Then I merged and renamed the file using bind_rows()

```{r}

bike_tripdata <- bind_rows(`202205-divvy-tripdata`,`202206-divvy-tripdata`,`202207-divvy-tripdata`,`202208-divvy-tripdata`,`202209-divvy-publictripdata`,`202210-divvy-tripdata`,`202211-divvy-tripdata`,`202212-divvy-tripdata`,`202301-divvy-tripdata`,`202302-divvy-tripdata`,`202303-divvy-tripdata`,`202304-divvy-tripdata`,`202305-divvy-tripdata`,`202306-divvy-tripdata`,`202307-divvy-tripdata`,`202308-divvy-tripdata`,`202309-divvy-tripdata`,`202310-divvy-tripdata`,`202311-divvy-tripdata`,`202312-divvy-tripdata`,`202401-divvy-tripdata`,`202402-divvy-tripdata`,`202403-divvy-tripdata`,`202404-divvy-tripdata`,`202405-divvy-tripdata`)

```

-I extracted the day from the started_at column.

```{r}

bike_tripdata <- bike_tripdata %>%  mutate(weekday = wday(started_at, label = TRUE, abbr = TRUE))

```

-I extracted the month from started_at column.

```{r}

bike_tripdata <- bike_tripdata %>% mutate(month = month(started_at,label=TRUE,abbr=TRUE))

```

- I extracted the time from the started_at and ended_at columns.

  ```{r}

  bike_tripdata <- bike_tripdata %>% mutate(start_time = format(started_at, "%H:%M:%S"))%>% mutate(end_time = format(ended_at, "%H:%M:%S")) %>% mutate(start_time = hms(start_time))%>% mutate(end_time = hms(end_time)

  ```

  -I extracted the hour from the started_at column.

  ```{r}

  bike_tripdata <- bike_tripdata %>% mutate(hour = hour(start_time))

  ```

  -Then, I created a duration column.

    ```{r}

    bike_tripdata$duration <- difftime(bike_tripdata$ended_at, bike_tripdata$started_at, units = "mins")

    ```

    ## Analyse and Share(viz)

  ### 1.Determine the number of members vs casual riders.

I wanted to determine the number of actual members vs the number of casual riders.

  ```{r}

table(bike_tripdata$member_casual)

```

casual              member 
4,624,654          7,556,352

```{r}

table(bike_tripdata$rideable_type)

```

classic_bike         docked_bike         electric_bike 
  5,728,999              2,28,488                  6,223,519

- I plotted the results.

I used RStudio to plot the results using the code:

```{r}

ggplot(data=bike_tripdata)+
geom_bar(mapping=aes(x=member_casual, fill=member_casual))

```

refer to the document for graph of Number of Member and Casual riders.

From May 2022 to May 2024, the company had more members (customers that pay a membership fee) than casual riders.

### 2. Number of rides per user and type of bikes.

Then, I wanted to see the number of rides per user (member and casual) by rides offered (electric, classic and docked bikes).

```{r}

bike_tripdata_number <- bike_tripdata %>% select(ride_id, member_casual, rideable_type) %>% group_by(rideable_type, member_casual) %>% count()

```

-I plotted the results using code:

```{r}

ggplot(data=bike_tripdata)+ geom_bar(mapping=aes(x=rideable_type,fill=rideable_type))+facet_wrap(~member_casual)+ scale_y_continuous(labels = scales::number_format(accuracy = 1)) + labs(y = "Count", title = "Usage of bikes between casual and members riders")

```

refer to the document to check graph.

-Electric bikes are the go-to bikes from casual riders, and Classic bikes from member riders.

-Classic bikes comes at 2nd and docked bikes at 3rd(least popular out of the three) from Casual riders.

-Electric bikes a close 2nd (next popular choice) and no/very few docked bikes from member riders.

-It shows that docked bikes are no more in the picture, as the users increasingly preferred dock less, e-assist bikes over traditional docked bikes according to various sources.

### 3. The number of rides per month.

I wondered which months had the most users.

```{r}

bike_trip_month <- bike_tripdata %>% select(member_casual, month) %>% group_by(member_casual, month)%>% count()%>% arrange(member_casual)

member_month <-  filter(bike_trip_month, member_casual =="member")

casual_month <-  filter(bike_trip_month, member_casual =="casual")

```

-I plotted the results in tableau , check the document for the graphs of the same.

-May to September resp. are busiest months for casual riders. It is no surprise to witness spring and summer months being the busiest month for the bike sharing company. The members are again more consistent in their use of the bike. And, for members, May is the busiest month followed by comparatively busy months from June to September.

-In both cases, May is the busiest month out of all the months. As it’s the cusp of Spring and Summer.

-In members, all 12 months have at least 200,000 rides which is surprising.

### 4.Determine the number of rides for each day.

I wanted to determine which day of the week was the busiest for each type of membership.

```{r}

bike_trip_weekdays <- bike_tripdata %>% select(member_casual, weekday) %>% group_by(member_casual, weekday)%>% count()%>% arrange(member_casual)

member_weekday <-  filter(bike_trip_weekdays, member_casual =="member")

casual_weekday <-  filter(bike_trip_weekdays, member_casual =="casual")

```

And I plotted the results in Tableau, check the document for the graphs of the same.

-According to the plots, member riders are the most consistent in using the bikes. 5 days of the week exceeding 1,000,000 rides in the member category with Tuesday, Wednesday, Thursday in the top 3.

-The casual riders mostly use the bikes on Saturday followed by Sunday (the weekends).

-The difference between member and casual riders can be explained through the idea of commute + fitness routine+ leisure for the member, but only leisure for the casual riders.

### 5.Getting the number of rides per hour.

I wanted to be more precise by having the number of rides per hour for each membership.

```{r}

bike_trip_hour <- bike_tripdata %>% select(member_casual, hour) %>% group_by(member_casual, hour)%>% count()%>% arrange(hour)

member_hour <-  filter(bike_trip_hour, member_casual =="member")

casual_hour <- filter(bike_trip_hour, member_casual =="casual")

```

I plotted the results for the same, check the document for the graphs of the same.

-Member riders mostly use the bike to commute back home with a massive peak of use between 4 pm and 6pm and 12 am (15h to 19h and 00h). They also ride at 8 am in the morning most likely due to commute and fitness.

-For casual riders, the peak hours are from 2 pm to 6 pm and 12 am, slight variation in timings from that of members. It can be said that members use bikes throughout the day with some consistency to the difference of the casual users that have a growing pattern from 11 am to 5 pm.

### 6.Finding the top 10 start stations for member and casual riders.

I have calculated the top 10 most common starting points for both the users to gain some insightful insights.

I have used the following code to do the calculation.

```{r}

station_counts <- bike_tripdata %>% select(start_station_name,start_station_id,member_casual)  %>% group_by(start_station_name,start_station_id,member_casual) %>%summarise(user_count = n()) %>% arrange(desc(user_count))

  member_counts<-  filter(station_counts, member_casual =="member")

  casual_counts <- filter(station_counts, member_casual =="casual")

  top_10_stations_member <- member_counts[2:11, ]

  top_10_stations_casual <- casual_counts[2:11, ]

```

I noted down the results for both casuals and members in the document.

### 7.Mean duration for member and casual riders.

```{r}

bike_trip_mean_duration <- bike_tripdata %>% select(member_casual, duration)%>% summarise(mean_duration = mean(duration))

```

=114.9405 mins

```{r}

casual_mean_duration <-  bike_tripdata %>% filter(member_casual == 'casual')

casual_mean_duration %>% select(member_casual, duration) %>% summarise(mean_duration = mean(duration))

```

= mean_duration
  180.5822 mins

  ```{r}

member_mean_duration <-  bike_tripdata %>% filter(member_casual == 'member')

member_mean_duration %>% select(member_casual, duration)%>% summarise(mean_duration = mean(duration))

```

= mean_duration
  74.7663 mins

-I put the values in tabular form and plotted the results in tableau. refer to the documents to check the results.

I can confirm that the mean duration of a bike trip by casual riders is more than 180 minutes (3 hrs) long compared to the 75 minutes (1.25 hrs) of the member riders. Along with the info that casual riders only ride during the weekends on average confirms that the casual riders use the bike for leisure.

## Act phase

•	As I said, over the two-year period that I covered (2022-2024), there were more members than casual riders. The members preferred the classic bikes and the electric bikes being close second. Whereas, casual riders preferred electric bikes.

•	Members ride more during the week than casual members that ride mostly during the weekend, but casual members have a longer ride duration than members. It can be explained by the fact that members ride to commute to work or to go home while casuals ride for pure leisure.

•	Members and casual riders ride more in the evening period, but members also ride a lot during the morning of the work week.

### Recommendations:
•	Plan campaigns to promote the health benefits of using bikes to commute to work and the environmental benefits.

•	Create an app that could track the activity of riders (ie. calories burned or CO2 rejections prevented).

•	Create partnerships with local companies to promote the usage of bikes through discount memberships or stations nearby.

•	Increase bike availability in the top 10 start stations.























