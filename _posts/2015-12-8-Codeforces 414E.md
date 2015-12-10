---
layout: post
title: 'CodeForces 414E'
date: 2015-12-08 20:40
comments: true
categories: 
---
周末的考试题...lj以为是水题..然而才28人A...<br>
当时比较naive......以为要lct..然而又想不出lct做法....想了想维护dfs序不太兹磁..就看题跑了..<br>
看了下题解...发现的确可以用维护dfs序来搞.<br>
具体做法就是..<br>
操作1,2用lct做就好啦<br>
操作3的话....对dfs序搞个upper,lower...表示这个区间内的deep范围..然后打个tag就能兹磁区间剪切了>_< <br>
搞upper,lower对的原因是因为这个序列是+1,-1的..所以对于每一个在lower和upper之间的数都能取到..<br>
用fhq_treap维护一下就好了<br>
![image](http://7xoz7t.com1.z0.glb.clouddn.com/未命名.jpg)<br>
Excited!
