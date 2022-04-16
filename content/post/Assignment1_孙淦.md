---
title: "DC算法6道题"
date: 2022-04-16T15:06:12+08:00
draft: false
description: "DC算法6道题，含分析、伪代码、正确性证明"
categories:
  - Algorithm
---



### 1 Divide and Conquer

​		Given an integer array numbers and an integer k, please return the k-th largest element in the array.
​		Your algorithm's runtime complexity must be in the order of O(logn), prove the correctness and analyze the complexity.(k is much smaller than n, n is the length of the array.)

(1) Describe algorithm in natural language

​		本题的解决方法是在快速排序的基础上做出选择。对数组A[l...r]做快速排序的过程是：首先从数组中选择一个元素x作为主元，将数组A[l...r]划分成俩个子数组A[l...q-1]、A[q+1...r]，使得A[l...q-1]中的每一个元素都小于A[q]，且A[q]小于A[q+1...r]中的每一个元素，x的最终位置就是q；然后通过递归调用快速排序对子数组A[l...q-1]、A[q+1...r]进行排序；因为子数组都是原地址排序的，所以不需要合并操作。

​		每次划分操作后，可以确定一个元素的最终位置，所以如果某次划分操作后的q为倒数第k个下标后，则返回A[q]，否则，如果q小于倒数第k个下标，就递归右边的子数组，否则递归左边的子数组，将原来递归俩个子数组变成了只递归一个子数组。

(2) Pseudo-code

```pseudocode
function findKthLargest(A, l, r, k1) //k1 = n - k, n is the lenth of A
1: q = partition(A, l, r)
2: if q == k1 then
3:     return A[k1];
4: else if q < k1 then
5:     return findKthLargest(A, q+1, r, k1)
6: else
7:     return findKthLargest(A, l, q-1, k1)
8: end if

function partition(A, l, r)
1: Choose an index x from [l,r] at random and use A[x] as pivot
2: Swap A[x] with A[r]
3: pivot = A[r]; i = l;
4: for j = l to r - 1 do
5:     if A[j] < pivot then
6:         Swap A[i] with A[j];
7:         i ++;
8:     end if
9: end for
10:Swap A[i] with A[r];
11:return i;
```

(4) Prove the correctness of algorithm

​		上述算法采用基于快速排序的选择方法。对于数组A[l,r]，每次选择一个元素作为pivot，设pivot下标为q，划分为俩个数组A[l, q-1]和A[q+1, r]，使得数组A[l, q-1]中的每一个元素都小于pivot，数组A[q+1, r]中的每一个元素都大于pivot，每经过一次划分数组，pivot元素的位置都是可以最终确定的，所以有以下三种情况：

​		若pivot的下标q = n - k，其中n是A数组的长度，则返回A[q]，A[q]即为答案；

​		若pivot的下标q < n - k，就对右子区间[q+1, r]实行递归调用；

​		若pivot的下标q > n - k，就对左子区间[l, q-1]实行递归调用；

​		因此，基于快速排序的选择方法必能找到第k大的数字。

(5) Analyze the complexity of algorithm

- 最坏时间复杂度

  若每次选择的都是数组中最大或者最小的元素，则有T(n) <= T(n-1) + cn，则复杂度为O(n^2)

- 最好时间复杂度

  若每次选择的为中间的元素，则有T(n) <= T(n/2) + cn，则时间复杂度为O(n)

- 平均时间复杂度

  O(n)
  
  

### 2 Divide and Conquer

​		Consider an n-node complete binary tree T, where n = 2^d − 1 for some d. Each node v of T is labeled with a real number Xv. You may assume that the real numbers labeling the nodes are all distinct. A node v of T is a local minimum if the label Xv is less than the label Xw for all nodes w that are joined to v by an edge.

​		You are given such a complete binary tree T, but the labeling is only specified in the following:

​		implicit way: for each node v, you can determine the value Xv by probing the node v.

​		Show how to find a local minimum of T using only O(logn) probes to the nodes of T.

(1) Describe algorithm in natural language

​		从二叉树的根节点出发，如果根节点的左右子节点的值均大于根节点的值，则返回根节点的值，否则至少有一个子节点的值小于其值。选择拥有较小值的子节点作为根节点递归。



(2) Pseudo-code

```pseudocode
function findLocalMinNode(root)
1: if root == NIL then
2:     return;
3: end if
4: if root.lchild != NIL and root.rchild != NIL then
5:     if root.val < root.lchild.val and root.val < root.rchild.val then
6:         return root.val;
7:     else if root.lchild.val < root.rchild.val then
8:         findLocalMinNode(root.lchild);
9:     else then
10:        findLocalMinNode(root.rchild);
11:    end if
12:else then
13:    return root.val;
14:end if
```



(4) Prove the correctness of algorithm

​		在上述算法过程中，从完全二叉树的根节点开始遍历，如果根节点的值小于俩个子节点的值，则根节点就是一个局部最小值点，返回根节点的值即可。否则，至少存在一个子节点的值小于根节点，将较小值的子节点作为根节点递归即可，这样一定能找到一个局部最小值。最坏的情况是遍历到叶节点，叶节点只其父节点与其相连，父节点的值大于叶节点的值，则该叶节点就是局部最小值。

(5) Analyze the complexity of algorithm

​		时间复杂度为O(logn)

### 3 Divide and Conquer

​		Given an integer array, one or more consecutive integers in the array form a sub-array. Find the maximum value of the sum of all sub-array.
​		Please give an algorithm with O(nlogn) complexity.

(1) Describe algorithm in natural language

​		将区间[l,r]划分为左区间[l,mid]和右区间[mid+1,r]，对左右子区间分治求解，当递归到区间长度为1的时候，这时需要将左区间[l,mid]的信息和右区间[mid+1,r]的信息合并成区间[l,r]的信息。

​		对于一个区间[l,r]，需要维护的变量有：lSum表示[l,r]内以l为左端点的最大子序列和，rSum表示[l,r]内以r为右端点的最大子序列和，mSum表示[l,r]内的最大子序列和，iSum表示[l,r]的区间和。将左右子区间的信息合并到[l,r]按照以下方式：

​		[l,r]区间的iSum就等于左区间的iSum加上右区间的iSum;

​		[l,r]区间的lSum就等于左区间的lSum和左区间的iSum加上右区间的lSum俩者的最大值；

​		[l,r]区间的rSum就等于右区间的rSum和右区间的iSum加上左区间的rSum俩者的最大值；

​		[l,r]区间的mSum取左区间的mSum，右区间的mSum，左区间的rSum加上右区间的lSum三者的最大值；

​		而对于长度为1的区间[i,i]上面4个值都等于A[i]，最终所求的mSum就是答案；

(2) Pseudo-code

```pseudocode
function maxSubArraySum(A, l, r)
1: if l == r then
2:     return {A[r], A[r], A[r], A[r]}; //返回数组中的元素从左到右分别是iSum,lSum,rSum,mSum
3: end if
4: mid = ⌊(l+r)/2⌋
5: lSub = maxSubArraySum(A, l, mid);
6: rSub = maxSubArraySum(A, mid+1, r);
7: iSum = lSub[0] + rSub[0];
8: lSum = max{lSub[1], lSub[0]+rSub[1]};
9: rSum = max{rSub[2], rSub[0]+lSub[2]};
10:mSum = max{lSub[3], rSub[3], lSub[2]+rSub[1]};
11:return {iSum, lSum, rSum, mSum}

```



(4) Prove the correctness of algorithm

​		本题采用分治解法。对于一个区间[l, r]，取 mid = ⌊(l+r)/2⌋，划分成俩个区间 [l, mid] 和 [mid+1, r]，同样对这俩个区间递归划分，直到区间的长度为1的时候，开始合并区间的信息。对于一个区间[l, r]需要维护的变量有4个：

- lSum 表示 [l,r] 内以 l 为左端点的最大子序列和
- rSum 表示 [l,r] 内以 r 为右端点的最大子序列和
- mSum 表示 [l,r] 内的最大子序列和
- iSum 表示 [l,r] 内所有元素的和

对于长度为1的区间[i,i]，以上4个变量相等，为A[i]；对于合并后长度大于1的区间，上述4个变量更新的方式如下：

- [l,r]区间的iSum就等于左区间的iSum加上右区间的iSum;
- [l,r]区间的lSum就等于左区间的lSum和左区间的iSum加上右区间的lSum俩者的最大值；
- [l,r]区间的rSum就等于右区间的rSum和右区间的iSum加上左区间的rSum俩者的最大值；
- [l,r]区间的mSum取左区间的mSum，右区间的mSum，左区间的rSum加上右区间的lSum三者的最大值；

最终的mSum就是整个区间的最大的子序列的和。

(5) Analyze the complexity of algorithm

​		时间复杂度为O(n)。



### 4 Divide and Conquer

​		Given an array of integers numbers sorted in ascending order, find the starting and ending position of a given target value. If the target is not found in the array, return [-1, -1]. For example, if the array is [5, 7, 7, 8, 8, 10] and the target is 8, then the output should be [3, 4].
​		Your algorithm's runtime complexity must be in the order of O(log n), prove the correctness and analyze the complexity.

(1) Describe algorithm in natural language

​		本题的解决方式为使用二分查找。需要寻找数组中第一个等于target的位置和最后一个等于target的位置。

​		二分查找第一个等于target的位置：当A[mid] >= target时，往左半区间寻找，r = mid；当A[mid] < target时，往右半区间查找，l = mid + 1；当l = r二分查找结束，如果A[r] != target说明数组中不存在target，返回[-1, -1]。

​		二分查找最后一个等于target的位置：当A[mid]<=target时，往右半区间查找，l = mid；当A[mid] > target时，往左半区间查找，r = mid - 1；当l = r二分查找结束，此时的r就是第二次二分查找的位置，返回区间即可。

(2) Pseudo-code

```pseudocode
function binarySearch(A, n, target)
1: if n == 0 then
2:     return {-1, -1};
3: end if
4: l = 0;
5: r = n - 1;
6: while l < r do //二分查找开始位置
7:     mid = ⌊(l+r)/2⌋
8:     if A[mid] >= target then
9:         r = mid;
10:    else
11:        l = mid + 1;
12:    end if
13:end while
14:if A[r] != target then
15:    return {-1, -1};
16:end if
17:L = r;
18:l = 0;
19:r = n - 1;
20:while l < r do //二分查找结束位置
21:    mid = ⌊(l+r+1)/2⌋
22:    if A[mid] <= target then
23:        l = mid;
24:    else
25:        r = mid - 1
26:    end if
27:end while
28:return {L,r};
```



(4) Prove the correctness of algorithm

​		本题采用二分查找的方式。需要查找目标值target第一次出现的位置和最后一次出现的位置。	

- 查找目标值第一次出现的位置：取mid = ⌊(l+r)/2⌋，将区间[l,r]划分成俩个区间[l,mid]和[mid+1,r]
  - 当 A[mid] >= target，往左半区间查找，r = mid;
  - 当 A[mid] < target，往右半区间查找，l = mid + 1;
  - 当 l = r，二分查找结束，如果A[r] != target，说明数组中不存在目标值target，返回[-1, -1]。
- 查找目标值最后一次出现的位置：取mid = ⌊(l+r+1)/2⌋，将区间[l,r]划分成俩个区间[l,mid-1]和[mid,r]
  - 当 A[mid] <= target，往右半区间查找，l = mid;
  - 当 A[mid] > target，往左半区间查找，r = mid - 1;
  - 当 l = r，二分查找结束，r就是目标值最后一次出现的位置。

因此，采用俩次二分查找可以找到目标值的开始出现位置和最后出现位置。

(5) Analyze the complexity of algorithm

​		时间复杂度：O(log n) ，其中 n 为数组的长度。二分查找的时间复杂度为O(log n)，一共会执行两次，因此总时间复杂度为O(log n)。



### 5 Divide and Conquer

​		Given a convex polygon with n vertices, we can divide it into several separated pieces, such that every piece is a triangle. When n = 4, there are two different ways to divide the polygon; When n = 5, there are five different ways.
​		Give an algorithm that decides how many ways we can divide a convex polygon with n vertices into triangles.

(1) Describe algorithm in natural language

​		一般 n 个顶点的凸多边形可以分成 m = n - 2 个三角形。

​		将n个顶点的凸多边形的顶点依次序标记为P1、P2、P3、...、Pn， 凸多边形的任意一条边必属于某一个三角形，以这条边为基准，设这条边的俩个顶点分别是P1、Pn，然后在凸多边形中找一个任意不属于该边的顶点Pk，（2<=k<=n-1），组成一个三角形，这个三角形将原来的凸多边形分割成俩个凸多边形，其中一个凸多边形的顶点数目为n-k+1，另一个凸多边形的顶点数目k；

​		则 f(n) 的解由 f(n-k+1) 和 f(k) 组成，f(n) = f(n-k+1) * f(k)，而 2<=k<=n-1，所以原式可以分解为 ：f(n) = f(n-1) * f(2) + f(n-2) * f(3) + ... + f(2) * f(n-1)，另外f(2) = f(3) = 1，f(2)表示这个三角形只将原凸多边形分割成一个凸多边形，可以认为f(2) = 1。这样就把 f(n) 问题的求解分成一个个子问题的求解，直到子问题的规模为n = 2或者n = 3。

(2) Pseudo-code

```pseudocode
function divideConvexPolygon(n)
1: initialize an array A with size of n + 1, and assign a value of 0 to each element;
2: A[2] = A[3] = 1;
3: return subProblem(A, n);
function subProblem(A, n)
1: if A[n] != 0 then
2:     return A[n];
3: end if
4: sum = 0
5: for k = 2 to n - 1 do
6:     if A[k] == 0 then    
7:         A[k] = subProblem(A, k);
8:     end if
9:     if A[n-k+1] == 0 then
10:        A[n-k+1] = subProblem(A, n-k+1);
11:    end if
12:    sum += A[k] * A[n-k+1];
13:end for
14:A[n] = sum;
15:return A[n];
```



(4) Prove the correctness of algorithm

​		本算法采用自顶向下的带记忆的递归解法。具有n个顶点的凸多边形的任意一条边必属于某一个三角形，那么以这条边为基准，然后在凸多边形中找一个任意不属于这条边上的顶点，组成一个三角形，这个三角形会将原凸多边形分割成俩个小的凸多边形（也可能只有一个小的凸多边形），这样就将大问题的解分解成俩个小问题的解，同样对于小凸多边形以同样的方法将其分解为更小的凸多边形。

​		假设凸多边形的n个顶点依次按序标记为P1、P2、P3、...、Pn，以P1-Pn作为基准边，任意选择一个不属于该边的顶点Pk，（2<=k<=n-1），组成的三角形将原凸多边形分割成俩个小的凸多边形，则f(n)的解由 f(n-k+1) 和 f(k) 组成，f(n) = f(n-k+1) * f(k)，k有n-2种选择，所以原式可以分解为 ：f(n) = f(n-1) * f(2) + f(n-2) * f(3) + ... + f(2) * f(n-1)，这样子问题递归分解下去直到遇到f(2)和f(3)不再分解，f(2) = f(3) = 1。所以这样自顶向下的递归分解子问题最终一定能求出f(n)的问题。

(5) Analyze the complexity of algorithm

​		由于该算法设置了一个数组A去记录每一步计算中产生的f(k)的子问题的解，避免f(k)子问题的重复计算，子问题就是f(2)，f(3)，...，f(n)，所以本算法的时间复杂度为O(n)。



### 6 Divide and Conquer

​		Given an array of k linked-lists lists, each linked-list is sorted in ascending order. Given an O(knlogk) algorithm to merge all the linked-lists into one sorted linked-list. (Note that the length of a linked-lists is n)

(1) Describe algorithm in natural language

​		将 k 个链表俩俩一组，共有 k/2 组，每一组俩个链表合并成一个链表，得到 k/2 个链表；将 k/2 个链表俩俩一组，共有 k/4 组，每一组俩个链表合并成一个链表，得到 k/4 个链表；依此类推，最终得到合并的结果。

(2) Pseudo-code

```pseudocode
function mergeKLists(A, l, r)
1: while l < r do
2:     mid = ⌊(l+r)/2⌋
3:     return merge(mergeKLists(A, l, mid), mergeKLists(A, mid+1, r));
4: end while
5: return A[l];

function merge(L1, L2)
1: a = L1, b = L2;
2: initialize two list node: head, tail;
3: tail = head;
4: while a != NIL && b != NIL do
5:     if a.val < b.val then
6:         tail.next = a;
7:         a = a.next;
8:     else then
9:         tail.next = b;
10:        b = b.next;
11:    end if
12:    tail = tail.next
13:end while
14:tail.next = a ? a : b;
15:return head.next;
```

(4) Prove the correctness of algorithm

​		将 k 个链表俩俩一组，共有 k/2 组，每一组俩个链表合并成一个链表，得到 k/2 个链表；将 k/2 个链表俩俩一组，共有 k/4 组，每一组俩个链表合并成一个链表，得到 k/4 个链表；依此类推，最终得到合并的结果。

(5) Analyze the complexity of algorithm

​		俩个长度均为n的升序链表的合并的时间复杂度为O(2n)，第一轮合并时有 k/2 组，每组的时间复杂度为O(2n)，则第一轮合并的总的时间复杂度为O(kn)；第二轮合并时有 k/4 组，每组的时间复杂度为O(4n)，则第二轮合并的总的时间复杂度为O(kn)；一共logk次合并，则总的时间复杂度为O(knlogk)。
