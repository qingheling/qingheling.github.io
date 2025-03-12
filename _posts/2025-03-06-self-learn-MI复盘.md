---
title: self-learn MI复盘
author: LingMj
data: 2025-03-06
categories: [self-learn]
tags: [Decision-making-and-forecasting]
description: 难度-Medium
---

>介绍：这里开展自学主题博客，主要是来让我学习理解关于机器学习，决策与预测，非参数统计，时间序列的课题内容代码部署，如果你看到这篇博客对应上述理解或者代码问题可以通过discord联系我我会及时纠正错误discord：lingmj
>

## 时间序列

>时间序列分解法构建模型，查考知识文件：https://wiki.mbalib.com/wiki/%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E5%88%86%E8%A7%A3%E6%B3%95, 手册地址：https://www.statsmodels.org/stable/generated/statsmodels.tsa.seasonal.seasonal_decompose.html
>

>数据为：
>

```
年份	季度	时间代码	销售量
2015	1	       1	    25
        2	       2	    32
        3	       3	    37
        4	       4	    26
2016	1	       5	    30
        2	       6	    38
        3	       7	    42
        4	       8	    30
2017	1	       9	    29
        2	       10	    39
        3	       11	    50
        4	       12	    35
2018	1	       13	    30
        2	       14	    39
        3	       15	    51
        4	       16	    37
2019	1	       17	    29
        2	       18	    42
        3              19	    55
        4	       20	    38
2020	1	       21	    31
        2	       22	    43
        3	       23	    54
        4	       24	    41

```

>python代码，编辑公式：Y=TSC
>

>前期工作：安装numpy，pandas，plt，openpyxl
>

>文件读取代码
>

```
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
from sklearn.linear_model import LinearRegression
from statsmodels.tsa.seasonal import seasonal_decompose

file_path = "/home/lingmj/MI/data.xlsx"

df = pd.read_excel(file_path,engine="openpyxl")

print("数据如下：",end='\n')
print(df)
```

>预处理
>

```
df['年份'] = df['年份'].ffill() 
df['时间代码'] = df['时间代码'].astype(int)
df['日期'] = pd.to_datetime(df['年份'].astype(int).astype(str) + '-' + df['季度'].astype(int).astype(str) + '-1', format='%Y-%m-%d')
df.set_index('日期', inplace=True)
```

>时间序列分解，按照周期行进行处理,我这里使用的加分的，时间序列分解分为加分和乘法，提取季节部分，趋势部分，残差（随机项），并且去除缺失值
>

```
decomposition = seasonal_decompose(sales_series, model='additive', period=4)
trend = decomposition.trend.dropna() 
seasonal = decomposition.seasonal.dropna() 
residual = decomposition.resid.dropna()
```

>使用乘法模型
>

```
decomposition = seasonal_decompose(sales_series, model='multiplicative', period=4)
trend = decomposition.trend.dropna()
seasonal = decomposition.seasonal.dropna()
residual = decomposition.resid.dropna()
```

>但是我们是Y=STC,所以我们需要使用乘法模型
>

```
X = np.arange(len(trend)).reshape(-1, 1)
y = trend.values
model = LinearRegression()
model.fit(X, y)

future_X = np.array([len(trend) + i for i in range(1, 5)]).reshape(-1, 1)
future_trend = model.predict(future_X)

seasonal_values = seasonal.values[:4] 

residual_mean = residual.mean()

predictions = future_trend * seasonal_values * residual_mean

results = []
for i in range(4):
    T = future_trend[i] + 1.58
    S = seasonal_values[i]
    C = residual_mean
    results.append([T, S, C, T * S * C])

results_df = pd.DataFrame(results, columns=['T', 'S', 'C', '预测值'], index=[1, 2, 3, 4])
print("2021年各季度预测结果：")
print(results_df)
```

>结果：
>

```
root@LingMj:/home/lingmj/MI# python3 MI.py 
2021年各季度预测结果：
           T         S        C        预测值
1  45.665902  0.792230  0.99627  36.042933
2  46.219568  1.042365  0.99627  47.997937
3  46.773233  1.275205  0.99627  59.422995
4  47.326898  0.890201  0.99627  41.973293
```

>因为结果有特殊误差和数据是我手动加的，进行了对应部分值单独研究和整理
>

>T值是一元线性回归，它可以利用原包进行系数和常数计算并且赋予预测
>

```
X = df[['时间代码']] 
y = df['销售量']     

model = LinearRegression()

model.fit(X, y)

b0 = model.intercept_ 
b1 = model.coef_[0]   

time_codes_2021 = [25, 26, 27, 28]

predictions_2021 = [b0 + b1 * tc for tc in time_codes_2021]

for i, value in enumerate(predictions_2021, start=1):
    print(f"2021 年第 {i} 季度预测销售量 (T值): {value:.4f}")
```


>接下来进行S值的计算，S值是移动平均，所以数值是可以直接公式获得
>

```
df['年份'] = df['年份'].ffill() 
df['时间代码'] = df['时间代码'].astype(int)
df['日期'] = pd.to_datetime(df['年份'].astype(int).astype(str) + '-' + df['季度'].astype(int).astype(str) + '-1', format='%Y-%m-%d')
df.set_index('日期', inplace=True)

sales_series = df['销售量']

decomposition = seasonal_decompose(sales_series, model='multiplicative', period=4)
seasonal = decomposition.seasonal.dropna() 

seasonal_values = seasonal.values[:4] 

for i, value in enumerate(seasonal_values, start=1):
    print(f"2021 年第 {i} 季度预测销售量 (S值): {value:.4f}")
```


>最后就是C值的预测，首先需要四项居中平均和居中平均与T值进行除数计算获得
>


>我对上述方法进行拆分，先进行四项居中平均操作
>

```
fore_averages = []

for i in range(3,len(df)):
    average = (df['销售量'][i-3] + df['销售量'][i-2] + df['销售量'][i-1] + df['销售量'][i])/4
    fore_averages.append(average)

df['四项居中平均'] = [None] * 3 + fore_averages
```

>接下来是居中平均
>

```
averages = []

for i in range(3,len(df)-1):
    averag = (df['四项居中平均'][i] + df['四项居中平均'][i+1])/2
    averages.append(averag)

df['居中平均'] = [None] * 3 + averages + [None]
```

>最后计算C值
>

```
predictions_C = []

for i in range(2, len(df['T'])):
    prediction_C = (df['居中平均'][i]/df['T'][i])
    predictions_C.append(round(prediction_C, 4))

df['C'] = [None] * 2 + predictions_C
```

>通过计算出的C值按照季节平均进行预测
>


```
for i in range(4):
    C_means = 0
    for j in range(i,len(df),4):

        if pd.isna(df['C'].iloc[j]):
            C_means += 0
        else:
            C_means += df['C'].iloc[j]
    
    residual_mean.append(round(C_means/5, 4)) 
```

>销售值预测
>

```
results = []
for i in range(4):
    T = future_trend[i]
    S = round(seasonal_values[i], 3)
    C = round(residual_mean[i], 3)
    results.append([T, S, C, T * S * C])
```

>完整代码示例
>

```
file_path = "/home/lingmj/MI/data.xlsx"

df = pd.read_excel(file_path,engine="openpyxl")

X = df[['时间代码']] 
y = df['销售量']     

model = LinearRegression()

model.fit(X, y)

b0 = model.intercept_ 
b1 = model.coef_[0]   

time_codes_2021 = [25, 26, 27, 28]

predictions_2021 = [b0 + b1 * tc for tc in time_codes_2021]

future_trend = []

for i, value in enumerate(predictions_2021, start=1):
    future_trend.append(round(value, 3))





df['年份'] = df['年份'].ffill() 
df['时间代码'] = df['时间代码'].astype(int)
df['日期'] = pd.to_datetime(df['年份'].astype(int).astype(str) + '-' + df['季度'].astype(int).astype(str) + '-1', format='%Y-%m-%d')
df.set_index('日期', inplace=True)

sales_series = df['销售量']

decomposition = seasonal_decompose(sales_series, model='multiplicative', period=4)
seasonal = decomposition.seasonal.dropna() 

seasonal_values = seasonal.values[:20]

print(seasonal_values[17])

residual_mean = []

for i in range(4):
    C_means = 0
    for j in range(i,len(df),4):

        if pd.isna(df['C'].iloc[j]):
            C_means += 0
        else:
            C_means += df['C'].iloc[j]
    
    residual_mean.append(round(C_means/5, 4)) 

results = []
for i in range(4):
    T = future_trend[i]
    S = round(seasonal_values[i], 3)
    C = round(residual_mean[i], 3)
    results.append([T, S, C, T * S * C])



results_df = pd.DataFrame(results, columns=['T', 'S', 'C', '预测值'], index=[1, 2, 3, 4])
print("2021年各季度预测结果：")
print(results_df)

x_values = []
y_values = []

df = df.reset_index(drop=True)
results_df = results_df.reset_index(drop=True)

for i in range(len(df['时间代码'])):
    x_values.append(df['时间代码'].iloc[i])

x_values = x_values + time_codes_2021


for i in range(len(df['销售量'])):
    y_values.append(df['销售量'].iloc[i])

for i in range(len(results_df)):
    y_values.append(results_df['预测值'].iloc[i])



plt.figure(figsize=(10, 6))  
plt.plot(x_values, y_values, marker='o', linestyle='-', color='b', label='Sales and Predictions')  
plt.title('Sales and Predictions Over Time')  
plt.xlabel('Time Code')  
plt.ylabel('Sales and Predictions') 
plt.legend()  
plt.grid(True)  

plt.savefig('/home/lingmj/MI/line_chart.png')  

plt.show()
```


>如果你有改进的方案欢迎discord联系我
>











