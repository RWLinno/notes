---
title : 后缀树
tags : 字符串
date : 2021-7-25
---



# 后缀树 suffix tree

### 概念

把所有后缀保存在字典树上

1.每一条root到叶子的路径，记录了一个后缀子串

2.compressed trie

3.由于被压缩，用[s,e]记录边上的子串信息

##### 应用

string matching

longest repeated substring

longest common substring

longest palindromic substring