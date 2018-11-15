# Hash

两种类型：

* Hash Map

map的实现，store \(key, value\) pairs.

* Hash Set

set的实现，针对 no repeated values的设计.  value is key!!!!

##### The Principle of Hash Table

The key idea of Hash Table is to use a hash function to`map keys to buckets`. To be more specific,

1. When we insert a new key, the hash function will decide which bucket the key should be assigned and the key will be stored in the corresponding bucket;
2. When we want to search for a key, the hash table will use the`same`hash function to find the corresponding bucket and search only in the specific bucket.

##### Keys to Design a Hash Table

* Hash Function 

The hash function is the most important component of a hash table which is used to map the key to a specific bucket.

The Hash function will depend on the range of key values and the number of buckets

It is an open problem to design a hash function. The idea is to try to assign the key to the bucket as`uniform as you can`

**Tradeoff:   the amount of buckets   vs   the capacity of a bucket.**

* Collision Resolution 

A collision resolution algorithm should solve the following questions:

1. How to organize the values in the same bucket?
2. What if too many values are assigned to the same bucket?
3. How to search a target value in a specific bucket?

   one  bucket  contains  N  keys.

N is constant and small  ----&gt;  use ARRAY

N is variable or large  ----&gt;  use  Height Balanced Binary Search Tree

##### Operations

* Insertion 
* Search 







