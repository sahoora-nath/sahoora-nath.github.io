---
layout: post
title: "Java, How is HashMap in Java implemented internally?"
date: 2018-05-02
---

## What are the pros and cons to use it? What are the complexities it provides for insert, delete and lookup?

There are four things we should know about before going into internals of how HashMap works -
*	HashMap works on the principal of hashing.
*	Map.Entry interface - This interface gives a map entry (key-value pair). HashMap in Java stores both key and value object, in bucket, as Entry object which implements this nested interface Map.Entry.
*	hashCode() -HashMap provides put(key, value) for storing and get(key) method forretrieving Values from HashMap. When put() method is used to store (Key, Value) pair, HashMap implementation calls hashcode on Key object to calculate a hash that is used to find a bucket where Entry object will be stored. When get() method is used to retrieve value, again key object is used to calculate a hash which is used then to find a bucket where that particular key is stored.
*	equals() - equals() method is used to compare objects for equality. In case of HashMap key object is used for comparison, also using equals() method Map knows how to handle hashing collision (hashing collision means more than one key having the same hash value, thus assigned to the same bucket. In that case objects are stored in a linked list.
*	Where hashCode method helps in finding the bucket where that key is stored, equals method helps in finding the right key as there may be more than one key-value pair stored in a single bucket.

![Alt](/../images/java/hash_map.png "HashMap")

**Pictorial representation of how Entry (key-value pair) objects will be stored in table array**

**How get() methods works internally**
As we already know how Entry objects are stored in a bucket and what happens in the case of Hash Collision it is easy to understand what happens when key object is passed in the get method of the HashMap to retrieve a value.
Using the key again hash value will be calculated to determine the bucket where that Entry object is stored, in case there are more than one Entry object with in the same bucket stored as a linked-list equals() method will be used to find out the correct key. As soon as the matching key is found get() method will return the value object stored in the Entry object.

**In case of null Key**
As we know that HashMap also allows null, though there can only be one null key in HashMap. While storing the Entry object HashMap implementation checks if the key is null, in case key is null, it always map to bucket 0 as hash is not calculated for null keys.

**HashMap changes in Java 8**
Though HashMap implementation provides constant time performance *O(1)* for get() and put() method but that is in the ideal case when the Hash function distributes the objects evenly among the buckets.
But the performance may worsen in the case hashCode() used is not proper and there are lots of hash collisions. As we know now that in case of hash collision entry objects are stored as a node in a linked-list and equals() method is used to compare keys. That comparison to find the correct key with in a linked-list is a linear operation so in a worst case scenario the complexity becomes *O(n)*.
To address this issue in Java 8 hash elements use balanced trees instead of linked lists after a certain threshold is reached. Which means HashMap starts with storing Entry objects in linked list but after the number of items in a hash becomes larger than a certain threshold, the hash will change from using a linked list to a balanced tree, this will improve the worst case performance from *O(n)* to *O(log n)*.


**hashCode()**
The hashCode() method of objects is used when you insert them into a HashTable, HashMap or HashSet.
When inserting an object into a hastable you use a key. The hash code of this key is calculated, and used to determine where to *store* the object internally. When you need to lookup an object in a hashtable you also use a key. The hash code of this key is calculated and used to determine where to *search* for the object.
The hash code only points to a certain "area" (or list, bucket etc) internally. Since different key objects could potentially have the same hash code, the hash code itself is no guarantee that the right key is found. The hashtable then iterates this area (all keys with the same hash code) and uses the key's equals() method to find the right key. Once the right key is found, the object stored for that key is returned.
So, as you can see, a combination of the hashCode() and equals() methods are used when storing and when looking up objects in a hashtable.

Here are two rules that are good to know about implementing the hashCode() method in your own classes, if the hashtables in the Java Collections API are to work correctly:
1.	If object1 and object2 are equal according to their equals() method, they must also have the same hash code.
2.	If object1 and object2 have the same hash code, they do NOT have to be equal too.
 
