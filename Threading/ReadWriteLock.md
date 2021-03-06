### 读写锁

日常我接触到的读写锁 又被称为 共享/独享锁。使用语义上比较简单。从意义上来说，对于不做修改的的读操作，可以允许多条线程并行访问；对于需要修改的写操作，需要保证共享内存部分单独使用。其他读或者写线程都要等 写锁 释放后方可访问这快区域。  
以保证每个线程对于该共享变量保持before-after atomity 的原子性。

#### 简单实现

一个互斥量，一个条件变量可以实现一个简单的读写锁。如果不考虑公平性， 实现如下：

```
class RWLock {
public:
    RWLock(): nreaders(0) {}

    void RLock() {
        std::unique_lock<std::mutex> lk(mtx);
        while(nreaders == -1) {
            cv.wait(&lk);
        }

        assert(nreaders!= -1);
        nreaders++;
    }

    void WLock() {
        std::unique_lock<std::mutex> lk(mtx);
        while(nreaders!=0) {
            cv.wait(&lk);
        }

        assert(nreaders == 0);
        nreaders = -1;
    }

    void RUnlock() {
        std::lock_guard<std::mutex> lk(mtx);
        assert(nreaders>0);
        if ((--nreaders) == 0) 
            cv.notify_one(); // suppose there is only 
    }

    void WUnlock() {
        std::lock_guard<std::mutex> lk(mtx);
        assert(nreaders == -1);
        nreaders = 0;

        cv.notify_all();
    }

private:
    std::mutex mtx;
    std::condition_variable cv;
    int nreaders;
};
```

#### 公平性

上述实现忽略了公平性的问题。比如， WUnlock的时候会唤醒所有Blocked的线程，包括之前被blocked的读线程和写线程。如果有多个不同的写线程多次抢得锁成功，就会造成读线程进入类似饿死状态。

我能想到的一种方法是 利用两个 条件变量来实现。所有读线程结束后通知写线程工作；写线程完成过后通知读线程。

```
class RWLock {
public:
    RWLock(): nreaders(0) {}

    void RLock() {
        std::unique_lock<std::mutex> lk(mtx);
        while(nreaders == -1) {
            readCv.wait(&lk);
        }

        assert(nreaders>=0);
        nreaders++;
    }

    void WLock() {
        std::unique_lock<std::mutex> lk(mtx);
        while(nreaders!=0) {
            writeCv.wait(&lk);
        }

        assert(nreaders == 0);
        nreaders = -1;
    }

    void RUnlock() {
        std::lock_guard<std::mutex> lk(mtx);
        assert(nreaders>0);
        if ((--nreaders) == 0) 
            writeCv.notify_one();
    }

    void WUnlock() {
        std::lock_guard<std::mutex> lk(mtx);
        assert(nreaders == -1);
        nreaders = 0;

        readCv.notify_all();
    }

private:
    std::mutex mtx;
    std::condition_variable readCv;
    std::condition_variable writeCv;
    int nreaders;
};
```

但是以上方法仍然没办法保证后来的reader 或者 writer 先来后到的顺序。 一个比较好的改进是：加入 pendingReaders, pendingWriters计数 并用notify\_one 来保证顺序.

```
class FairRWLock{
public:
    FairRWLock(): nreaders(0), nPendingWriters(0), nPendingReaders(0) {}

    void RLock() {
        std::unique_lock<std::mutex> lk(mtx);
        nPendingReaders++;
        
        if (nPendingWriters > 0) {
            // 有writer比此次reader先到， 允许且仅允许一个writer先进行
            readCv.wait(lk); // wait will release the lock and writer lock will get it.
        }
        
        while(nreaders == -1) {
            readCv.wait(lk);
        }
        
        assert(nreaders>=0);
        nPendingReaders--;
        nreaders++;
        
        //allow one more reader to come in.
        readCv.notify_one();
    }

    void WLock() {
        std::unique_lock<std::mutex> lk(mtx);
        nPendingWriters++;
        
        while(nreaders!=0) {  
            writeCv.wait(lk);
        }

        assert(nreaders == 0);
        nPendingWriters--;
        nreaders = -1;
    }

    void RUnlock() {
        std::unique_lock<std::mutex> lk(mtx);
        assert(nreaders>0);
        if ((--nreaders) == 0 && nPendingWriters > 0) 
            writeCv.notify_one(); // wake one writer if needed.
    }

    void WUnlock() {
        std::unique_lock<std::mutex> lk(mtx);
        assert(nreaders == -1);
        nreaders = 0;
        
        if (nPendingReaders > 0) // writer finish , let one reader come first
            readCv.notify_one(); // wake one up , it will wake another read or writer.
        else if (nPendingWriters > 0)
            writeCv.notify_one();
    }

private:
    std::mutex mtx;
    std::condition_variable readCv;
    std::condition_variable writeCv;
    int nreaders;

    int nPendingWriters;
    int nPendingReaders;
};
```



