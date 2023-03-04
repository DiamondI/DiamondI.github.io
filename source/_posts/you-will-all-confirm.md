---
title: Puzzle 1 - You Will All Confirm
categories: Misc
---

# Programming for the Puzzled

这是MIT的一门公开课，讲师是Prof. Srini Devadas，课程的level是本科生。

<!-- more -->

**Prof. Srini Devadas**

![Prof. Srini Devadas](https://www.csail.mit.edu/sites/default/files/styles/headshot/public/images/migration/devadas.jpg?h=5636fc5d&itok=EUGLSbU-)
## Puzzle 1: You Will All Conform

### Puzzle Description

[You Will All Confirm(PDF)](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-s095-programming-for-the-puzzled-january-iap-2018/puzzle-1-you-will-all-conform/MIT6_S095IAP18_Puzzle_1.pdf)

**问题描述（用自己的话描述）**

有一个队列的人排队进入棒球比赛的赛场，他们都戴有棒球帽，有的人帽檐朝前（Forward-F），有的人帽檐朝后（Backward-B）。而看门人需要让他们的帽檐都朝同一个方向，才能放这些排队的人入场。看门人可以对队列里的人发出“第i个位置的人调整帽檐方向”以及“第i到第j个位置的人调整帽檐方向”的命令。问看门人最少需要发出多少个命令，能够使这些人帽檐方向一致呢？

显然，这个问题找的是连续的'F'或'B'所出现的最小次数。

### Natural Algorithm

最自然的想法，就是遍历两遍，第一遍的时候找出所有连续的'F'并记录个数，第二遍找出所有连续的'B'并记录个数。两相比较，选择小的那个输出。

不过，稍加思索便会发现，其实用不着两次遍历，一次遍历就可以记录下这些信息。只需要在帽檐方向发生变化的时候增加连续的'F'或'B'的个数即可。

**Natural Algorithm - One Pass:**

```python
def pleaseConform(caps):
    #Initialization
    start = 0
    forward = 0
    backward = 0
    intervals = []

    #Determine intervals where caps are on in the same direction
    for i in range(1, len(caps)):
        if caps[start] != caps[i]:
            # each interval is a tuple with 3 elements (start, end, type)
            intervals.append((start, i - 1, caps[start]))
            
            if caps[start] == 'F':
                forward += 1
            else:
                backward += 1
            start = i

    #Need to add the last interval after for loop completes execution
    intervals.append((start, len(caps) - 1, caps[start]))
    if caps[start] == 'F':
        forward += 1
    else:
        backward += 1
 
##    print (intervals)
##    print (forward, backward)
    if forward < backward:
        flip = 'F'
    else:
        flip = 'B'
    for t in intervals:
        if t[2] == flip:
            #Exercise: if t[0] == t[1] change the printing!
            print ('People in positions', t[0],
                   'through', t[1], 'flip your caps!')
```

可是这样写有一个小问题，那就是每次记录次数的时候都是发生在帽檐朝向变化的时候。这样一来，当一次遍历结束时，最后一个连续的区间会被忽略掉。为了处理这个问题，上面的代码在遍历结束之后，显式地将最后一个区间考虑了进去。

---

这样写似乎不够优雅，有什么办法能够把这个特殊处理去掉呢？

讲师Prof. Srini Devadas提出了一个非常优雅的方案：在代表排队的人的list之后添加一个'End'元素。这样一来，遍历到'End'元素的时候，不论最后一个区间是什么，它都会被考虑进来。

**Natural Algorithm - One Pass & Optimization:**

```python
def pleaseConformOpt(caps):
    #Initialization
    start = 0
    forward = 0
    backward = 0
    intervals = []

    caps = caps + ['END']

    #Determine intervals where caps are on in the same direction
    for i in range(1, len(caps)):
        if caps[start] != caps[i]:
            # each interval is a tuple with 3 elements (start, end, type)
            intervals.append((start, i - 1, caps[start]))
            
            if caps[start] == 'F':
                forward += 1
            else:
                backward += 1
            start = i

    if forward < backward:
        flip = 'F'
    else:
        flip = 'B'
    for t in intervals:
        if t[2] == flip:
            #Exercise: if t[0] == t[1] change the printing!
            print ('People in positions', t[0], 'through', t[1], 'flip your caps!')
```

这么一来，就可以用最少的代码来处理边界情况了。

### Another Optimization

课程讲到上面的代码的时候，我以为已经是最优的写法了。没想到讲师Prof. Srini Devadas又给出了一段只有**8行**的代码！

**Natural Algorithm - One Pass & Another Optimization:**

```python
def pleaseConformOnepass(caps):
    caps = caps + [caps[0]]
    for i in range(1, len(caps)):
        if caps[i] != caps[i-1]:
            if caps[i] != caps[0]:
                print('People in positions', i, end='')
            else:
                print(' through', i-1, 'flip your caps!')
```

这段代码将list中的第一个元素放在了末尾，并且最后输出的时候只输出了与list中第一个元素不同的元素，即：如果第一个元素是'F'，那么这段代码只输出'B'的区间；反之亦然。

初见时百思不得其解，为什么只要输出与第一个元素不同的元素就好了啊！！！这段代码怕不是有bug！！！一定是讲师写错了！！！在这样的念头驱动下，我不停地尝试找反例，希望能找到一个例子使得上面代码的结果不符合题目要求。然而我想了好久都没想到反例，这时我才开始尝试论证上述代码的正确性。

---

**并不严谨的正确性论证：**

证明第一个元素的连续区间数大于等于另一个元素，更具体地，第一个元素的连续区间数或者等于另一个元素，或者等于另一个元素的连续区间数+1。

引理L：对于末尾的元素来说，无法增加它的区间数，而不增加另一种元素的区间数。

引理L的不严谨证明：不妨设末尾元素是'B'，那么要想增加'B'的区间数，必须先用'F'元素来将此区间断开，然后再继续添加元素'B'，而一旦这么做了，'F'的区间数就增加了1个。

不妨设第一个元素是'F'，此时连续的'F'的区间数为1，比连续的'B'的区间数大1。此时可以在后面添加若干个'B'，使得连续的'B'的区间数也为1，此时'F'和'B'的区间相等。此时末尾元素是'B'，根据引理L，这之后无法增加'B'的区间数而不增加'F'的区间数。因此无论如何，'F'的区间数要么等于'B'的区间数，要么比'B'的区间数大一。

---

在想清楚这一点之后，才终于明白为什么用8行代码就可以解决这个问题。不过，若是出现的元素多于两个，该命题就不再成立，因此也无法再简单地用8行代码解决帽檐朝向问题了。
