---
title: LeetCode Shell复盘
author: LingMj
data: 2025-04-27
categories: [LeetCode]
tags: [upload]
description: 难度-Easy
---

## LeetCode题目练习

>尝试一下LeetCode的shell题目，正常普通一共四道题，我做了一下感觉不如群主出的题目有意思不过练习一下还行，我的方式很普通所以你有好的点子或者你的方法优雅可以联系我，我很愿意接受知识，哈哈哈
>

## 题目一

![picture 0](../assets/images/c4ad14defb4f7fc6322670174da371537cce14ea225d0ad24398b3f056cf96fd.png)  

>这里我一看直接head加tail即可，但是这个题目埋了一个位置是不满足10算最后一个所以我将方法直接使用为sed,同样awk也是可以完成的
>

```
sed -n '10p' file.txt||sed -n '$p' file.txt
awk 'NR==10{print;exit}END{if(!NR)print}' file.txt
```

## 题目二

![picture 1](../assets/images/e5e9934c634e3dc463e1e96ec4c87f2bd12e02eaf7d6569b823fa3cc48054daf.png)  

>这个我一开始就使用了grep的笨方法但是我记得可以进行统一部分不动异或动形式但是没通过
>

```
grep -E '^([0-9]{3}-[0-9]{3}-[0-9]{4}|\([0-9]{3}\) [0-9]{3}-[0-9]{4})$' file.txt
grep -P '^([0-9]{3}-|\([0-9]{3}\) )?[0-9]{3}-[0-9]{4}$' file.txt
awk '/^[0-9]{3}-[0-9]{3}-[0-9]{4}$|^\([0-9]{3}\) [0-9]{3}-[0-9]{4}$/' file.txt
```

## 题目三

![picture 2](../assets/images/045a9c06a0e1198457444445db62e519da55e094a363b4e0970041a9c21de879.png)  

>这个我真没啥思路，看了gtp思路是awk加for，当然我觉得不优雅而且肯定有更好方案，不过我不会，希望有大佬提供方案
>

```
awk '{
    for(i=1; i<=NF; i++){
        a[i] = (a[i] ? a[i] " " $i : $i)
    }
} END {
    for(i=1; i<=NF; i++) print a[i]
}' file.txt
```

## 题目四

![picture 3](../assets/images/121529505dfa3b3b4ac787952d3ea9e6e723e7a3fd95b7ce48c399b8ef9995db.png)  

>这个的话先进行换行操作，让数据一行一个，然后去除空行，接着awk进行操作，sort -k2 nr进行按第二列降序,不过我觉得还有更好的方案，目前没想到希望有大佬给我来点操作
>

```
cat words.txt| tr ' ' '\n' | sed '/^$/d' | awk '{word[$1]++} END {for (i in word) print i, word[i]}' | sort -k2 -nr
```



>由于leetcode就四道shell题，所以这个文章到这里结束了
>

