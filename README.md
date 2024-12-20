# Cognifyz Test
## Overview
This analysis involves a deep dive into a restaurant dataset to uncover trends and insights broken in two (2) levels.

## Background
To uncover these insights, each level focuses on Answering key questions including:
- Level 1: 
In this level finding the following is focused on: 
    - Top Cuisine.
    - City Analysis.
    - Price Range Distribution.
    - Online Delivery.

- Level 2: 
In this level finding the following is focused on:
    - Restaurant Rating.
    - Cuisine Combination.
    - Geographic Analysis.
    - Restaurant Chain.

## Tools Used
In my quest to achieving these objectives, I utilised the following tools:  

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights. 
I also used the following Python libraries:
    
    - **Pandas Library:** This was used to analyze the data.
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals.
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.  

## Analysis
### Data Preparation and Cleanup

- Data Preparation

```python
# Loading Libraries 
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from collections import Counter
import folium
```
- Data Cleanup
```python
# Check for missing values
df.isnull().sum()

# Fill missing 'Cuisines' Values with 'Unknown'
df['Cuisines'].fillna('Unknown', inplace=True)

# Standardize Categorical Columns to ensure consistency

df['Has Table booking'] = df['Has Table booking'].str.strip().str.lower().replace({'yes': 'Yes', 'no': 'No'})
df['Has Online delivery'] = df['Has Online delivery'].str.strip().str.lower().replace({'yes': 'Yes', 'no': 'No'})
df['Is delivering now'] = df['Is delivering now'].str.strip().str.lower().replace({'yes': 'Yes', 'no': 'No'})

# Save Cleaned Dataset
df.to_csv('Cleaned_Dataset.csv', index = False)
```

### The Analysis
Each Jupyter notebook for this project aimed at investigating specific tasks of each level of the restaurant analysis. Here’s how I approached each task:

#### Level 1

**Task 1: Top Cuisine**

This task requires the following: 
- Determining the top three most common cuisines in the dataset.

- Calculating the percentage of restaurants that serve each of the top cuisines. 

I determined the top three most common cuisines in the dataset by getting a count of the cuisines and selected the three (3) cuisines  with the most values.

**Visualization**
```python
# Get the count of each cuisine

cuisine_count = df_cleaned['Cuisines'].value_counts()

# Get the top three cuisines
top_cuisines = cuisine_count.head(3)

# Bar Plot for the top 3 cuisines
plt.figure(figsize=(8,5))
top_cuisines.plot(kind='bar', color='skyblue')
plt.title("Top 3 Cuisines", fontsize=16)
plt.ylabel("Number of Restaurants", fontsize=12)
plt.xlabel("Cuisine", fontsize=12)
plt.xticks(rotation=45)
plt.show
```

**Result**
![Top 3 Cuisines](Images\Top_3_Cuisines.png) Top 3 Cuisines

To calculate the percentage of restaurants that serve each of the top cuisines, I  calculated the total number of restaurants in the dataset -assuming each row represents a unique restaurant.
Afterwards, I calculated the proportion of restaurants serving each top cuisine.Multiplying by 100 to convert the proportion into a percentage.

**Visualization**
```python
# Calculate the percentage of restaurants serving each top cuisine
total_restaurants = len(df_cleaned)

top_cuisine_percentages = (top_cuisines / total_restaurants) * 100

# Pie Chart for Percentage of restaurants serving top cuisines
plt.figure(figsize=(8,8))
top_cuisine_percentages.plot.pie(autopct='%1.1f%%', startangle=140, colors=['skyblue', 'lightgreen', 'salmon'])
plt.title("Percentage of Restaurants Serving Top Cuisines", fontsize=16)
plt.ylabel("")  # Hides the default ylabel
plt.show()
```

**Results**
![Percentage of Restaurants Serving Top Cuisines](Images\Percentage_of_Restaurants_Serving_Top_Cuisines.png) Percentage of Restaurants Serving Top Cuisines

**Task 2: City Analysis**

This task requires the following:
- Identifying the city with the highest number
of restaurants in the dataset.
- Calculating the average rating for
restaurants in each city.
- Determining the city with the highest
average rating.

To Identify the city with the highest number
of restaurants in the dataset, I made a count of the restaurants in each city, focusing on the top ten by value count.

**Visualization**
```python
# Count the number of restaurants in each city
city_counts = df_cleaned['City'].value_counts()

# Get the city with the highest number of restaurants
city_highest_restaurants = city_counts.idxmax()
highest_restaurant_count = city_counts.max()

# Top 10 cities with the most restaurants
top_cities = city_counts.head(10)

plt.figure(figsize=(10, 6))
top_cities.plot(kind='bar', color='skyblue')
plt.title("Top 10 Cities with Most Restaurants", fontsize=16)
plt.xlabel("City", fontsize=12)
plt.ylabel("Number of Restaurants", fontsize=12)
plt.xticks(rotation=45)
plt.grid(True)
plt.show()
```

**Result**
![Top 10 Cities with Most Restaurants](Images\Top_10_Cities_with_Most_Restaurants.png) The City with the Most Restaurants

I calculated the average restaurant rating for each city in the dataset, sorting the results in descending order of the average ratings to identify cities with the highest-rated restaurants, the highest average rating and the highest average rating value.

**Visualization**
```python
average_rating_per_city = df_cleaned.groupby('City')['Aggregate rating'].mean().sort_values(ascending=False)

# Top 10 cities with the highest average ratings
top_avg_ratings = average_rating_per_city.head(10)
plt.figure(figsize=(10, 6))
top_avg_ratings.plot(kind='bar', color='lightgreen')
plt.title("Top 10 Cities with Highest Average Ratings", fontsize=16)
plt.xlabel("City", fontsize=12)
plt.ylabel("Average Rating", fontsize=12)
plt.xticks(rotation=45)
plt.grid(True)
plt.show()
```

**Results**
![Top 10 Cities with Highest Average Ratings](Images\Top_10_Cities_with_Highest_Average_Ratings.png) Top 10 Cities with Highest Average Ratings

**Insights**
- New Delhi with 5473 restaurants is the city with the highest number of restaurants.
- Inner City, Quezon City, Makati City, Pasig City, Mandaluyong City with average ratings; 4.900000, 4.800000, 4.650000, 4.633333, 4.625000 respectively are the top 5 cities with the highest average ratings.
- Inner City with rating 4.90 is the city with the highest average rating.

**Task 3: Price Range Distribution**

This task requires the following:
- Creating a bar chart to
visualize the distribution of price ranges
among the restaurants.
- Calculating the percentage of restaurants
in each price range category.

To achieve this, I had a count of the number of restaurants in each price range, plotting the price range against the number of restaurants in a bar chart.

**Visualization**
```python
# Count the number of restaurants in each price range
price_range_counts = df_cleaned['Price range'].value_counts().sort_index()

# Plot a bar chart for price range distribution
plt.figure(figsize=(8, 6))
price_range_counts.plot(kind='bar', color='skyblue')
plt.title("Distribution of Price Ranges Among Restaurants", fontsize=16)
plt.xlabel("Price Range", fontsize=12)
plt.ylabel("Number of Restaurants", fontsize=12)
plt.xticks(rotation=0)
plt.grid(True)
plt.show()
```
**Result**
![Distribution of Price Ranges Among Restaurants](Images\Price_Range_Distribution_Among_Restaurants.png) Distribution of Price Ranges Among Restaurants

I calculated the percentage of restaurants
in each price range category by calculating the proportion of price ranges for each restaurant.

**Visualization**
```python
# Calculate the percentage of restaurants in each price range
price_range_percentages = (price_range_counts / len(df_cleaned) * 100)

# Plot a pie chart for price range percentages
plt.figure(figsize=(8,8))
price_range_percentages.plot(kind='pie', autopct='%1.1f%%', startangle=90, colors=['#ff9999','#66b3ff','#99ff99','#ffcc99'])
plt.title("Percentage Distribution of Price Ranges", fontsize=16)
plt.ylabel("")  # Remove y-axis label for aesthetics
plt.show()
```

**Result**
![Percentage Distribution of Price Ranges](Images\Percentage_Distribution_of_Price_Ranges.png) Percentage Distribution of Price Ranges

**Task 4: Online Delivery**

This task requires the following:
- Determining the percentage of restaurants
that offer online delivery.
- Comparing the average ratings of restaurants
with and without online delivery.

The percentage of restaurants that offer online delivery was gotten from the count of restaurants with online delivery from the "Has Online delivery" column and calculating the proportion of restaurants with online delivery, multiplying by 100 to convert the proportion to percentage.

**Visualization**
```python
# Calculate the percentage of restaurants offering online delivery
online_delivery_counts = df_cleaned['Has Online delivery'].value_counts()
online_delivery_percentages = (online_delivery_counts / len(df_cleaned)) * 100

# Pie Chart for Percentage of restaurants serving top cuisines
plt.figure(figsize=(6,5))
online_delivery_percentages.plot.pie(autopct='%1.1f%%', startangle=140, colors=['skyblue', 'lightgreen'])
plt.title("Percentage of Restaurants Offering Online Delivery", fontsize=16)
plt.xlabel("Has Online Delivery", fontsize=12)
plt.ylabel("")  # Hides the default ylabel
plt.show()
```

**Result**
![Percentage of Restaurants Offering Online Delivery](Images\Restaurant_Percentage_Offering_Online_Delivery.png)Percentage of Restaurants Offering Online Delivery

To compare the average ratings of restaurants
with and without online delivery, I grouped the "Has Online delivery" column by the mean Aggregate rating.

**Visualization**
```python
# Group by 'Has Online delivery' and calculate the average rating
average_ratings = df_cleaned.groupby('Has Online delivery')['Aggregate rating'].mean()

# Bar chart for average ratings
plt.figure(figsize=(6,5))
average_ratings.plot(kind='bar', color=['skyblue', 'orange'])
plt.title("Average Ratings: With vs Without Online Delivery", fontsize=16)
plt.xlabel("Has Online Delivery", fontsize=12)
plt.ylabel("Average Rating", fontsize=12)
plt.xticks(rotation=0)
plt.show()
```

**Result**
![Average Ratings: With vs Without Online Delivery](Images\Average_Ratings.png)Average Ratings: With vs Without Online Delivery

**Insights**
- Only **25.66%** of Restaurants **offers online delivery** while **74.34%** of Restaurants **do not offers online delivery**.
- On comparison, the average ratings of restaurants **with online delivery** is **3.248837** while the average ratings of restaurants **without online delivery** is **2.465296**

#### Level 2

**Task 1: Restaurant Ratings**

This task requires the following:
- Analyzing the distribution of aggregate
ratings to determine the most common
rating range.
- Calculating the average number of votes
received by restaurants.

To determine the most common
rating range, I calculated the count of frequency for each unique value in the "Aggregate rating" column, identifying the rating value of the most frequently occurring value.

**Visualization**

```python
# Plot the distribution of aggregate ratings
plt.figure(figsize=(8,6))
plt.hist(df_cleaned['Aggregate rating'], bins=10, color='skyblue', edgecolor='black')
plt.title("Distribution of Aggregate Ratings", fontsize=16)
plt.xlabel("Aggregate Rating", fontsize=12)
plt.ylabel("Frequency", fontsize=12)
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()

# Determine the most common rating range
most_common_range = df_cleaned['Aggregate rating'].value_counts().idxmax()
```
**Results**
![Distribution of Aggregate Ratings](Images\Distribution_of_Aggregate_Ratings.png) Distribution of Aggregate Ratings

To calculate the average number of votes
received by restaurants, I calculated the mean value of all the numbers in the Votes column.

```python
# Calculate the average number of votes
average_votes = df_cleaned['Votes'].mean()
```

**Insights**
- A substantial number of restaurants have an aggregate rating of 0. This could indicate:
    - Restaurants that have not been rated yet.
    - A category for unrated or inactive restaurants.

- The average number of votes received by restaurants is: 156.91

**Task 2: Cuisine Combination**

This task requires the following:

- Identify the most common combinations of
cuisines in the dataset.
- Determine if certain cuisine combinations
tend to have higher ratings.

 To identify the most common combinations of cuisines in the dataset, 
 - I calculated a count of the frequency of each unique cuisine combination in the dataset to retrieve the top 10 most common cuisine combinations.
- Also, I converted the list of cuisine combination and count into a pandas DataFrame for easier analysis and presentation.

**Visualization**
```python
# Count occurrences of each cuisine combination
common_combos = Counter(df_cleaned['Cuisine Combination']).most_common(10)

# Convert to DataFrame
common_combos_df = pd.DataFrame(common_combos, columns=['Cuisine Combination', 'Count'])
print(common_combos_df)

# Bar Chart for Most Common Cuisine Combinations

plt.figure(figsize=(12,6))
plt.bar(merged_data['Cuisine Combination'], merged_data['Count'], color='skyblue')
plt.title('Top 10 Most Common Cuisine Combinations')
plt.xlabel('Cuisine Combination')
plt.ylabel('Count')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()
```

**Result**
![Top 10 Most Common Cuisine Combinations](Images\Top_10_Most_Common_Cuisine_Combinations.png)Top 10 Most Common Cuisine Combinations

To determine if certain cuisine combinations
tend to have higher ratings, I calculated the average rating for each cuisine combination, merging it with the count of those combinations, and created a combined dataset for analysis.

**Visualization**
```python
# Calculate average rating for each combination
cuisine_ratings = (
    df_cleaned.groupby('Cuisine Combination')['Aggregate rating']
    .mean()
    .sort_values(ascending=False)
    .reset_index()
)

# Merge the counts and ratings
merged_data = pd.merge(common_combos_df, cuisine_ratings, on='Cuisine Combination', how='left')
merged_data.columns = ['Cuisine Combination', 'Count', 'Average Rating']

# Scatter Plot of Rating vs. Frequency
plt.figure(figsize=(10,6))
plt.scatter(merged_data['Count'], merged_data['Average Rating'], color='orange', alpha=0.7)
plt.title('Cuisine Combination Frequency vs Average Rating')
plt.xlabel('Count (Frequency of Combination)')
plt.ylabel('Average Rating')
plt.grid(True)
plt.tight_layout()
plt.show()
```

**Result**
![Cuisine Combination Frequency vs Average Rating](Images\Cuisine_Combination_Frequency_vs_Average_Rating.png)Cuisine Combination Frequency vs Average Rating

**Insights**
- **North Indian** with a count of **936** is the **most common cuisine combination** 
- **Cafes** are generally well-liked with the highest average rating **(2.89)** among the combinations listed, despite having a moderate frequency **(299 occurrences)**.
- **Mughlai, North Indian (2.77)** and **Chinese**, **Mughlai**, **North Indian (2.62)** have high average ratings, indicating that these combinations are appreciated, likely due to complementary flavors or appeal to specific cultural preferences.
- **Bakery, Desserts** has a higher rating **(2.37)** than **Bakery** alone **(1.92)**. Adding desserts enhances the appeal of bakery offerings.

**Task 3: Geographic Analysis**

This task requires the following:

- Plot the locations of restaurants on a
map using longitude and latitude
coordinates.

- Identify any patterns or clusters of
restaurants in specific areas.

By Computing the mean latitude and longitude to center the map, I Initialized a base map adding clustered markers to reduce visual clutter to make the map interactive.

```python
# Create a base map
center_coordinates = [df_cleaned['Latitude'].mean(), df_cleaned['Longitude'].mean()]
map_restaurants = folium.Map(location=center_coordinates, zoom_start=10)

# Create a clustered map
marker_cluster = MarkerCluster().add_to(map_restaurants)
for _, row in df_cleaned.iterrows():
    folium.Marker(
        location=[row['Latitude'], row['Longitude']],
        popup=row['Restaurant Name'],
    ).add_to(marker_cluster)


heat_data = [[row['Latitude'], row['Longitude']] for _, row in df_cleaned.iterrows()]
HeatMap(heat_data).add_to(map_restaurants)
```

**Results**
![Restaurant Map](Images\Restaurant_Map_Screenshot.PNG)Restaurant Map

![Restaurant Map with Heat Map](Images\Restaurant_Heatmap_Screenshot.PNG) Restaurant Map with Heat Map

**Task 4: Restaurant Chain**

This task requires the following:

- Identify if there are any restaurant chains
present in the dataset.
- Analyze the ratings and popularity of
different restaurant chains.

To identify restaurant chains:
- Count the occurrences of each restaurant name to identify chains.
- Filter the data for chain restaurants only.
- Group the filtered data by restaurant name and calculate:
    - Average rating.
    - Total votes (a measure of popularity).- 
    - Number of locations.
- Sort and display the top 10 chains with the most locations.

**Visualization**
```python
# Identify restaurant chains
chains = df_cleaned['Restaurant Name'].value_counts()
chain_restaurants = chains[chains > 1]  # Restaurants with more than one location
# Merge chain data for analysis
chain_data = df_cleaned[df_cleaned['Restaurant Name'].isin(chain_restaurants.index)]

# Analyze average ratings and popularity
chain_analysis = (
    chain_data.groupby('Restaurant Name')
    .agg(
        Avg_Rating=('Aggregate rating', 'mean'),
        Total_Votes=('Votes', 'sum'),
        Locations=('Restaurant ID', 'count')
    )
    .sort_values(by='Locations', ascending=False)
)

# Top chains by number of locations
top_chains = chain_analysis.head(10)
print(top_chains)

# Top chains by number of locations
plt.figure(figsize=(10,6))
top_chains['Locations'].plot(kind='bar', color='skyblue')
plt.title("Top Chains by Number of Locations", fontsize=14)
plt.xlabel("Restaurant Name", fontsize=12)
plt.ylabel("Number of Locations", fontsize=12)
plt.xticks(rotation=45)
plt.show()

# Top chains by average rating
plt.figure(figsize=(10,6))
top_chains.sort_values(by='Avg_Rating', ascending=False)['Avg_Rating'].plot(kind='bar', color='green')
plt.title("Top Chains by Average Rating", fontsize=14)
plt.xlabel("Restaurant Name", fontsize=12)
plt.ylabel("Average Rating", fontsize=12)
plt.xticks(rotation=45)
plt.show()

# Top chains by popularity (votes)
plt.figure(figsize=(10,6))
top_chains.sort_values(by='Total_Votes', ascending=False)['Total_Votes'].plot(kind='bar', color='orange')
plt.title("Top Chains by Popularity (Votes)", fontsize=14)
plt.xlabel("Restaurant Name", fontsize=12)
plt.ylabel("Total Votes", fontsize=12)
plt.xticks(rotation=45)
plt.show()
```

**Results**
![Top Chains by Number of Locations](Images\Top_Chains_by_Number_of_Locations.png)Top Chains by Number of Locations

![Top Chains by Average Rating](Images\Top_Chains_by_Average_Rating.png)Top Chains by Average Rating

![Top Chains by Popularity (Votes)](Images\Top_Chains_by_Popularity_(Votes).png)Top Chains by Popularity (Votes)