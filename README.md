<div>
  <img src="/Images/img_1.jpg" ></img>
</div>

# Welcome to Climate Change Analysis Project
<div>
<img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="github"/>
<img src="https://img.shields.io/badge/Made%20with-Jupyter-orange?style=for-the-badge&logo=Jupyter" alt="jupyter" />
<img src="http://ForTheBadge.com/images/badges/built-with-science.svg" />
</div>
<em><p>Climate change is the long-term alteration of temperature and typical weather patterns in a place. Climate change could refer to a particular location or the planet as a whole. Climate change may cause weather patterns to be less predictable.</p></em>

## Project Info
<p>In this project, We will be developing Climate Prediction Model. We will be using the Temperature Variation Dataset of Earth to analyse, observe and develop a model to predict the climate change across the globe.
</p>

## Tools Used

<div>
  <img src="https://img.shields.io/badge/Numpy-777BB4?style=for-the-badge&logo=numpy&logoColor=white" /> 
  <img src="https://img.shields.io/badge/Pandas-2C2D72?style=for-the-badge&logo=pandas&logoColor=white" /> 
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="python"/>
  <img src="https://img.shields.io/badge/conda-342B029.svg?&style=for-the-badge&logo=anaconda&logoColor=white"  />
  <img src="https://img.shields.io/badge/Made%20with-Jupyter-orange?style=for-the-badge&logo=Jupyter" alt="jupyter" />
  <img src="https://img.shields.io/badge/Plotly-239120?style=for-the-badge&logo=plotly&logoColor=white" />

</div>

## Dataset Information
<p> The Dataset Chosen for this Project is from <a href="http://berkeleyearth.org/archive/about/"><b>Berkeley Earth</b></a>.
<br>Berkeley Earth supplies comprehensive open-source world air pollution data and highly user-accessible global temperature data that is timely, impartial, and verified.</p>
<p>Given this complexity, there are a range of organizations that collate climate trends data. The three most cited land and ocean temperature data sets are NOAA’s MLOST, NASA’s GISTEMP and the UK’s HadCrut.

We have repackaged the data from a newer compilation put together by the Berkeley Earth, which is affiliated with Lawrence Berkeley National Laboratory. The Berkeley Earth Surface Temperature Study combines 1.6 billion temperature reports from 16 pre-existing archives. It is nicely packaged and allows for slicing into interesting subsets (for example by country). They publish the source data and the code for the transformations they applied. They also use methods that allow weather observations from shorter time series to be included, meaning fewer observations need to be thrown away.
  
The Dataset contain several attributes/fields,such as:
  - Date: starts in 1750 for average land temperature and 1850 for max and min land temperatures and global ocean and land temperatures
  - LandAverageTemperature: global average land temperature in celsius
  - LandAverageTemperatureUncertainty: the 95% confidence interval around the average
  - LandMaxTemperature: global average maximum land temperature in celsius
  - LandMaxTemperatureUncertainty: the 95% confidence interval around the maximum land temperature
  - LandMinTemperature: global average minimum land temperature in celsius
  - LandMinTemperatureUncertainty: the 95% confidence interval around the minimum land temperature
  - LandAndOceanAverageTemperature: global average land and ocean temperature in celsius
  - LandAndOceanAverageTemperatureUncertainty: the 95% confidence interval around the global average land and ocean temperature
  
  
The raw data comes from the Berkeley Earth data page.</p>
<p> Dataset Link: <a href="https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data"> Berkeley Climate Change Dataset</a></p> 


The Dataset consist of multiple type of Data:

* Global Land and Ocean-and-Land Temperatures (GlobalTemperatures.csv)
* Global Average Land Temperature by Country (GlobalLandTemperaturesByCountry.csv)
* Global Average Land Temperature by State (GlobalLandTemperaturesByState.csv)
* Global Land Temperatures By Major City (GlobalLandTemperaturesByMajorCity.csv)
* Global Land Temperatures By City (GlobalLandTemperaturesByCity.csv)
  
## Installation of Tools and Libraries 
### Make Sure you have installed Python 3.x + version for this project

### Using PIP 

You would be needing multiple libraries such as Math, Numpy, Pandas and Seabon for basic operations and many pre-processing and ML Model libraries.

```sh
pip install [Library_Name]
```

### Using Conda

If you are using Anaconda, then I would recommend you to create an seperate Enviornment for this project. (Not mandatory! Just a Good Practice)
You can use the command mentioned below to install libraries in your enviornment. (Make sure that the appropriate Environment is activated to avoid Code Errors and redundancy!)

```sh
conda install [Library_Name]
```

## Getting Started with the Project

### Importing the required Libraries 

```python
import pandas as pd
import numpy as np
import os
import matplotlib.pyplot as plt
import seaborn as sns
import sys
```

### Reading the CSV File from the local machine

The cwd and files command is to see the current directory details. 

```python
# Reading the dataset 
# Get the current working directory (cwd)
cwd = os.getcwd()   
# Get all the files in that directory
files = os.listdir(cwd) 
print("Files in %r: %s" % (cwd, files))

#Reading the GLOBAL TEMPERATURE BY CITY CSV 
data = pd.read_csv(f"Datasets\GlobalLandTemperaturesByCity.csv",delimiter=",")
data
```

### Basic Exploration on the Dataset 

This section of code helps us to comprehend the basic understanding of the dataset. We retrieved the dataset in a form of Pandas Data Frame.

```python
# Gives the Basic Structure of the Data Frame such as Data type and Col names.
data.info()

# Show the basic Statistical Values of the data such as Mean, Count, StD, Min and Quartiles
data.describe()

# Shape of the dataset
data.shape

# Names of the Column 
data.columns

# First 5 Records in the dataset
data.head()

# Last 5 Records in the dataset
data.tail()

# Here we can see the unique values exist in each column
data.nunique()

# Shows the number of Null values in each column 
data.isnull().sum()
```

<u>Note: There exist few Null values.</u>
<p>
Many real world datasets contain missing values, often encoded as blanks, NaNs or other placeholders.
  
  * A basic strategy to use incomplete datasets is to discard entire rows and/or columns
    containing missing values or NaN.
  * Another Strategy is to use Imputer Class, it replaces the empty/null values with the       mean, median or most frequent
  
  
Since, the dataset is quite large, so we will be dropping the null records
</p>

```python
# Dropping all null values
data = data.dropna(how='any' ,axis=0)
data.shape
```

### Manipulation in the Data Frame

Since, we are going to perform Time Series Analysis on the Dataset. So, now we will be changing the structure according to our ease and analysis techniques.

Transforming the Dt column to Date column ( Object datatype to Date Type)

```python
# Converting Dt into Date
data['Date'] = pd.to_datetime(data['Date'])
# Setting the Index 
data.set_index('Date',inplace = True)
data.index
```

Now, we will make a new Column namely "Year" 

```python
# Now we use year as index
data['Year']= data.index.year
data.head()
```

## Interactive Tableau Dashboard 

You can access the Tableau Dashboard to learn more about the pattern and trends in the Dataset in more interactive and fun manner.

https://public.tableau.com/views/ClimateChangeAnalysisDashboardDA2/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link


## Time Series Analysis of Cities

I am going to perform Time Series Analysis on some Cities. So, Let's take **Delhi** for Case Study.

```python
delhi = data.loc[data['City'] == ('New Delhi' or "Delhi"), ['Date','AverageTemperature']]
delhi.columns = ['Date','Temp']

delhi['Date'] = pd.to_datetime(delhi['Date'])
delhi.reset_index(drop=True, inplace=True)
delhi.set_index('Date', inplace=True)
delhi
``` 

So, here we sliced our required Data from the Dataset.
- Now, We will see the Temperature Variation of Delhi from 1980-2013


```python
plt.figure(figsize=(22,6))
sns.lineplot(x=delhi.index, y=delhi['Temp'])
plt.title('Temperature Variation in Delhi from 1980 until 2013')
plt.xlabel("Date",fontsize=14)
plt.ylabel("Temperature in C'",fontsize=14)
plt.legend()
plt.savefig(f"./DataViz/Delhi/1980-2013Temp.jpeg") # To Save the Plot as an JPEG Image
plt.show()
```
<div>
  <img src="/DataViz/Delhi/1980-2013Temp.jpeg" ></img>
</div>

- For better analysis and getting more insight on trends and pattern of temperature change in the New Delhi
  We will be creating a SpreadSheet/Pivot Table to map temperature change in each month 

```python
pivot = pd.pivot_table(delhi, values='Temp', index='Month', columns='Year', aggfunc='mean')
pivot.plot(figsize=(20,6),colormap="coolwarm")
plt.title('Yearly Delhi temperatures')
plt.xlabel('Months')
plt.ylabel('Temperatures')

plt.xticks([x for x in range(1,13)])
plt.legend().remove()
plt.savefig(f"./DataViz/Delhi/TemperatureChangeDelhi.png")
plt.show()
```
<div>
  <img src="/DataViz/Delhi/TemperatureChangeDelhi.png" ></img>
</div>

**It is clearly visible through the plot that their exist a Temperature rise in month of May, June and July and the temperature descent in month of November and December**

``` python

monthly_seasonality = pivot.mean(axis=1)
monthly_seasonality.plot(figsize=(20,6))
plt.title('Monthly Temperatures in Delhi')
plt.xlabel('Months',fontsize=14)
plt.ylabel('Temperature',fontsize=14)
plt.xticks([x for x in range(1,13)])
plt.savefig(f"./DataViz/Delhi/AvgTempChangeMonth.png")
plt.show()
```
<div> <img src="/DataViz/Delhi/AvgTempChangeMonth.png" ></img></div>

- Yearly Average Temperature in Delhi

```python

year_avg = pd.pivot_table(delhi, values='Temp', index='Year', aggfunc='mean')
# The rolling mean of Temp in last 10 years
# Rolling mean window is 3
year_avg['10 Years MA'] = year_avg['Temp'].rolling(3).mean()
year_avg[['Temp','10 Years MA']].plot(kind="line",
                                      figsize=(20,6),
                                      colormap="cool",
                                      marker='o',)

plt.title('Yearly AVG Temperatures in Delhi')
plt.xlabel('Years')
plt.ylabel('Temperature')
plt.xticks([x for x in range(1980,2012,2)])

plt.savefig(f"./DataViz/Delhi/TempChangeVsRollingMean.png")
plt.show()
```
<div> <img src="/DataViz/Delhi/TempChangeVsRollingMean.png" ></img></div> 

## Time Series Analysis using Trends and ADCF Testing

Before moving forward, lets first split the dataset into Training, Validation and Testing. Now since its not any random sequence data therfore use of Random value and sample bias doesnt apply here. We can simply split the data using simple slicing and cutting operation on DataFrame

```python
# Train Size is 200
df_train = delhi[-300:]
# Val size is 100
df_val = delhi[200:300]
# Test Size is 100
df_test = delhi[:-200]
```
Now, We will perform ADCF Test to check whether the Data is Stationary or Non-Stationary.

>ADCF Test: ADCF stands for Augmented Dickey-Fuller test which is a statistical unit root test. It gives us various values which can help us identifying stationarity.
It comprises Test Statistics & some critical values for some confidence levels. If the Test statistics is less than the critical values, we can reject the null hypothesis      & say that the series is stationary. 
The Null hypothesis says that time series is non-stationary. THE ADCF test also gives us a p-value. According to the null hypothesis, lower values of p is better.

```python
from statsmodels.tsa.stattools import adfuller

print('Augmented Ducky Fuller Test Results')

test_data = adfuller(df_train.iloc[:,0].values,autolag='AIC')

data_output = pd.Series(test_data[0:4],index=['Test Statistic','p-value','Lags Used','Number of Observation Used'])

for key,value in test_data[4].items():
    
    data_output['Critical value (%s)'%key] = value
    
print(data_output)
```

>Augmented Ducky Fuller Test Results
>Test Statistic     :              -3.340807<br>
>p-value             :              0.013150<br> 
>Lags Used               :       13.000000<br>
>Number of Observation Used  :  286.000000<br>
>Critical value (1%)         :   -3.453423<br>
>Critical value (5%)          :  -2.871699<br>
>Critical value (10%)          : -2.572183<br>
>dtype: float64


Now, We will be plotting the result of ADCF Test.

```
import math
def check_stationarity(y, lags_plots=48, figsize=(22,8)):
       
    # Creating plots of the DF
    y = pd.Series(y)
    fig = plt.figure()

    ax1 = plt.subplot2grid((3, 3), (0, 0), colspan=2)
    ax2 = plt.subplot2grid((3, 3), (1, 0))
    ax3 = plt.subplot2grid((3, 3), (1, 1))
    ax4 = plt.subplot2grid((3, 3), (2, 0), colspan=2)

    y.plot(ax=ax1, figsize=figsize)
   
    ax1.set_title('Delhi Temperature Variation')
    plot_acf(y, lags=lags_plots, zero=False, ax=ax2);
    plot_pacf(y, lags=lags_plots, zero=False, ax=ax3);
    sns.distplot(y, bins=int(sqrt(len(y))), ax=ax4)
    ax4.set_title('Distribution Chart')

    plt.tight_layout()
    
    plt.savefig(f"./DataViz/Delhi/AdfullerTest.png")
    
    print('Results of Dickey-Fuller Test:')
    adfinput = adfuller(y)
    adftest = pd.Series(adfinput[0:4], index=['Test Statistic','p-value','Lags Used','Number of Observations Used'])
    adftest = round(adftest,4)
    
    for key, value in adfinput[4].items():
        adftest["Critical Value (%s)"%key] = value.round(4)
        
    print(adftest)
    
    if adftest[0].round(2) < adftest[5].round(2):
        print('\nThe Test Statistics is lower than the Critical Value of 5%.\nThe serie seems to be stationary')
    else:
        print("\nThe Test Statistics is higher than the Critical Value of 5%.\nThe serie isn't stationary")
        
# The first approach is to check the series without any transformation
check_stationarity(df_train["Temp"])

```

<div> <img src="/DataViz/Delhi/AdfullerTest.png" ></img></div>

```python
#Now we break the data into sub section
from statsmodels.tsa.seasonal import seasonal_decompose

decomp= seasonal_decompose(df_train["Temp"],period=3)
trend = decomp.trend
seasonal = decomp.seasonal
residual = decomp.resid

plt.subplot(411)
plt.figure(figsize=(20,8))
plt.plot(df_train["Temp"],'r-')
plt.xlabel('Original')
plt.figure(figsize=(20,8))

plt.subplot(412)
plt.plot(trend,'k-')
plt.xlabel('Trend')
plt.figure(figsize=(20,8))

plt.subplot(413)
plt.plot(seasonal,'g-')
plt.xlabel('Seasonal')
plt.figure(figsize=(20,8))

plt.subplot(414)
plt.plot(residual,'y-')
plt.xlabel('Residual')
plt.figure(figsize=(20,8))

plt.tight_layout()
```

<div>
 <img src="/DataViz/Delhi/Original.png" ></img>
 <img src="/DataViz/Delhi/Residual.png" ></img>
 <img src="/DataViz/Delhi/Sesasonal.png" ></img>
 <img src="/DataViz/Delhi/Trend.png" ></img>
</div>

Pandas dataframe.diff() is used to find the first discrete difference of objects over the given axis. We can provide a period value to shift for forming the difference.

```python
check_stationarity(df_train['Temp'].diff(12).dropna())
```
>Results of Dickey-Fuller Test:
>Test Statistic     :             -6.4568<br>
>p-value             :             0.0000<br>
>Lags Used                       12.0000<br>
>Number of Observations Used   : 275.0000<br>
>Critical Value (1%)          :   -3.4544<br>
>Critical Value (5%)           :  -2.8721<br>
>Critical Value (10%)          :  -2.5724<br>
>dtype: float64<br>

The Test Statistics is lower than the Critical Value of 5%.
The serie seems to be stationary

<div><img src="/DataViz/Delhi/Variation.png" ></img></div>
