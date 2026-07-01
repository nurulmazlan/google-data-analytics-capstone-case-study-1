
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

### **1. ASK**

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
  
### **2. PREPARE**

**Key task**
- Download data and store it appropriately.
- Identify how it’s organized.
- Sort and filter the data.
- Determine the credibility of the data.
#

*Download the data ( https://divvy-tripdata.s3.amazonaws.com/index.html )*

<img width="731" height="423" alt="Screenshot 2026-06-16 164125" src="https://github.com/user-attachments/assets/b030a142-288c-4f66-93b4-2928d5cd8fb8" />

#

*Save it in a folder and make a subfolder for raw data and cleaned data*

<img width="766" height="237" alt="Screenshot 2026-06-24 101330" src="https://github.com/user-attachments/assets/3124f7d5-cf32-456d-9421-762ffc8b7527" />

#

**Determine data crediblity**

- Credibility and Bias: The data is reliable, original, comprehensive, current, and cited, provided by Lyft Bikes and Scooters, LLC.
- Licensing, Privacy, Security, Accessibility: The data is made available by Motivate International Inc. under this license https://www.divvybikes.com/data-license-agreement
- Data Integrity: The data was examined and verified for consistency in columns and data types.

#

### **3. PROCESS**

**Key tasks**
- Check the data for errors.
- Choose your tools.
- Transform the data so you can work with it effectively.
- Document the cleaning process.

### **Clean and transform the data**

 *Combine all table into one*

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

 *Create new table, remove null and fix durations*

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

### **4. ANALYZE**

**Key task**
- Aggregate the data.
- Organize and format your data.
- Perform calculations.
- Identify trends and relationships.
#

 *Total rides and Average ride length*

```sql
SELECT
  member_casual,
  COUNT(ride_id) AS total_rides,
  ROUND(AVG(TIMESTAMP_DIFF(ended_at, started_at, MINUTE)), 2) AS avg_ride_length
FROM
`cyclistic-case-study-489307.Cyclistic_bike_data.Cleaned_data_2021`
GROUP BY
  member_casual;
```
#

 *Weekly ride pattern (Day of the Week)*

```sql
SELECT
  member_casual,
EXTRACT(DAYOFWEEK FROM STARTED_AT) AS day_of_week_numeric,
FORMAT_TIMESTAMP('%A', started_at) AS day_of_week_name,
COUNT(ride_id) AS total_rides,
ROUND(AVG(TIMESTAMP_DIFF(ended_at, started_at, MINUTE)), 2) AS avg_ride_length
FROM
  `cyclistic-case-study-489307.Cyclistic_bike_data.Cleaned_data_2021`
GROUP BY
  member_casual,
  day_of_week_numeric,
  day_of_week_name;
```
#

### **5. SHARE**

- Determine the best way to share findings.
- Create effective data visualizations.
- Present findings.
- Ensure work is accessible.
#

**Visualization tool: https://public.tableau.com/views/GoogleDataAnalyticsProfessionalCertificateCaseStudy1/Dashboard1**

  *Total rides and Average ride length:*

<img width="944" height="161" alt="Total ride and Average ride length" src="https://github.com/user-attachments/assets/8a9dc63c-86a6-4ee4-9b7a-773ecb9d54e5" />

#

  *Ride pattern by Day of the Week:*

<img width="945" height="446" alt="Ride pattern by Day of the Week" src="https://github.com/user-attachments/assets/2e42a208-a279-46be-b1d5-707ca107e4c4" />

#

### **Sharing findings:**

**Average ride length by member type:**
  + Casual riders have longer average ride length (26.74 minutes) compared to annual members (13.13 minutes). This show that casual riders may be using the bikes for leisure or longer trips, while annual members likely use them for shorter trips.

**Ride pattern by Day of the Week:**
  + Casual riders have higher ride counts on weekends, especially Saturdays and Sundays. And annual members have a more consistent ride count throughout the week..

#

### **6. ACT**

**Key task**
- Create your portfolio.
- Add your case study.
- Practice presenting your case study to a friend or family member.

 **Recommendations:**

1. Targeted marketing:
 - Since the data show spikes on weekend, create weekend membership with benefits. Through this it will attract more riders who only used it on weekend.
 
2. Targeted location advertisement:
 - Place ads near popular spots to gained more subscription for weekend membership.

3. Annual member incentive:
 - Offer incentive for annual member such as, special promotion during summer and discount 20% after a certain amount of rides to make it more appealing for casual riders.


### **Conclusion:**

Introducing targeted weekend passes and alerts at peak leisure stations will successfully convert casual riders into annual subscription.

