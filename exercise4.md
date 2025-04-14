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
    version: 3.9.6
  nbformat: 4
  nbformat_minor: 2
---

::: {.cell .markdown}
# Exercise 4. - Data Cleansing {#exercise-4---data-cleansing}
:::

::: {.cell .markdown}
## Handling Missing Values in Python
:::

::: {.cell .markdown}
![Missing values](missing.png)
:::

::: {.cell .markdown}
Real world data is messy and often contains a lot of missing values.

There could be multiple reasons for the missing values but primarily the
reason for missingness can be attributed to:

          Reason for missing Data
  ----------------------------------------
            Data doesn\'t exist
   Data not collected due to human error.
          Data deleted accidently
:::

::: {.cell .markdown}
## A guide to handling missing values

Please read this tutorial on handling missing values first, before
working on dirty data this week: [TUTORIAL](a_guide_to_na.ipynb).
:::

::: {.cell .markdown}
# Dirty data
:::

::: {.cell .code execution_count="38"}
``` python
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestClassifier
import warnings
import ssl
# Suppress warnings
warnings.filterwarnings('ignore')
# Disable SSL verification
ssl._create_default_https_context = ssl._create_unverified_context
import requests
from io import StringIO
```
:::

::: {.cell .markdown}
Load the dataset from the provided URL using pandas.
:::

::: {.cell .code execution_count="39"}
``` python
url = "https://raw.github.com/edwindj/datacleaning/master/data/dirty_iris.csv"
response = requests.get(url, verify=False)
data = StringIO(response.text)
dirty_iris = pd.read_csv(data, sep=",")
print(dirty_iris.head())
```

::: {.output .stream .stdout}
       Sepal.Length  Sepal.Width  Petal.Length  Petal.Width     Species
    0           6.4          3.2           4.5          1.5  versicolor
    1           6.3          3.3           6.0          2.5   virginica
    2           6.2          NaN           5.4          2.3   virginica
    3           5.0          3.4           1.6          0.4      setosa
    4           5.7          2.6           3.5          1.0  versicolor
:::
:::

::: {.cell .markdown}
## Introduce Missing Values

Randomly introduce missing values into the dataset to mimic the Python
code behavior.
:::

::: {.cell .code execution_count="40"}
``` python
# Load additional data
carseats = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/Carseats.csv")

# Set random seed for reproducibility
np.random.seed(123)

# Introduce missing values in 'Income' column
income_missing_indices = np.random.choice(carseats.index, size=20, replace=False)
carseats.loc[income_missing_indices, 'Income'] = np.nan

# Set another random seed for reproducibility
np.random.seed(456)

# Introduce missing values in 'Urban' column
urban_missing_indices = np.random.choice(carseats.index, size=10, replace=False)
carseats.loc[urban_missing_indices, 'Urban'] = np.nan

```
:::

::: {.cell .markdown}
# Introduction

Analysis of data is a process of inspecting, cleaning, transforming, and
modeling data with the goal of highlighting useful information,
suggesting conclusions and supporting decision making.
:::

::: {.cell .markdown}
![Descriptive Statistics](images/ds.png)
:::

::: {.cell .markdown}
Many times in the beginning we spend hours on handling problems with
missing values, logical inconsistencies or outliers in our datasets. In
this tutorial we will go through the most popular techniques in data
cleansing.
:::

::: {.cell .markdown}
We will be working with the messy dataset `iris`. Originally published
at UCI Machine Learning Repository: Iris Data Set, this small dataset
from 1936 is often used for testing out machine learning algorithms and
visualizations. Each row of the table represents an iris flower,
including its species and dimensions of its botanical parts, sepal and
petal, in centimeters.

Take a look at this dataset here:
:::

::: {.cell .code execution_count="41"}
``` python
dirty_iris
```

::: {.output .execute_result execution_count="41"}
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
      <th>Sepal.Length</th>
      <th>Sepal.Width</th>
      <th>Petal.Length</th>
      <th>Petal.Width</th>
      <th>Species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6.4</td>
      <td>3.2</td>
      <td>4.5</td>
      <td>1.5</td>
      <td>versicolor</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6.3</td>
      <td>3.3</td>
      <td>6.0</td>
      <td>2.5</td>
      <td>virginica</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6.2</td>
      <td>NaN</td>
      <td>5.4</td>
      <td>2.3</td>
      <td>virginica</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5.0</td>
      <td>3.4</td>
      <td>1.6</td>
      <td>0.4</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.7</td>
      <td>2.6</td>
      <td>3.5</td>
      <td>1.0</td>
      <td>versicolor</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>145</th>
      <td>6.7</td>
      <td>3.1</td>
      <td>5.6</td>
      <td>2.4</td>
      <td>virginica</td>
    </tr>
    <tr>
      <th>146</th>
      <td>5.6</td>
      <td>3.0</td>
      <td>4.5</td>
      <td>1.5</td>
      <td>versicolor</td>
    </tr>
    <tr>
      <th>147</th>
      <td>5.2</td>
      <td>3.5</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>148</th>
      <td>6.4</td>
      <td>3.1</td>
      <td>NaN</td>
      <td>1.8</td>
      <td>virginica</td>
    </tr>
    <tr>
      <th>149</th>
      <td>5.8</td>
      <td>2.6</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>versicolor</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 5 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
## Detecting NA

A missing value, represented by NaN in Python, is a placeholder for a
datum of which the type is known but its value isn\'t. Therefore, it is
impossible to perform statistical analysis on data where one or more
values in the data are missing. One may choose to either omit elements
from a dataset that contain missing values or to impute a value, but
missingness is something to be dealt with prior to any analysis.
:::

::: {.cell .markdown}
![Descriptive Statistics](images/ds.png)
:::

::: {.cell .markdown}
Can you see that many values in our dataset have status NaN = Not
Available? Count (or plot), how many (%) of all 150 rows is complete.
:::

::: {.cell .code execution_count="42"}
``` python
# Count the number of complete cases (rows without any missing values)
complete_cases = dirty_iris.dropna().shape[0]

# Calculate the percentage of complete cases
percentage_complete = (complete_cases / dirty_iris.shape[0]) * 100

print(f"Number of complete cases: {complete_cases}")
print(f"Percentage of complete cases: {percentage_complete:.2f}%")
```

::: {.output .stream .stdout}
    Number of complete cases: 96
    Percentage of complete cases: 64.00%
:::
:::

::: {.cell .markdown}
Does the data contain other special values? If it does, replace them
with NA.
:::

::: {.cell .code execution_count="43"}
``` python
# Define a function to check for special values
def is_special(x):
    if np.issubdtype(x.dtype, np.number):
        return ~np.isfinite(x)
    else:
        return pd.isna(x)

# Apply the function to each column and replace special values with NaN
for col in dirty_iris.columns:
    dirty_iris[col] = dirty_iris[col].apply(lambda x: np.nan if is_special(pd.Series([x]))[0] else x)

# Display summary of the data
print(dirty_iris.describe(include='all'))
```

::: {.output .stream .stdout}
            Sepal.Length  Sepal.Width  Petal.Length  Petal.Width     Species
    count     140.000000   133.000000    131.000000   137.000000         150
    unique           NaN          NaN           NaN          NaN           3
    top              NaN          NaN           NaN          NaN  versicolor
    freq             NaN          NaN           NaN          NaN          50
    mean        6.559286     3.390977      4.449962     1.207299         NaN
    std         6.800940     3.315310      5.769299     0.764722         NaN
    min         0.000000    -3.000000      0.000000     0.100000         NaN
    25%         5.100000     2.800000      1.600000     0.300000         NaN
    50%         5.750000     3.000000      4.500000     1.300000         NaN
    75%         6.400000     3.300000      5.100000     1.800000         NaN
    max        73.000000    30.000000     63.000000     2.500000         NaN
:::
:::

::: {.cell .markdown}
## Checking consistency

Consistent data are technically correct data that are fit for
statistical analysis. They are data in which missing values, special
values, (obvious) errors and outliers are either removed, corrected or
imputed. The data are consistent with constraints based on real-world
knowledge about the subject that the data describe.
:::

::: {.cell .markdown}
![Iris](images/iris.png)
:::

::: {.cell .markdown}
We have the following background knowledge:

-   Species should be one of the following values: setosa, versicolor or
    virginica.

-   All measured numerical properties of an iris should be positive.

-   The petal length of an iris is at least 2 times its petal width.

-   The sepal length of an iris cannot exceed 30 cm.

-   The sepals of an iris are longer than its petals.

Define these rules in a separate object \'RULES\' and read them into
Python. Print the resulting constraint object.
:::

::: {.cell .code execution_count="44"}
``` python
# Define the rules as functions
def check_rules(df):
    rules = {
        "Sepal.Length <= 30": df["Sepal.Length"] <= 30,
        "Species in ['setosa', 'versicolor', 'virginica']": df["Species"].isin(['setosa', 'versicolor', 'virginica']),
        "Sepal.Length > 0": df["Sepal.Length"] > 0,
        "Sepal.Width > 0": df["Sepal.Width"] > 0,
        "Petal.Length > 0": df["Petal.Length"] > 0,
        "Petal.Width > 0": df["Petal.Width"] > 0,
        "Petal.Length >= 2 * Petal.Width": df["Petal.Length"] >= 2 * df["Petal.Width"],
        "Sepal.Length > Petal.Length": df["Sepal.Length"] > df["Petal.Length"]
    }
    return rules

# Apply the rules to the dataframe
rules = check_rules(dirty_iris)

# Print the rules
for rule, result in rules.items():
    print(f"{rule}: {result.all()}")
```

::: {.output .stream .stdout}
    Sepal.Length <= 30: False
    Species in ['setosa', 'versicolor', 'virginica']: True
    Sepal.Length > 0: False
    Sepal.Width > 0: False
    Petal.Length > 0: False
    Petal.Width > 0: False
    Petal.Length >= 2 * Petal.Width: False
    Sepal.Length > Petal.Length: False
:::
:::

::: {.cell .markdown}
Now we are ready to determine how often each rule is broken
(violations). Also we can summarize and plot the result.
:::

::: {.cell .code execution_count="45"}
``` python
# Check for rule violations
violations = {rule: ~result for rule, result in rules.items()}

# Summarize the violations
summary = {rule: result.sum() for rule, result in violations.items()}

# Print the summary of violations
print("Summary of Violations:")
for rule, count in summary.items():
    print(f"{rule}: {count} violations")
```

::: {.output .stream .stdout}
    Summary of Violations:
    Sepal.Length <= 30: 12 violations
    Species in ['setosa', 'versicolor', 'virginica']: 0 violations
    Sepal.Length > 0: 11 violations
    Sepal.Width > 0: 19 violations
    Petal.Length > 0: 20 violations
    Petal.Width > 0: 13 violations
    Petal.Length >= 2 * Petal.Width: 34 violations
    Sepal.Length > Petal.Length: 30 violations
:::
:::

::: {.cell .markdown}
What percentage of the data has no errors?
:::

::: {.cell .code execution_count="46"}
``` python
import matplotlib.pyplot as plt
# Plot the violations
violation_counts = pd.Series(summary)
ax = violation_counts.plot(kind='bar', figsize=(10, 6))
plt.title('Summary of Rule Violations')
plt.xlabel('Rules')
plt.ylabel('Number of Violations')

# Add percentage labels above the bars
for p in ax.patches:
    ax.annotate(f'{p.get_height() / len(dirty_iris) * 100:.1f}%', 
                (p.get_x() + p.get_width() / 2., p.get_height()), 
                ha='center', va='center', xytext=(0, 10), 
                textcoords='offset points')

plt.show()
```

::: {.output .display_data}
![](vertopal_3fd1402d95714cb9b5ddc6297e1a74bf/51fd9c191a3b9d0df6175778c893d4690b755264.png)
:::
:::

::: {.cell .markdown}
Find out which observations have too long sepals using the result of
violations.
:::

::: {.cell .code execution_count="47"}
``` python
# Check for rule violations
violations = {rule: ~result for rule, result in rules.items()}
# Combine violations into a DataFrame
violated_df = pd.DataFrame(violations)
violated_rows = dirty_iris[violated_df["Sepal.Length <= 30"]]
print(violated_rows)
```

::: {.output .stream .stdout}
         Sepal.Length  Sepal.Width  Petal.Length  Petal.Width     Species
    14            NaN          3.9          1.70          0.4      setosa
    18            NaN          4.0           NaN          0.2      setosa
    24            NaN          3.0          5.90          2.1   virginica
    27           73.0         29.0         63.00          NaN   virginica
    29            NaN          2.8          0.82          1.3  versicolor
    57            NaN          2.9          4.50          1.5  versicolor
    67            NaN          3.2          5.70          2.3   virginica
    113           NaN          3.3          5.70          2.1   virginica
    118           NaN          3.0          5.50          2.1   virginica
    119           NaN          2.8          4.70          1.2  versicolor
    124          49.0         30.0         14.00          2.0      setosa
    137           NaN          3.0          4.90          1.8   virginica
:::
:::

::: {.cell .markdown}
Find outliers in sepal length using boxplot approach. Retrieve the
corresponding observations and look at the other values. Any ideas what
might have happened? Set the outliers to NA (or a value that you find
more appropiate)
:::

::: {.cell .code execution_count="48"}
``` python
# Boxplot for Sepal.Length
plt.figure(figsize=(10, 6))
plt.boxplot(dirty_iris['Sepal.Length'].dropna())
plt.title('Boxplot of Sepal Length')
plt.ylabel('Sepal Length')
plt.show()
```

::: {.output .display_data}
![](vertopal_3fd1402d95714cb9b5ddc6297e1a74bf/f890077316f3886af42f538b57cfaa2f7c31b7ea.png)
:::
:::

::: {.cell .code execution_count="49"}
``` python
# Find outliers in Sepal.Length
outliers = dirty_iris['Sepal.Length'][np.abs(dirty_iris['Sepal.Length'] - dirty_iris['Sepal.Length'].mean()) > (1.5 * dirty_iris['Sepal.Length'].std())]
outliers_idx = dirty_iris.index[dirty_iris['Sepal.Length'].isin(outliers)]

# Print the rows with outliers
print("Outliers:")
print(dirty_iris.loc[outliers_idx])
```

::: {.output .stream .stdout}
    Outliers:
         Sepal.Length  Sepal.Width  Petal.Length  Petal.Width    Species
    27           73.0         29.0          63.0          NaN  virginica
    124          49.0         30.0          14.0          2.0     setosa
:::
:::

::: {.cell .markdown}
They all seem to be too big\... may they were measured in mm i.o cm?
:::

::: {.cell .code execution_count="50"}
``` python
# Adjust the outliers (assuming they were measured in mm instead of cm)
dirty_iris.loc[outliers_idx, ['Sepal.Length', 'Sepal.Width', 'Petal.Length', 'Petal.Width']] /= 10

# Summary of the adjusted data
print("Summary of adjusted data:")
print(dirty_iris.describe())
```

::: {.output .stream .stdout}
    Summary of adjusted data:
           Sepal.Length  Sepal.Width  Petal.Length  Petal.Width
    count    140.000000   133.000000    131.000000   137.000000
    mean       5.775000     2.991729      3.920954     1.194161
    std        0.969842     0.708075      2.455417     0.766463
    min        0.000000    -3.000000      0.000000     0.100000
    25%        5.100000     2.800000      1.600000     0.300000
    50%        5.700000     3.000000      4.400000     1.300000
    75%        6.400000     3.300000      5.100000     1.800000
    max        7.900000     4.200000     23.000000     2.500000
:::
:::

::: {.cell .markdown}
Note that simple boxplot shows an extra outlier!
:::

::: {.cell .code execution_count="51"}
``` python
import seaborn as sns
plt.figure(figsize=(10, 6))
sns.boxplot(x='Species', y='Sepal.Length', data=dirty_iris)
plt.title('Boxplot of Sepal Length by Species')
plt.xlabel('Species')
plt.ylabel('Sepal Length')
plt.show()
```

::: {.output .display_data}
![](vertopal_3fd1402d95714cb9b5ddc6297e1a74bf/8018761abdfa2290ed8e3dc54999f652cbb4db51.png)
:::
:::

::: {.cell .markdown}
## Correcting

Replace non positive values from Sepal.Width with NA:
:::

::: {.cell .code execution_count="52"}
``` python
# Define the correction rule
def correct_sepal_width(df):
    df.loc[(~df['Sepal.Width'].isna()) & (df['Sepal.Width'] <= 0), 'Sepal.Width'] = np.nan
    return df

# Apply the correction rule to the dataframe
mydata_corrected = correct_sepal_width(dirty_iris)

# Print the corrected dataframe
print(mydata_corrected)
```

::: {.output .stream .stdout}
         Sepal.Length  Sepal.Width  Petal.Length  Petal.Width     Species
    0             6.4          3.2           4.5          1.5  versicolor
    1             6.3          3.3           6.0          2.5   virginica
    2             6.2          NaN           5.4          2.3   virginica
    3             5.0          3.4           1.6          0.4      setosa
    4             5.7          2.6           3.5          1.0  versicolor
    ..            ...          ...           ...          ...         ...
    145           6.7          3.1           5.6          2.4   virginica
    146           5.6          3.0           4.5          1.5  versicolor
    147           5.2          3.5           1.5          0.2      setosa
    148           6.4          3.1           NaN          1.8   virginica
    149           5.8          2.6           4.0          NaN  versicolor

    [150 rows x 5 columns]
:::
:::

::: {.cell .markdown}
Replace all erroneous values with NA using (the result of)
localizeErrors:
:::

::: {.cell .code execution_count="53"}
``` python
# Apply the rules to the dataframe
rules = check_rules(dirty_iris)
violations = {rule: ~result for rule, result in rules.items()}
violated_df = pd.DataFrame(violations)

# Localize errors and set them to NA
for col in violated_df.columns:
    dirty_iris.loc[violated_df[col], col.split()[0]] = np.nan
```
:::

::: {.cell .markdown}
## NA\'s pattern detection

Here we are going to use **missingno** library to diagnose the
missingness pattern for the \'dirty_iris\' dataset.
:::

::: {.cell .code execution_count="54"}
``` python
import missingno as msno
```
:::

::: {.cell .markdown}
### Matrix Plot (msno.matrix): {#matrix-plot-msnomatrix}

This visualization shows which values are missing in each column. Each
bar represents a column, and white spaces in the bars indicate missing
values.

If you see many white spaces in one column, it means that column has a
lot of missing data. If the white spaces are randomly scattered, the
missing data might be random. If they are clustered in specific areas,
it might indicate a pattern.
:::

::: {.cell .code execution_count="55"}
``` python
msno.matrix(dirty_iris);
```

::: {.output .display_data}
![](vertopal_3fd1402d95714cb9b5ddc6297e1a74bf/a01264e083f24cfb97f3c116f993b5528f72532b.png)
:::
:::

::: {.cell .markdown}
### Heatmap Plot (msno.heatmap): {#heatmap-plot-msnoheatmap}

This visualization shows the correlations between missing values in
different columns. If two columns have a high correlation (dark colors),
it means that if one column has missing values, the other column is also
likely to have missing values.

Low correlations (light colors) indicate that missing values in one
column are not related to missing values in another column.
:::

::: {.cell .code execution_count="56"}
``` python
msno.heatmap(dirty_iris);
```

::: {.output .display_data}
![](vertopal_3fd1402d95714cb9b5ddc6297e1a74bf/b7ba33fd65592ebbb7bfe45f294813adf362bb50.png)
:::
:::

::: {.cell .markdown}
### Dendrogram Plot (msno.dendrogram): {#dendrogram-plot-msnodendrogram}

This visualization groups columns based on the similarity of their
missing data patterns. Columns that are close to each other in the
dendrogram have similar patterns of missing data.

This can help identify groups of columns that have similar issues with
missing data.

Based on these visualizations, we can identify which columns have the
most missing data, whether the missing data is random or patterned, and
which columns have similar patterns of missing data.
:::

::: {.cell .code execution_count="57"}
``` python
msno.dendrogram(dirty_iris);
```

::: {.output .display_data}
![](vertopal_3fd1402d95714cb9b5ddc6297e1a74bf/4caafb83011fec00ea2891cc8cd9f115f821cfe7.png)
:::
:::

::: {.cell .markdown}
*Based on the dendrogram plot, we can interpret the pattern of missing
data in the \"dirty iris\" dataset as follows:*

**Grouping of Columns:**

The dendrogram shows that the columns \"Species\" and \"Petal.Width\"
are grouped together, indicating that they have similar patterns of
missing data.

Similarly, \"Sepal.Width\" and \"Petal.Length\" are grouped together,
suggesting they also share a similar pattern of missing data.

\"Sepal.Length\" is somewhat separate from the other groups, indicating
it has a different pattern of missing data compared to the other
columns.

**Pattern of Missing Data:**

The grouping suggests that missing data in \"Species\" is likely to be
associated with missing data in \"Petal.Width\".

Similarly, missing data in \"Sepal.Width\" is likely to be associated
with missing data in \"Petal.Length\".

\"Sepal.Length\" appears to have a distinct pattern of missing data that
is not strongly associated with the other columns.

*From this dendrogram, we can infer that the missing data is not
completely random. Instead, there are specific patterns where certain
columns tend to have missing data together. This indicates a systematic
pattern of missing data rather than a purely random one.*
:::

::: {.cell .markdown}
## Imputing NA\'s
:::

::: {.cell .markdown}
Imputation is the process of estimating or deriving values for fields
where data is missing. There is a vast body of literature on imputation
methods and it goes beyond the scope of this tutorial to discuss all of
them.

There is no one single best imputation method that works in all cases.
The imputation model of choice depends on what auxiliary information is
available and whether there are (multivariate) edit restrictions on the
data to be imputed.

The availability of Python software for imputation under edit
restrictions is, to our best knowledge, limited. However, a viable
strategy for imputing numerical data is to first impute missing values
without restrictions, and then minimally adjust the imputed values so
that the restrictions are obeyed. Separately, these methods are
available in Python.

We can mention several approaches to imputation:

1.  For the **quantitative** variables:

-   imputing by **mean**/**median**/**mode**

-   **hotdeck** imputation

-   **KNN** \-- K-nearest-neighbors approach

-   **RPART** \-- random forests multivariate approach

-   **mice** - Multivariate Imputation by Chained Equations approach

1.  For the **qualitative** variables:

-   imputing by **mode**

-   **RPART** \-- random forests multivariate approach

-   **mice** - Multivariate Imputation by Chained Equations approach

    \... and many others. Please read the theoretical background if you
    are interested in those techniques.
:::

::: {.cell .markdown}
***Exercise 1.*** Use ***kNN*** imputation (\'sklearn\' package) to
impute all missing values. The KNNImputer from sklearn requires all data
to be numeric. Since our dataset contains categorical data (e.g., the
Species column), you need to handle these columns separately. One
approach is to use one-hot encoding for categorical variables before
applying the imputer.
:::

::: {.cell .code execution_count="58"}
``` python
from sklearn.impute import KNNImputer
from sklearn.preprocessing import OneHotEncoder
# Replace infinite values with NaN
dirty_iris.replace([np.inf, -np.inf], np.nan, inplace=True)

# Separate numeric and categorical columns
numeric_cols = dirty_iris.select_dtypes(include=[np.number]).columns
categorical_cols = dirty_iris.select_dtypes(exclude=[np.number]).columns
# One-hot encode categorical columns
encoder = OneHotEncoder(sparse_output=False, handle_unknown='ignore')

encoded_categorical = pd.DataFrame(encoder.fit_transform(dirty_iris[categorical_cols]), columns=encoder.get_feature_names_out(categorical_cols))

# Combine numeric and encoded categorical columns
combined_data = pd.concat([dirty_iris[numeric_cols], encoded_categorical], axis=1)

# Initialize the KNNImputer
imputer = KNNImputer(n_neighbors=3)

# Perform kNN imputation
imputed_data = imputer.fit_transform(combined_data)

# Convert the imputed data back to a DataFrame
imputed_df = pd.DataFrame(imputed_data, columns=combined_data.columns)

# Decode the one-hot encoded columns back to original categorical columns
decoded_categorical = pd.DataFrame(encoder.inverse_transform(imputed_df[encoded_categorical.columns]), columns=categorical_cols)

# Combine numeric and decoded categorical columns
final_imputed_data = pd.concat([imputed_df[numeric_cols], decoded_categorical], axis=1)

# Print the imputed data
print(final_imputed_data)
```

::: {.output .stream .stdout}
         Sepal.Length  Sepal.Width  Petal.Length  Petal.Width     Species
    0        6.400000     3.200000      4.500000     1.500000  versicolor
    1        6.300000     3.300000      6.000000     2.500000   virginica
    2        6.200000     3.033333      5.400000     2.300000   virginica
    3        5.000000     3.400000      1.600000     0.400000      setosa
    4        5.700000     2.600000      3.500000     1.000000  versicolor
    ..            ...          ...           ...          ...         ...
    145      6.700000     3.100000      5.600000     2.400000   virginica
    146      5.600000     3.000000      4.500000     1.500000  versicolor
    147      5.200000     3.500000      1.500000     0.200000      setosa
    148      6.533333     3.100000      5.166667     1.800000   virginica
    149      5.800000     2.600000      3.833333     1.066667  versicolor

    [150 rows x 5 columns]
:::
:::

::: {.cell .markdown}
## Transformations

Finally, we sometimes encounter the situation where we have problems
with skewed distributions or we just want to transform, recode or
perform discretization. Let\'s review some of the most popular
transformation methods.

First, standardization (also known as normalization):

-   **Z-score** approach - standardization procedure, using the formula:
    $z=\frac{x-\mu}{\sigma}$ where $\mu$ = mean and $\sigma$ = standard
    deviation. Z-scores are also known as standardized scores; they are
    scores (or data values) that have been given a common *standard*.
    This standard is a mean of zero and a standard deviation of 1.

-   **minmax** approach - An alternative approach to Z-score
    normalization (or standardization) is the so-called MinMax scaling
    (often also simply called \"normalization\" - a common cause for
    ambiguities). In this approach, the data is scaled to a fixed
    range - usually 0 to 1. The cost of having this bounded range - in
    contrast to standardization - is that we will end up with smaller
    standard deviations, which can suppress the effect of outliers. If
    you would like to perform MinMax scaling - simply substract minimum
    value and divide it by range:$(x-min)/(max-min)$

In order to solve problems with very skewed distributions we can also
use several types of simple transformations:

-   log
-   log+1
-   sqrt
-   x\^2
-   x\^3
:::

::: {.cell .markdown}
***Exercise 2.*** Standardize incomes and present the transformed
distribution of incomes on boxplot.
:::

::: {.cell .code execution_count="59"}
``` python
# your code goes here
df_clean = carseats.dropna(subset=['Income'])
df_clean['Income_standardized'] = (df_clean['Income'] - df_clean['Income'].mean()) / df_clean['Income'].std()
plt.figure(figsize=(8, 5))
sns.boxplot(data=df_clean, x='Income_standardized', orient='h', color='skyblue')
plt.title('Standardized Income Distribution')
plt.xlabel('Z-Score')
plt.grid(axis='x', linestyle='--')
plt.show()
```

::: {.output .display_data}
![](vertopal_3fd1402d95714cb9b5ddc6297e1a74bf/0b2ad743cc5452c06fce9ce732d2e15682863ce3.png)
:::
:::

::: {.cell .markdown}
## Binning

Sometimes we just would like to perform so called \'binning\' procedure
to be able to analyze our categorical data, to compare several
categorical variables, to construct statistical models etc. Thanks to
the \'binning\' function we can transform quantitative variables into
categorical using several methods:

-   **quantile** - automatic binning by quantile of its distribution

-   **equal** - binning to achieve fixed length of intervals

-   **pretty** - a compromise between the 2 mentioned above

-   **kmeans** - categorization using the K-Means algorithm

-   **bclust** - categorization using the bagged clustering algorithm
:::

::: {.cell .markdown}
**Exercise 3.** Using quantile approach perform binning of the variable
\'Income\'.
:::

::: {.cell .code execution_count="60"}
``` python
np.random.seed(123)
missing_indices = np.random.choice(carseats.index, size=20, replace=False)
carseats.loc[missing_indices, 'Income'] = np.nan

carseats['Income'] = carseats['Income'].fillna(carseats['Income'].mean())

carseats['Income_binned_quantiles'] = pd.qcut(carseats['Income'], q=4, labels=["Low", "Medium", "High", "Very High"])

print(carseats['Income_binned_quantiles'].value_counts())
```

::: {.output .stream .stdout}
    Income_binned_quantiles
    Medium       105
    Low          100
    Very High    100
    High          95
    Name: count, dtype: int64
:::
:::

::: {.cell .markdown}
**Exercise 4.** Recode the original distribution of incomes using fixed
length of intervals and assign them labels.
:::

::: {.cell .code execution_count="61"}
``` python
labels = ['Very Low', 'Low', 'Medium', 'High']
carseats['Income_binned_equal'] = pd.cut(carseats['Income'], bins=4, labels=labels)
print(carseats['Income_binned_equal'].value_counts())
```

::: {.output .stream .stdout}
    Income_binned_equal
    Low         115
    Very Low    104
    Medium      103
    High         78
    Name: count, dtype: int64
:::
:::

::: {.cell .markdown}
In case of statistical modeling (i.e. credit scoring purposes) - we need
to be aware of the fact, that the ***optimal*** discretization of the
original distribution must be achieved. The \'*binning_by*\' function
comes with some help here.
:::

::: {.cell .markdown}
## Optimal binning with binary target
:::

::: {.cell .markdown}
**Exercise 5.** Perform discretization of the variable \'Advertising\'
using optimal binning.
:::

::: {.cell .code execution_count="62"}
``` python
from optbinning import OptimalBinning
from sklearn.datasets import load_breast_cancer

data = load_breast_cancer()
df = pd.DataFrame(data.data, columns=data.feature_names)
print(df.head())
```

::: {.output .stream .stdout}
       mean radius  mean texture  mean perimeter  mean area  mean smoothness  \
    0        17.99         10.38          122.80     1001.0          0.11840   
    1        20.57         17.77          132.90     1326.0          0.08474   
    2        19.69         21.25          130.00     1203.0          0.10960   
    3        11.42         20.38           77.58      386.1          0.14250   
    4        20.29         14.34          135.10     1297.0          0.10030   

       mean compactness  mean concavity  mean concave points  mean symmetry  \
    0           0.27760          0.3001              0.14710         0.2419   
    1           0.07864          0.0869              0.07017         0.1812   
    2           0.15990          0.1974              0.12790         0.2069   
    3           0.28390          0.2414              0.10520         0.2597   
    4           0.13280          0.1980              0.10430         0.1809   

       mean fractal dimension  ...  worst radius  worst texture  worst perimeter  \
    0                 0.07871  ...         25.38          17.33           184.60   
    1                 0.05667  ...         24.99          23.41           158.80   
    2                 0.05999  ...         23.57          25.53           152.50   
    3                 0.09744  ...         14.91          26.50            98.87   
    4                 0.05883  ...         22.54          16.67           152.20   

       worst area  worst smoothness  worst compactness  worst concavity  \
    0      2019.0            0.1622             0.6656           0.7119   
    1      1956.0            0.1238             0.1866           0.2416   
    2      1709.0            0.1444             0.4245           0.4504   
    3       567.7            0.2098             0.8663           0.6869   
    4      1575.0            0.1374             0.2050           0.4000   

       worst concave points  worst symmetry  worst fractal dimension  
    0                0.2654          0.4601                  0.11890  
    1                0.1860          0.2750                  0.08902  
    2                0.2430          0.3613                  0.08758  
    3                0.2575          0.6638                  0.17300  
    4                0.1625          0.2364                  0.07678  

    [5 rows x 30 columns]
:::
:::

::: {.cell .markdown}
We choose a variable to discretize and the binary target.
:::

::: {.cell .code execution_count="63"}
``` python
variable = "mean radius"
x = df[variable].values
y = data.target
```
:::

::: {.cell .markdown}
Import and instantiate an OptimalBinning object class. We pass the
variable name, its data type, and a solver, in this case, we choose the
constraint programming solver.
:::

::: {.cell .code execution_count="64"}
``` python
optb = OptimalBinning(name=variable, dtype="numerical", solver="cp")
```
:::

::: {.cell .markdown}
We fit the optimal binning object with arrays x and y.
:::

::: {.cell .code execution_count="65"}
``` python
optb.fit(x, y)
```

::: {.output .execute_result execution_count="65"}
```{=html}
<style>#sk-container-id-2 {
  /* Definition of color scheme common for light and dark mode */
  --sklearn-color-text: #000;
  --sklearn-color-text-muted: #666;
  --sklearn-color-line: gray;
  /* Definition of color scheme for unfitted estimators */
  --sklearn-color-unfitted-level-0: #fff5e6;
  --sklearn-color-unfitted-level-1: #f6e4d2;
  --sklearn-color-unfitted-level-2: #ffe0b3;
  --sklearn-color-unfitted-level-3: chocolate;
  /* Definition of color scheme for fitted estimators */
  --sklearn-color-fitted-level-0: #f0f8ff;
  --sklearn-color-fitted-level-1: #d4ebff;
  --sklearn-color-fitted-level-2: #b3dbfd;
  --sklearn-color-fitted-level-3: cornflowerblue;

  /* Specific color for light theme */
  --sklearn-color-text-on-default-background: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, black)));
  --sklearn-color-background: var(--sg-background-color, var(--theme-background, var(--jp-layout-color0, white)));
  --sklearn-color-border-box: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, black)));
  --sklearn-color-icon: #696969;

  @media (prefers-color-scheme: dark) {
    /* Redefinition of color scheme for dark theme */
    --sklearn-color-text-on-default-background: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, white)));
    --sklearn-color-background: var(--sg-background-color, var(--theme-background, var(--jp-layout-color0, #111)));
    --sklearn-color-border-box: var(--sg-text-color, var(--theme-code-foreground, var(--jp-content-font-color1, white)));
    --sklearn-color-icon: #878787;
  }
}

#sk-container-id-2 {
  color: var(--sklearn-color-text);
}

#sk-container-id-2 pre {
  padding: 0;
}

#sk-container-id-2 input.sk-hidden--visually {
  border: 0;
  clip: rect(1px 1px 1px 1px);
  clip: rect(1px, 1px, 1px, 1px);
  height: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  width: 1px;
}

#sk-container-id-2 div.sk-dashed-wrapped {
  border: 1px dashed var(--sklearn-color-line);
  margin: 0 0.4em 0.5em 0.4em;
  box-sizing: border-box;
  padding-bottom: 0.4em;
  background-color: var(--sklearn-color-background);
}

#sk-container-id-2 div.sk-container {
  /* jupyter's `normalize.less` sets `[hidden] { display: none; }`
     but bootstrap.min.css set `[hidden] { display: none !important; }`
     so we also need the `!important` here to be able to override the
     default hidden behavior on the sphinx rendered scikit-learn.org.
     See: https://github.com/scikit-learn/scikit-learn/issues/21755 */
  display: inline-block !important;
  position: relative;
}

#sk-container-id-2 div.sk-text-repr-fallback {
  display: none;
}

div.sk-parallel-item,
div.sk-serial,
div.sk-item {
  /* draw centered vertical line to link estimators */
  background-image: linear-gradient(var(--sklearn-color-text-on-default-background), var(--sklearn-color-text-on-default-background));
  background-size: 2px 100%;
  background-repeat: no-repeat;
  background-position: center center;
}

/* Parallel-specific style estimator block */

#sk-container-id-2 div.sk-parallel-item::after {
  content: "";
  width: 100%;
  border-bottom: 2px solid var(--sklearn-color-text-on-default-background);
  flex-grow: 1;
}

#sk-container-id-2 div.sk-parallel {
  display: flex;
  align-items: stretch;
  justify-content: center;
  background-color: var(--sklearn-color-background);
  position: relative;
}

#sk-container-id-2 div.sk-parallel-item {
  display: flex;
  flex-direction: column;
}

#sk-container-id-2 div.sk-parallel-item:first-child::after {
  align-self: flex-end;
  width: 50%;
}

#sk-container-id-2 div.sk-parallel-item:last-child::after {
  align-self: flex-start;
  width: 50%;
}

#sk-container-id-2 div.sk-parallel-item:only-child::after {
  width: 0;
}

/* Serial-specific style estimator block */

#sk-container-id-2 div.sk-serial {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: var(--sklearn-color-background);
  padding-right: 1em;
  padding-left: 1em;
}


/* Toggleable style: style used for estimator/Pipeline/ColumnTransformer box that is
clickable and can be expanded/collapsed.
- Pipeline and ColumnTransformer use this feature and define the default style
- Estimators will overwrite some part of the style using the `sk-estimator` class
*/

/* Pipeline and ColumnTransformer style (default) */

#sk-container-id-2 div.sk-toggleable {
  /* Default theme specific background. It is overwritten whether we have a
  specific estimator or a Pipeline/ColumnTransformer */
  background-color: var(--sklearn-color-background);
}

/* Toggleable label */
#sk-container-id-2 label.sk-toggleable__label {
  cursor: pointer;
  display: flex;
  width: 100%;
  margin-bottom: 0;
  padding: 0.5em;
  box-sizing: border-box;
  text-align: center;
  align-items: start;
  justify-content: space-between;
  gap: 0.5em;
}

#sk-container-id-2 label.sk-toggleable__label .caption {
  font-size: 0.6rem;
  font-weight: lighter;
  color: var(--sklearn-color-text-muted);
}

#sk-container-id-2 label.sk-toggleable__label-arrow:before {
  /* Arrow on the left of the label */
  content: "▸";
  float: left;
  margin-right: 0.25em;
  color: var(--sklearn-color-icon);
}

#sk-container-id-2 label.sk-toggleable__label-arrow:hover:before {
  color: var(--sklearn-color-text);
}

/* Toggleable content - dropdown */

#sk-container-id-2 div.sk-toggleable__content {
  max-height: 0;
  max-width: 0;
  overflow: hidden;
  text-align: left;
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-0);
}

#sk-container-id-2 div.sk-toggleable__content.fitted {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-0);
}

#sk-container-id-2 div.sk-toggleable__content pre {
  margin: 0.2em;
  border-radius: 0.25em;
  color: var(--sklearn-color-text);
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-0);
}

#sk-container-id-2 div.sk-toggleable__content.fitted pre {
  /* unfitted */
  background-color: var(--sklearn-color-fitted-level-0);
}

#sk-container-id-2 input.sk-toggleable__control:checked~div.sk-toggleable__content {
  /* Expand drop-down */
  max-height: 200px;
  max-width: 100%;
  overflow: auto;
}

#sk-container-id-2 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {
  content: "▾";
}

/* Pipeline/ColumnTransformer-specific style */

#sk-container-id-2 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {
  color: var(--sklearn-color-text);
  background-color: var(--sklearn-color-unfitted-level-2);
}

#sk-container-id-2 div.sk-label.fitted input.sk-toggleable__control:checked~label.sk-toggleable__label {
  background-color: var(--sklearn-color-fitted-level-2);
}

/* Estimator-specific style */

/* Colorize estimator box */
#sk-container-id-2 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-2);
}

#sk-container-id-2 div.sk-estimator.fitted input.sk-toggleable__control:checked~label.sk-toggleable__label {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-2);
}

#sk-container-id-2 div.sk-label label.sk-toggleable__label,
#sk-container-id-2 div.sk-label label {
  /* The background is the default theme color */
  color: var(--sklearn-color-text-on-default-background);
}

/* On hover, darken the color of the background */
#sk-container-id-2 div.sk-label:hover label.sk-toggleable__label {
  color: var(--sklearn-color-text);
  background-color: var(--sklearn-color-unfitted-level-2);
}

/* Label box, darken color on hover, fitted */
#sk-container-id-2 div.sk-label.fitted:hover label.sk-toggleable__label.fitted {
  color: var(--sklearn-color-text);
  background-color: var(--sklearn-color-fitted-level-2);
}

/* Estimator label */

#sk-container-id-2 div.sk-label label {
  font-family: monospace;
  font-weight: bold;
  display: inline-block;
  line-height: 1.2em;
}

#sk-container-id-2 div.sk-label-container {
  text-align: center;
}

/* Estimator-specific */
#sk-container-id-2 div.sk-estimator {
  font-family: monospace;
  border: 1px dotted var(--sklearn-color-border-box);
  border-radius: 0.25em;
  box-sizing: border-box;
  margin-bottom: 0.5em;
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-0);
}

#sk-container-id-2 div.sk-estimator.fitted {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-0);
}

/* on hover */
#sk-container-id-2 div.sk-estimator:hover {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-2);
}

#sk-container-id-2 div.sk-estimator.fitted:hover {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-2);
}

/* Specification for estimator info (e.g. "i" and "?") */

/* Common style for "i" and "?" */

.sk-estimator-doc-link,
a:link.sk-estimator-doc-link,
a:visited.sk-estimator-doc-link {
  float: right;
  font-size: smaller;
  line-height: 1em;
  font-family: monospace;
  background-color: var(--sklearn-color-background);
  border-radius: 1em;
  height: 1em;
  width: 1em;
  text-decoration: none !important;
  margin-left: 0.5em;
  text-align: center;
  /* unfitted */
  border: var(--sklearn-color-unfitted-level-1) 1pt solid;
  color: var(--sklearn-color-unfitted-level-1);
}

.sk-estimator-doc-link.fitted,
a:link.sk-estimator-doc-link.fitted,
a:visited.sk-estimator-doc-link.fitted {
  /* fitted */
  border: var(--sklearn-color-fitted-level-1) 1pt solid;
  color: var(--sklearn-color-fitted-level-1);
}

/* On hover */
div.sk-estimator:hover .sk-estimator-doc-link:hover,
.sk-estimator-doc-link:hover,
div.sk-label-container:hover .sk-estimator-doc-link:hover,
.sk-estimator-doc-link:hover {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-3);
  color: var(--sklearn-color-background);
  text-decoration: none;
}

div.sk-estimator.fitted:hover .sk-estimator-doc-link.fitted:hover,
.sk-estimator-doc-link.fitted:hover,
div.sk-label-container:hover .sk-estimator-doc-link.fitted:hover,
.sk-estimator-doc-link.fitted:hover {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-3);
  color: var(--sklearn-color-background);
  text-decoration: none;
}

/* Span, style for the box shown on hovering the info icon */
.sk-estimator-doc-link span {
  display: none;
  z-index: 9999;
  position: relative;
  font-weight: normal;
  right: .2ex;
  padding: .5ex;
  margin: .5ex;
  width: min-content;
  min-width: 20ex;
  max-width: 50ex;
  color: var(--sklearn-color-text);
  box-shadow: 2pt 2pt 4pt #999;
  /* unfitted */
  background: var(--sklearn-color-unfitted-level-0);
  border: .5pt solid var(--sklearn-color-unfitted-level-3);
}

.sk-estimator-doc-link.fitted span {
  /* fitted */
  background: var(--sklearn-color-fitted-level-0);
  border: var(--sklearn-color-fitted-level-3);
}

.sk-estimator-doc-link:hover span {
  display: block;
}

/* "?"-specific style due to the `<a>` HTML tag */

#sk-container-id-2 a.estimator_doc_link {
  float: right;
  font-size: 1rem;
  line-height: 1em;
  font-family: monospace;
  background-color: var(--sklearn-color-background);
  border-radius: 1rem;
  height: 1rem;
  width: 1rem;
  text-decoration: none;
  /* unfitted */
  color: var(--sklearn-color-unfitted-level-1);
  border: var(--sklearn-color-unfitted-level-1) 1pt solid;
}

#sk-container-id-2 a.estimator_doc_link.fitted {
  /* fitted */
  border: var(--sklearn-color-fitted-level-1) 1pt solid;
  color: var(--sklearn-color-fitted-level-1);
}

/* On hover */
#sk-container-id-2 a.estimator_doc_link:hover {
  /* unfitted */
  background-color: var(--sklearn-color-unfitted-level-3);
  color: var(--sklearn-color-background);
  text-decoration: none;
}

#sk-container-id-2 a.estimator_doc_link.fitted:hover {
  /* fitted */
  background-color: var(--sklearn-color-fitted-level-3);
}
</style><div id="sk-container-id-2" class="sk-top-container"><div class="sk-text-repr-fallback"><pre>OptimalBinning(name=&#x27;mean radius&#x27;)</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class="sk-container" hidden><div class="sk-item"><div class="sk-estimator  sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-2" type="checkbox" checked><label for="sk-estimator-id-2" class="sk-toggleable__label  sk-toggleable__label-arrow"><div><div>OptimalBinning</div></div><div><span class="sk-estimator-doc-link ">i<span>Not fitted</span></span></div></label><div class="sk-toggleable__content "><pre>OptimalBinning(name=&#x27;mean radius&#x27;)</pre></div> </div></div></div></div>
```
:::
:::

::: {.cell .markdown}
You can check if an optimal solution has been found via the status
attribute:
:::

::: {.cell .code execution_count="66"}
``` python
optb.status
```

::: {.output .execute_result execution_count="66"}
    'OPTIMAL'
:::
:::

::: {.cell .markdown}
You can also retrieve the optimal split points via the splits attribute:
:::

::: {.cell .code execution_count="67"}
``` python
optb.splits
```

::: {.output .execute_result execution_count="67"}
    array([11.42500019, 12.32999992, 13.09499979, 13.70499992, 15.04500008,
           16.92500019])
:::
:::

::: {.cell .markdown}
The binning table
:::

::: {.cell .markdown}
The optimal binning algorithms return a binning table; a binning table
displays the binned data and several metrics for each bin. Class
OptimalBinning returns an object BinningTable via the binning_table
attribute.
:::

::: {.cell .code execution_count="68"}
``` python
binning_table = optb.binning_table

type(binning_table)
```

::: {.output .execute_result execution_count="68"}
    optbinning.binning.binning_statistics.BinningTable
:::
:::

::: {.cell .markdown}
The binning_table is instantiated, but not built. Therefore, the first
step is to call the method build, which returns a pandas.DataFrame.
:::

::: {.cell .code execution_count="69"}
``` python
binning_table.build()
```

::: {.output .execute_result execution_count="69"}
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
      <th>Bin</th>
      <th>Count</th>
      <th>Count (%)</th>
      <th>Non-event</th>
      <th>Event</th>
      <th>Event rate</th>
      <th>WoE</th>
      <th>IV</th>
      <th>JS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>(-inf, 11.43)</td>
      <td>118</td>
      <td>0.207381</td>
      <td>3</td>
      <td>115</td>
      <td>0.974576</td>
      <td>-3.12517</td>
      <td>0.962483</td>
      <td>0.087205</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[11.43, 12.33)</td>
      <td>79</td>
      <td>0.138840</td>
      <td>3</td>
      <td>76</td>
      <td>0.962025</td>
      <td>-2.710972</td>
      <td>0.538763</td>
      <td>0.052198</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[12.33, 13.09)</td>
      <td>68</td>
      <td>0.119508</td>
      <td>7</td>
      <td>61</td>
      <td>0.897059</td>
      <td>-1.643814</td>
      <td>0.226599</td>
      <td>0.025513</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[13.09, 13.70)</td>
      <td>49</td>
      <td>0.086116</td>
      <td>10</td>
      <td>39</td>
      <td>0.795918</td>
      <td>-0.839827</td>
      <td>0.052131</td>
      <td>0.006331</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[13.70, 15.05)</td>
      <td>83</td>
      <td>0.145870</td>
      <td>28</td>
      <td>55</td>
      <td>0.662651</td>
      <td>-0.153979</td>
      <td>0.003385</td>
      <td>0.000423</td>
    </tr>
    <tr>
      <th>5</th>
      <td>[15.05, 16.93)</td>
      <td>54</td>
      <td>0.094903</td>
      <td>44</td>
      <td>10</td>
      <td>0.185185</td>
      <td>2.002754</td>
      <td>0.359566</td>
      <td>0.038678</td>
    </tr>
    <tr>
      <th>6</th>
      <td>[16.93, inf)</td>
      <td>118</td>
      <td>0.207381</td>
      <td>117</td>
      <td>1</td>
      <td>0.008475</td>
      <td>5.283323</td>
      <td>2.900997</td>
      <td>0.183436</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Special</td>
      <td>0</td>
      <td>0.000000</td>
      <td>0</td>
      <td>0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Missing</td>
      <td>0</td>
      <td>0.000000</td>
      <td>0</td>
      <td>0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>Totals</th>
      <td></td>
      <td>569</td>
      <td>1.000000</td>
      <td>212</td>
      <td>357</td>
      <td>0.627417</td>
      <td></td>
      <td>5.043925</td>
      <td>0.393784</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
Let's describe the columns of this binning table:

Bin: the intervals delimited by the optimal split points.\
Count: the number of records for each bin.\
Count (%): the percentage of records for each bin.\
Non-event: the number of non-event records (𝑦=0) for each bin.\
Event: the number of event records (𝑦=1) for each bin.\
Event rate: the percentage of event records for each bin.\
WoE: the Weight-of-Evidence for each bin.\
IV: the Information Value (also known as Jeffrey's divergence) for each
bin.\
JS: the Jensen-Shannon divergence for each bin.\
The last row shows the total number of records, non-event records, event
records, and IV and JS.

You can use the method plot to visualize the histogram and WoE or event
rate curve. Note that the Bin ID corresponds to the binning table index.
:::

::: {.cell .code execution_count="70"}
``` python
binning_table.plot(metric="woe")
```

::: {.output .display_data}
![](vertopal_3fd1402d95714cb9b5ddc6297e1a74bf/5792a195c79becd0ae15f5cce9fd40abbe6e77b8.png)
:::
:::

::: {.cell .code execution_count="71"}
``` python
binning_table.plot(metric="event_rate")
```

::: {.output .display_data}
![](vertopal_3fd1402d95714cb9b5ddc6297e1a74bf/452b7810de993ec8dc690f78da5a9ec8d92708df.png)
:::
:::

::: {.cell .markdown}
Note that WoE is inversely related to the event rate, i.e., a
monotonically ascending event rate ensures a monotonically descending
WoE and vice-versa. We will see more monotonic trend options in the
advanced tutorial.
:::

::: {.cell .markdown}
Read more here:
<https://gnpalencia.org/optbinning/tutorials/tutorial_binary.html>
:::

::: {.cell .markdown}
## Working with \'missingno\' library
:::

::: {.cell .markdown}
```{=html}
<iframe width="560" height="315" src="https://www.youtube.com/embed/Wdvwer7h-8w?si=pVqCbOXb4CaCsmnJ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen>
```
```{=html}
</iframe>
```
:::

::: {.cell .markdown}
**Exercise 6.** Your turn!

Work with the \'carseats\' dataset, find the best way to perform full
diagnostic (dirty data, outliers, missing values). Fix problems.
:::

::: {.cell .code execution_count="72"}
``` python
print(carseats.head())

def check_rules(df):
    rules={
        "Sales >= 0":df["Sales"]>=0,
        "CompPrice >= 0":df["CompPrice"]>=0,
        "Income >= 0":df["Income"]>=0,
        "Advertising >= 0":df["Advertising"]>=0,
        "Population >= 0":df["Population"]>=0,
        "Price>= 0":df["Price"]>=0,
        "120 >Age >= 0" : (df["Age"]>=0) & (df["Age"]<120),
        "Urban in ['Yes','No']":df["Urban"].isin(["Yes","No"]),
    }
    return rules
rules= check_rules(carseats)

for rule, result in rules.items():
    print(f"{rule}: {result.all()}")

violations = {rule: ~result for rule, result in rules.items()}

summary = {rule: result.sum() for rule, result in violations.items()}

print("Summary of Violations:")

for rule, count in summary.items():
    print(f"{rule}: {count} violations")

def correct_sales(df):
    df.loc[(~df['Sales'].isna()) & (df['Sales'] < 0), 'Sales'] = np.nan
    return df

mydata_corrected = correct_sales(carseats)

print(mydata_corrected)
```

::: {.output .stream .stdout}
       Sales  CompPrice  Income  Advertising  Population  Price ShelveLoc  Age  \
    0   9.50        138    73.0           11         276    120       Bad   42   
    1  11.22        111    48.0           16         260     83      Good   65   
    2  10.06        113    35.0           10         269     80    Medium   59   
    3   7.40        117   100.0            4         466     97    Medium   55   
    4   4.15        141    64.0            3         340    128       Bad   38   

       Education Urban   US Income_binned_quantiles Income_binned_equal  
    0         17   Yes  Yes                    High              Medium  
    1         10   Yes  Yes                  Medium                 Low  
    2         12   Yes  Yes                     Low            Very Low  
    3         14   Yes  Yes               Very High                High  
    4         13   Yes   No                  Medium                 Low  
    Sales >= 0: True
    CompPrice >= 0: True
    Income >= 0: True
    Advertising >= 0: True
    Population >= 0: True
    Price>= 0: True
    120 >Age >= 0: True
    Urban in ['Yes','No']: False
    Summary of Violations:
    Sales >= 0: 0 violations
    CompPrice >= 0: 0 violations
    Income >= 0: 0 violations
    Advertising >= 0: 0 violations
    Population >= 0: 0 violations
    Price>= 0: 0 violations
    120 >Age >= 0: 0 violations
    Urban in ['Yes','No']: 10 violations
         Sales  CompPrice  Income  Advertising  Population  Price ShelveLoc  Age  \
    0     9.50        138    73.0           11         276    120       Bad   42   
    1    11.22        111    48.0           16         260     83      Good   65   
    2    10.06        113    35.0           10         269     80    Medium   59   
    3     7.40        117   100.0            4         466     97    Medium   55   
    4     4.15        141    64.0            3         340    128       Bad   38   
    ..     ...        ...     ...          ...         ...    ...       ...  ...   
    395  12.57        138   108.0           17         203    128      Good   33   
    396   6.14        139    23.0            3          37    120    Medium   55   
    397   7.41        162    26.0           12         368    159    Medium   40   
    398   5.94        100    79.0            7         284     95       Bad   50   
    399   9.71        134    37.0            0          27    120      Good   49   

         Education Urban   US Income_binned_quantiles Income_binned_equal  
    0           17   Yes  Yes                    High              Medium  
    1           10   Yes  Yes                  Medium                 Low  
    2           12   Yes  Yes                     Low            Very Low  
    3           14   Yes  Yes               Very High                High  
    4           13   Yes   No                  Medium                 Low  
    ..         ...   ...  ...                     ...                 ...  
    395         14   Yes  Yes               Very High                High  
    396         11    No  Yes                     Low            Very Low  
    397         18   Yes  Yes                     Low            Very Low  
    398         12   Yes  Yes                    High              Medium  
    399         16   Yes  Yes                     Low            Very Low  

    [400 rows x 13 columns]
:::
:::

::: {.cell .code}
``` python
rules = check_rules(mydata_corrected)
violations = {rule: ~result for rule, result in rules.items()}
violated_df = pd.DataFrame(violations)

for col in violated_df.columns:
    carseats.loc[violated_df[col], col.split()[0]] = np.nan
summary = {rule: result.sum() for rule, result in violations.items()}
print(summary)
```
:::
