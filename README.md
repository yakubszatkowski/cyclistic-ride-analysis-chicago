# Case study, How does a bike-share navigate speedy success?

<p style="align, center"><img src="./misc/bike.JPG"></p>

This case study is capstone project of Google Data Analytics course, here I will be analysing data provided by 
Chicago's bike share company to understand how it's users are using their services and propose solutions to increase
company profits by converting casual users to premium one (with membership). The case study uses workflow proposed by
Google consisting of 6 steps: Ask, Prepare, Process, Analyze, Share and Act.

### Tech stack:
Python:
<ul>
    <li>pandas</li>
    <li>matplotlib</li>
    <li>plotly</li>
    <li>folium</li>
    <li>geopandas</li>
</ul>

# Business Task
Design marketing strategies aimed at converting casual riders into annual members.

# Sources
<ul>
<li>
  <strong>Bike Ride Data:</strong>
  The <a href="https://divvy-tripdata.s3.amazonaws.com/index.html">data</a> is provided by Motivate International Inc. and is available under the
  <a href="https://divvybikes.com/data-license-agreement" target="_blank">Divvy Data License Agreement</a>.
</li>
<li>
  <strong>Chicago Community Boundaries:</strong>
  Community boundary data is sourced from 
  <a href="https://www.kaggle.com/datasets/kylescissons/city-of-chicago-community-boundaries-geojson" target="_blank">
    this Kaggle dataset</a>.<br>
</li>
</ul>
<p>Note: Some rides originated outside Chicago, particularly in Evanston. To ensure accurate geographical analysis, 
Evanston was manually added to the GeoJSON file using <a href="https://geojson.io/" target="_blank">geojson.io</a>.</p>


# Ask:
* How do annual members and casual riders use Cyclistic bikes differently?
* Why would casual riders buy Cyclistic annual memberships? 
* How can Cyclistic use digital media to influence casual riders to become members?

# Prepare:
* The data has been imported from 2024 statistics from <a href="https://divvy-tripdata.s3.amazonaws.com/index.html">here</a>:
    * 202401-divvy-tripdata.csv
    * 202402-divvy-tripdata.csv
    * 202403-divvy-tripdata.csv
    * 202404-divvy-tripdata.csv
    * 202405-divvy-tripdata.csv
    * 202406-divvy-tripdata.csv
    * 202407-divvy-tripdata.csv
    * 202408-divvy-tripdata.csv
    * 202409-divvy-tripdata.csv
    * 202410-divvy-tripdata.csv
    * 202411-divvy-tripdata.csv
    * 202412-divvy-tripdata.csv
* Loaded source data into pandas dataframe with revelant columns and converted to more efficient datatypes for better 
memory processing. Which resulted in change from 3.3 GB memory usage to 409.7 MB

```
<class 'pandas.core.frame.DataFrame'>
Index: 5860568 entries, 0 to 178371
Data columns (total 11 columns):
 #   Column              Dtype         
---  ------              -----         
 0   ride_id             string        
 1   rideable_type       category      
 2   started_at          datetime64[ns]
 3   ended_at            datetime64[ns]
 4   start_station_name  category      
 5   end_station_name    category      
 6   member_casual       category      
 7   start_lat           category      
 8   start_lng           category      
 9   end_lat             category      
 10  end_lng             category      
dtypes: category(8), datetime64[ns](2), string(1)
memory usage: 409.7 MB
```

# Process:
* Preliminary data analysis
* Removed from dataframe:
    * records that had N/A values in station names
    * records with duplicate ride id
    * records with same start, end time and same start, end station
    * records with rides that lasted less than 10 seconds
* Created new columns needed for further analysis:
    * ride duration column
    * weekday of when ride started
    * month of when ride started
* Sorted dataframe by when ride started

<!-- <p align="center"><img class="img" src="./misc/processed_df.png" ></p> -->

# Analysis & Share
<i>Analysis and Share were done in same step so I can instantly visualise data when searching for patterns.</i>
