---
layout: post
title: "Java, Difference between Array-list and Linked-list?"
date: 2018-06-02
---

ArrayList in an array backed by List. It’s just a grow able array.

![Alt](/../images/java/arraylist.png "ArrayList")

While adding element into array-list, if you hit at the end of the list, it needs to resize the backing array.
In order to avoid resize the backing array every time you add elements to it, the array-list doubles the size of the backing array when it runs out of storage.

So, if your initial size of the array is 2 and you add the first element and then the 2nd element and when you add 3rd element it jumps the size to 4. Similarly, when next time when you add 5th element, it jumps the size to 8 and when you add 9th element the size will jump to 16 and so on.

*Hence the resizing is less frequent when the list grows.*

![Alt](/../images/java/linkedlist.png "LinkedList")

LinkedList in java is doubly linked list. That means each element in linkedlist has a pointer to next element and a pointer to its previous element. It keeps the reference of its head and tail of the list.
Each node within the list has a pointer to the element that stored there and next and pervious elements in the list.

So, when you are looping through your linked-list, you have to continually going through the pointers to find the next elements in the list. It’s expensive to get the 5th element form a linked-list as you have to jump 5 times by chasing the pointers whereas in an arraylist you just the reference of 5th element.

This is really very good, if you repeated add/remove elements from the linked-list because all you have to do is break that link of pointers and add a new element in it.
However, if you add an element in the middle of an arraylist, you have to copy over all elements to shuffle them one by one.


Big(O) annotation is an indicative of how the operation scales with the size of the list.

![Alt](/../images/java/arraylistvslinkedlist.png "Compare ArrayList vs LinkedList")

In this case O(1) means the size is constant. Suppose we have a list with million elements in it, getting the 100th element is just as fast as getting element from 1000 position.

O(N) means it grows linearly with the size of the list. Hence the big list you get the more expensive it is.
