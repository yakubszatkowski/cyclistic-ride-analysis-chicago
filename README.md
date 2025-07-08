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

<img src="./misc/processed_df.png" alt="Processed df">

# Analysis & Share
<i>Analysis and Share were done in same step so I can instantly visualise data when searching for patterns.</i>
<ul>

  <li>
    <strong>User Type Activity (2024)</strong><br><br>
    <img src="./graphs/01. User Type Activity (2024).png" alt="User Type Activity">
    <ul>
      <li>Registered members' activity is about 1.8 times that of casual users (over 1.1 million records).</li>
    </ul>

<br><br>
  <li>
    <strong>Ridable Type Activity (2024)</strong><br><br>
    <img src="./graphs/02. Ridable Type Activity (2024).png" alt="Ridable Type Activity">
    <ul>
      <li>Classic bikes are used about twice as often as electric bikes.</li>
      <li>Electric scooters have very low usage - potential area for growth (requires further analysis).</li>
    </ul>
  </li>

<br><br>
  <li>
    <strong>Weekday Users Activity by Subscription Type (2024)</strong><br><br>
    <img src="./graphs/03. Weekday Users Activity by Subscription type (2024).png" alt="Weekday Users Activity">
    <ul>
      <li>Members show weekday commuting patterns, indicating work-related use.</li>
      <li>Casual users peak on weekends - mostly used for leisure, shopping, and errands.</li>
    </ul>
  </li>

<br><br>
  <li>
    <strong>Monthly Ride Activity by Subscription Type (2024)</strong><br><br>
    <img src="./graphs/04. Monthly Ride Activity by Subscription Type (2024).png" alt="Monthly Ride Activity by Subscription">
    <ul>
      <li>Membership usage exceeds casual usage by 10,000-30,000 rides throughout the year.</li>
      <li>Significant rise in activity between May and September; drop-off begins in October.</li>
      <li>Late summer shows peak activity for both groups.</li>
    </ul>
  </li>

<br><br>
  <li>
    <strong>Monthly Ride Activity by Bike Type (2024)</strong><br><br>
    <img src="./graphs/05. Monthly Ride Activity by Bike Type (2024).png" alt="Monthly Ride Activity by Bike Type">
    <ul>
      <li>Classic bikes dominate during warmer months - show more seasonal volatility.</li>
      <li>Electric bikes maintain consistent usage throughout the year.</li>
      <li>Electric scooters were discontinued after 5 weeks.</li>
    </ul>
  </li>

<br><br>
  <li>
    <strong>Monthly Ride Activity by Subscription and Bike Type (2024)</strong><br><br>
    <img src="./graphs/06. Monthly Ride Activity by Subscription and Bike Type (2024).png" alt="Monthly Ride Activity by Subscription and Bike Type">
    <ul>
      <li>Both membership and casual users show preference towards Classic Bikes</li>
    </ul>
  </li>

<br><br>
  <li>
    <strong>Monthly Ride Duration Average by Subscription Type (2024)</strong><br><br>
    <img src="./graphs/07. Montly Ride Duration Average by Subscription Type (2024).png" alt="Ride Duration by Subscription Type">
    <ul>
      <li>Casual riders have longer average ride durations than members.</li>
      <li>Ride durations increase during warmer months.</li>
      <li>
        Spikes observed:
        <ul>
          <li>Late December - likely due to holidays.</li>
          <li>
            Mid-January - possibly caused by:
            <ul>
              <li>Dedicated sports riders with longer ride habits.</li>
              <li>Slow speeds due to icy conditions (coldest week per <a href="https://www.timeanddate.com/weather/usa/chicago/historic?month=1&year=2024">weather report</a>).</li>
            </ul>
          </li>
        </ul>
      </li>
    </ul>
  </li>

<br><br>
  <li>
    <strong>Casual vs. Member Activity at the Most Popular Station (2024)</strong><br><br>
    <img src="./graphs/08. Casual vs. Member Activity at the Most Popular Cyclistic Station (2024).png" alt="Most Popular Station Usage">
    <ul>
      <li>Streeter Dr & Grand Ave is the most used station, dominated by casual users.</li>
      <li>Different usage patterns imply different station roles (commuting vs. tourism).</li>
    </ul>
  </li>

<br><br>
  <li>
    <strong>Casual vs. Member Usage at the Most Popular Station by Casual Riders (2024)</strong><br><br>
    <img src="./graphs/09. Casual vs. Member Usage at the Most Popular Cyclistic Station by Casual Riders (2024).png" alt="Popular Station by Casual Riders">
    <ul>
      <li>High casual activity at stations near scenic areas suggests tourist or occasional use.</li>
    </ul>
  </li>

<br><br>
  <li>
    <strong>Ride Activity by Chicago Communities (2024)</strong><br><br>
    <img src="./graphs/10. Ride Activity by Chicago&apos;s Communities (2024).png" alt="Ride Activity by Community">
    <ul>
      <li>Most ride activity occurs in central-northern lakefront communities.</li>
    </ul>
  </li>

<br><br>
  <li>
    <strong>Casual vs. Member Activity in Chicago Communities (2024)</strong><br><br>
    <img src="./graphs/11. Casual vs. Member Activity in Chicago Communities (2024).png" alt="Community Usage Split"><br>
    <img src="./graphs/12. Casual vs. Member Activity in Top Chicago Communities (2024).png" alt="Top Communities by Casual Riders">
    <ul>
      <li>Communities with significant casual rider presence:
        <ul>
          <li>Near North Side</li>
          <li>Loop</li>
          <li>Near West Side</li>
          <li>Lincoln Park</li>
          <li>Lake View</li>
          <li>West Town</li>
          <li>Hyde Park</li>
          <li>Near South Side</li>
          <li>Uptown</li>
          <li>Logan Square</li>
        </ul>
      </li>
    </ul>
  </li>
</ul>

# Act

* <b>How do annual members and casual riders use Cyclistic bikes differently?</b>
    * <b>Weekday usage</b> - Users with memberships use services for daily commuting, showing work-related patterns, while casual riders' activity peaks on weekends, indicating they use services for tourism and recreation
    * <b>Seasonal pattern</b> - The gap between member and casual user activities tends to be biggest in colder seasons, and the gap closes during warmer seasons
    * <b>Ride duration</b> - Casual riders tend to take longer trips lasting more than 20 minutes, compared to members who average around 12 minutes
    * <b>Location preference</b> - Percentage-wise, tourist areas attract more casual riders, whereas users with memberships are spread more evenly across Chicago communities

* <b>Why would casual riders buy Cyclistic annual memberships?</b>
    * <b>Discounts</b> - Casual riders who realize they are using the service often enough that they could save money by buying a membership
    * <b>Convinient usage</b> - Members can rent a bike quicker without needing to purchase a pass each time
    * <b>Sport active lifestyle</b> - Casual riders who decide to commit to riding every day for sport
    * <b>Work</b> - Casual riders who start working in Chicago, making biking a daily part of their commute
    * <b>Eco-friendliness</b> - Some may upgrate for environmental reasons

* <b>How can Cyclistic use digital media to influence casual riders to become members?</b>
    * <b>Geo-targeted advertisements</b> - Chicago's central-northern lakefront communities show the biggest activity of casual users, which are good locations to start an advertisement campaign
    * <b>Pre summer campaign</b> - Launch an advertisement campaign right before peak seasonal activity when the casual user base rises
    * <b>Usage Pattern Messaging</b> - Highlight how much casual users would save after their rides if they were subscribers, compare their monthly expenses as casuals to membership costs
    * <b>Weekend package</b> - Offer a weekend-only package for users using the services for leisure and recreation
    * <b>Tourist plan</b> - One- or two-week-long membership plans for tourists visiting
    * <b>Group plan membership</b> - Casual users visiting Chicago during weekends can use group plans
    * <b>Duration discount</b> - Casual users on average ride longer than members - introduce duration based discounts
    
