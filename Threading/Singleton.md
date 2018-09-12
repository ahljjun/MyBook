### 问题

如何安全的实现多线程环境下的单件模式

参考1 给出了详细介绍，最初java大神们发现了double-check-locking不能完全保证单件模式在多线程环境下的安全性。参考2是Meyers对这一问题在C++领域中给出了详细的介绍。

参考1进一步介绍了C++11后，这一问题的多种解决方式。 comments也提到了 陈硕书中指到过的利用 pthread\_once\(c++11 中std::once\_flag, std::call\_once\) 来保证初始化的唯一性。

### 实现\(TODO\)

### 参考

1. [http://www.cs.umd.edu/~pugh/java/memoryModel/DoubleCheckedLocking.html](http://www.cs.umd.edu/~pugh/java/memoryModel/DoubleCheckedLocking.html)
2. [http://www.aristeia.com/Papers/DDJ\_Jul\_Aug\_2004\_revised.pdf](http://www.aristeia.com/Papers/DDJ_Jul_Aug_2004_revised.pdf)



