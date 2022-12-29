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

Finding Age from a column of date of birth

```
from datetime import datetime
from datetime import date

def calculate_age(born):
    born = datetime.strptime(born, "%d/%m/%Y").date()
    today = date.today()
    return today.year - born.year - ((today.month, today.day) < (born.month, born.day))
```
