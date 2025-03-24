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
# Exercise 2. - Filtering and Sorting Data {#exercise-2---filtering-and-sorting-data}

Check out [Euro 12 Exercises Video
Tutorial](https://youtu.be/iqk5d48Qisg) to watch a data scientist go
through the exercises
:::

::: {.cell .markdown}
This time we are going to pull data directly from the internet.

### Step 1. Import the necessary libraries {#step-1-import-the-necessary-libraries}
:::

::: {.cell .code execution_count="52" collapsed="false"}
``` python
import pandas as pd
```
:::

::: {.cell .markdown}
### Step 2. Import the dataset from this [address](https://raw.githubusercontent.com/kflisikowsky/pandas_exercises/refs/heads/main/Euro_2012_stats_TEAM.csv). {#step-2-import-the-dataset-from-this-address}
:::

::: {.cell .code execution_count="53"}
``` python
df = pd.read_csv('Euro_2012_stats_TEAM.csv')
```
:::

::: {.cell .markdown}
### Step 3. Assign it to a variable called euro12. {#step-3-assign-it-to-a-variable-called-euro12}
:::

::: {.cell .code execution_count="54" collapsed="false"}
``` python
euro12 = df
euro12
```

::: {.output .execute_result execution_count="54"}
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
      <th>Team</th>
      <th>Goals</th>
      <th>Shots on target</th>
      <th>Shots off target</th>
      <th>Shooting Accuracy</th>
      <th>% Goals-to-shots</th>
      <th>Total shots (inc. Blocked)</th>
      <th>Hit Woodwork</th>
      <th>Penalty goals</th>
      <th>Penalties not scored</th>
      <th>...</th>
      <th>Saves made</th>
      <th>Saves-to-shots ratio</th>
      <th>Fouls Won</th>
      <th>Fouls Conceded</th>
      <th>Offsides</th>
      <th>Yellow Cards</th>
      <th>Red Cards</th>
      <th>Subs on</th>
      <th>Subs off</th>
      <th>Players Used</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Croatia</td>
      <td>4</td>
      <td>13</td>
      <td>12</td>
      <td>51.9%</td>
      <td>16.0%</td>
      <td>32</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>13</td>
      <td>81.3%</td>
      <td>41</td>
      <td>62</td>
      <td>2</td>
      <td>9</td>
      <td>0</td>
      <td>9</td>
      <td>9</td>
      <td>16</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Czech Republic</td>
      <td>4</td>
      <td>13</td>
      <td>18</td>
      <td>41.9%</td>
      <td>12.9%</td>
      <td>39</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>9</td>
      <td>60.1%</td>
      <td>53</td>
      <td>73</td>
      <td>8</td>
      <td>7</td>
      <td>0</td>
      <td>11</td>
      <td>11</td>
      <td>19</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Denmark</td>
      <td>4</td>
      <td>10</td>
      <td>10</td>
      <td>50.0%</td>
      <td>20.0%</td>
      <td>27</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>10</td>
      <td>66.7%</td>
      <td>25</td>
      <td>38</td>
      <td>8</td>
      <td>4</td>
      <td>0</td>
      <td>7</td>
      <td>7</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>England</td>
      <td>5</td>
      <td>11</td>
      <td>18</td>
      <td>50.0%</td>
      <td>17.2%</td>
      <td>40</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>22</td>
      <td>88.1%</td>
      <td>43</td>
      <td>45</td>
      <td>6</td>
      <td>5</td>
      <td>0</td>
      <td>11</td>
      <td>11</td>
      <td>16</td>
    </tr>
    <tr>
      <th>4</th>
      <td>France</td>
      <td>3</td>
      <td>22</td>
      <td>24</td>
      <td>37.9%</td>
      <td>6.5%</td>
      <td>65</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>6</td>
      <td>54.6%</td>
      <td>36</td>
      <td>51</td>
      <td>5</td>
      <td>6</td>
      <td>0</td>
      <td>11</td>
      <td>11</td>
      <td>19</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Germany</td>
      <td>10</td>
      <td>32</td>
      <td>32</td>
      <td>47.8%</td>
      <td>15.6%</td>
      <td>80</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>...</td>
      <td>10</td>
      <td>62.6%</td>
      <td>63</td>
      <td>49</td>
      <td>12</td>
      <td>4</td>
      <td>0</td>
      <td>15</td>
      <td>15</td>
      <td>17</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Greece</td>
      <td>5</td>
      <td>8</td>
      <td>18</td>
      <td>30.7%</td>
      <td>19.2%</td>
      <td>32</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>...</td>
      <td>13</td>
      <td>65.1%</td>
      <td>67</td>
      <td>48</td>
      <td>12</td>
      <td>9</td>
      <td>1</td>
      <td>12</td>
      <td>12</td>
      <td>20</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Italy</td>
      <td>6</td>
      <td>34</td>
      <td>45</td>
      <td>43.0%</td>
      <td>7.5%</td>
      <td>110</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>20</td>
      <td>74.1%</td>
      <td>101</td>
      <td>89</td>
      <td>16</td>
      <td>16</td>
      <td>0</td>
      <td>18</td>
      <td>18</td>
      <td>19</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Netherlands</td>
      <td>2</td>
      <td>12</td>
      <td>36</td>
      <td>25.0%</td>
      <td>4.1%</td>
      <td>60</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>12</td>
      <td>70.6%</td>
      <td>35</td>
      <td>30</td>
      <td>3</td>
      <td>5</td>
      <td>0</td>
      <td>7</td>
      <td>7</td>
      <td>15</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Poland</td>
      <td>2</td>
      <td>15</td>
      <td>23</td>
      <td>39.4%</td>
      <td>5.2%</td>
      <td>48</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>6</td>
      <td>66.7%</td>
      <td>48</td>
      <td>56</td>
      <td>3</td>
      <td>7</td>
      <td>1</td>
      <td>7</td>
      <td>7</td>
      <td>17</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Portugal</td>
      <td>6</td>
      <td>22</td>
      <td>42</td>
      <td>34.3%</td>
      <td>9.3%</td>
      <td>82</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>10</td>
      <td>71.5%</td>
      <td>73</td>
      <td>90</td>
      <td>10</td>
      <td>12</td>
      <td>0</td>
      <td>14</td>
      <td>14</td>
      <td>16</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Republic of Ireland</td>
      <td>1</td>
      <td>7</td>
      <td>12</td>
      <td>36.8%</td>
      <td>5.2%</td>
      <td>28</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>17</td>
      <td>65.4%</td>
      <td>43</td>
      <td>51</td>
      <td>11</td>
      <td>6</td>
      <td>1</td>
      <td>10</td>
      <td>10</td>
      <td>17</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Russia</td>
      <td>5</td>
      <td>9</td>
      <td>31</td>
      <td>22.5%</td>
      <td>12.5%</td>
      <td>59</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>10</td>
      <td>77.0%</td>
      <td>34</td>
      <td>43</td>
      <td>4</td>
      <td>6</td>
      <td>0</td>
      <td>7</td>
      <td>7</td>
      <td>16</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Spain</td>
      <td>12</td>
      <td>42</td>
      <td>33</td>
      <td>55.9%</td>
      <td>16.0%</td>
      <td>100</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>...</td>
      <td>15</td>
      <td>93.8%</td>
      <td>102</td>
      <td>83</td>
      <td>19</td>
      <td>11</td>
      <td>0</td>
      <td>17</td>
      <td>17</td>
      <td>18</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Sweden</td>
      <td>5</td>
      <td>17</td>
      <td>19</td>
      <td>47.2%</td>
      <td>13.8%</td>
      <td>39</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>8</td>
      <td>61.6%</td>
      <td>35</td>
      <td>51</td>
      <td>7</td>
      <td>7</td>
      <td>0</td>
      <td>9</td>
      <td>9</td>
      <td>18</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Ukraine</td>
      <td>2</td>
      <td>7</td>
      <td>26</td>
      <td>21.2%</td>
      <td>6.0%</td>
      <td>38</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>13</td>
      <td>76.5%</td>
      <td>48</td>
      <td>31</td>
      <td>4</td>
      <td>5</td>
      <td>0</td>
      <td>9</td>
      <td>9</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
<p>16 rows × 35 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 4. Select only the Goal column. {#step-4-select-only-the-goal-column}
:::

::: {.cell .code execution_count="55" collapsed="false"}
``` python
euro12.Goals
```

::: {.output .execute_result execution_count="55"}
    0      4
    1      4
    2      4
    3      5
    4      3
    5     10
    6      5
    7      6
    8      2
    9      2
    10     6
    11     1
    12     5
    13    12
    14     5
    15     2
    Name: Goals, dtype: int64
:::
:::

::: {.cell .markdown}
### Step 5. How many team participated in the Euro2012? {#step-5-how-many-team-participated-in-the-euro2012}
:::

::: {.cell .code execution_count="56" collapsed="false"}
``` python
euro12.Team.count()
```

::: {.output .execute_result execution_count="56"}
    np.int64(16)
:::
:::

::: {.cell .markdown}
### Step 6. What is the number of columns in the dataset? {#step-6-what-is-the-number-of-columns-in-the-dataset}
:::

::: {.cell .code execution_count="57" collapsed="false"}
``` python
euro12.shape
len(euro12.columns)
```

::: {.output .execute_result execution_count="57"}
    35
:::
:::

::: {.cell .markdown}
### Step 7. View only the columns Team, Yellow Cards and Red Cards and assign them to a dataframe called discipline {#step-7-view-only-the-columns-team-yellow-cards-and-red-cards-and-assign-them-to-a-dataframe-called-discipline}
:::

::: {.cell .code execution_count="58" collapsed="false"}
``` python
discipline = euro12[['Team', 'Yellow Cards', 'Red Cards']]
print(discipline.head())
```

::: {.output .stream .stdout}
                 Team  Yellow Cards  Red Cards
    0         Croatia             9          0
    1  Czech Republic             7          0
    2         Denmark             4          0
    3         England             5          0
    4          France             6          0
:::
:::

::: {.cell .markdown}
### Step 8. Sort the teams by Red Cards, then to Yellow Cards {#step-8-sort-the-teams-by-red-cards-then-to-yellow-cards}
:::

::: {.cell .code execution_count="59" collapsed="false" scrolled="true"}
``` python
discipline.sort_values(['Red Cards', 'Yellow Cards'], ascending=False)
```

::: {.output .execute_result execution_count="59"}
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
      <th>Team</th>
      <th>Yellow Cards</th>
      <th>Red Cards</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>Greece</td>
      <td>9</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Poland</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Republic of Ireland</td>
      <td>6</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Italy</td>
      <td>16</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Portugal</td>
      <td>12</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Spain</td>
      <td>11</td>
      <td>0</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Croatia</td>
      <td>9</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Czech Republic</td>
      <td>7</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Sweden</td>
      <td>7</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>France</td>
      <td>6</td>
      <td>0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Russia</td>
      <td>6</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>England</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Netherlands</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Ukraine</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Denmark</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Germany</td>
      <td>4</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 9. Calculate the mean Yellow Cards given per Team {#step-9-calculate-the-mean-yellow-cards-given-per-team}
:::

::: {.cell .code execution_count="60" collapsed="false"}
``` python
discipline['Yellow Cards'].mean()
```

::: {.output .execute_result execution_count="60"}
    np.float64(7.4375)
:::
:::

::: {.cell .markdown}
### Step 10. Filter teams that scored more than 6 goals {#step-10-filter-teams-that-scored-more-than-6-goals}
:::

::: {.cell .code execution_count="61" collapsed="false"}
``` python
euro12[euro12.Goals > 6]
```

::: {.output .execute_result execution_count="61"}
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
      <th>Team</th>
      <th>Goals</th>
      <th>Shots on target</th>
      <th>Shots off target</th>
      <th>Shooting Accuracy</th>
      <th>% Goals-to-shots</th>
      <th>Total shots (inc. Blocked)</th>
      <th>Hit Woodwork</th>
      <th>Penalty goals</th>
      <th>Penalties not scored</th>
      <th>...</th>
      <th>Saves made</th>
      <th>Saves-to-shots ratio</th>
      <th>Fouls Won</th>
      <th>Fouls Conceded</th>
      <th>Offsides</th>
      <th>Yellow Cards</th>
      <th>Red Cards</th>
      <th>Subs on</th>
      <th>Subs off</th>
      <th>Players Used</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>Germany</td>
      <td>10</td>
      <td>32</td>
      <td>32</td>
      <td>47.8%</td>
      <td>15.6%</td>
      <td>80</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>...</td>
      <td>10</td>
      <td>62.6%</td>
      <td>63</td>
      <td>49</td>
      <td>12</td>
      <td>4</td>
      <td>0</td>
      <td>15</td>
      <td>15</td>
      <td>17</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Spain</td>
      <td>12</td>
      <td>42</td>
      <td>33</td>
      <td>55.9%</td>
      <td>16.0%</td>
      <td>100</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>...</td>
      <td>15</td>
      <td>93.8%</td>
      <td>102</td>
      <td>83</td>
      <td>19</td>
      <td>11</td>
      <td>0</td>
      <td>17</td>
      <td>17</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 35 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 11. Select the teams that start with G {#step-11-select-the-teams-that-start-with-g}
:::

::: {.cell .code execution_count="62" collapsed="false"}
``` python
euro12[euro12.Team.str.startswith('G')]
```

::: {.output .execute_result execution_count="62"}
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
      <th>Team</th>
      <th>Goals</th>
      <th>Shots on target</th>
      <th>Shots off target</th>
      <th>Shooting Accuracy</th>
      <th>% Goals-to-shots</th>
      <th>Total shots (inc. Blocked)</th>
      <th>Hit Woodwork</th>
      <th>Penalty goals</th>
      <th>Penalties not scored</th>
      <th>...</th>
      <th>Saves made</th>
      <th>Saves-to-shots ratio</th>
      <th>Fouls Won</th>
      <th>Fouls Conceded</th>
      <th>Offsides</th>
      <th>Yellow Cards</th>
      <th>Red Cards</th>
      <th>Subs on</th>
      <th>Subs off</th>
      <th>Players Used</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>Germany</td>
      <td>10</td>
      <td>32</td>
      <td>32</td>
      <td>47.8%</td>
      <td>15.6%</td>
      <td>80</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>...</td>
      <td>10</td>
      <td>62.6%</td>
      <td>63</td>
      <td>49</td>
      <td>12</td>
      <td>4</td>
      <td>0</td>
      <td>15</td>
      <td>15</td>
      <td>17</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Greece</td>
      <td>5</td>
      <td>8</td>
      <td>18</td>
      <td>30.7%</td>
      <td>19.2%</td>
      <td>32</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>...</td>
      <td>13</td>
      <td>65.1%</td>
      <td>67</td>
      <td>48</td>
      <td>12</td>
      <td>9</td>
      <td>1</td>
      <td>12</td>
      <td>12</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 35 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 12. Select the first 7 columns {#step-12-select-the-first-7-columns}
:::

::: {.cell .code execution_count="63" collapsed="false"}
``` python
euro12.iloc[:, :7]
```

::: {.output .execute_result execution_count="63"}
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
      <th>Team</th>
      <th>Goals</th>
      <th>Shots on target</th>
      <th>Shots off target</th>
      <th>Shooting Accuracy</th>
      <th>% Goals-to-shots</th>
      <th>Total shots (inc. Blocked)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Croatia</td>
      <td>4</td>
      <td>13</td>
      <td>12</td>
      <td>51.9%</td>
      <td>16.0%</td>
      <td>32</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Czech Republic</td>
      <td>4</td>
      <td>13</td>
      <td>18</td>
      <td>41.9%</td>
      <td>12.9%</td>
      <td>39</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Denmark</td>
      <td>4</td>
      <td>10</td>
      <td>10</td>
      <td>50.0%</td>
      <td>20.0%</td>
      <td>27</td>
    </tr>
    <tr>
      <th>3</th>
      <td>England</td>
      <td>5</td>
      <td>11</td>
      <td>18</td>
      <td>50.0%</td>
      <td>17.2%</td>
      <td>40</td>
    </tr>
    <tr>
      <th>4</th>
      <td>France</td>
      <td>3</td>
      <td>22</td>
      <td>24</td>
      <td>37.9%</td>
      <td>6.5%</td>
      <td>65</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Germany</td>
      <td>10</td>
      <td>32</td>
      <td>32</td>
      <td>47.8%</td>
      <td>15.6%</td>
      <td>80</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Greece</td>
      <td>5</td>
      <td>8</td>
      <td>18</td>
      <td>30.7%</td>
      <td>19.2%</td>
      <td>32</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Italy</td>
      <td>6</td>
      <td>34</td>
      <td>45</td>
      <td>43.0%</td>
      <td>7.5%</td>
      <td>110</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Netherlands</td>
      <td>2</td>
      <td>12</td>
      <td>36</td>
      <td>25.0%</td>
      <td>4.1%</td>
      <td>60</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Poland</td>
      <td>2</td>
      <td>15</td>
      <td>23</td>
      <td>39.4%</td>
      <td>5.2%</td>
      <td>48</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Portugal</td>
      <td>6</td>
      <td>22</td>
      <td>42</td>
      <td>34.3%</td>
      <td>9.3%</td>
      <td>82</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Republic of Ireland</td>
      <td>1</td>
      <td>7</td>
      <td>12</td>
      <td>36.8%</td>
      <td>5.2%</td>
      <td>28</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Russia</td>
      <td>5</td>
      <td>9</td>
      <td>31</td>
      <td>22.5%</td>
      <td>12.5%</td>
      <td>59</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Spain</td>
      <td>12</td>
      <td>42</td>
      <td>33</td>
      <td>55.9%</td>
      <td>16.0%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Sweden</td>
      <td>5</td>
      <td>17</td>
      <td>19</td>
      <td>47.2%</td>
      <td>13.8%</td>
      <td>39</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Ukraine</td>
      <td>2</td>
      <td>7</td>
      <td>26</td>
      <td>21.2%</td>
      <td>6.0%</td>
      <td>38</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### Step 13. Select all columns except the last 3. {#step-13-select-all-columns-except-the-last-3}
:::

::: {.cell .code execution_count="64" collapsed="false"}
``` python
euro12.iloc[:, :-3].shape
```

::: {.output .execute_result execution_count="64"}
    (16, 32)
:::
:::

::: {.cell .markdown}
### Step 14. Present only the Shooting Accuracy from England, Italy and Russia {#step-14-present-only-the-shooting-accuracy-from-england-italy-and-russia}
:::

::: {.cell .code execution_count="65" collapsed="false"}
``` python
euro12.set_index('Team', inplace=True)
euro12.loc[['England', 'Italy', 'Russia'], 'Shooting Accuracy']
```

::: {.output .execute_result execution_count="65"}
    Team
    England    50.0%
    Italy      43.0%
    Russia     22.5%
    Name: Shooting Accuracy, dtype: object
:::
:::
