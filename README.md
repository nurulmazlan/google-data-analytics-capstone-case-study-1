
#  Case Study 1: How does a bike-share navigate speedy success?

### **Introduction**

**Scenario** 

I am a junior data analyst working on the marketing analyst team at Cyclistic, a bike-share
company in Chicago. I have been tasked with understanding how casual riders and annual members use Cyclistic bikes differently. The director of marketing believes that converting casual riders into annual members is the key to the company’s future success. In order to achieve this, we need to provide data-backed insights and professional visualizations to Cyclistic executives.

### **Company history**

In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown
to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across
Chicago. The bikes can be unlocked from one station and returned to any other station in the
system anytime.


### **Data analysis process**

### **ASK**

**Business task**

- Identify the business task: How annual members and casual use bikes differently?

- Consider key stakeholders:

  + Director of Marketing Lily Moreno: Responsible for the development of campaigns and initiatives to promote the bike-share program.
  
  + Marketing Analytics Team: Collecting, analyzing, and reporting data.
  
  + Executive Team: In charge of approving the recommended marketing program.
  
**Deliverables**
- A clear statement of business task: Identify key factor that will attract casual riders into annual members.
- Problem statement: How casual riders and annual members use Cyclistic bikes differently?
- Insight: maximizing the number of annual members
  
### **PREPARE**

**Key task**
- Download data and store it appropriately.
- Identify how it’s organized.
- Sort and filter the data.
- Determine the credibility of the data.

**1.Download the data and save in a folder**
**2.Identify how the data is organized**
**3.sort and filter the data**

- Download the data ( https://divvy-tripdata.s3.amazonaws.com/index.html ). Save it in a folder and make a subfolder for raw data and cleaned data

<img width="731" height="423" alt="Screenshot 2026-06-16 164125" src="https://github.com/user-attachments/assets/b030a142-288c-4f66-93b4-2928d5cd8fb8" />

<img width="766" height="237" alt="Screenshot 2026-06-24 101330" src="https://github.com/user-attachments/assets/3124f7d5-cf32-456d-9421-762ffc8b7527" />

#

### **PROCESS**

**Key tasks**
- Check the data for errors.
- Choose your tools.
- Transform the data so you can work with it effectively.
- Document the cleaning process.

**Clean and transform the data**
- Combine all table into one

```sql
CREATE OR REPLACE TABLE `cyclistic-case-study-489307.Cyclistic_bike_data.Data_for_2021` AS

SELECT * FROM `cyclistic-case-study-489307.Cyclistic_bike_data.Jan2021`
UNION ALL
SELECT * FROM `cyclistic-case-study-489307.Cyclistic_bike_data.Feb2021`
UNION ALL
SELECT * FROM `cyclistic-case-study-489307.Cyclistic_bike_data.Mar2021`
UNION ALL
SELECT * FROM `cyclistic-case-study-489307.Cyclistic_bike_data.Apr2021`
UNION ALL
SELECT * FROM `cyclistic-case-study-489307.Cyclistic_bike_data.May2021`
UNION ALL
SELECT * FROM `cyclistic-case-study-489307.Cyclistic_bike_data.June2021`
UNION ALL
SELECT * FROM `cyclistic-case-study-489307.Cyclistic_bike_data.July2021`
UNION ALL
SELECT * FROM `cyclistic-case-study-489307.Cyclistic_bike_data.Aug2021`
UNION ALL
SELECT * FROM `cyclistic-case-study-489307.Cyclistic_bike_data.Sept2021`
UNION ALL
SELECT * FROM `cyclistic-case-study-489307.Cyclistic_bike_data.Oct2021`
UNION ALL
SELECT * FROM `cyclistic-case-study-489307.Cyclistic_bike_data.Nov2021`
UNION ALL
SELECT * FROM `cyclistic-case-study-489307.Cyclistic_bike_data.Dec2021`;
```
#

- Remove null and fix durations

```sql
CREATE OR REPLACE TABLE `cyclistic-case-study-489307.Cyclistic_bike_data.Cleaned_data_2021` AS 
SELECT
  ride_id,
  rideable_type,
  started_at,
  ended_at,
  TIMESTAMP_DIFF(ended_at, started_at, MINUTE) AS ride_length_minutes,
  EXTRACT(DAYOFWEEK FROM started_at) AS day_of_week,
  member_casual
FROM
  `cyclistic-case-study-489307.Cyclistic_bike_data.Data_for_2021`
WHERE
  ride_id IS NOT NULL 
  AND started_at IS NOT NULL 
  AND ended_at IS NOT NULL
  AND member_casual IS NOT NULL
  AND TIMESTAMP_DIFF(ended_at, started_at, MINUTE) >= 1
  AND TIMESTAMP_DIFF(ended_at, started_at, MINUTE) <= 1440;
```
#

**Determine data crediblity**
are there issues bias/credibility in the data
How are you addressing licensing, privacy, security, and accessibility?
How did you verify the data’s integrity?
How does it help you answer your question?
Are there any problems with the data?

- Credibility and Bias: The data is reliable, original, comprehensive, current, and cited, provided by Lyft Bikes and Scooters, LLC.
- Licensing, Privacy, Security, Accessibility: The data is made available by Motivate International Inc. under this license https://www.divvybikes.com/data-license-agreement
- Data Integrity: The data was examined and verified for consistency in columns and data types.


### **ANALYZE**
turn the data you’ve gathered, prepared, and processed into actionable information



### **SHARE**
share what you’ve learned with your stakeholders!



### **ACT**


