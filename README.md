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










