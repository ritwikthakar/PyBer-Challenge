# PyBer Analysis
## Overview of the analysis
The purpose of this exercise is to use Python skills and knowledge of Pandas, to create a summary Data Frame of the ride-sharing data by city type of PyBer. Then, using Pandas and Matplotlib, the aim is to plot multiple-line graph that shows the total weekly fares for each city type. Finally, a written analysis will help summarizes how the data differs by city type and how those differences can be used by decision-makers at PyBer.
```python

# Add Matplotlib inline magic command
%matplotlib inline
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd

# File to Load (Remember to change these)
city_data_to_load = "C:/Users/ritwi/OneDrive/Desktop/Classwork/Weekly Challenges/Challenge 5/Resources/city_data.csv"
ride_data_to_load = "C:/Users/ritwi/OneDrive/Desktop/Classwork/Weekly Challenges/Challenge 5/Resources/ride_data.csv"

# Read the City and Ride Data
city_data_df = pd.read_csv(city_data_to_load)
ride_data_df = pd.read_csv(ride_data_to_load)

# Combine the data into a single dataset
pyber_data_df = pd.merge(ride_data_df, city_data_df, how="left", on=["city", "city"])

# Display the data table for preview
pyber_data_df.head()

#  1. Get the total rides for each city type
df1=pyber_data_df.groupby(["type"]).count()[["ride_id"]]
df1_clos = ["Total Rides"]
df1.columns = df1_clos
df1
# 2. Get the total drivers for each city type
df2=city_data_df.groupby(["type"]).sum()[["driver_count"]]
df2_clos = ["Total Drivers"]
df2.columns = df2_clos
df2
#  3. Get the total amount of fares for each city type
df3=pyber_data_df.groupby(["type"]).sum()[["fare"]]
df3_clos = ["Total fares"]
df3.columns = df3_clos
df3
#  4. Get the average fare per ride for each city type. 
df4=pyber_data_df.groupby(["type"]).mean()[["fare"]]
df4_clos = ["Average Fare per Ride"]
df4.columns = df4_clos
df4
# 5. Get the average fare per driver for each city type.
dfs=pyber_data_df.groupby(["type"]).sum()["fare"]/city_data_df.groupby(["type"]).sum()["driver_count"]
df5 = dfs.to_frame(name='Avergae Fare per Driver')
df5
#  6. Create a PyBer summary DataFrame.
pyber_summary_df = pd.concat([df1,df2,df3,df4,df5], axis =1)
pyber_summary_df
#  7. Cleaning up the DataFrame. Delete the index name
pyber_summary_df.index.name = None
#  8. Format the columns.

pyber_summary_df["Total fares"] = pyber_summary_df["Total fares"].map("${:,.1f}".format)
pyber_summary_df["Average Fare per Ride"] = pyber_summary_df["Average Fare per Ride"].map("${:,.1f}".format)
pyber_summary_df["Avergae Fare per Driver"] = pyber_summary_df["Avergae Fare per Driver"].map("${:,.1f}".format)
pyber_summary_df

```
|               | Total Rides   | Total Drivers  |     Total Fare| Average Fare Per Ride   | Average Fare Per Ride  |
| ------------- |:-------------:| --------------:|--------------:|:-----------------------:| ----------------------:|
| Rural         | 125           | 78             | $4,327.93     | right-aligned           | $1600                  |
| Suburban      | 625           | 490            | $19,356.33    | centered                |   $12                  |
| Urban         | 1,625         | 2405           | $39,854.38    | are neat                |    $1                  |







## Results
### Rural
There are 78 drivers currently operating in the rural area with an average fare per driver at $55.49. A volume of 125 rides within a quarter generated total revenue of $4,327.93 with an average revenue per ride of $34.62. The average fare per driver and average revenue per ride is highest in rural areas however total revenue, total rides and number of drivers is the lowest. Low population density may be a major factor for these results and distance in rural areas are also longer compared to other places. 
### Suburban
There are 490 drivers currently operating in the suburban area with an average fare per driver at $39.50. A volume of 625 rides within a quarter generated total revenue of $19,356.33 with an average revenue per ride of $30.97.
### Urban
There are 2,405 drivers currently operating in the urban area with an average fare per driver at $16.57. A volume of 1,625 rides within a quarter generated total revenue of $39,854.38 with an average revenue per ride of $24.53. The average fare per driver and average revenue per ride is lowest in urban areas however total revenue, total rides and number of drivers is the highest. High population density may be a major factor for these results and distance in urban areas are also shorter compared to other places. 
![Total Fare by City Type](https://github.com/ritwikthakar/PyBer-Challenge/blob/main/Challenge%205/Resources/analysis/PyBer_fare_summary.png)

## Summary
Based on the results, we can make 3 business recommendations to the CEO for addressing the disparities among the city types.
- To increase total revenue in suburban and rural areas, Total number of rides need to increase however this is not feasible given the population density in these areas. We can recommend PyBer to expand portfolio in these areas by having drivers provide other services like food delivery & grocery delivery.
- Given that there is significant demand in urban areas, Pyber can also contemplate increasing fares per ride by 5% in the following year to increase revenue
- By expanding its portfolio in rural and suburban areas, Pyber can also reduce it’s fare by 5% in these areas to stimulate the volume of total rides.
