---
title: self-learn MI-two复盘
author: LingMj
data: 2025-03-19
categories: [self-learn]
tags: [Decision-making-and-forecasting]
description: 难度-Medium
---

>这是我的关于这个主题第二篇文章，这里会介绍的是非参数统计中符号检验，时间序列二次多项式曲线模型预测，龚珀兹曲线模型预测
>

>对于补充原表和增值表的行数不同报错问题我这里将提供2个方案解决（方法区别在于是否已经完成大量的数据填充在表里，方法一是没进行实验之前的数据表，方法二是进行大部分实验进行的数据表）
>

>第一我们可以在导入数据表时先构建一个较大的表让小的表进行插入,填充获取通过增值形式加行数
>

```
df = df.reindex(range(len(df.index)))
df['T'] = range(1, 9)
```

>第二个方法是进行新表创建和原表合并
>

```
new_df = pd.DataFrame({'字段': 数据})

df = pd.concat([df, new_df], axis=1)
```



## 先说介绍非参数统计的符号检验

>符号检验是用于观测数据在某一个情况下的变化，通常用于表达数据上升或者下降的趋势检验，主要表达你使用这个决策对于整体是否有系统性质的提升
>

>这里符号检验分为简单符号检验和Wilcoxon符号秩检验
>

>例题一，我使用12家商品进行促销销售前，销售后的增幅和减弱进行分析销售额变化，这样可以看出我选择促销能否让销售数据有系统提升
>


```
连锁店  促销前销售额  促销后销售额 符号
0     1      42      40  +
1     2      57      60  -
2     3      38      38  0
3     4      49      47  +
4     5      63      65  -
5     6      36      39  -
6     7      48      49  -
7     8      58      50  +
8     9      47      47  0
9    10      51      52  -
10   11      83      72  +
11   12      27      33  -
```

>因为数据量少可以进行手动检验，我们可以观察到销售前比销售低符号为‘-’，高为‘+’，0为无差异
>

>所以整体的促销的对象为‘-’和‘+’的样品数据，为10，2个无异常
>

>显著性水平 α=0.05 时，单侧检验临界值为8。实际观测到6个负号，未达到显著性阈值，无法拒绝原假设，促销未显著。
>

>代码示例
>

```
positive = df['符号'].value_counts().get('+', 0)
negative = df['符号'].value_counts().get('-', 0)
zero = len(df['符号']) - (positive + negative) 

n = positive + negative

k = min(positive, negative)

test_values = binomtest(k=k, n=n, p=0.5, alternative='two-sided')
p_values = test_values.pvalue

alpha = 0.05

if p_values > alpha:
    print(f"统计量为{p_values},无法拒绝原假设：促销未达显著效果！")
else:
    print(f"统计量为{p_values},拒绝原假设：促销达显著效果！")
```

```
统计量为0.75390625 > 0.05 ,无法拒绝原假设：促销未达显著效果！
```

>接着进行一下Wilcoxon符号秩检验分析销售额变化幅度
>

```
delta = df['促销前销售额'] - df['促销后销售额']

_, p_norm = shapiro(delta)
print(f"正态性检验P值：{p_norm:.4f}")
```

```
正态性检验P值：0.1228
```

>可以观察到数据进行了少许的有优化但是还是无法通过检验，这就证明在使用符号检验的情况下，12家店的促销不能够让这12家店的销售额系统上升。改进方法加大样品数或者使用其他检验方法观测
>


## 二次多项式曲线模型

>选取模型前先检查二次差分，查看是否符合使用二次多项式曲线，二次差分可以先通过数据画曲线如果有明显的二次函数曲线趋势可以直接选择进行二次多项式曲线模型
>

>例题二，我使用的是农民年份和捕鱼量去预测下一年的捕鱼量是多少
>

>数据
>

```
root@LingMj:/home/lingmj/MI# python3 MI2.py 
0    2015.0
1    2016.0
2    2017.0
3    2018.0
4    2019.0
5    2020.0
6    2021.0
7       NaN
Name: 年份, dtype: float64 0    2790.0
1    2950.0
2    3140.0
3    3350.0
4    3588.0
5    3862.0
6    4168.0
7       NaN
Name: 捕捞量, dtype: float64
```

>首先进行曲线分析
>

```
x = df['年份']
y = df['捕捞量']

plt.figure(figsize=(10, 6))  
plt.plot(x, y, marker='o', linestyle='-', color='b', label='Catch years')  
plt.title('Catch curve')  
plt.xlabel('years')  
plt.ylabel('Catch') 
plt.legend()  
plt.grid(True) 
plt.show()
```

>接下来进行数据统计统计今年距离前一年的增值
>

```
D1_values = []

for i in range(1,len(df['捕捞量'])):
    D1_value = df['捕捞量'][i] - df['捕捞量'][i-1]
    D1_values.append(D1_value)

df['D1'] = [None] + D1_values

D2_values = []
```

>继续进行操作增值后再进行增值观察第二次捕捞量的增值计算
>

```
for i in range(2,len(df['D1'])):
    D2_value = df['D1'][i] - df['D1'][i-1]
    D2_values.append(D2_value)

df['D2'] = [None] * 2 + D2_values
```

>接下来进行二次曲线代入计算，a = 14.357，b = 113.93，c = 2664
>

```
for i in range(1, len(df['T']) + 1):
    predit_value = a * i * i + b * i + c
    predit_values.append(predit_value)

df['预测值'] = predit_values
```

>结果表格:
>

```
年份	捕捞量	D1	D2	T	预测值
2015	2790			1	2792.287
2016	2950	160		2	2949.288
2017	3140	190	30	3	3135.003
2018	3350	210	20	4	3349.432
2019	3588	238	28	5	3592.575
2020	3862	274	36	6	3864.432
2021	4168	306	32	7	4165.003
                8	4494.288
```

>通过上述计算可以观察出预测值的误差在个位，可以表明预测的准确性
>


## 龚珀兹曲线模型预测

>龚珀兹曲线是描述‘S’型的增长曲线，主要描述生长过程的数学模型
>

>例题三，6年灯具的销售量
>

```
年份t	销售量(万架)
1	8.7
2	10.6
3	13.3
4	16.5
5	20.6
6	26
```

>首先可以观察到数据是呈现生长形势，对数据进行lg的运算
>

```
lg_values = []

for i in range(len(df['销售量(万架)'])):
    lg_value = np.log10(df['销售量(万架)'][i])
    lg_values.append(lg_value)

df['lg'] = lg_values
```

>按2年为一个集合进行数据lg统计
>

```
lg_sum_values = []

for i in range(0,len(df['lg']),2):
    lg_sum_value = df['lg'][i] + df['lg'][i + 1]
    lg_sum_values.append(lg_sum_value)
```

>接着计算b值，a值，lga，k，lgk
>

```
b = ((lg_sum_values[2] - lg_sum_values[1]) / (lg_sum_values[1] - lg_sum_values[0])) ** (1/2)

lga = (lg_sum_values[1] - lg_sum_values[0]) * (b-1) / (b ** 2 - 1) ** 2

a = 10 ** lga

lgk = (lg_sum_values[0] - lga * (b ** 2 -1) / (b - 1)) / 2

k = 10 ** lgk
```

>根据获得的值代入公式k*a^(b^(年-1))
>

```
primt_values = []

for i in range(len(df['年份t'])):
    primt_value = k * a ** (b ** (df['年份t'][i] - 1))
    primt_values.append(primt_value)

primt = k * a ** (b ** (0 - 1))

primt_values.append(primt)

new_df = pd.DataFrame({'预测值': primt_values})

df = pd.concat([df, new_df], axis=1)
```

```
年份t	销售量(万架)	预测值
1	    8.7			   8.63025479
2	    10.6	       10.68566366
3	    13.3	       13.27162933
4	    16.5	       16.53527194
5	    20.6	       20.66724422
6	    26	           25.91540479
                       6.991516377

```

>通过结果可以获取到预测值的误差与真实值的误差非常小，证明龚珀兹曲线模型对这道题有着较好的预测
>


>这里主要是给出例题和解决例题的方法具体的模型介绍还是以官方介绍为主我主要介绍用途，如果就有更好的模型改进和代码修改意见给我记得联系我的discord.
>

