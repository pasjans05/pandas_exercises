---
jupyter:
  anaconda-cloud: {}
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
  language_info:
    codemirror_mode:
      name: ipython
      version: 3
    file_extension: .py
    mimetype: text/x-python
    name: python
    nbconvert_exporter: python
    pygments_lexer: ipython3
    version: 3.13.2
  nbformat: 4
  nbformat_minor: 0
---

::: {.cell .markdown}
# Exercise 1. - Getting and Knowing your Data {#exercise-1---getting-and-knowing-your-data}
:::

::: {.cell .markdown}
This time we are going to pull data directly from the internet. Special
thanks to: <https://github.com/justmarkham> for sharing the dataset and
materials.

Check out [Occupation Exercises Video
Tutorial](https://www.youtube.com/watch?v=W8AB5s-L3Rw&list=PLgJhDSE2ZLxaY_DigHeiIDC1cD09rXgJv&index=4)
to watch a data scientist go through the exercises

### Step 1. Import the necessary libraries! {#step-1-import-the-necessary-libraries}
:::

::: {.cell .code execution_count="1" collapsed="false"}
``` python
import pandas as pd 
```
:::

::: {.cell .markdown}
### Step 2. Import the dataset from this [address](https://raw.githubusercontent.com/justmarkham/DAT8/master/data/u.user). {#step-2-import-the-dataset-from-this-address}
:::

::: {.cell .code execution_count="2"}
``` python
url = 'https://raw.githubusercontent.com/justmarkham/DAT8/master/data/u.user'
df = pd.read_csv(url, sep = '|')
```
:::

::: {.cell .markdown}
### Step 3. Assign it to a variable called users and use the \'user_id\' as index {#step-3-assign-it-to-a-variable-called-users-and-use-the-user_id-as-index}
:::

::: {.cell .code execution_count="3" collapsed="false"}
``` python
users = df
users.set_index('user_id', inplace=True)
```
:::

::: {.cell .markdown}
### Step 4. See the first 25 entries {#step-4-see-the-first-25-entries}
:::

::: {.cell .code execution_count="4" collapsed="false" scrolled="true"}
``` python
users.head(25)
```

::: {.output .execute_result execution_count="4"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>gender</th>
      <th>occupation</th>
      <th>zip_code</th>
    </tr>
    <tr>
      <th>user_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>24</td>
      <td>M</td>
      <td>technician</td>
      <td>85711</td>
    </tr>
    <tr>
      <th>2</th>
      <td>53</td>
      <td>F</td>
      <td>other</td>
      <td>94043</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23</td>
      <td>M</td>
      <td>writer</td>
      <td>32067</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24</td>
      <td>M</td>
      <td>technician</td>
      <td>43537</td>
    </tr>
    <tr>
      <th>5</th>
      <td>33</td>
      <td>F</td>
      <td>other</td>
      <td>15213</td>
    </tr>
    <tr>
      <th>6</th>
      <td>42</td>
      <td>M</td>
      <td>executive</td>
      <td>98101</td>
    </tr>
    <tr>
      <th>7</th>
      <td>57</td>
      <td>M</td>
      <td>administrator</td>
      <td>91344</td>
    </tr>
    <tr>
      <th>8</th>
      <td>36</td>
      <td>M</td>
      <td>administrator</td>
      <td>05201</td>
    </tr>
    <tr>
      <th>9</th>
      <td>29</td>
      <td>M</td>
      <td>student</td>
      <td>01002</td>
    </tr>
    <tr>
      <th>10</th>
      <td>53</td>
      <td>M</td>
      <td>lawyer</td>
      <td>90703</td>
    </tr>
    <tr>
      <th>11</th>
      <td>39</td>
      <td>F</td>
      <td>other</td>
      <td>30329</td>
    </tr>
    <tr>
      <th>12</th>
      <td>28</td>
      <td>F</td>
      <td>other</td>
      <td>06405</td>
    </tr>
    <tr>
      <th>13</th>
      <td>47</td>
      <td>M</td>
      <td>educator</td>
      <td>29206</td>
    </tr>
    <tr>
      <th>14</th>
      <td>45</td>
      <td>M</td>
      <td>scientist</td>
      <td>55106</td>
    </tr>
    <tr>
      <th>15</th>
      <td>49</td>
      <td>F</td>
      <td>educator</td>
      <td>97301</td>
    </tr>
    <tr>
      <th>16</th>
      <td>21</td>
      <td>M</td>
      <td>entertainment</td>
      <td>10309</td>
    </tr>
    <tr>
      <th>17</th>
      <td>30</td>
      <td>M</td>
      <td>programmer</td>
      <td>06355</td>
    </tr>
    <tr>
      <th>18</th>
      <td>35</td>
      <td>F</td>
      <td>other</td>
      <td>37212</td>
    </tr>
    <tr>
      <th>19</th>
      <td>40</td>
      <td>M</td>
      <td>librarian</td>
      <td>02138</td>
    </tr>
    <tr>
      <th>20</th>
      <td>42</td>
      <td>F</td>
      <td>homemaker</td>
      <td>95660</td>
    </tr>
    <tr>
      <th>21</th>
      <td>26</td>
      <td>M</td>
      <td>writer</td>
      <td>30068</td>
    </tr>
    <tr>
      <th>22</th>
      <td>25</td>
      <td>M</td>
      <td>writer</td>
      <td>40206</td>
    </tr>
    <tr>
      <th>23</th>
      <td>30</td>
      <td>F</td>
      <td>artist</td>
      <td>48197</td>
    </tr>
    <tr>
      <th>24</th>
      <td>21</td>
      <td>F</td>
      <td>artist</td>
      <td>94533</td>
    </tr>
    <tr>
      <th>25</th>
      <td>39</td>
      <td>M</td>
      <td>engineer</td>
      <td>55107</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 5. See the last 10 entries {#step-5-see-the-last-10-entries}
:::

::: {.cell .code execution_count="5" collapsed="false" scrolled="true"}
``` python
users.tail(10)
```

::: {.output .execute_result execution_count="5"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>gender</th>
      <th>occupation</th>
      <th>zip_code</th>
    </tr>
    <tr>
      <th>user_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>934</th>
      <td>61</td>
      <td>M</td>
      <td>engineer</td>
      <td>22902</td>
    </tr>
    <tr>
      <th>935</th>
      <td>42</td>
      <td>M</td>
      <td>doctor</td>
      <td>66221</td>
    </tr>
    <tr>
      <th>936</th>
      <td>24</td>
      <td>M</td>
      <td>other</td>
      <td>32789</td>
    </tr>
    <tr>
      <th>937</th>
      <td>48</td>
      <td>M</td>
      <td>educator</td>
      <td>98072</td>
    </tr>
    <tr>
      <th>938</th>
      <td>38</td>
      <td>F</td>
      <td>technician</td>
      <td>55038</td>
    </tr>
    <tr>
      <th>939</th>
      <td>26</td>
      <td>F</td>
      <td>student</td>
      <td>33319</td>
    </tr>
    <tr>
      <th>940</th>
      <td>32</td>
      <td>M</td>
      <td>administrator</td>
      <td>02215</td>
    </tr>
    <tr>
      <th>941</th>
      <td>20</td>
      <td>M</td>
      <td>student</td>
      <td>97229</td>
    </tr>
    <tr>
      <th>942</th>
      <td>48</td>
      <td>F</td>
      <td>librarian</td>
      <td>78209</td>
    </tr>
    <tr>
      <th>943</th>
      <td>22</td>
      <td>M</td>
      <td>student</td>
      <td>77841</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 6. What is the number of observations in the dataset? {#step-6-what-is-the-number-of-observations-in-the-dataset}
:::

::: {.cell .code execution_count="6" collapsed="false"}
``` python
users.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    Index: 943 entries, 1 to 943
    Data columns (total 4 columns):
     #   Column      Non-Null Count  Dtype 
    ---  ------      --------------  ----- 
     0   age         943 non-null    int64 
     1   gender      943 non-null    object
     2   occupation  943 non-null    object
     3   zip_code    943 non-null    object
    dtypes: int64(1), object(3)
    memory usage: 36.8+ KB
:::
:::

::: {.cell .markdown}
### Step 7. What is the number of columns in the dataset? {#step-7-what-is-the-number-of-columns-in-the-dataset}
:::

::: {.cell .code execution_count="7" collapsed="false"}
``` python
users.columns
```

::: {.output .execute_result execution_count="7"}
    Index(['age', 'gender', 'occupation', 'zip_code'], dtype='object')
:::
:::

::: {.cell .markdown}
### Step 8. Print the name of all the columns. {#step-8-print-the-name-of-all-the-columns}
:::

::: {.cell .code execution_count="9" collapsed="false"}
``` python
users.columns
```

::: {.output .execute_result execution_count="9"}
    Index(['age', 'gender', 'occupation', 'zip_code'], dtype='object')
:::
:::

::: {.cell .markdown}
### Step 9. How is the dataset indexed? {#step-9-how-is-the-dataset-indexed}
:::

::: {.cell .code execution_count="8" collapsed="false"}
``` python
users.index
```

::: {.output .execute_result execution_count="8"}
    Index([  1,   2,   3,   4,   5,   6,   7,   8,   9,  10,
           ...
           934, 935, 936, 937, 938, 939, 940, 941, 942, 943],
          dtype='int64', name='user_id', length=943)
:::
:::

::: {.cell .markdown}
### Step 10. What is the data type of each column? {#step-10-what-is-the-data-type-of-each-column}
:::

::: {.cell .code execution_count="10" collapsed="false"}
``` python
users.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    Index: 943 entries, 1 to 943
    Data columns (total 4 columns):
     #   Column      Non-Null Count  Dtype 
    ---  ------      --------------  ----- 
     0   age         943 non-null    int64 
     1   gender      943 non-null    object
     2   occupation  943 non-null    object
     3   zip_code    943 non-null    object
    dtypes: int64(1), object(3)
    memory usage: 36.8+ KB
:::
:::

::: {.cell .markdown}
### Step 11. Print only the occupation column {#step-11-print-only-the-occupation-column}
:::

::: {.cell .code execution_count="11" collapsed="false"}
``` python
users.occupation.head()
```

::: {.output .execute_result execution_count="11"}
    user_id
    1    technician
    2         other
    3        writer
    4    technician
    5         other
    Name: occupation, dtype: object
:::
:::

::: {.cell .markdown}
### Step 12. How many different occupations are in this dataset? {#step-12-how-many-different-occupations-are-in-this-dataset}
:::

::: {.cell .code execution_count="12" collapsed="false"}
``` python
users.occupation.nunique()
```

::: {.output .execute_result execution_count="12"}
    21
:::
:::

::: {.cell .markdown}
### Step 13. What is the most frequent occupation? {#step-13-what-is-the-most-frequent-occupation}
:::

::: {.cell .code execution_count="13" collapsed="false"}
``` python
users.occupation.value_counts()
```

::: {.output .execute_result execution_count="13"}
    occupation
    student          196
    other            105
    educator          95
    administrator     79
    engineer          67
    programmer        66
    librarian         51
    writer            45
    executive         32
    scientist         31
    artist            28
    technician        27
    marketing         26
    entertainment     18
    healthcare        16
    retired           14
    lawyer            12
    salesman          12
    none               9
    homemaker          7
    doctor             7
    Name: count, dtype: int64
:::
:::

::: {.cell .markdown}
### Step 14. Summarize the DataFrame. {#step-14-summarize-the-dataframe}
:::

::: {.cell .code execution_count="14" collapsed="false"}
``` python
users.describe(include='all')
```

::: {.output .execute_result execution_count="14"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>gender</th>
      <th>occupation</th>
      <th>zip_code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>943.000000</td>
      <td>943</td>
      <td>943</td>
      <td>943</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>NaN</td>
      <td>2</td>
      <td>21</td>
      <td>795</td>
    </tr>
    <tr>
      <th>top</th>
      <td>NaN</td>
      <td>M</td>
      <td>student</td>
      <td>55414</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>NaN</td>
      <td>670</td>
      <td>196</td>
      <td>9</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>34.051962</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>std</th>
      <td>12.192740</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>min</th>
      <td>7.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>25.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>31.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>43.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>max</th>
      <td>73.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 15. Summarize all the columns {#step-15-summarize-all-the-columns}
:::

::: {.cell .code execution_count="15" collapsed="false"}
``` python
users.describe()
```

::: {.output .execute_result execution_count="15"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>943.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>34.051962</td>
    </tr>
    <tr>
      <th>std</th>
      <td>12.192740</td>
    </tr>
    <tr>
      <th>min</th>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>25.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>31.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>43.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>73.000000</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 16. Summarize only the occupation column {#step-16-summarize-only-the-occupation-column}
:::

::: {.cell .code execution_count="16" collapsed="false"}
``` python
print(users['occupation'].value_counts())
print(users['occupation'].describe())
```

::: {.output .stream .stdout}
    occupation
    student          196
    other            105
    educator          95
    administrator     79
    engineer          67
    programmer        66
    librarian         51
    writer            45
    executive         32
    scientist         31
    artist            28
    technician        27
    marketing         26
    entertainment     18
    healthcare        16
    retired           14
    lawyer            12
    salesman          12
    none               9
    homemaker          7
    doctor             7
    Name: count, dtype: int64
    count         943
    unique         21
    top       student
    freq          196
    Name: occupation, dtype: object
:::
:::

::: {.cell .markdown}
### Step 17. What is the mean age of users? {#step-17-what-is-the-mean-age-of-users}
:::

::: {.cell .code execution_count="17" collapsed="false"}
``` python
users.age.mean()
```

::: {.output .execute_result execution_count="17"}
    np.float64(34.05196182396607)
:::
:::

::: {.cell .markdown}
### Step 18. What is the age with least occurrence? {#step-18-what-is-the-age-with-least-occurrence}
:::

::: {.cell .code execution_count="18" collapsed="false"}
``` python
users.age.value_counts(ascending=True)
```

::: {.output .execute_result execution_count="18"}
    age
    7      1
    11     1
    73     1
    10     1
    66     1
          ..
    27    35
    28    36
    22    37
    25    38
    30    39
    Name: count, Length: 61, dtype: int64
:::
:::
