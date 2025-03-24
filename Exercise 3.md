---
jupyter:
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
# Exercise 3. - GroupBy {#exercise-3---groupby}
:::

::: {.cell .markdown}
### Introduction:

GroupBy can be summarized as Split-Apply-Combine.

Special thanks to: <https://github.com/justmarkham> for sharing the
dataset and materials.

Check out this [Diagram](http://i.imgur.com/yjNkiwL.png)

Check out [Alcohol Consumption Exercises Video
Tutorial](https://youtu.be/az67CMdmS6s) to watch a data scientist go
through the exercises

### Step 1. Import the necessary libraries {#step-1-import-the-necessary-libraries}
:::

::: {.cell .code execution_count="15"}
``` python
import pandas as pd 
import numpy as np
```
:::

::: {.cell .markdown}
### Step 2. Import the dataset from this [address](https://raw.githubusercontent.com/justmarkham/DAT8/master/data/drinks.csv). {#step-2-import-the-dataset-from-this-address}
:::

::: {.cell .code execution_count="16"}
``` python
url = 'https://raw.githubusercontent.com/justmarkham/DAT8/master/data/drinks.csv'
df = pd.read_csv(url)
```
:::

::: {.cell .markdown}
### Step 3. Assign it to a variable called drinks. {#step-3-assign-it-to-a-variable-called-drinks}
:::

::: {.cell .code execution_count="17"}
``` python
drinks = df 
drinks.head()
```

::: {.output .execute_result execution_count="17"}
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
      <th>country</th>
      <th>beer_servings</th>
      <th>spirit_servings</th>
      <th>wine_servings</th>
      <th>total_litres_of_pure_alcohol</th>
      <th>continent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.0</td>
      <td>AS</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>89</td>
      <td>132</td>
      <td>54</td>
      <td>4.9</td>
      <td>EU</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>25</td>
      <td>0</td>
      <td>14</td>
      <td>0.7</td>
      <td>AF</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Andorra</td>
      <td>245</td>
      <td>138</td>
      <td>312</td>
      <td>12.4</td>
      <td>EU</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Angola</td>
      <td>217</td>
      <td>57</td>
      <td>45</td>
      <td>5.9</td>
      <td>AF</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 4. Which continent drinks more beer on average? {#step-4-which-continent-drinks-more-beer-on-average}
:::

::: {.cell .code execution_count="18"}
``` python
gb = drinks.groupby('continent').agg({
    'beer_servings' : 'mean'
})
gb
```

::: {.output .execute_result execution_count="18"}
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
      <th>beer_servings</th>
    </tr>
    <tr>
      <th>continent</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>AF</th>
      <td>61.471698</td>
    </tr>
    <tr>
      <th>AS</th>
      <td>37.045455</td>
    </tr>
    <tr>
      <th>EU</th>
      <td>193.777778</td>
    </tr>
    <tr>
      <th>OC</th>
      <td>89.687500</td>
    </tr>
    <tr>
      <th>SA</th>
      <td>175.083333</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .code execution_count="19"}
``` python
gb.sort_values('beer_servings')
```

::: {.output .execute_result execution_count="19"}
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
      <th>beer_servings</th>
    </tr>
    <tr>
      <th>continent</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>AS</th>
      <td>37.045455</td>
    </tr>
    <tr>
      <th>AF</th>
      <td>61.471698</td>
    </tr>
    <tr>
      <th>OC</th>
      <td>89.687500</td>
    </tr>
    <tr>
      <th>SA</th>
      <td>175.083333</td>
    </tr>
    <tr>
      <th>EU</th>
      <td>193.777778</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 5. For each continent print the statistics for wine consumption. {#step-5-for-each-continent-print-the-statistics-for-wine-consumption}
:::

::: {.cell .code execution_count="20"}
``` python
gb = drinks.groupby('continent').agg({
    'beer_servings': 'describe'
})
gb
```

::: {.output .execute_result execution_count="20"}
```{=html}
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="8" halign="left">beer_servings</th>
    </tr>
    <tr>
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>continent</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>AF</th>
      <td>53.0</td>
      <td>61.471698</td>
      <td>80.557816</td>
      <td>0.0</td>
      <td>15.00</td>
      <td>32.0</td>
      <td>76.00</td>
      <td>376.0</td>
    </tr>
    <tr>
      <th>AS</th>
      <td>44.0</td>
      <td>37.045455</td>
      <td>49.469725</td>
      <td>0.0</td>
      <td>4.25</td>
      <td>17.5</td>
      <td>60.50</td>
      <td>247.0</td>
    </tr>
    <tr>
      <th>EU</th>
      <td>45.0</td>
      <td>193.777778</td>
      <td>99.631569</td>
      <td>0.0</td>
      <td>127.00</td>
      <td>219.0</td>
      <td>270.00</td>
      <td>361.0</td>
    </tr>
    <tr>
      <th>OC</th>
      <td>16.0</td>
      <td>89.687500</td>
      <td>96.641412</td>
      <td>0.0</td>
      <td>21.00</td>
      <td>52.5</td>
      <td>125.75</td>
      <td>306.0</td>
    </tr>
    <tr>
      <th>SA</th>
      <td>12.0</td>
      <td>175.083333</td>
      <td>65.242845</td>
      <td>93.0</td>
      <td>129.50</td>
      <td>162.5</td>
      <td>198.00</td>
      <td>333.0</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 6. Print the mean alcohol consumption per continent for every column {#step-6-print-the-mean-alcohol-consumption-per-continent-for-every-column}
:::

::: {.cell .code execution_count="31"}
``` python
numeric_cols = drinks.select_dtypes(include=['number']).columns

gb = drinks.groupby('continent')[numeric_cols].mean()
gb
```

::: {.output .execute_result execution_count="31"}
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
      <th>beer_servings</th>
      <th>spirit_servings</th>
      <th>wine_servings</th>
      <th>total_litres_of_pure_alcohol</th>
    </tr>
    <tr>
      <th>continent</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>AF</th>
      <td>61.471698</td>
      <td>16.339623</td>
      <td>16.264151</td>
      <td>3.007547</td>
    </tr>
    <tr>
      <th>AS</th>
      <td>37.045455</td>
      <td>60.840909</td>
      <td>9.068182</td>
      <td>2.170455</td>
    </tr>
    <tr>
      <th>EU</th>
      <td>193.777778</td>
      <td>132.555556</td>
      <td>142.222222</td>
      <td>8.617778</td>
    </tr>
    <tr>
      <th>OC</th>
      <td>89.687500</td>
      <td>58.437500</td>
      <td>35.625000</td>
      <td>3.381250</td>
    </tr>
    <tr>
      <th>SA</th>
      <td>175.083333</td>
      <td>114.750000</td>
      <td>62.416667</td>
      <td>6.308333</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 7. Print the median alcohol consumption per continent for every column {#step-7-print-the-median-alcohol-consumption-per-continent-for-every-column}
:::

::: {.cell .code execution_count="35"}
``` python
numeric_cols = drinks.select_dtypes(include=['number']).columns

gb = drinks.groupby('continent')[numeric_cols].median()
gb
```

::: {.output .execute_result execution_count="35"}
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
      <th>beer_servings</th>
      <th>spirit_servings</th>
      <th>wine_servings</th>
      <th>total_litres_of_pure_alcohol</th>
    </tr>
    <tr>
      <th>continent</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>AF</th>
      <td>32.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>2.30</td>
    </tr>
    <tr>
      <th>AS</th>
      <td>17.5</td>
      <td>16.0</td>
      <td>1.0</td>
      <td>1.20</td>
    </tr>
    <tr>
      <th>EU</th>
      <td>219.0</td>
      <td>122.0</td>
      <td>128.0</td>
      <td>10.00</td>
    </tr>
    <tr>
      <th>OC</th>
      <td>52.5</td>
      <td>37.0</td>
      <td>8.5</td>
      <td>1.75</td>
    </tr>
    <tr>
      <th>SA</th>
      <td>162.5</td>
      <td>108.5</td>
      <td>12.0</td>
      <td>6.85</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 8. Print the mean, min and max values for spirit consumption. {#step-8-print-the-mean-min-and-max-values-for-spirit-consumption}

#### This time output a DataFrame
:::

::: {.cell .code execution_count="36"}
``` python
drinks.spirit_servings.describe()
```

::: {.output .execute_result execution_count="36"}
    count    193.000000
    mean      80.994819
    std       88.284312
    min        0.000000
    25%        4.000000
    50%       56.000000
    75%      128.000000
    max      438.000000
    Name: spirit_servings, dtype: float64
:::
:::
