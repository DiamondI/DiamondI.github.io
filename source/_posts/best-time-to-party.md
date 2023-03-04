---
title: Puzzle 2 - The Best Time To Party
categories: Misc
---

# Programming for Puzzled

这是MIT的一门公开课，讲师是Prof. Srini Devadas，课程的level是本科生。

<!-- more -->

**Prof. Srini Devadas**

![Prof. Srini Devadas](https://www.csail.mit.edu/sites/default/files/styles/headshot/public/images/migration/devadas.jpg?h=5636fc5d&itok=EUGLSbU-)

## Puzzle 2: The Best Time To Party

### Puzzle Description

[The Best Time to Party (PDF)](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-s095-programming-for-the-puzzled-january-iap-2018/puzzle-2-the-best-time-to-party/MIT6_S095IAP18_Puzzle_2.pdf)

**问题描述（用自己的话描述）**

有一个聚会（party）将要举行，而聚会中有许多的小项目（celebrity）。现在给出了这些小项目的开始时间和结束时间（左闭右开），问何时参加聚会，可以同时看到最多的小项目？

题目假设开始时间和结束时间是以`[i, j)`，即左闭右开区间的形式出现，表示该小项目是`i-th`小时开始，`j-th`小时结束。并且假设参加时间也是整点。

**注：** 左闭右开意味着，即便在一个小项目结束的时间点参加进去，也无法看到该项目了。

举例如下：

| Celebrity | Comes | Goes |
| --- | --- | --- |
| Beyoncé | 6 | 7 |
| Taylor | 7 | 9 |
| Brad | 10 | 11 |
| Katy | 10 | 12 |
| Tom | 8 | 10 |
| Drake | 9 | 11 |
| Alicia | 6 | 8 

## Naive Algorithm

在课堂上，Devadas提问同学，同学提出了一个非常直觉也非常naive的算法：

>对每个小时都检查一遍，在该小时里同时举行的小项目有多少个，找出最多的那个即可。

这个算法的正确性显然。效率呢？假设party开始时间是m，结束时间是n，celebrity数量是x，那么复杂度就是`O((n - m)x)`。若将`(n - m)`看作是常数，似乎是线性的，而且在时间间隔是小时的时候，将其看作常数确实未尝不可；但若时间间隔是分甚至是秒的时候，`(n - m)`便无法再简单地看作是常数了，复杂度也就不再是线性的了。

老师给出的示例代码如下：

```python
#Programming for the Puzzled -- Srini Devadas
#yThe Best Time to Party
#Given a list of intervals when celebrities will be at the party
#Output is the time that you want to go the party when the maximum number of
#celebrities are still there.
#Brute force algorithm implemented here

sched = [(6, 8), (6, 12), (6, 7), (7, 8), (7, 10), (8, 9), (8, 10), (9, 12),
            (9, 10), (10, 11), (10, 12), (11, 12)]

def bestTimeToParty(schedule):
    #Find start time and end time
    start = schedule[0][0]
    end = schedule[0][1]
    for c in schedule:
        start = min(c[0], start)
        end = max(c[1], end)

    #compute count of celebrities at each time
    count = celebrityDensity(schedule, start, end)
    
##    print (count)
    maxcount = 0
    #Range over times to find the time when the maximum celebrities are around.
    for i in range(start, end + 1):
        if count[i] > maxcount:
            maxcount = count[i]
            time = i

##    maxcount = max(count[start:end + 1])
##    time = count.index(maxcount)

    #Output the best time to party.
    #Note that the \ means the statement continues on the next line.
    print ('Best time to attend the party is at', time,\
           'o\'clock', ':', maxcount, 'celebrities will be attending!')


def celebrityDensity(sched, start, end):

    #Initialize a list of length end + 1 to all 0's
    count = [0] * (end + 1)
    # i ranges over different times
    for i in range(start, end + 1):
        count[i] = 0
        for c in sched:
            #Check if celebrity c is around at time i
            if c[0] <= i and c[1] > i:
                count[i] += 1
                
    return count
                

bestTimeToParty(sched)
```

代码中首先找到了schedule中所有celebrity中最早的开始时间和最晚的结束时间，即确定了遍历的时间范围。然后对每个小时都进行一次检查，最后得到最多项目同时进行的时间。

### Improve

上面的算法依赖时间的粒度，因此一旦时间粒度变得更细了，上面的代码就需要修改。不仅如此，时间粒度变细之后也会使得计算量增加。

Devadas在课上又提出了新的思路：先将每个项目的开始时间和结束时间进行排序，然后对每个时间进行遍历，同时维护一个变量`count`来记录目前有多少个正在进行的项目。每当有项目开始的时候，就`count += 1`，而每当有项目结束的时候，就`count -= 1`。这样就可以找到最多同时有多少个项目在同时进行了。**注意当有项目的开始时间和另一个项目的结束时间相同时，需要先考虑结束时间，这是因为左闭右开的性质。**

这样的话，假设小项目数有x个的话，那么时间复杂度由排序的复杂度决定，可以达到`O(xlogx)`的复杂度，显然比之前的算法要好。不仅如此，这个算法不依赖时间粒度，即便粒度达到了分或秒，都不需要更改大量的代码，只要确保时间仍然能够比较大小即可。

老师的示例代码如下：

```python
#Programming for the Puzzled -- Srini Devadas
#The Best Time to Party
#Given a list of intervals when celebrities will be at the party
#Output is the time that you want to go the party when the maximum number of
#celebrities are still there.
#Clever algorithm that will work with fractional times

sched = [(6, 8), (6, 12), (6, 7), (7, 8), (7, 10), (8, 9), (8, 10), (9, 12),
            (9, 10), (10, 11), (10, 12), (11, 12)]
sched2 = [(6.0, 8.0), (6.5, 12.0), (6.5, 7.0), (7.0, 8.0), (7.5, 10.0), (8.0, 9.0),
          (8.0, 10.0), (9.0, 12.0), (9.5, 10.0), (10.0, 11.0), (10.0, 12.0), (11.0, 12.0)]
sched3 = [(6, 7), (7,9), (10, 11), (10, 12), (8, 10), (9, 11), (6, 8),
          (9, 10), (11, 12), (11, 13), (11, 14)]


def bestTimeToPartySmart(schedule):
    #Convert schedule to list of start times and end times marked as such
    times = []
    for c in schedule:
        times.append((c[0], 'start'))
        times.append((c[1], 'end'))

    #Sort the list of times.
    #Each time is a start or end time of a celebrity sighting.
    sortlist(times)
##    print times

    maxcount, time = chooseTime(times)


    #Output best time to party
    print ('Best time to attend the party is at', time,\
           'o\'clock', ':', maxcount, 'celebrities will be attending!')
    

#Sort the elements of tlist in ascending order
#Sorting is based on the value of the element tuple (both items!)
#The original code had a bug in that it did not look at the second
#item of each tuple and ensure that (x, 'end') of one interval
#is sorted before (x, 'start') of a different tuple.
def sortlist(tlist):
    for index in range(len(tlist)-1):
        ismall = index
        for i in range(index, len(tlist)):
            #Sort based on first item of tuple
            if tlist[ismall][0] > tlist[i][0] or \
               (tlist[ismall][0] == tlist[i][0] and \
                tlist[ismall][1] > tlist[i][1]):
                ismall = i
        #Swap the positions of the elements at index and ismall indices
        tlist[index], tlist[ismall] = tlist[ismall], tlist[index]
    
    return


def chooseTime(times):
    
    rcount = 0
    maxcount = 0
    time = 0
    
    #Range through the times computing a running count of celebrities
    for t in times:
        if t[1] == 'start':
            rcount = rcount + 1
        elif t[1] == 'end':
            rcount = rcount - 1
        if rcount > maxcount:
            maxcount = rcount
            time = t[0]
            
    return maxcount, time


##bestTimeToPartySmart(sched)
bestTimeToPartySmart(sched2)
bestTimeToPartySmart (sched3)
```

奇怪的是，Devadas在这里并没有使用Python内置的sort函数，不仅如此，老师自己实现的sort是选择排序，效率是`O(x^2)`的。在这样的情况下，该算法与上面的算法孰优孰劣还不一定。但是老师一口咬定这个算法会更快，有点让我感到困惑🤔。

### Exercises

#### Exercise 1

**Exercise 1:** Suppose you are yourself a busy celebrity and don’t have complete freedom in choosing when you can go to the party. Add arguments to the procedure **bestTimeToPartySmart** and modify it so it determines the maximum number of celebrities you can see within a given time range between `ystart` and `yend`. As with celebrities the interval is `[ystart, yend)`,  so you are available at all times such that `ystart <= t < yend`.

其实就是问在给定的时间范围内，最多能同时看到几个celebrity。只需要在上面的代码里增加限制条件即可：

**老师给出的**`partysmart-exercise1.py`**代码**

```python
#Programming for the Puzzled -- Srini Devadas
#The Best Time to Party
#Given a list of intervals when celebrities will be at the party
#Output is the time that you want to go the party when the maximum number of
#celebrities are still there.
#Clever algorithm that will work with fractional times

sched = [(6, 8), (6, 12), (6, 7), (7, 8), (7, 10), (8, 9), (8, 10), (9, 12),
            (9, 10), (10, 11), (10, 12), (11, 12)]
sched2 = [(6.0, 8.0), (6.5, 12.0), (6.5, 7.0), (7.0, 8.0), (7.5, 10.0), (8.0, 9.0),
          (8.0, 10.0), (9.0, 12.0), (9.5, 10.0), (10.0, 11.0), (10.0, 12.0), (11.0, 12.0)]
sched3 = [(6, 7), (7,9), (10, 11), (10, 12), (8, 10), (9, 11), (6, 8),
          (9, 10), (11, 12), (11, 13), (11, 14)]

#[ystart, yend) is the time that you can meet with the celebrities
def bestTimeToPartySmart(schedule, ystart, yend):
    #Convert schedule to list of start times and end times marked as such
    times = []
    for c in schedule:
        times.append((c[0], 'start'))
        times.append((c[1], 'end'))

    #Sort the list of times.
    #Each time is a start or end time of a celebrity sighting.
    sortlist(times)
##    print times

    maxcount, time = chooseTimeConstrained(times, ystart, yend)

    #Output best time to party
    print ('Best time to attend the party is at', time,\
           'o\'clock', ':', maxcount, 'celebrities will be attending!')
    

#Sort the elements of tlist in ascending order
#Sorting is based on the value of the element tuple (both items!)
#The original code had a bug in that it did not look at the second
#item of each tuple and ensure that (x, 'end') of one interval
#is sorted before (x, 'start') of a different tuple.
def sortlist(tlist):
    for index in range(len(tlist)-1):
        ismall = index
        for i in range(index, len(tlist)):
            #Sort based on first item of tuple
            if tlist[ismall][0] > tlist[i][0] or \
               (tlist[ismall][0] == tlist[i][0] and \
                tlist[ismall][1] > tlist[i][1]):
                ismall = i
        #Swap the positions of the elements at index and ismall indices
        tlist[index], tlist[ismall] = tlist[ismall], tlist[index]
    
    return


def chooseTimeConstrained(times, ystart, yend):

    rcount = 0
    maxcount = 0
    time = 0
    
    #Range through the times computing a running count of celebrities
    for t in times:
        if t[1] == 'start':
            rcount = rcount + 1
        elif t[1] == 'end':
            rcount = rcount - 1
        #Make sure that you are available during this time t[0]!
        if rcount > maxcount and t[0] >= ystart and t[0] < yend:
            maxcount = rcount
            time = t[0]

    return maxcount, time


#bestTimeToPartySmart(sched2, 7.0, 9.0)
bestTimeToPartySmart(sched2, 10.0, 12.0)
```

#### Exercise 2

**Exercise 2:** There is an alternative way of computing the best time to party that does not depend on the granularity of time. We choose each celebrity interval in turn, and determine how many other celebrity intervals contain the chosen celebrity’s start time. We pick the time to attend the party to be the start time of the celebrity whose start time is contained in the maximum number of other celebrity intervals. Code this algorithm and verify that it produces the same answer as the algorithm based on sorting.

这个练习提出了一个新的思路：检查每个celebrity的开始时间，找出哪一个celebrity的开始时间被最多的celebrity的时间间隔所包含，这就是所求的答案。

为什么这个思路是正确的呢？原因很简单，在基于排序的算法中，维护的最大数量只有在一个新的celebrity开始的时候才有可能会更新。所以题目所求的时间必然是某个celebrity的开始时间。

code就懒得写了😂，反正也不难。

#### Exercise 3

**Puzzle Exercise 3:** Imagine that there is a weight associated with each celebrity dependent on how much you like that particular celebrity. This can be represented in the schedule as a 3-tuple, e.g., `(6.0, 8.0, 3)`. The start time is 6.0, end time is 8.0 and the weight is 3. Modify the code so you find the time that the celebrities with maximum total weight are available. For example, given:

![Example](http://ww3.sinaimg.cn/large/006tNc79gy1g5dbixezi7j30fa0920tc.jpg)

We want to return the time corresponding to the right dotted line even though there are only two celebrities available at that time. This is because the weight associated with those two celebrities is 4, which is greater than the total weight of 3 associated with the three celebrities available during the first dotted line.

Here’s a more complex example:

```
sched3 = [(6.0, 8.0, 2), (6.5, 12.0, 1), (6.5, 7.0, 2), (7.0, 8.0, 2), (7.5, 10.0, 3), (8.0, 9.0, 2),(8.0, 10.0, 1), (9.0, 12.0, 2), (9.5, 10.0, 4), (10.0, 11.0, 2), (10.0, 12.0, 3), (11.0, 12.0, 7)]
```

For this schedule of celebrities, you want to attend at 11.0 o'clock where the weight of attending celebrities is 13 and maximum!

我的思路仍然是先将每个项目的开始时间和结束时间进行排序，同时记录下该项目的权重，然后有项目开始就增加相应的权重，结束就减少相应的权重。

代码如下：

```python
import profile

sched3 = [
            (6.0, 8.0, 2), (6.5, 12.0, 1),
            (6.5, 7.0, 2), (7.0, 8.0, 2),
            (7.5, 10.0, 3), (8.0, 9.0, 2),
            (8.0, 10.0, 1), (9.0, 12.0, 2),
            (9.5, 10.0, 4), (10.0, 11.0, 2),
            (10.0, 12.0, 3), (11.0, 12.0, 7)
        ]

def sortlist(times):
    times.sort(key=lambda x: (x[0], x[1]))

def bestTimeToParty(shed):
    times = []
    for i in shed:
        times += [(i[0], 'start', i[-1])]
        times += [(i[1], 'end', i[-1])]

    sortlist(times)

    count, maxcount = 0, 0
    for i in times:
        if i[1] == 'start':
            count += i[-1]
            if count > maxcount:
                maxcount = count
                time = i[0]
        else:
            count -= i[-1]
    print ('Best time to attend the party is at', time,\
           'o\'clock', ':', maxcount, 'values in total!')

profile.run('bestTimeToParty(sched3)')
```

结果如下：

```
Best time to attend the party is at 11.0 o'clock : 13 values in total!
         32 function calls in 0.013 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.000    0.000 :0(exec)
        1    0.000    0.000    0.000    0.000 :0(print)
        1    0.012    0.012    0.012    0.012 :0(setprofile)
        1    0.000    0.000    0.000    0.000 :0(sort)
        1    0.000    0.000    0.000    0.000 <string>:1(<module>)
        1    0.000    0.000    0.000    0.000 ex3.py:12(sortlist)
       24    0.000    0.000    0.000    0.000 ex3.py:13(<lambda>)
        1    0.000    0.000    0.000    0.000 ex3.py:15(bestTimeToParty)
        1    0.000    0.000    0.013    0.013 profile:0(bestTimeToParty(sched3))
        0    0.000             0.000          profile:0(profiler)

```
