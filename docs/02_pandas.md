---
layout: default
title: Pandas
nav_order: 2
---
Checking Null in Data

```python
df.isnull()
df.isnull().sum()
```

Visualizing Null Values using seaborn libraries.

```python
import seaborn as sns
sns.heatmap(df.isnull(), yticklabels=False)
```

Removing rows having `na` values

```python
df.dropna()
```

Removing Duplicates entries in a column

```python
df.drop_duplicates(subset=['Email_address'])
df.drop_duplicates(subset=['Email address'], keep='last', inplace=True)
```

Getting values of a cell based on row and column index

```python
df.iloc[3,9]
```

Updating Sheet

```python
ws = gc.open_by_key('1x-LNryM-mGCdque_t1JCqjQw3Z20C2W-pqO4bhHJf1g').worksheet('Results')

ws.update([df.iloc[0:,1:].columns.values.tolist()] + df.iloc[0:,1:].values.tolist())
```

Updating column data

```python
df["Name"] = df["Name"].str.title()
```

Creating Histogram

```python
df["District"].hist()
plt.show()
```

Selecting part of rows based on some column

```
df.loc[df['column_name'] == some_value]
df.loc[df['Percetage'].between(60,75)]['Roll NO'].count()
df.loc[(df['column_name'] >= A) & (df['column_name'] <= B)]
df.query('100 >= Percentage >= 75')
```







Finding Age from a column of date of birth

```
from datetime import datetime
from datetime import date

def calculate_age(born):
    born = datetime.strptime(born, "%d/%m/%Y").date()
    today = date.today()
    return today.year - born.year - ((today.month, today.day) < (born.month, born.day))
```

Opeing Sheet in Google Colab

```
from google.colab import auth
import gspread
from google.auth import default
#autenticating to google
auth.authenticate_user()
creds, _ = default()
gc = gspread.authorize(creds)
```

Getting Data from Sheet

```
from numpy import NaN
from pandas.core.internals.blocks import na_value_for_dtype
import pandas as pd
#defining my worksheet
worksheet = gc.open_by_key('1x-LNryM-mGCdque_t1JCqjQw3Z20C2W-pqO4bhHJf1g').sheet1
#get_all_values gives a list of rows
rows = worksheet.get_all_values()
#Convert to a DataFrame 
df = pd.DataFrame(rows[1:], columns=rows[0])
```

Saving to Sheet

```
ws = gc.open_by_key('1x-LNryM-mGCdque_t1JCqjQw3Z20C2W-pqO4bhHJf1g').worksheet('Results')
ws.update([df.iloc[0:,1:].columns.values.tolist()] + df.iloc[0:,1:].values.tolist())
```

Converting to Datetime 

```
from datetime import datetime 
df['Date'] = pd.to_datetime(df['Date'])
```

Plotting in pandas

```python
# The Pandas Plot Function
df.plot(
    x=None,         # Values to use for x axis
    y=None,         # Values to use for y axis
    kind='line',    # The type of chart to make
    title=None,     # The title to use
    legend=False,   # Whether to show a legend
    xlabel=None,    # What the x-axis label should be
    ylabel=None     # What the y-axis label should be
    c=None,         # The color to use for the dots
    s=None          # How to size dots (single number or column)
)
```

Making Histogram of a Column in Pandas
```
df['Total'].hist(bins=10, alpha=0.8)
```