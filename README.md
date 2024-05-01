# Flight-data-eda-preprocessing
Join us on a thrilling journey through flight data! Discover departure times, delays, distances, and more. Let's uncover patterns and clues about flight delays together! Fasten your seatbelts for an adventure! 🛫✈️🕵️‍♂️

**Mission : **
Our mission is to play detective, identifying patterns and uncovering clues that shed light on what causes flight delays. So, fasten your seatbelts and prepare for an exhilarating journey through this rich flight data!:

```markdown
### Setting up Dependencies for Data Analysis

1. `import warnings`: This imports the `warnings` module, allowing us to control warning messages displayed during code execution.

2. `warnings.filterwarnings("ignore")`: This line suppresses all warning messages, which can be useful to avoid clutter in output.

3. `import numpy as np`: This imports the NumPy library and aliases it as `np`, providing support for large, multi-dimensional arrays and matrices, along with a collection of mathematical functions.

4. `import pandas as pd`: This imports the pandas library and aliases it as `pd`, which is used for data manipulation and analysis, particularly with tabular data.

5. `import matplotlib.pyplot as plt`: This imports the pyplot module from the Matplotlib library and aliases it as `plt`, enabling creation of visualizations such as plots, histograms, and more.

6. `import seaborn as sns`: This imports the seaborn library, which provides a high-level interface for drawing attractive and informative statistical graphics.

7. `from sklearn.impute import KNNImputer`: This imports the KNNImputer class from the impute module of the scikit-learn library, which is used for imputing missing values using the k-Nearest Neighbors approach.

8. `from sklearn.preprocessing import StandardScaler`: This imports the StandardScaler class from the preprocessing module of scikit-learn, which is used for standardizing features by removing the mean and scaling to unit variance.

9. `from scipy import stats`: This imports the stats module from the SciPy library, which contains a large number of probability distributions and statistical functions.

10. `%matplotlib inline`: This is a magic command used in Jupyter Notebooks to display Matplotlib plots directly within the notebook interface, without the need to call `plt.show()` after each plot.

```python
import warnings
warnings.filterwarnings("ignore")

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.impute import KNNImputer
from sklearn.preprocessing import StandardScaler
from scipy import stats
%matplotlib inline
```
Certainly! You can provide explanations for each line of code using Markdown syntax in your GitHub README. Here's how you can format it:

```markdown
### Setting up Figure Resolution and Seaborn Plot Styles

```python
# Set the resolution of the plotted figures
plt.rcParams['figure.dpi'] = 120

# Configure Seaborn plot styles: Set background color and use dark grid
sns.set(rc={'axes.facecolor': '#FFA500'}, style='darkgrid')
```markdown
### Reading the Dataset

```python
# Read dataset
import pandas as pd

# Replace backslashes with double backslashes
df = pd.read_csv('C:\\Users\\itsme\\OneDrive\\Documents\\flights.csv\\flights1.csv')
```


This code snippet demonstrates how to read a dataset using pandas' `read_csv()` function.
The file path provided in the `read_csv()` function should be adjusted to match the location of your dataset. In this case, backslashes in the file path are escaped by using double backslashes (`\\`) to ensure proper file path parsing.
```

### Dataset Description



| Variable        | Description                                                                                                                          |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------|
| id              | A unique identifier assigned to each flight record in this dataset.                                                                  
| year            | The year in which the flight took place. The dataset includes flights from the year 2013.                                            
| month           | The month of the year in which the flight occurred, represented by an integer ranging from 1 (January) to 12 (December).            
| day             | The day of the month on which the flight took place, represented by an integer from 1 to 31.                                         
| dep_time        | The actual departure time of the flight, represented in 24-hour format (hhmm).                                                       
| sched_dep_time  | The locally scheduled departure time of the flight, presented in a 24-hour format (hhmm).                                            
| dep_delay       | The delay in flight departure, calculated as the difference (in minutes) between the actual and scheduled departure times.           
| arr_time        | The actual arrival time of the flight, represented in 24-hour format (hhmm).                                                         
| sched_arr_time  | The locally scheduled arrival time of the flight, presented in a 24-hour format (hhmm).                                              
| arr_delay       | The delay in flight arrival, calculated as the difference (in minutes) between the actual and scheduled arrival times.               
| carrier         | A two-letter code representing the airline carrier responsible for the flight.                                                      
| flight          | The designated number of the flight.                                                                                                 
| tailnum         | A unique identifier associated with the aircraft used for the flight.                                                                
| origin          | A three-letter code signifying the airport from which the flight departed.                                                           
| dest            | A three-letter code representing the airport at which the flight arrived.                                                            
| air_time        | The duration of the flight, measured in minutes.                                                                                     
| distance        | The total distance (in miles) between the origin and destination airports.                                                            
| hour            | The hour component of the scheduled departure time, expressed in local time.                                                          
| minute          | The minute component of the scheduled departure time, expressed in local time.                                                        
| time_hour       | The scheduled departure time of the flight, represented in local time and formatted as "yyyy-mm-dd hh:mm:ss".                        
| name            | The full name of the airline carrier responsible for the flight.                                                                     


```python
df.info()
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/1396e113-651e-475c-b4c6-9d81618250f2)
Considering the feature explanations and data types provided earlier, it's evident that while "id" and "flight" features are numerical in data type, they hold categorical semantics. To ensure accurate analysis and interpretation, it's advisable to convert these features to string (object) data type.
```python
# Convert 'id' and 'flight' to object data type
df['id'] = df['id'].astype(str)
df['flight'] = df['flight'].astype(str)
```
```python
df.describe().T
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/c310f9c3-45fb-43f0-a5cb-ecd4f5ef9cf8)

### Data Insights

#### Year
- All records are from the year 2013, indicating no variation.

#### Month, Day, Hour, Minute
- These features represent the scheduled departure date and time.
- They exhibit a good range and appear to be evenly distributed throughout the year and day.

#### Departure and Arrival Times
- 'dep_time', 'sched_dep_time', 'arr_time', 'sched_arr_time' represent actual and scheduled departure and arrival times.
- They are in the 24-hour format and cover all possible values.

#### Departure and Arrival Delays
- 'dep_delay' and 'arr_delay' are our target variables, indicating departure and arrival delays in minutes.
- Values range from negative (early departure or arrival) to positive (late departure or arrival).

#### Flight Duration and Distance
- 'air_time' represents flight duration in minutes, ranging from 20 to 695 minutes.
- 'distance' indicates the total distance between origin and destination airports, varying from 17 to 4983 miles.

### Summary Statistics for Categorical Variables
```python
df.describe(include='object')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/e03cc978-f9e0-4dc4-867d-c9accb8fa9ea)

### Additional Insights

#### Unique Identifiers
- 'id' and 'flight' are unique identifiers with a large number of unique values.

#### Airline Carriers
- 'carrier' and 'name' represent airline carrier codes and names respectively.
- There are 16 unique carriers in the dataset.

#### Aircraft Identifier
- 'tailnum' serves as a unique identifier for the aircraft used for the flight.
- It also has a large number of unique values.

#### Airport Codes
- 'origin' and 'dest' are airport codes for departure and arrival respectively.
- There are 3 unique origin airports and 105 unique destination airports in the dataset.

#### Scheduled Departure Time
- 'time_hour' represents the scheduled departure time of the flight in local time.
- It is formatted as "yyyy-mm-dd hh:mm:ss" and contains 6936 unique times in the dataset.

### Univariate Analysis Plan

#### Numerical Data:
- For numerical data columns, such as 'dep_delay', 'arr_delay', 'air_time', and 'distance', we will use histograms to visualize the data distribution.
- We will choose an appropriate number of bins to represent the data well and provide insights into the distribution of each numerical feature.

#### Categorical Data:
- For categorical data columns, including 'carrier', 'origin', 'dest', and 'time_hour', we will use bar plots to visualize the frequency of each category.
- Bar plots will help us understand the distribution of categorical variables and identify any dominant categories or patterns.

```python
import matplotlib.pyplot as plt

# Set color for the plots
color = '#8502d1'

# Define function to plot histograms
def plot_hist(column, bins, title, xlabel, fontsize=8, rotation=0):
    try:
        plt.figure(figsize=(15,5))
        counts, bins, patches = plt.hist(column, bins=bins, color=color, edgecolor='white')
        plt.title(title, fontsize=15)
        plt.xlabel(xlabel, fontsize=12)
        plt.ylabel('Frequency', fontsize=12)

        # Add text annotation for frequencies
        for count, x in zip(counts, bins[:-1]):
            if count > 0:
                plt.text(x, count, str(int(count)), fontsize=fontsize, ha='center', va='bottom', rotation=rotation)
        plt.show()
    except Exception as e:
        print(f"An error occurred: {e}")

# Define function to plot bar plots
def plot_bar(column, title, xlabel, fontsize=8, rotation=0):
    try:
        plt.figure(figsize=(15,5))
        counts = column.value_counts()
        counts.plot(kind='bar', color=color, edgecolor='white')
        plt.title(title, fontsize=15)
        plt.xlabel(xlabel, fontsize=12)
        plt.ylabel('Frequency', fontsize=12)
        
        # Add text annotation for frequencies with rotation and larger font size
        for i, v in enumerate(counts):
            plt.text(i, v, str(v), fontsize=fontsize, ha='center', va='bottom', rotation=rotation)
        plt.show()
    except Exception as e:
        print(f"An error occurred: {e}")

# Example usage:
# plot_histogram(data['column_name'], bins=20, title='Histogram', xlabel='X Label')
# plot_bar(data['column_name'], title='Bar Plot', xlabel='X Label')
```
```python
# The year in which the flight took place. The dataset includes flights from the year 2013.
plot_bar(df['year'], 'Year', 'Year of Flight')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/8433811e-1ffa-434d-866a-eefcfae5fa77)

```python
# The month of the year in which the flight occurred, represented by an integer ranging from 1 (January) to 12 (December).
plot_hist(df['month'], bins=12, title='Month', xlabel='Month of Flight')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/bdc79678-428d-4024-b5b7-037535191cc2)

### Findings

#### Distribution of Flights Across Months
- The histogram reveals that the distribution of flights across different months is approximately uniform.
- There is a slight decrease in February, which is likely due to the fewer number of days in that month.

```python
# The day of the month on which the flight took place, represented by an integer from 1 to 31.
plot_hist(df['day'], bins=31, title='Day', xlabel='Day of Flight', fontsize=7, rotation=45)
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/3fe6482a-1ccf-4145-a339-d0ffc2980461)
### Findings

#### Distribution of Flights Across Days of the Month
- The histogram shows a mostly uniform distribution of flights across the days of the month.
- There are slight decreases at the end of the month, which can be attributed to some months having fewer than 31 days.
```python
plot_hist(df['dep_time'].dropna(), bins=24, title='Departure Time', xlabel='Departure Time (24-hour format)')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/75ae7b38-0c15-403f-b264-d7c0f8adc38d)

### Findings

#### Distribution of Flight Departure Times
- The histogram displays a bimodal distribution, suggesting two peak periods for flight departures.
- The first peak occurs in the morning around 06:00 hours, while the second peak occurs in the evening around 18:00 hours.
- There are fewer flights during the night hours, particularly from 23:00 to 04:00 hours.

```python
# The locally scheduled departure time of the flight, presented in a 24-hour format (hhmm).
plot_hist(df['sched_dep_time'], bins=24, title='Scheduled Departure Time', xlabel='Scheduled Departure Time (24-hour format)')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/fcb17afb-829d-4d4d-8e2a-f4b5daf318f9)

### Findings

#### Distribution of Scheduled Flight Departure Times
- The histogram exhibits a similar pattern to the departure time histogram.
- It also reveals two peak periods for scheduled flight departures, mirroring the actual departure times.
- The peak periods coincide with the morning around 06:00 hours and the evening around 18:00 hours.

```python
# The delay in flight departure, calculated as the difference (in minutes) between the actual and scheduled departure times. 
# Positive values indicate a delay, while negative values indicate an early departure.
plot_hist(df['dep_delay'].dropna(), bins=30, title='Departure Delay', xlabel='Departure Delay (minutes)', fontsize=7)
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/ca2d7a26-a3b3-4862-8fbc-3afe314ed018)

### Findings

#### Distribution of Departure Delays
- The histogram illustrates that a significant portion of flights depart close to their scheduled departure time, as indicated by the peak of the distribution around zero.
- However, there are also numerous flights with departure delays, as evidenced by the long tail to the right of the distribution.

```python
# The actual arrival time of the flight, represented in 24-hour format (hhmm).
plot_hist(df['arr_time'].dropna(), bins=24, title='Arrival Time', xlabel='Arrival Time (24-hour format)')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/eb572100-2731-426e-b888-9a031c3e6ddc)

### Findings

#### Distribution of Flight Arrival Times
- The histogram displays a bimodal distribution, suggesting two peak periods for flight arrivals.
- These peak periods are similar to the peak periods observed for flight departures.
```python
# The locally scheduled arrival time of the flight, presented in a 24-hour format (hhmm).
plot_hist(df['sched_arr_time'], bins=24, title='Scheduled Arrival Time', xlabel='Scheduled Arrival Time (24-hour format)')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/063887b7-7299-49b3-b41a-a6270370e6dc)

### Findings

#### Distribution of Scheduled Flight Arrival Times
- The histogram closely resembles the arrival time histogram, indicating consistency between scheduled and actual arrival times.

```python
# The delay in flight arrival, calculated as the difference (in minutes) between the actual and scheduled arrival times. 
# Positive values indicate a delay, while negative values indicate an early arrival.
plot_hist(df['arr_delay'].dropna(), bins=30, title='Arrival Delay', xlabel='Arrival Delay (minutes)', fontsize=7)
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/6c9a7cb6-2deb-442d-b771-65aee822723e)

### Findings

#### Distribution of Arrival Delays
- The histogram indicates that a significant portion of flights arrive close to their scheduled arrival time, as evidenced by the peak of the distribution around zero.
- However, there are also numerous flights with arrival delays, as indicated by the long tail to the right of the distribution.

```python
# A two-letter code representing the airline carrier responsible for the flight.
plot_bar(df['carrier'], 'Carrier', 'Airline Carrier Code')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/4bc48e8e-c144-4afe-a48d-b9833a0dd827)

### Findings

#### Frequency of Flights by Carrier
- The bar plot reveals that carriers with the codes UA, B6, EV, and DL operate the most flights in this dataset.

```python
# A three-letter code signifying the airport from which the flight departed.
plot_bar(df['origin'], 'Origin', 'Departure Airport Code')
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/3b1306ba-1cbf-4424-9ca1-8779ceddf7df)

### Findings

#### Frequency of Flights by Origin Airport
- The bar plot indicates that the majority of flights in this dataset depart from the airport with the code EWR.

```python
# A three-letter code representing the airport at which the flight arrived.
plt.figure(figsize=(15,5))
df['dest'].value_counts().plot(kind='bar', color=color)
plt.title('Destination', fontsize=15)
plt.xlabel('Arrival Airport Code', fontsize=12)
plt.ylabel('Frequency', fontsize=12)
plt.xticks(fontsize=6)
plt.show()
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/ea012ac8-2991-40a2-9c29-3fd87bb0771a)

### Findings

#### Frequency of Flights by Destination Airport
- The bar plot indicates that the most common destination airports are ORD, ATL, and LAX.

```python
# The duration of the flight, measured in minutes.
plot_hist(df['air_time'].dropna(), bins=30, title='Air Time', xlabel='Flight Duration (minutes)', fontsize=7)
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/d67ea344-0a94-4c0f-aa6d-b37e15f4404c)

### Findings

#### Distribution of Flight Air Times
- The histogram reveals that most flights have an air time of around 50 to 200 minutes.
- There are a few flights with significantly longer air times.

```python
# The total distance (in miles) between the origin and destination airports.
plot_hist(df['distance'], bins=30, title='Distance', xlabel='Flight Distance (miles)', fontsize=7)
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/8f360464-47d0-4c5d-8ad1-9561cbd0ddc2)

### Findings

#### Distribution of Flight Distances
- The histogram indicates that most flights travel a distance of around 500 to 1000 miles.
- There are a few flights that travel significantly longer distances.

```python
# The hour component of the scheduled departure time, expressed in local time.
plot_hist(df['hour'], bins=24, title='Hour', xlabel='Hour of Scheduled Departure Time', fontsize=7)
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/f069210f-8f01-4568-922a-67910112bfca)

### Findings

#### Distribution of Hour Component in Scheduled Departure Time
- The histogram unveils two peak periods for the hour component of the scheduled departure time.
- These peaks correspond to the morning and evening peaks observed in the departure time histograms.

```python
# The minute component of the scheduled departure time, expressed in local time.
plot_hist(df['minute'], bins=60, title='Minute', xlabel='Minute of Scheduled Departure Time', fontsize=7, rotation=90)
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/58ee4e3e-93ab-4e87-8211-ddd6ef58d151)

### Findings

#### Distribution of Minute Component in Scheduled Departure Time
- The histogram illustrates an almost uniform distribution for the minute component of the scheduled departure time.
- This suggests that flights are evenly scheduled across all minutes of an hour.

```python
# The full name of the airline carrier responsible for the flight.
plot_bar(df['name'], 'Name', 'Airline Carrier Name')
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/e8e6ebc0-29d6-47b0-b43d-2edd53003a72)

### Findings

#### Frequency of Flights by Airline
- The bar plot reveals that United Air Lines Inc., JetBlue Airways, and ExpressJet Airlines Inc. are the airlines that operate the most flights in this dataset.

#### Bivariate Analysis
For our bivariate analysis, we'll consider the arr_delay column as the target. We can analyze the relationship between arr_delay and other columns. To do this, we can use scatter plots for numerical columns and violin plots for categorical columns. We skip id, flight, tailnum, time_hour as they are identifiers or contain redundant information.
```python
# Define color palette with different shades of color #8502d1 for boxplots
colors_box = sns.dark_palette("#8502d1", as_cmap=False)

# Define colormap with different shades of color #8502d1 for scatter plots
colors_scatter = sns.dark_palette("#8502d1", as_cmap=True)

# Define the function to plot scatter plots
def plot_scatter(x, y, title, xlabel, ylabel):
    plt.figure(figsize=(15,5))
    plt.scatter(x, y, c=y, cmap=colors_scatter, s=2)
    plt.title(title, fontsize=15)
    plt.xlabel(xlabel, fontsize=12)
    plt.ylabel(ylabel, fontsize=12)
    plt.colorbar(label=ylabel)
    plt.show()

# Define the function to plot violin plots
def plot_violin(x, y, title, xlabel, ylabel, fontsize=8):
    plt.figure(figsize=(15,5))
    sns.violinplot(x=x, y=y, palette=colors_box)
    plt.title(title, fontsize=15)
    plt.xlabel(xlabel, fontsize=12)
    plt.ylabel(ylabel, fontsize=12)
    plt.xticks(rotation=90, fontsize=fontsize)
    plt.show()
```
```python
# year vs arr_delay
plot_violin(df['year'], df['arr_delay'], 'Year vs Arrival Delay', 'Year', 'Arrival Delay')

```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/35e08a35-00e8-498f-85a1-a14894f20119)

### Findings

#### Effectiveness of Violin Plot
- The violin plot does not provide much information due to the limited variation in the dataset, which contains flights from only one year.

```python
# month vs arr_delay
plot_violin(df['month'], df['arr_delay'], 'Month vs Arrival Delay', 'Month', 'Arrival Delay')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/f8b41512-959c-4891-8b72-d610341e18dc)

### Findings

#### Variation of Arrival Delays by Month
- The violin plot illustrates that the distribution of arrival delays varies by month.
- Months such as June, July, and December exhibit wider distributions, suggesting a higher variability in arrival delays during those months.

```python
# day vs arr_delay
plot_violin(df['day'], df['arr_delay'], 'Day vs Arrival Delay', 'Day', 'Arrival Delay')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/bf2adbee-07d1-4756-bcd1-bb3ef664456a)

### Findings

#### Impact of Day of the Month on Arrival Delays
- The violin plot suggests that the day of the month does not have a significant impact on the distribution of arrival delays.

```python
# dep_time vs arr_delay
plot_scatter(df['dep_time'], df['arr_delay'], 'Departure Time vs Arrival Delay', 'Departure Time', 'Arrival Delay')
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/02107703-23a8-4f06-ad97-16efb5ea4c32)

### Findings

#### Relationship Between Departure Time and Delays
- The scatter plot reveals a slight trend suggesting that flights departing later in the day tend to experience more delays.

```python
# sched_dep_time vs arr_delay
plot_scatter(df['sched_dep_time'], df['arr_delay'], 'Scheduled Departure Time vs Arrival Delay', 'Scheduled Departure Time', 'Arrival Delay')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/4efc75d4-135d-41f7-a19c-581289d9693a)

### Findings

#### Relationship Between Scheduled Departure Time and Delays
- This plot exhibits a similar pattern to the previous one, suggesting that flights scheduled to depart later in the day tend to experience more delays.

```python
# dep_delay vs arr_delay
plot_scatter(df['dep_delay'], df['arr_delay'], 'Departure Delay vs Arrival Delay', 'Departure Delay', 'Arrival Delay')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/930e39f8-d25c-410e-9f7f-728f6b69e216)

### Findings

#### Correlation Between Departure Delay and Arrival Delay
- As expected, there is a strong positive correlation between departure delay and arrival delay.

```python
# arr_time vs arr_delay
plot_scatter(df['arr_time'], df['arr_delay'], 'Arrival Time vs Arrival Delay', 'Arrival Time', 'Arrival Delay')
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/8b578db8-710b-41f1-a4c2-9ecdaef1b62b)

### Findings

#### Relationship Between Arrival Time and Delays
- The scatter plot reveals that flights arriving later in the day tend to experience more delays.
- This observation is consistent with the findings related to departure time.

```python
# sched_arr_time vs arr_delay
plot_scatter(df['sched_arr_time'], df['arr_delay'], 'Scheduled Arrival Time vs Arrival Delay', 'Scheduled Arrival Time', 'Arrival Delay')
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/d01c027f-a66c-40d9-a866-886570ff9ecf)

### Findings

#### Relationship Between Scheduled Arrival Time and Delays
- This plot further supports the observation that flights scheduled to arrive later in the day tend to experience more delays.

```python
# carrier vs arr_delay
plot_violin(df['carrier'], df['arr_delay'], 'Carrier vs Arrival Delay', 'Carrier', 'Arrival Delay')
```
<img width="505" alt="image" src="https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/507b23ee-a26a-4fdf-8f14-8c317e215e82">

### Findings

#### Variation of Arrival Delays by Carrier
- The violin plot illustrates that different carriers exhibit different distributions of arrival delays.
- Some carriers tend to have more severe delays, while others have milder delays.

```python
# origin vs arr_delay
plot_violin(df['origin'], df['arr_delay'], 'Origin vs Arrival Delay', 'Origin', 'Arrival Delay')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/e92df4be-a144-4692-a145-bf92ddca0884)

### Findings

#### Variation of Arrival Delays by Origin Airport
- The violin plot reveals that flights from different origin airports exhibit different distributions of arrival delays.
- Some airports tend to have flights with more severe delays, while others have flights with milder delays.

```python
# dest vs arr_delay
plot_violin(df['dest'], df['arr_delay'], 'Destination vs Arrival Delay', 'Destination', 'Arrival Delay', fontsize=6)
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/41cc3ddd-9d4d-4bd4-819e-132070e2b7f1)

### Findings

#### Variation of Arrival Delays by Destination Airport
- The violin plot indicates that flights going to different destinations exhibit different distributions of arrival delays.
- Some destination airports tend to receive flights with more severe delays, while others receive flights with milder delays.

```python
# air_time vs arr_delay
plot_scatter(df['air_time'], df['arr_delay'], 'Air Time vs Arrival Delay', 'Air Time', 'Arrival Delay')
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/e2b28f84-6690-4a60-88ab-bec20d51349c)

### Findings

#### Relationship Between Flight Duration and Arrival Delay
- The scatter plot does not exhibit a clear trend, indicating that the duration of the flight (air_time) does not have a significant impact on the arrival delay.

```python
# distance vs arr_delay
plot_scatter(df['distance'], df['arr_delay'], 'Distance vs Arrival Delay', 'Distance', 'Arrival Delay')
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/945b070b-8c78-413b-9a35-8d1eed365400)

### Findings

#### Relationship Between Flight Distance and Arrival Delay
- This plot also lacks a clear trend, suggesting that the distance of the flight does not have a significant impact on the arrival delay.

```python
# hour vs arr_delay
plot_violin(df['hour'], df['arr_delay'], 'Hour vs Arrival Delay', 'Hour', 'Arrival Delay')
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/f20eb74a-8ae7-4b06-b3ad-68b55199b3a2)

### Findings

#### Variation of Arrival Delays by Departure Hour
- The violin plot indicates that flights departing at different hours of the day exhibit different distributions of arrival delays.
- Flights departing later in the day tend to have a higher variability in arrival delays.

```python
# minute vs arr_delay
plot_violin(df['minute'], df['arr_delay'], 'Minute vs Arrival Delay', 'Minute', 'Arrival Delay')
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/96e03ec2-8f30-46a3-85a3-9d873a6352b8)

### Findings

#### Impact of Minute Component of Departure Time on Arrival Delays
- The violin plot suggests that the minute of the hour of the departure time does not have a significant impact on the distribution of arrival delays.

```python
# name vs arr_delay
plot_violin(df['name'], df['arr_delay'], 'Name vs Arrival Delay', 'Name', 'Arrival Delay')
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/d4e749c4-0e93-4d0d-9184-292a6f8a9ed4)

### Findings

#### Variation of Arrival Delays by Airline
- The violin plot illustrates that different airlines (represented by their names) exhibit different distributions of arrival delays.
- Some airlines tend to have more severe delays, while others have milder delays.


#### Based on the bivariate analysis, the following features have a noticeable impact on arrival delay:

- Month
- Departure Time and Scheduled Departure Time
- Departure Delay
- Arrival Time and Scheduled Arrival Time
- Carrier
- Origin
- Destination
- Hour

However, the following features do not seem to significantly influence arrival delay:

- Day
- Air Time
- Distance


#### Multivariate Analysis
### Multivariate Analysis Plan

A multivariate analysis will be conducted to understand the interactions between different features of the dataset and how they collectively impact the target variable (arrival delay).

Due to the complexity of multivariate plots, we will only consider a few key features. The choice of these features is based on the results from the bivariate analysis: month, departure time, departure delay, carrier, origin, destination, and hour.

```python
# Define color palette with different shades of color #8502d1 for multivariate plots
colors_multi = sns.dark_palette("#8502d1", as_cmap=False)

# For correlation heatmap, let's consider the numeric features only
numeric_features = ['month', 'dep_time', 'dep_delay', 'hour']
correlation_matrix = df[numeric_features].corr()

plt.figure(figsize=(10,8))
sns.heatmap(correlation_matrix, annot=True, cmap=colors_multi, annot_kws={"size": 8})
plt.xticks(fontsize=7)
plt.yticks(fontsize=7)
plt.title('Correlation Heatmap', fontsize=15)
plt.show()
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/0d1a4981-c36f-483e-9378-bb131de9a932)

### Findings

#### Correlation Analysis
- The heatmap displays the correlation coefficients between the numeric features.
- Departure delay (dep_delay) exhibits a strong positive correlation with arrival delay (arr_delay), indicating that as the departure delay increases, the arrival delay also tends to increase.
- However, the other features (month, departure time, and hour) show very weak correlations with arrival delay, suggesting that these features by themselves do not strongly influence the arrival delay.

```python
# 'dep_delay' vs 'dep_time' across different 'carrier'
g = sns.FacetGrid(df, col="carrier", col_wrap=4, height=4, aspect=1)
g.map(plt.scatter, "dep_time", "dep_delay", color=colors_multi[-2], s=10)
g.fig.suptitle('Departure Delay vs Departure Time across Carriers', fontsize=15, y=1.02)
plt.show()
```
![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/3b026e84-b797-47ff-a721-6579a1d1bd2d)

### Findings

#### Relationship between Departure Delay and Departure Time by Carrier
- The scatter plots illustrate the relationship between departure delay and departure time for different carriers.
- Some carriers exhibit a more pronounced trend of increased departure delay with later departure times, while others do not.
- This suggests that the interaction between carrier and departure time could be a significant factor influencing arrival delay.

```python
# 'dep_delay' vs 'dep_time' across different 'origin'
g = sns.FacetGrid(df, col="origin", col_wrap=4, height=5, aspect=1)
g.map(plt.scatter, "dep_time", "dep_delay", color=colors_multi[-2], s=10)
g.fig.suptitle('Departure Delay vs Departure Time across Origins', fontsize=15, y=1.02, x=0.38)
plt.show()
```

![image](https://github.com/vummanenidilip/Flight-data-eda-preprocessing/assets/44915745/1ee47873-9499-4d51-a68a-c4db73dc9353)

### Findings

#### Relationship between Departure Delay and Departure Time by Origin Airport
- The scatter plots depict the relationship between departure delay and departure time for flights from different origin airports.
- Similar to the carrier plots, some origin airports exhibit a more pronounced trend of increased departure delay with later departure times, while others do not.
- This suggests that the interaction between origin airport and departure time could also be a significant factor influencing arrival delay.










































I
