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
<p>name_of_data.isnull().sum()</p>

```plthon
data.isnull().sum()
```
<br>

# Print unique values to see how many value we have to work on it
<p>data['name_of_column'].unique()</p>

```python
data['going out'].unique()
```
<br>

# Fill in the missing value with the word "May be"
<p>data['name_of_column'].fillna("word_you_want_to_put", inplace=True)</p>

```phthon
data['going out'].fillna("May be", inplace=True)
```
<br>

# Print percent of each values

```python
data['going out'].value_counts(normalize=True) * 100
```

<br>

# Replace the name of value
data['name_of_column'].replace({"vulue_name":"word_you_want_to_use", "Other": "Don't say"}, inplace=True)</p>

```python
data['sex'].replace({"I'd rather not say":"Don't say", "Other": "Don't say"}, inplace=True)
```
<br>

# Convert values to number 
<p>"errors='coerce" (if there are non-numeric values pandas should replace those values with NaN (Not a Number))
</p>

```python
data['age'] = pd.to_numberic(data['age'], errors= 'coerce')
```
















