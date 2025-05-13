---
title: 牛客 SHELL复盘
author: LingMj
data: 2025-05-13
categories: [SHELL]
tags: [upload]
description: 难度-Easy
---

## 牛客SHELL

>这是大佬给我提到的shell题目，打完下来感觉挺有意思的就是题目有点少了，一共34道题
>

## 第一题 统计文件的行数

![picture 0](../assets/images/30efc0e218016dbc3467c707f9e1e6203627d0fa785c501dbbfd49e49e4fa8b9.png)  

```
wc -l
```

```
awk 'END{print NR}'
```

>这个我直接工具操作，我也会给出大佬优雅的处理方案
>

## 第二题 打印文件的最后5行

![picture 1](../assets/images/58fcfef7c8415fc2e624c48e6835096e8f2d8e3221fa1ef811e5b843528d88b3.png)  

```
tail -5
```

>我还是使用工具
>

## 第三题输出0到500中7的倍数

![picture 2](../assets/images/2426d8047d2baee14bf9ca16c936f3b400e6d0f490dd2de0167dc2e2f0f2b993.png)  


```
for i in {0..500..7};do echo $i;done
```

```
seq 0  7 500
```

>我写得完全是最朴素方案，
>

## 第四题 打印文件第五行内容

![picture 3](../assets/images/abf95955e56ae61ed310f79ae681a78b6b0ada943eb547ffddd28f26de99e8d7.png)  


```
head -5|tail -1
```

```
awk 'NR==5'
```

>面向答案编写
>

## 第五题 打印空行的行号

![picture 4](../assets/images/b1513eab275be1b24f1c5d5d49728534e45fc8bdb391c588f2a0bd1cb62a3d4d.png)  


```
grep -n '^$'|awk -F: '{print $1}'
```

```
sed  -n '/^$/='
```

>主要是找到空行的行数
>


## 第六题 去掉空行

![picture 5](../assets/images/41ba4aae5028b4b9ca2d858668b4d561c3cfe5affca7bd88ea7da3c4cd90bf66.png)  


```
xargs|tr ' ' '\n'
```

```
grep -v '^$'
```

>记得xargs挺好用的
>

## 第七题 打印字母数小于8的单词

![picture 6](../assets/images/11b54b34a06a023df5083ad1ed16bab45b828d58d9dd46c08579f180a211d27b.png)  


```
awk '{for(i=1;i<=NF;i++){if(length($i)<8)print $i}}'
```

>循环每一行判断是否小于八
>

## 第八题 统计所有进程占用内存百分比的和

![picture 7](../assets/images/941ef31608a9802b9aa71cba216d40e9f9cf1dc97a9612b5f552c3474e2e6230.png)  


```
awk '{a+=$4}END{print a}'
```

>取出内容列相加
>

## 第九题 统计每个单词出现的个数

![picture 8](../assets/images/49bcc83814f804a8121cbc2ecbabb77eadc4f1fe69a54027ed9180d3e02362bf.png)  


```
awk '{for(i=1;i<=NF;i++){print $i}}'|sort|uniq -c| sort -n | awk '{print $2,$1}'
```

>这个方案选中需要的行数开始计数输出
>


## 第十题 第二列是否有重复

![picture 9](../assets/images/7a49e66badd2be08b5ad7c8f8daba26998a37efe5546a5624fc59c290486fa38.png)  


```
awk -F ' ' '{print $2}'|sort |uniq -cd |sort -n
```

```
awk '{if(!a[$2]){a[$2]=1}else{a[$2]++}}END{for(i in a){if(a[i]>1)print a[i],i}}'
```

>跟统计数方案差不多
>

## 第十一题 转置文件的内容

```
cat nowcoder.txt|awk -F ' ' '{print $1}'|xargs && cat nowcoder.txt|awk -F ' ' '{print $2}'|xargs
```

```
awk '{for(i=1;i<=NF;i++){a[NR,i]=$i}}END{for(i=1;i<=NF;i++){for(j=1;j<=NR;j++){printf a[j,i]" "}print x}}'
```


>面向答案编程
>

## 第十二题 打印每一行出现的数字个数

![picture 10](../assets/images/27650f99e1670c47a72a0bcb96e39e14cf599984eff1ff56ec17f05069420329.png)  


```
awk -F "[1-5]" '{a+=NF-1;print "line"NR " number: "NF-1} END {print "sum is "a}'
```

>主要找出数字
>


## 第十三题 去掉所有包含this的句子

![picture 11](../assets/images/d200f2d3b5b0537f0c36685c682eb90e75b992c7ebff428b125520b50431ac70.png)  


```
grep -v 'this'
```

>直接grep
>


## 第十四题 求平均值

![picture 12](../assets/images/80c48d80929e715e5cb0494a9af5aad5823a3963e567484ecd317b030f83c38e.png)  


```
awk 'NR>1{a+=$0}END{b=a/(NR-1);printf "%.3f",b}'
```

>循环行数进行操作
>

## 第十五题 去掉不需要的单词

![picture 13](../assets/images/aae07d886d1ad9ac9c7cd95fc91121e480ceb75b810d5192b453baafd363b6f0.png)  


```
grep -v 'b\|B'
```

>还是grep
>

## 第十六题 判断输入的是否为IP地址

![picture 14](../assets/images/24316cd49221e15eaccfa3848d4181165779b07d70656d55567266cfda095590.png)  


```
awk -F '.' 'NF!=4{print "error";next}NF==4{f=1;for(i=1;i<=NF;i++){if($i>255||$i<0){f=0}}}{if(f){print "yes"}else{print "no"}}'
```

>这个先判断行是否.分四段，接着确认范围操作
>

## 第十七题 将字段逆序输出文件的每行

![picture 15](../assets/images/2f00169d9c30a712a7d85a99d50c3c093539eb0ee3badee4f5530d2b5bf56845.png)  


```
awk -F ':' '{print $7":"$6":"$5":"$4":"$3":"$2":"$1}'
```

```
awk 'BEGIN{FS=OFS=":"}{for(i=NF;i>1;i--){printf $i":"}print $1}'
```

>面向答案编程
>

## 第十八题 域名进行计数排序处理

![picture 16](../assets/images/07043dd7fbce8576f4849f4b0d6aeaf4ee49b345f4a2ddfcab7a362a743fcee7.png)  


```
awk -F '/' '{print $3}'|sort |uniq -c|sort -nr|awk -F ' ' '{print $1" "$2}'
```

```
awk -F/ '{a[$3]++}END{for(i in a){print a[i],i}}'|sort -nrk 1
```

>对于排序我都是一个逻辑
>


## 第十九题 打印等腰三角形

![picture 17](../assets/images/ff47a25806553dabc3bc3bdbeaba7c49b83606b50df2761f349e092a9fa11a7b.png)  


```
awk 'BEGIN{n=6;l=n*2-1;for(i=1;i<=n;i++){for(j=1;j<=n-i;j++){printf " "}for(k=1;k<=i;k++){printf "* "}print x}}'
```

>经典题目
>


## 第二十题 打印只有一个数字的行

![picture 18](../assets/images/b5be65184a6f6d4b5e7ccfece6bd57303e24b8ab2e0c63d7183f40c2be9cdd15.png)  


```
awk -F '[0-9]' '{if(NF==2) print $0}'
```

>这个行调试输出
>

## 第二十一题 格式化输出

```
xargs -n1 printf "%'d\n"
```

```
perl -pe 's/(?<!^)(?=(\d{3})+$)/,/g'
```

>这个perl是独一档
>


## 第二十二题 处理文本

![picture 19](../assets/images/8703225ca0c0a029b561ef80f026109f52e413fbc4f5924966631becaa852460.png)  


```
awk -F ':' '{if(!a[$1]){a[$1]=$2}else{a[$1]=a[$1]"\n"$2}}END{for(i in a){print "["i"]\n"a[i]}}'
```

>这个主要是去掉重复的[？？？]剩下输出
>

## 第二十三题 Nginx日志分析1-IP访问次数统计

![picture 20](../assets/images/2706a8961d3cac75e88efbbdbaef366c0eea7e64800ae62f7a8ec66870b7b150.png)  


```
grep '23/Apr/2020'|awk -F ' ' '{print $1}'|sort|uniq -c|sort -r|awk -F ' ' '{print $1 " " $2}'
```

```
awk '/23\/Apr\/2020/{a[$1]++}END{for(i in a){print a[i],i}}'|sort -nrk 1
```

>最朴素方案
>

## 第二十四题 Nginx日志分析2-统计某个时间段的IP访问量

![picture 21](../assets/images/69f39d6825dcdaf0f1960089ea1955dc76a782a3a74bd53396ac35a77df137c6.png)  


```
grep '23/Apr/2020:2[0-3]'|sort|awk -F ' ' '{print $1}'|uniq|wc -l|awk -F ' ' '{print $1}'
```

```
awk -F'[ :]+' '/23\/Apr/&&($5>=20||$5<=23){a[$1]++}END{print length(a)}'
```

>没直接echo算是很朴素了
>


## 第二十五题 nginx日志分析3-统计访问3次以上的IP

![picture 22](../assets/images/800ba3210fc24a7d00bc41c9bd8ea0a2066c308830bdf87deb1573429c9d199a.png)  

```
sort|awk -F ' ' '{print $1}'|uniq -c|sort -r|awk '$1 > 3'|awk -F ' ' '{print $1 " " $2}'
```

>还是这个逻辑
>

## 第二十六题 Nginx日志分析4-查询某个IP的详细访问情况

![picture 23](../assets/images/239bce8ed77e1675a01a0a56631c39345ac359ef97eba4e981548ee4d32d5521.png)  


```
grep '192.168.1.22'|awk -F ' ' '{print $7}'|sort|uniq -c|awk -F ' ' '{print $1 " " $2}'
```

```
awk '$1=="192.168.1.22"{a[$7]++}END{for(i in a){print a[i],i}}'|sort -nrk 1
```

>这几个出的感觉一样
>

## 第二十七题 nginx日志分析5-统计爬虫抓取404的次数

![picture 24](../assets/images/5687b1cfed7fc915cd96767eba3d6cb1f4c7b027ff1b9da800b9763ed3101984.png)  


```
grep 'www.baidu.com'|grep '404'|awk -F ' ' '{print $1}'|wc -w|awk -F ' ' '{print $1}'
```

```
awk '$9=="404"&&/spider.html/{a++}END{print a}'
```

>朴素
>

## 第二十八题 Nginx日志分析6-统计每分钟的请求数

```
awk -F ':' '{print $2":"$3}'|sort|uniq -c|sort -r|awk -F ' ' '{print $1 " " $2}'
```

```
awk -F'[ :]+' '{a[$5":"$6]++}END{for(i in a){print a[i],i}}'|sort -nrk 1
```

>看好分钟是那列
>

## 第二十九题 netstat练习1-查看各个状态的连接数

![picture 25](../assets/images/19ff8f8f7ad61465829d5ad40a678d151faa5ece3d0545ec2d1d122ce15738d6.png)  


```
grep 'tcp'|awk -F ' ' '{print $6}'|xargs|sed 's/ /\n/g'|sort|uniq -cd|sort -rn|awk -F ' ' '{print $2 " " $1}'
```

```
awk '/tcp/{a[$NF]++}END{for(i in a){print i,a[i]}}'|sort -nrk 2
```

>确认为tcp
>


## 第三十题 netstat练习2-查看和3306端口建立的连接

![picture 26](../assets/images/3782e2d966762eba2662dc32b8020dc8ef566ec8bc18cec38d2bb1a23c653605.png)  


```
grep 'ESTABLISHED'|grep '3306'|awk -F ' ' '{print $5}'|awk -F: '{print $1}'|sort|uniq -c|sort -nr|awk -F ' ' '{print $1 " " $2}'
```

```
awk -F'[ :]+'  '/tcp/&&/:3306/&&/ESTA/{a[$6]++}END{for(i in a){print a[i],i}}'|sort -nrk 1
```

>这个还行
>

## 第三十一题 netstat练习3-输出每个IP的连接数

![picture 27](../assets/images/0543c6ecddf3d00b096c851580f719f3903e0104d9839b5f8df2f9f2431a4361.png)  


```
grep 'tcp'|awk -F ' ' '{print $5}'|awk -F: '{print $1}'|sort |uniq -c|sort -rn|awk -F ' ' '{print $2 " " $1}' 
```

```
awk -F'[ :]+' '/tcp/{a[$6]++}END{for(i in a){print i,a[i]}}'|sort -nrk 2
```

>和上一个没啥区别的感觉
>

## 第三十二题

![picture 28](../assets/images/9d9e944f44023c16bdf9481de81478e2763b449d9df7694cb136a492bb9c36d0.png)  


```
grep '3306'|awk -F ' ' '{print $5}'|awk -F: '{print $1}'|sort|uniq|wc -w|awk -F ' ' '{print "TOTAL_IP "$1}'&&cat nowcoder.txt|grep '3306'|awk -F ' ' '{print $6}'|awk -F: '{print $1}'|sort|uniq -c|awk -F ' ' '{print $2 " " $1 "\n" "TOTAL_LINK "$1}'
```

```
awk -F '[ :]+' 'NR>2&&/:3306/{a[$6]++;n++;b[$NF]++}END{print "TOTAL_IP "length(a);print "ESTABLISHED "b["ESTABLISHED"];print "TOTAL_LINK "n}'
```

>这个真答案编写代码，不过个人觉得比直接echo要好
>

## 第三十三题 业务分析-提取值

![picture 29](../assets/images/e3594e46f1eeeff167ff9fbee3381c1419cd2fbd6748a4c88b75276ece164ad0.png)  


```
awk  'BEGIN{FS="[:,]"}/version/{print "serverVersion:"$NF}/number/{print "serverName:"$NF}/OS/{print "osName:"$(NF-2);print "osVersion:"$NF}'
```


>这个逻辑好奇怪给人一种echo解决的方案，写死了
>

![picture 30](../assets/images/ba948224d0fe0ea28663e0fcb6c93de797d2e3ab3f9ed26cb603aa065e3d7a59.png)  

>评论区的吐槽
>


## 第三十四题 ps分析-统计VSZ,RSS各自总和

![picture 31](../assets/images/ed098b0d46a9b2ad692a6d447c292e991e850841996dc75101757076f7edb83b.png)  


```
awk 'BEGIN{print "MEM TOTAL"}{a+=$5;b+=$6}END{c=a/1024;d=b/1024;printf "VSZ_SUM:%.1fM,RSS_SUM:%.3fM",c,d}'
```

>这些数据分析题有种前面题综合到这里,好了结束了有些对我来说还是有点难想的也算给自己查漏补缺
>
