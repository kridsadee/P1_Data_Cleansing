# Practice Data Cleansing 1
<p><b>This is what we going to do in this project</b></p>
<ul>
<li>First, we dropped all the useless columns and columns with too much NaN.</li>
<li>Then, we renamed columns for easeness.</li>
<li>We cleaned the going out column by replacing NaNs with the 'Maybe' word.</li>
<li>We cleaned the sex column by replacing NaNs with the 'Didn't say' word.</li>
<li>We cleaned the age column by replacing NaNs with the median of the ages.</li>
<li>We cleaned the country column by replacing NaNs with the 'Other' word, and we used fuzzywuzzy to deal with the problem of different writing ways.</li>
<li>We cleaned the area column by dropping it.</li>
<li>We cleaned the Q6 columns by replacing NaNs with the 'Unknown' word.</li>
<li>We cleaned the dress column by replacing NaNs with the 'Other' word.</li>
<li>We cleaned the day column by replacing NaNs with the 'Other' word.</li>
Finally, we checked the data and made sure that there is no wrong data type, no NaN values, and all the columns are clean and ready for the next step EDA.</ul>
</ul>

<p><b> I'll break down the code we used. But it's not the entire code, just an overview, and code with many explanation </p></b>

# Importing packages and loading data
<ul>
  <li><b>NumPy</b> is widely used for numerical computations in Python, especially for handling arrays and mathematical operations.</li>
  <li><b>Pandas</b> is a powerful library for data manipulation and analysis in Python. It provides data structures like DataFrame and Series, which are essential for working with structured data.</li>
  <li><b>Matplotlib</b> is a plotting library in Python, and pyplot provides a MATLAB-like interface for creating plots and visualizations.</li>
  <li><b>Seaborn</b> is a statistical data visualization library based on Matplotlib. It provides a high-level interface for creating attractive and informative statistical graphics.</li>
  <li><b>%matplotlib inline</b> is a magic command for Jupyter Notebooks that allows Matplotlib plots to be displayed directly in the notebook output cells.</li>
  <li><b>warnings</b> provide a way to handle warnings issued by Python code. It allows you to control whether warnings should be ignored, displayed, or turned into errors.</li>
</ul>

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
import warnings

data = pd.read_excel("./candyhierarchy2017.xlsx")
data 
```
<br>

# Checking how many missing in each columns

**`name_of_data.isnull().sum()`**

```plthon
data.isnull().sum()
```
```python
Output:
going out        0
sex              0
age              0
country          0
100 Grand Bar    0
                ..
daily dish       0
science          0
espn             0
yahoo            0
No page          0
Length: 114, dtype: int64
```

# Print unique values to see how many value we have to work on it

**`data['name_of_column'].unique()`**

```python
data['going out'].unique()
```

```python
Output:
array([41, 44, 49, 40, 23, 53, 33, 43, 56, 64, 37, 59, 48, 54, 36, 45, 25,
       34, 35, 38, 58, 50, 47, 16, 52, 63, 65, 27, 31, 61, 46, 42, 62, 29,])
```

# Fill in the missing value with the word "May be"

**` data['name_of_column'].fillna("word_you_want_to_put", inplace=True)`**

```phthon
data['going out'].fillna("May be", inplace=True)
```
<br>

# Print percent of each values

<p><b>value_counts</b> method to count the occurrences of each unique value in the 'age' column and then normalizes the counts by dividing by the total number of non-null values in the column</p>
<p><b>(normalize=True)</b> method to count the occurrences of each unique value in the 'age' column and then normalizes the counts by dividing by the total number of non-null values in the column</p>

```python
data['going out'].value_counts(normalize=True) * 100
```

```phthon
Output:
No       82.845528
Yes      12.682927
Maybe     4.471545
```

# The way you can use with value_count

<ul>
<li><b>normalize:</b> If set to <b>True</b>, it returns the relative frequencies of unique values.</li>
<li><b>ascending:</b> If set to <b>False</b>, the unique values are sorted in descending order.</li>

  **`print(data['column_name'].value_counts(ascending=False)`**
    
<li><b>dropna:</b> If set to <b>True</b>, it excludes NA/null values from the result.</li>

 **`print(data['column_name'].value_counts(dropna=True)`**
 
<li><b>bins:</b> This parameter is used to specify the number of equal-width bins to divide the range of values into. It's useful for binning continuous data into discrete intervals. <b>precision=0</b> control the number of decimal places</li>

**`pd.cut(data['age'], bins=3, precision=0).value_counts()`**

```python
Output:
age
(31.0, 53.0]    1654 
(53.0, 74.0]    429 
(10.0, 31.0]    377
Name: count, dtype: int64
```

</ul>
<br>

# Replace the name of value

**` data['name_of_column'].replace({"vulue_name":"word_you_want_to_use", "Other": "Don't say"}, inplace=True)`**

```python
data['sex'].replace({"I'd rather not say":"Don't say", "Other": "Don't say"}, inplace=True)
```
<br>

# Convert values to number 

**`data['column_name'] = pd.to_numberic(data['column_name'], errors= 'coerce')`**

<p>"errors='coerce" (if there are non-numeric values pandas should replace those values with NaN (Not a Number))
</p>

```python
data['age'] = pd.to_numberic(data['age'], errors= 'coerce')
```

# Get an overview of the central tendency and spread of the 'age' variable

**`sns.boxplot(x=data['column_name'])`**

```python
sns.boxplot(x=data['age'])
```
![Screenshot 2567-03-08 at 10 17 12](https://github.com/kridsadee/P1_Data_Cleansing/assets/124231320/643831ee-68fe-4c1d-8ab8-9fb1353080aa)

# Interquartile range (IQR) method

<p><b>calculates the lower and upper bounds for potential outliers in the 'age' column based on the Interquartile Range (IQR) method</b>
</p>
<ul>
  <li><b>q1 = data['age'].quantile(0.25):</b> This line calculates the first quartile (25th percentile) of the 'age' column in the DataFrame data and assigns it to the variable q1.</li>
  <li><b>q3 = data['age'].quantile(0.75):</b> This line calculates the third quartile (75th percentile) of the 'age' column in the DataFrame data and assigns it to the variable q3.</li>
  <li><b>iqr = q3 - q1:</b> This line calculates the interquartile range (IQR) by subtracting the first quartile (q1) from the third quartile (q3) and assigns it to the variable iqr.</li>
  <li><b>lower_bound = q1 - (1.5 * iqr):</b> This line calculates the lower bound for outliers detection using the IQR method. It subtracts 1.5 times the IQR from the first quartile (q1) and assigns it to the variable lower_bound.</li>
  <li><b>upper_bound = q3 + (1.5 * iqr):</b> This line calculates the upper bound for outliers detection using the IQR method. It adds 1.5 times the IQR to the third quartile (q3) and assigns it to the variable upper_bound.</li>
  <li><b>data['age'] = data['age'].apply(lambda x: np.nan if x < lower_bound or x > upper_bound else x):</b> This line applies a function to each value in the 'age' column of the DataFrame data. The function checks if the value is less than the lower_bound or greater than the upper_bound. If it is, it replaces that value with np.nan (indicating a missing value), otherwise, it keeps the original value.</li>
</ul>

```python
q1 = data['age'].quantile(0.25)
q3 = data['age'].quantile(0.75)
iqr = q3 - q1
lower_bound = q1 - (1.5 * iqr)
upper_bound = q3 + (1.5 * iqr)
data['age'] = data['age'].apply(lambda x: np.nan if x < lower_bound or x > upper_bound else x)
```

# fuzzywuzzy
<ul>
  <li><b>fuzzywuzzy</b> library provides tools for fuzzy string matching.</li>
  <li><b>val_countries = ['usa', 'united states', 'united states of america', 'netherlands', 'norway']:</b> This line defines a list named <b>val_countries</b> containing various strings representing country names. These strings will be used for matching against the country names in the dataset.</li>
  <li><b>data['country'] = data['country'].str.lower():</b> This line converts all country names in the 'country' column of the DataFrame data to lowercase</li>
  <li><b>for country in val_countries:</b> This line starts a loop where each element <b>(country)</b> from the <b>val_countries</b> list is iterated over.</li>
  <li><b>matches = process.extract(country, data['country'], limit=data.shape[0]):<b> For each country in the loop, this line uses the <b>process.extract()</b> function from the <b>fuzzywuzzy</b> library to find matches between the current country and all the country names in the 'country' column of the DataFrame data. It returns a list of tuples where each tuple contains a matched country name and its similarity score.</li>
    <li><b>for sub in matches::</b> This line starts another loop where each tuple <b>(sub)</b> from the </li>matches list is iterated over.</li>
    <li><b>if sub[1] >= 80::</b> This line checks if the similarity score <b>(sub[1])</b> of the current match is greater than or equal to 80. If it is, it means that the match is considered sufficiently similar.</li>
    <li><b>data.loc[data['country'] == sub[0], 'country'] = country:</b> If the similarity score is sufficient, this line updates the corresponding country name in the 'country' column of the DataFrame data to the standardized name (country) from the val_countries list.</li>
</ul>

```python
from fuzzywuzzy import process

val_countries = ['usa', 'united states', 'united states of america', 'america', 'uk', 'england', 'united kingdom', \
    'japan', 'canada', 'france', 'germany', 'spain', 'australia', 'sweden', 'switzerland', 'netherlands', 'norway']

data['country'] = data['country'].str.lower()
for country in val_countries:
        matches = process.extract(country, data['country'], limit=data.shape[0])
        for sub in matches:
            if sub[1] >= 80:
                data.loc[data['country'] == sub[0], 'country'] = country
```

# Loop across data columns

<p><b>loop across data columns started with Q6 and fill in missing value by 'Unknown'
</b></p>


```python
for col in data.columns:
    if col.startswith('Q6'):
        data[col].fillna('Unknown', inplace=True)
```

<p><b>loop across data columns starting from column 4 up to column 6 of the end, and rename it by remove first 5 character
</b></p>
<p>for item in sequence:
    <br># Code block to be executed for each item
</p>

```python
for col in data.columns[4:-6]:
    data.rename(columns={col: col[5:]}, inplace=True)
```

# Chart

![image](https://github.com/kridsadee/P1_Data_Cleansing/assets/124231320/4626932f-6383-4fcd-bc81-f4f4cb30ce85)

![image](https://github.com/kridsadee/P1_Data_Cleansing/assets/124231320/fa7e064a-326e-4b3f-90d9-918e0390d1e3)










