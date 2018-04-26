

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
stocks = ['STLD', 'NUE', 'CLF', 'RS','XME','X','AA','ARNC']
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
        close_price = query_url[x]['close']
        open_price = query_url[x]['open']
        volume = query_url[x]['volume']
        percent_change = query_url[x]['changePercent']
        
        stock_data.append({'Stock' : symbol, 'Date' : trade_date, 'Open' : open_price, 'Close' : close_price, 'Volume' : volume, 'Percent_Change' : percent_change})  
```


```python
stock_df = pd.DataFrame(stock_data)
```


```python
stock_final = stock_df.loc[(stock_df['Date'] >= '2018-02-23') & (stock_df['Date'] <= '2018-03-31')]
stld_only = stock_df.loc[(stock_df['Stock'] == 'STLD')]
stld_final = stld_only.loc[(stld_only['Date'] >= '2018-02-23') & (stld_only['Date'] <= '2018-03-31')]
nue_only = stock_df.loc[(stock_df['Stock'] == 'NUE')]
nue_final = nue_only.loc[(nue_only['Date'] >= '2018-02-23') & (nue_only['Date'] <= '2018-03-31')]
clf_only = stock_df.loc[(stock_df['Stock'] == 'CLF')]
clf_final = clf_only.loc[(clf_only['Date'] >= '2018-02-23') & (clf_only['Date'] <= '2018-03-31')]
rs_only = stock_df.loc[(stock_df['Stock'] == 'RS')]
rs_final = rs_only.loc[(rs_only['Date'] >= '2018-02-23') & (rs_only['Date'] <= '2018-03-31')]
xme_only = stock_df.loc[(stock_df['Stock'] == 'XME')]
xme_final = xme_only.loc[(xme_only['Date'] >= '2018-02-23') & (xme_only['Date'] <= '2018-03-31')]
x_only = stock_df.loc[(stock_df['Stock'] == 'X')]
x_final = x_only.loc[(x_only['Date'] >= '2018-02-23') & (x_only['Date'] <= '2018-03-31')]
aa_only = stock_df.loc[(stock_df['Stock'] == 'AA')]
aa_final = aa_only.loc[(aa_only['Date'] >= '2018-02-23') & (aa_only['Date'] <= '2018-03-31')]
arnc_only = stock_df.loc[(stock_df['Stock'] == 'ARNC')]
arnc_final = arnc_only.loc[(arnc_only['Date'] >= '2018-02-23') & (arnc_only['Date'] <= '2018-03-31')]
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
      <th>Volume</th>
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
      <td>2299443</td>
    </tr>
    <tr>
      <th>37</th>
      <td>47.8108</td>
      <td>2018-02-26</td>
      <td>48.1393</td>
      <td>1.159</td>
      <td>STLD</td>
      <td>3547149</td>
    </tr>
    <tr>
      <th>38</th>
      <td>47.3030</td>
      <td>2018-02-27</td>
      <td>47.6813</td>
      <td>-1.062</td>
      <td>STLD</td>
      <td>2815207</td>
    </tr>
    <tr>
      <th>39</th>
      <td>46.0485</td>
      <td>2018-02-28</td>
      <td>47.4523</td>
      <td>-2.652</td>
      <td>STLD</td>
      <td>2988943</td>
    </tr>
    <tr>
      <th>40</th>
      <td>47.8904</td>
      <td>2018-03-01</td>
      <td>47.4723</td>
      <td>4.000</td>
      <td>STLD</td>
      <td>4357322</td>
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


![png](output_8_0.png)



```python
sns.set(context = 'poster', style = 'darkgrid', palette ='colorblind', font = 'Arial', font_scale = 4)
g = sns.factorplot('Date', 'Percent_Change', hue = 'Stock', data = stock_final, markers = 'o', size = 30, aspect = 1.5, scale = 2)
g.set_xticklabels(rotation=80)
plt.ylim(-12, 12)


plt.title('March 2018 Stock Data', weight = 'bold')
plt.xlabel('Date', weight = 'bold')
plt.ylabel('Percent Change in Price', weight = 'bold')
plt.show()
```


![png](output_9_0.png)



```python
sns.set(context = 'poster', style = 'darkgrid', palette='deep', font = 'Arial', font_scale = 4)
g = sns.factorplot('Date', 'Close', color='pink', data = stld_final, markers = 'o', size = 30, aspect = 1.5, scale = 2)
g.set_xticklabels(rotation=80)
plt.ylim(40,50)

label = ['STLD']
plt.legend((label), bbox_to_anchor=(1.15, .7))
plt.title('March 2018 Stock Data (STLD)', weight = 'bold')
plt.xlabel('Date', weight = 'bold')
plt.ylabel('Close Price', weight = 'bold')
plt.show()
```


![png](output_10_0.png)



```python
sns.set(context = 'poster', style = 'darkgrid', palette='deep', font = 'Arial', font_scale = 4)
g = sns.factorplot('Date', 'Close', color='orange', data = nue_final, markers = 'o', size = 30, aspect = 1.5, scale = 2)
g.set_xticklabels(rotation=80)
plt.ylim(55, 72)

label = ['NUE']
plt.legend((label), bbox_to_anchor=(1.15, .7))
plt.title('March 2018 Stock Data (NUE)', weight = 'bold')
plt.xlabel('Date', weight = 'bold')
plt.ylabel('Close Price', weight = 'bold')
plt.show()
```


![png](output_11_0.png)



```python
sns.set(context = 'poster', style = 'darkgrid', palette='deep', font = 'Arial', font_scale = 4)
g = sns.factorplot('Date', 'Close', color='lightgreen', data = clf_final, markers = 'o', size = 30, aspect = 1.5, scale = 2)
g.set_xticklabels(rotation=80)
plt.ylim(6, 9)

label = ['CLF']
plt.legend((label), bbox_to_anchor=(1.15, .7))
plt.title('March 2018 Stock Data (CLF)', weight = 'bold')
plt.xlabel('Date', weight = 'bold')
plt.ylabel('Close Price', weight = 'bold')
plt.show()
```


![png](output_12_0.png)



```python
sns.set(context = 'poster', style = 'darkgrid', palette='deep', font = 'Arial', font_scale = 4)
g = sns.factorplot('Date', 'Close', color='green', data = rs_final, markers = 'o', size = 30, aspect = 1.5, scale = 2)
g.set_xticklabels(rotation=80)
plt.ylim(80, 100)

label = ['RS']
plt.legend((label), bbox_to_anchor=(1.15, .7))
plt.title('March 2018 Stock Data (RS)', weight = 'bold')
plt.xlabel('Date', weight = 'bold')
plt.ylabel('Close Price', weight = 'bold')
plt.show()
```


![png](output_13_0.png)



```python
sns.set(context = 'poster', style = 'darkgrid', palette='deep', font = 'Arial', font_scale = 4)
g = sns.factorplot('Date', 'Close', color='lightblue', data = xme_final, markers = 'o', size = 30, aspect = 1.5, scale = 2)
g.set_xticklabels(rotation=80)
plt.ylim(30, 40)

label = ['XME']
plt.legend((label), bbox_to_anchor=(1.15, .7))
plt.title('March 2018 Stock Data (XME)', weight = 'bold')
plt.xlabel('Date', weight = 'bold')
plt.ylabel('Close Price', weight = 'bold')
plt.show()
```


![png](output_14_0.png)



```python
sns.set(context = 'poster', style = 'darkgrid', palette='deep', font = 'Arial', font_scale = 4)
g = sns.factorplot('Date', 'Close', color='blue', data = x_final, markers = 'o', size = 30, aspect = 1.5, scale = 2)
g.set_xticklabels(rotation=80)
plt.ylim(30, 50)

label = ['X']
plt.legend((label), bbox_to_anchor=(1.15, .7))
plt.title('March 2018 Stock Data (X)', weight = 'bold')
plt.xlabel('Date', weight = 'bold')
plt.ylabel('Close Price', weight = 'bold')
plt.show()
```


![png](output_15_0.png)



```python
sns.set(context = 'poster', style = 'darkgrid', palette='deep', font = 'Arial', font_scale = 4)
g = sns.factorplot('Date', 'Close', color='purple', data = aa_final, markers = 'o', size = 30, aspect = 1.5, scale = 2)
g.set_xticklabels(rotation=80)
plt.ylim(40, 55)

label = ['AA']
plt.legend((label), bbox_to_anchor=(1.15, .7))
plt.title('March 2018 Stock Data (AA)', weight = 'bold')
plt.xlabel('Date', weight = 'bold')
plt.ylabel('Close Price', weight = 'bold')
plt.show()
```


![png](output_16_0.png)



```python
sns.set(context = 'poster', style = 'darkgrid', palette='deep', font = 'Arial', font_scale = 4)
g = sns.factorplot('Date', 'Close', color='magenta', data = arnc_final, markers = 'o', size = 30, aspect = 1.5, scale = 2)
g.set_xticklabels(rotation=80)
plt.ylim(20, 30)

label = ['ARNC']
plt.legend((label), bbox_to_anchor=(1.15, .7))
plt.title('March 2018 Stock Data (ARNC)', weight = 'bold')
plt.xlabel('Date', weight = 'bold')
plt.ylabel('Close Price', weight = 'bold')
plt.show()
```


![png](output_17_0.png)



```python
fig, ax1 = plt.subplots()
# fig = plt.gcf()
fig.set_size_inches(20, 10)


color = 'tab:red'
plt.title('Close Price and Percentage Change in March (STLD)', size=20, fontweight='bold')
ax1.set_ylabel('Close($)', size=32, fontweight='bold')
ax1.plot(stld_final['Date'], stld_final['Close'], color=color, marker = 'o', linewidth=3)
ax1.tick_params(axis='y', labelsize=20)
ax1.tick_params(axis='x', rotation=80, labelsize=20)
ax1.grid(b='off')
ax1.set_aspect('auto')

ax2 = ax1.twinx() 
color = 'tab:blue'
ax2.set_ylabel('Percent Change(%)', size=26, fontweight='bold')
ax2.plot(stld_final['Date'], stld_final['Percent_Change'], color=color, marker = 'o', linewidth=3)
ax2.tick_params(axis='y', labelsize= 20)
ax2.set_aspect('auto')
plt.ylim(-8,8)
lines, labels = ax1.get_legend_handles_labels()
lines2, labels2 = ax2.get_legend_handles_labels()
ax2.legend(lines + lines2, labels + labels2, loc=0, fontsize=20)

fig.savefig('test2png.png', dpi=200)

```




    <matplotlib.legend.Legend at 0x1a1cbc4588>




![png](output_18_1.png)



```python
fig, ax1 = plt.subplots()
# fig = plt.gcf()
fig.set_size_inches(20, 10)


color = 'tab:red'
plt.title('Close Price and Percentage Change in March (NUE)', size=32, fontweight='bold')
ax1.set_ylabel('Close ($)', size=26, fontweight='bold')
ax1.plot(nue_final['Date'], nue_final['Close'], color=color, marker = 'o', linewidth=3)
ax1.tick_params(axis='y', labelsize=20)
ax1.tick_params(axis='x', rotation=80, labelsize=20)
ax1.grid(b='off')
ax1.set_aspect('auto')
plt.ylim(58,72)

ax2 = ax1.twinx() 
color = 'tab:blue'
ax2.set_ylabel('Percent Change (%)', size=26, fontweight='bold')
ax2.plot(nue_final['Date'], nue_final['Percent_Change'], color=color, marker = 'o', linewidth=3)
ax2.tick_params(axis='y', labelsize= 20)
ax2.set_aspect('auto')
plt.ylim(-8,8)
lines, labels = ax1.get_legend_handles_labels()
lines2, labels2 = ax2.get_legend_handles_labels()
ax2.legend(lines + lines2, labels + labels2, loc=0, fontsize=20)


```




    <matplotlib.legend.Legend at 0x1a26c82ba8>




![png](output_19_1.png)



```python
fig, ax1 = plt.subplots()
# fig = plt.gcf()
fig.set_size_inches(20, 10)


color = 'tab:red'
plt.title('Close Price and Percentage Change in March (CLF)', size=32, fontweight='bold')
ax1.set_ylabel('Close ($)', size=26, fontweight='bold')
ax1.plot(clf_final['Date'], clf_final['Close'], color=color, marker = 'o', linewidth=3)
ax1.tick_params(axis='y', labelsize=20)
ax1.tick_params(axis='x', rotation=80, labelsize=20)
ax1.grid(b='off')
ax1.set_aspect('auto')


ax2 = ax1.twinx() 
color = 'tab:blue'
ax2.set_ylabel('Percent Change (%)', size=26, fontweight='bold')
ax2.plot(clf_final['Date'], clf_final['Percent_Change'], color=color, marker = 'o', linewidth=3)
ax2.tick_params(axis='y', labelsize= 20)
ax2.set_aspect('auto')
lines, labels = ax1.get_legend_handles_labels()
lines2, labels2 = ax2.get_legend_handles_labels()
ax2.legend(lines + lines2, labels + labels2, loc=0, fontsize=20)

```




    <matplotlib.legend.Legend at 0x1a257fb7b8>




![png](output_20_1.png)



```python
fig, ax1 = plt.subplots()
# fig = plt.gcf()
fig.set_size_inches(20, 10)


color = 'tab:red'
plt.title('Close Price and Percentage Change in March (RS)', size=32, fontweight='bold')
ax1.set_ylabel('Close ($)', size=26, fontweight='bold')
ax1.plot(rs_final['Date'], rs_final['Close'], color=color, marker = 'o', linewidth=3)
ax1.tick_params(axis='y', labelsize=20)
ax1.tick_params(axis='x', rotation=80, labelsize=20)
ax1.grid(b='off')
ax1.set_aspect('auto')


ax2 = ax1.twinx() 
color = 'tab:blue'
ax2.set_ylabel('Percent Change (%)', size=26, fontweight='bold')
ax2.plot(rs_final['Date'], rs_final['Percent_Change'], color=color, marker = 'o', linewidth=3)
ax2.tick_params(axis='y', labelsize= 20)
ax2.set_aspect('auto')
plt.ylim(-7,7)
lines, labels = ax1.get_legend_handles_labels()
lines2, labels2 = ax2.get_legend_handles_labels()
ax2.legend(lines + lines2, labels + labels2, loc=0, fontsize=20)

```




    <matplotlib.legend.Legend at 0x1a27587ba8>




![png](output_21_1.png)



```python
fig, ax1 = plt.subplots()
# fig = plt.gcf()
fig.set_size_inches(20, 10)


color = 'tab:red'
plt.title('Close Price and Percentage Change in March (XME)', size=32, fontweight='bold')
ax1.set_ylabel('Close ($)', size=26, fontweight='bold')
ax1.plot(xme_final['Date'], xme_final['Close'], color=color, marker = 'o', linewidth=3)
ax1.tick_params(axis='y', labelsize=20)
ax1.tick_params(axis='x', rotation=80, labelsize=20)
ax1.grid(b='off')
ax1.set_aspect('auto')


ax2 = ax1.twinx() 
color = 'tab:blue'
ax2.set_ylabel('Percent Change (%)', size=26, fontweight='bold')
ax2.plot(xme_final['Date'], xme_final['Percent_Change'], color=color, marker = 'o', linewidth=3)
ax2.tick_params(axis='y', labelsize= 20)
ax2.set_aspect('auto')
plt.ylim(-7,7)
lines, labels = ax1.get_legend_handles_labels()
lines2, labels2 = ax2.get_legend_handles_labels()
ax2.legend(lines + lines2, labels + labels2, loc=0, fontsize=20)

```




    <matplotlib.legend.Legend at 0x1a25f69a90>




![png](output_22_1.png)



```python
fig, ax1 = plt.subplots()
# fig = plt.gcf()
fig.set_size_inches(20, 10)


color = 'tab:red'
plt.title('Close Price and Percentage Change in March (X)', size=32, fontweight='bold')
ax1.set_ylabel('Close ($)', size=26, fontweight='bold')
ax1.plot(x_final['Date'], x_final['Close'], color=color, marker = 'o', linewidth=3)
ax1.tick_params(axis='y', labelsize=20)
ax1.tick_params(axis='x', rotation=80, labelsize=20)
ax1.grid(b='off')
ax1.set_aspect('auto')


ax2 = ax1.twinx() 
color = 'tab:blue'
ax2.set_ylabel('Percent Change (%)', size=26, fontweight='bold')
ax2.plot(x_final['Date'], x_final['Percent_Change'], color=color, marker = 'o', linewidth=3)
ax2.tick_params(axis='y', labelsize= 20)
ax2.set_aspect('auto')
plt.ylim(-12,12)
lines, labels = ax1.get_legend_handles_labels()
lines2, labels2 = ax2.get_legend_handles_labels()
ax2.legend(lines + lines2, labels + labels2, loc=0, fontsize=20)

```




    <matplotlib.legend.Legend at 0x1a29f849b0>




![png](output_23_1.png)



```python
fig, ax1 = plt.subplots()
# fig = plt.gcf()
fig.set_size_inches(20, 10)


color = 'tab:red'
plt.title('Close Price and Percentage Change in March (AA)', size=32, fontweight='bold')
ax1.set_ylabel('Close ($)', size=26, fontweight='bold')
ax1.plot(aa_final['Date'], aa_final['Close'], color=color, marker = 'o', linewidth=3)
ax1.tick_params(axis='y', labelsize=20)
ax1.tick_params(axis='x', rotation=80, labelsize=20)
ax1.grid(b='off')
ax1.set_aspect('auto')


ax2 = ax1.twinx() 
color = 'tab:blue'
ax2.set_ylabel('Percent Change (%)', size=26, fontweight='bold')
ax2.plot(aa_final['Date'], aa_final['Percent_Change'], color=color, marker = 'o', linewidth=3)
ax2.tick_params(axis='y', labelsize= 20)
ax2.set_aspect('auto')
plt.ylim(-7,7)
lines, labels = ax1.get_legend_handles_labels()
lines2, labels2 = ax2.get_legend_handles_labels()
ax2.legend(lines + lines2, labels + labels2, loc=0, fontsize=20)

```




    <matplotlib.legend.Legend at 0x1a2a3c6c88>




![png](output_24_1.png)



```python
fig, ax1 = plt.subplots()
# fig = plt.gcf()
fig.set_size_inches(20, 10)


color = 'tab:red'
plt.title('Close Price and Percentage Change in March (ARNC)', size=32, fontweight='bold')
ax1.set_ylabel('Close ($)', size=26, fontweight='bold')
ax1.plot(arnc_final['Date'], arnc_final['Close'], color=color, marker = 'o', linewidth=3)
ax1.tick_params(axis='y', labelsize=20)
ax1.tick_params(axis='x', rotation=80, labelsize=20)
ax1.grid(b='off')
ax1.set_aspect('auto')


ax2 = ax1.twinx() 
color = 'tab:blue'
ax2.set_ylabel('Percent Change (%)', size=26, fontweight='bold')
ax2.plot(arnc_final['Date'], arnc_final['Percent_Change'], color=color, marker = 'o', linewidth=3)
ax2.tick_params(axis='y', labelsize= 20)
ax2.set_aspect('auto')
plt.ylim(-7,7)
lines, labels = ax1.get_legend_handles_labels()
lines2, labels2 = ax2.get_legend_handles_labels()
ax2.legend(lines + lines2, labels + labels2, loc=0, fontsize=20)

```




    <matplotlib.legend.Legend at 0x1a2a807550>




![png](output_25_1.png)

