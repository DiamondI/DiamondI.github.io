---
title: Priority Queue
categories: Data Structure
---

# Priority Queue

I have introduced a data structure, priority queue, in [previous article](https://diamondi.github.io/2018/09/12/215/) to solve question *Kth Largest Element in an Array*. And now let me introduce something about `priority_queue` in C++.

<!-- more -->

## Definition <span id=123></span>

A priority queue is a container adaptor that provides constant time lookup of the largest (by default) element, at the expense of logarithmic insertion and extraction. In C++, priority queue is a container defined in header `<queue>`. And here's the definition:

```cpp
template<
    class T,
    class Container = std::vector<T>,
    class Compare = std::less<typename Container::value_type>
> class priority_queue;
```

From this definition, we shall see that it accepts three parameters: the first one is the type of the stored elements; the second one is the container used to store the elements; the last one is the definition of priority. Besides, `priority_quque` will be implemented with vector and use less standing for priority if not specified.

## Functions

- top: accesses the top element, const time complexity
- empty: checks whether the underlying container is empty
- size: returns the number of elements
- push: inserts element and sorts the underlying container
- pop: removes the top element

Click [here](https://en.cppreference.com/w/cpp/container/priority_queue) for more information about priority_queue.