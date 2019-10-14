```python
# libraries
import pandas as pd
import numpy as np
import geopandas as gpd
import seaborn as sns
import matplotlib.pyplot as plt
import matplotlib.ticker as mtick
from matplotlib.ticker import FormatStrFormatter
import matplotlib as mpl
import shapely as shape
import mplleaflet
from PIL import Image
import seaborn as sbn
%matplotlib inline
```


```python
# Embellish plotting styles
plt.style.use('ggplot')
```

# Build indicators by area


```python
def path(folder, area, base_year, end_year):
    """
    Returns a list of string with the name of the dataframe to be read.
    """
    path = []
    for i in range(base_year,end_year):
        path.append('../data/'+folder+'/'+area+'_indicators_'+str(i)+'.csv')
        
    return path
```


```python
# parameter
parcels_path = path('bln', 'parcel', 2006, 2013)
```


```python
def non_filtered_series(path, column, sum_value, mean_value, median_value):
    """
    Returns an aggregated value for the specified columns.    
    """
    if sum_value:
        series_value = (pd.read_csv(path)[column].sum())
        
    if mean_value:
        series_value = (pd.read_csv(path)[column].mean())
        
    if median_value:
        series_value = (pd.read_csv(path)[column].median())
    
    return series_value
```


```python
def build_indicator(indicator, base_year, end_year):
    
    y = []
    for i in parcels_path:
        y.append(non_filtered_series(i, indicator, False, True, False)) #pasar parametro aca para controlar
        x = [i for i in range(base_year, end_year)]
        
    df = pd.DataFrame({'year':x, 'indicator':y})
    
    return df
        
```

# Plot indicators


```python
# parameter
hh = build_indicator('density_households', 2006, 2013)
```


```python
# parameter
jobs = build_indicator('density_jobs', 2006, 2013)
```


```python
# BASELINE
def plot_indicator(title1, title2, df1, df2):
    plt.figure(figsize=(20,10))

    plt.subplot(2,2,1)
    ax1 = sns.lineplot(x="year", y="indicator", color = 'red', data=df1)
    #ax1.yaxis.set_major_formatter(mpl.ticker.StrMethodFormatter('{x:,.0f}'))

    plt.title(title1)

    plt.subplot(2,2,2)
    ax2 = sns.lineplot(x="year", y="indicator", color = 'blue', data=df2)
    #ax2.yaxis.set_major_formatter(mpl.ticker.StrMethodFormatter('{x:,.0f}'))

    plt.title(title2);
```


```python
# parameters
title1, title2= 'density_hh', 'density_jobs'
```


```python
plot_indicator(title1, title2, hh, jobs)
```


![png](output_12_0.png)



```python

```
