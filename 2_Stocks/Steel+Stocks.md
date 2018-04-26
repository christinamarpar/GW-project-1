

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import json
import requests
import time
from config import stock_key
```


```python
function = 'TIME_SERIES_DAILY'
stock = 'XME'
apikey = stock_key

url = 'https://www.alphavantage.co/'
```


```python
stock_data = []
       
dates = ['2018-03-01', '2018-03-08']    

for x in dates:
    query_url = requests.get(url + 'query?function=' + function + '&' + 'symbol=' + stock + '&' + 'apikey=' 
                    + apikey).json()
    open_price = query_url['Time Series (Daily)'][x]['1. open']
    close_price = query_url['Time Series (Daily)'][x]['4. close']   
    low_day = query_url['Time Series (Daily)'][x]['3. low']   
    volume_day = query_url['Time Series (Daily)'][x]['5. volume']                                       
                                
                                         
    stock_data.append({'Stock':  stock, 'Date' : x, 'Open Price' : open_price, 'Close_Price' : close_price, 'Low': low_day, 'Volume' : volume_day})
    data_final = pd.DataFrame(date_final)
```


```python
data_final
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Close_Price</th>
      <th>Date</th>
      <th>Low</th>
      <th>Open Price</th>
      <th>Stock</th>
      <th>Volume</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>36.9600</td>
      <td>2018-03-01</td>
      <td>36.1300</td>
      <td>36.5100</td>
      <td>XME</td>
      <td>5556517</td>
    </tr>
    <tr>
      <th>1</th>
      <td>36.9100</td>
      <td>2018-03-08</td>
      <td>36.5500</td>
      <td>37.7700</td>
      <td>XME</td>
      <td>3605512</td>
    </tr>
  </tbody>
</table>
</div>


