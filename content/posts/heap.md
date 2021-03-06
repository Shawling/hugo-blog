+++
title = "堆和堆的应用"
date = 2021-02-02T18:56:36+08:00
tags = [""]
categories = [""]
draft = false

+++



1. 是一个完全二叉树
2. 堆中每个节点的值都大于等于（或小于等于）其左右子节点的值，分别对应大顶堆，小顶堆



因为完全二叉树适合用数组存储，所以堆通常放在一个数组中

堆的存储下标从1开始，方便计算（哨兵）。加入节点为i，左节点的地址为2*i，右节点的位置为2*i + 1

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn9e32yb5nj30ai0ba0sw.jpg)

插入

由于性质1，堆的插入直接放在数组尾部。假设是大顶堆，如果插入值比父节点小，则直接计算地址交换，直到循环到根节点。

插入操作时间复杂度为O(logn)

这是从下往上的堆化方法

删除顶部元素

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn9e3x8bv3j30vq0nstb2.jpg)

删除顶部节点后，拿最右侧节点填补，并拿取左右子节点的最大值进行交换，直到到达底部，这是从上往下的堆化方法



堆排序

建堆

使用从上往下的方法建堆，时间复杂度O(n)

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn9e4865i7j30vy0m4jt3.jpg)

排序

先建堆，然后逐个将顶部元素交换到最后，再堆化，可以得到一个最大值。循环整个数组，可以得到排好序的数组。时间复杂度O(nlogn)



堆排序时间复杂度跟快排差不多，但是

1. 在遍历有序数组时，每次选择时内存不连续，无法很好用到cpu缓存
2. 建堆的第一步会把数组打乱，对于相对有序的数据，增加了逆序度，变相增加了操作次数。而快排的操作次数不会多于逆序度

所以快排在排序方面比堆排序更常用





堆的其他应用

堆在排序时，没有快排好用。但是如果需要一个插入，取出性能稳定的有序数据集时，堆比较好用。

堆的插入，删除数据并堆化复杂度为O(logn)。如果是快排排序号的数组，插入复杂度为O(n) （二分查找到位置后再逐个后移元素插入），删除同理。所以堆的动态更新性能比已排序数组好。



优先队列类型

优先队列一般用堆来实现，插入数据，删除数据都是O(logn)

合并有序小文件（比如日志文件）

建立一个堆，节点数量等于文件数量k，从多个小文件中读取第一个数据进堆并堆化。然后从堆顶取出一个数据，同时从它所属的文件中再读取一个数据放到堆顶并堆化，复杂度O(logk)。总的复杂度O(nlogk)，需要的额外内存空间O(k)

计时器



求TOPK类型



求动态数据集的中位数

先将数据从小到大排序，将前半部分n/2或n/2+1放入大顶堆，将后半部分放入小顶堆。这样子两个堆头（偶数）或者大顶堆堆头（奇数）就是好数据集的中位数。

在有新数据时，先与大顶堆比较，如果小于等于堆头，则放入大顶堆，如果节点数超过一半，则删除堆头并放入小顶堆，维持两个堆规范。

这样子求动态数据集的中位数复杂度为O(1)，插入新数据复杂度为O(logn)



求百分位数据同理