---
title: "Greedy算法3道题"
date: 2022-04-16T15:06:12+08:00
draft: false
description: "Greedy算法3道题，含分析、伪代码、正确性证明"
categories:
  - Algorithm
---



### 3 Cross the River

Some people want to cross a river by boat. Each person has a weight, and each boat can carry an equal maximum weight limit. Each boat carries at most 2 people at the same time, provided the sum of the weight of those people is at most boat’s weight limit. Return the minimum number of boats to carry every given person. 

Note that it is guaranteed each person can be carried by a boat.

(1). Describe the basic idea of your algorithm in natural language

首先将 people 数组升序排序，定义俩个指针，初始时，一个指向数组的头部，另一个指向数组的尾部，俩个指针所指向的人的体重之和如果小于或等于 limit，那么这俩个人可以坐一艘船离开，然后俩个指针相向各移动一位；否则，只能是体重大的乘船离开，指向尾部的指针向左移动一位。

(2). Pseudo-code

```pseudocode
function crossTheRiver(people, limit)
1: sort people in ascending order;
2: i = 0;
3: j = people.num - 1;
4: boats = 0;
5: while i <= j do
6:     if people[i] + people[j] <= limit then
7:         i += 1;
8:         j -= 1;
9:         boats += 1;
10:    else then
11:        j -= 1;
12:        boats += 1;
13:    end if
14:end while
15:return boats;
```

(3). Describe the greedy-choice property and optimal substructure

贪心选择思想：体重最小的人选择和体重最大的人乘同一艘船，这样能最大利用船的承重，如果这俩个人不满足要求，则体重最大的人单独乘船离开。

从 people 中去除乘船离开的人，对于剩下的人而言，问题的结构没有发生改变，即问题的规模由 n 变成 n -1 或者 n - 2，使用同样的方法来分配乘船。

所以最优子结构是：每次在当前剩余的人当中选择体重最小和体重最大的一起乘船，如果不能同时乘船，那么让体重最大的人单独乘船。

(4). Prove the correctness of your algorithm

首先对所有人的体重从小到大排序，这样对于体重最小的人，如果他能和体重最大的人乘同一艘船，那么他能和所有人乘同一艘船。所以贪心选择的思想是这个体重最小的人选择和体重最大的人乘同一艘船，可以最大利用船的承重。当俩人离开后，对剩下的人而言问题的结构没有发生变化，采用同样的方法来分配乘船。

而对于体重最小的人，如果他不能和体重最大的人乘同一艘船，那么没有人能和体重最大的人乘同一艘船，所以体重最大的人应该单独乘船离开。当他离开后，对剩下的人而言问题的结构没有发生变化，采用同样的方法来分配乘船。

(5). Analyse the complexity of your algorithm

排序的时间复杂度为 O(nlogn)，而使用双指针计算的复杂度为 O(n)，所以总的时间复杂度为 O(nlogn)。



### 6 Maximum Number of Coins You Can Get

There are 3n piles of coins of different size, you and your friends will take piles of coins as follows: In each step, you will choose any 3 piles of coins (not necessarily consecutive). Your friend Alice will pick the pile with the maximum number of coins. You will pick the pile with submaximal number of coins. Your friend Bob will pick the last pile. Repeat until there are no more piles of coins. Given an array of integers piles where piles[i] is the number of coins in the i-th pile. Return the maximum number of coins which you can have.

(1). Describe the basic idea of your algorithm in natural language

首先将 piles 按照升序排序，piles 的大小为 3n，则能够获得的最多的硬币的数量的组成的堆为第 n+1 堆，第 n + 3堆，...，第 3n-1 堆的硬币数量和。

(2).  Pseudo-code

```pseudocode
function maxNumOfCoins(piles)
1: sort piles in ascending order;
2: n = piles.length / 3;
3: max = 0;
4: for i = n to 3*n-2 , i += 2 do
5:     max += piles[i];
6: end for
7: return max;
```

(3). Describe the greedy-choice property and optimal substructure

贪心选择思想：3n堆硬币中，因为Alice每次必须选择最多的硬币，我必须获得第二多的硬币才能使自己的收益最大，为了使Bob不影响我的收益，Bob取最少的硬币；所以每次取的3堆中，第一堆和第二堆的硬币数量应该是当前所有堆中最多的和第二多的，第三堆的硬币数量该是当前堆中硬币最少的。

取当前所有堆中第一多和第二多以及最少的3堆，除去这3堆后，对于剩下的 3n-3 堆，问题的结构不变，仍以同样方法取即可。

所以最优子结构是：每次都是取当前所有堆中的第一多、第二多、最少的3堆，这3堆中，我取第二多的堆，所有堆的硬币总数就是我的最大收益。

(4). Prove the correctness of your algorithm

3n堆硬币中，硬币最多的一堆无论如何都会被Alice取走，当Alice每次取走第一多的，显然我必须取下剩余的最多的一堆，才使自己的收益最大。所以关键是Bob怎么取。

如果Bob第一次取了 3n 堆中第3多的，第二次我只能取第5多的，而如果Bob第一次取的是很少的一堆硬币，第二次我能取第4多的，显然Bob的取法会影响我的最大收益，如果Bob每次都是取最少的一堆硬币，就把影响降至最低。

(5). Analyse the complexity of your algorithm

n 为 piles 数组的长度，则排序的时间复杂度为 O(nlogn)，而遍历数组的时间复杂度为 O(n)，所以总的时间复杂度为 O(nlogn)。



### 4 Permutation Partition

Bob is given a permutation p~1~, p~2~, . . . , p~n~ of integers from 1 to n and an integer k, such that 1 ≤ k ≤ n. A permutation means that every number from 1 to n is contained exactly once.

Consider all partitions of this permutation into k disjoint segments. Formally, a partition is a set of segments {[s~0~, s~1~], [s~1~ + 1, s~2~], . . . , [s~k−1~ + 1, s~k~]}, such that: 1 = s~0~ < s~1~ < s~2~ < . . . < s~k−1~ < s~k~ = n. Two partitions are different if there exists a segment that lies in one partition but not the other.

Bob wants to calculate the partition value, defined as  i=1 max s~i−1~≤ j ≤ s~i~ p~j~ for all possible partitions of the permutation into k disjoint segments. Please help him find the maximum possible partition value over all such partitions, and the number of ways to make partition with this value.

(1). Describe the basic idea of your algorithm in natural language

定义变量 max 为最大分区值并初始化为0，定义变量 num 为达到这个最大分区值的方法数并初始化为1，定义变量 prev 记录上一个分组的最大值的位置并初始化为0，遍历排列中的每个元素，如果某个元素的值大于 n - k，则将 max 加上其值，且如果不是第一个元素，则将该元素的位置与 prev 的差值和 num 相乘，再将 prev 指向该元素的位置。

(2).  Pseudo-code

```pseudocode
function partition(permutation, k, n)
1: max = 0;
2: num = 1;
3: prev = 0;
4: for i = 0 to n - 1 do
5:     if permutation[i] > n - k then
6:         max += permutation[i];
7:         if i > 0 then
8:             num = num * (i - prev)
9:         end if
10:	       prev = i
11:    end if
12:end for
13:return max, num
```

(3). Describe the greedy-choice property and optimal substructure

贪心选择思想：选出排列中的最大的 k 个数，将这 k 个数分别划分在 k 个不同组中。

(4). Prove the correctness of your algorithm

对于将排列分k组，选出最大的分区值，显然最大的分区值为排列中最大的 k 个数的相加和，所以把这 k 个数分别划分在 k 个不同组即可。

对如如何取得最大的 k 个值，本算法没有选择排序，只需要在遍历排列元素的时候判断该元素值是否大于 n - k 即可，因为这个排列是由 1 ~ n 组成的无重复元素的 n 大小的排列，只要元素值大于 n - k 即可判断属于最大的 k 个值之列。

在确定了 k 个最大值之后，每两个最大值之间的划分方法为这俩个最大值之间的元素的个数加1，所以在所有最大值之间划分的方法就是这些值逐个相乘即可。

(5). Analyse the complexity of your algorithm

该算法需要遍历整个排列，所以时间复杂度为 O(n)，n 为排列的长度。