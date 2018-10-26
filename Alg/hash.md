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





Simple Implementation 

```
// HashMap
class MyHashMap {
private:
    int hashFn(int key) {
        return key % 100;
    }
    
    struct KeyNode {
        int key;
        int val;
        KeyNode(int k, int v) : key(k), val(v) {}
    };
    std::array<std::list<KeyNode>, 100> buckets;

public:
    /** Initialize your data structure here. */
    MyHashMap() {
        
    }
    
    /** value will always be non-negative. */
    void put(int key, int value) {
        int idx = hashFn(key);
        auto it = std::find_if(buckets[idx].begin(), buckets[idx].end(), [&key] (const KeyNode& node) { return node.key == key ;});
        if (it != buckets[idx].end()) {
            buckets[idx].erase(it);
        } 
        // emplace_back reduce unnecessary copies.
        buckets[idx].emplace_back(KeyNode(key, value));
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int idx = hashFn(key);
        auto it = std::find_if(buckets[idx].begin(), buckets[idx].end(), [&key] (const KeyNode& node) { return node.key == key ;});
        if (it == buckets[idx].end()) {
            return -1;
        } else {
            return (*it).val;
        }
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int idx = hashFn(key);
        auto it = std::find_if(buckets[idx].begin(), buckets[idx].end(), [&key] (const KeyNode& node) { return node.key == key ;});
        if (it != buckets[idx].end())
            buckets[idx].erase(it);
    }
};


// HashSet
class MyHashSet {
private:
    static const int NBUCKETS = 1000;
    std::array<std::list<int>, NBUCKETS> buckets; /// ----> no need key, since key is the same as value.
    
    int hashFn(int k)  { return k % NBUCKETS; }; 
    
public:
    /** Initialize your data structure here. */
    MyHashSet() {
        
    }
    
    void add(int key) {
        int idx = hashFn(key);
        if (!contains(key)) buckets[idx].emplace_back(key);
    }
    
    void remove(int key) {
        int idx = hashFn(key);
        auto it = std::find(buckets[idx].begin(), buckets[idx].end(), key);
        if ( it != buckets[idx].end() ) {
            buckets[idx].erase(it);
        }
    }
    
    /** Returns true if this set contains the specified element */
    bool contains(int key) {
        int idx = hashFn(key);
        auto it = std::find(buckets[idx].begin(), buckets[idx].end(), key);
        return it != buckets[idx].end();
    }
};
```



