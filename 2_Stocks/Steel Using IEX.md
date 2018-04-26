

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import json
import requests
import time
from iexfinance import stock
```


```python
url = 'https://api.iextrading.com/1.0/stock'
```


```python
stocks = ['STLD', 'NUE', 'CLF', 'RS']
```


```python
start_date = pd.datetime(2018, 1 ,2)
end_date = pd.datetime(2018, 3, 31)
date_list = (pd.bdate_range(start_date, end_date))
dates = (date_list.strftime('%Y-%m-%d')).tolist()

close_dates = ['2018-01-15', '2018-02-19', '2018-03-30']

final_dates = []

for day in dates:
    if day not in close_dates:
        final_dates.append(day)
        
instance_number = range(len(final_dates))
```


```python
stock_data = []

for symbol in stocks: 
    for x in instance_number:
        query_url = requests.get(url + '/' + symbol + '/chart' + '/ytd').json()
        trade_date = query_url[x]['date']
        open_price = query_url[x]['open']
        close_price = query_url[x]['close']
        percent_change = query_url[x]['changePercent']
        
        stock_data.append({'Stock' : symbol, 'Date' : trade_date, 'Open' : open_price, 'Close' : close_price, 'Percent_Change' : percent_change})       
```


```python
stock_df = pd.DataFrame(stock_data)
```


```python
stock_final = stock_df.loc[(stock_df['Date'] >= '2018-02-23') & (stock_df['Date'] <= '2018-03-31')]
```


```python
stock_final.head()
```




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
      <th>Close</th>
      <th>Date</th>
      <th>Open</th>
      <th>Percent_Change</th>
      <th>Stock</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>36</th>
      <td>47.2632</td>
      <td>2018-02-23</td>
      <td>49.1250</td>
      <td>-1.104</td>
      <td>STLD</td>
    </tr>
    <tr>
      <th>37</th>
      <td>47.8108</td>
      <td>2018-02-26</td>
      <td>48.1393</td>
      <td>1.159</td>
      <td>STLD</td>
    </tr>
    <tr>
      <th>38</th>
      <td>47.3030</td>
      <td>2018-02-27</td>
      <td>47.6813</td>
      <td>-1.062</td>
      <td>STLD</td>
    </tr>
    <tr>
      <th>39</th>
      <td>46.0485</td>
      <td>2018-02-28</td>
      <td>47.4523</td>
      <td>-2.652</td>
      <td>STLD</td>
    </tr>
    <tr>
      <th>40</th>
      <td>47.8904</td>
      <td>2018-03-01</td>
      <td>47.4723</td>
      <td>4.000</td>
      <td>STLD</td>
    </tr>
  </tbody>
</table>
</div>




```python
sns.set(context = 'poster', style = 'darkgrid', palette ='colorblind', font = 'Arial', font_scale = 4)
g = sns.factorplot('Date', 'Close', hue = 'Stock', data = stock_final, markers = 'o', size = 30, aspect = 1.5, scale = 2)
g.set_xticklabels(rotation=80)
plt.ylim(0, 100)


plt.title('March 2018 Stock Data', weight = 'bold')
plt.xlabel('Date', weight = 'bold')
plt.ylabel('Close Price', weight = 'bold')
plt.show()
```

    /Users/tamaradaniels/anaconda3/lib/python3.6/site-packages/seaborn/categorical.py:1508: FutureWarning: remove_na is deprecated and is a private function. Do not use.
      stat_data = remove_na(group_data[hue_mask])



![png](output_8_1.png)

