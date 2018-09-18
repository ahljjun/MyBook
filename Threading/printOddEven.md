#### Problem:

从1~100,顺序打印，一个线程负责打印基数，一个线程负责打印偶数。

```
#include <iostream>
#include <thread>
#include <condition_variable>
#include <mutex>
#include <functional>

class PrintEvenOdd final{
public:
    explicit PrintEvenOdd(unsigned int number): target(number), num(1){
    }

    void printOdd() {
        while(num < target) {
            std::unique_lock<std::mutex> lk(mtx);
            while(!(num % 2)) {
                wantOddCv.wait(lk);
            }
            std::cout<<std::this_thread::get_id()<<": "<<num<<std::endl;
            num++;
            wantEvenCv.notify_one();
            lk.unlock();
        }
    }                           

    void printEven() {          
        while(num < target) {   
            std::unique_lock<std::mutex> lk(mtx);
            while(num % 2) {    
                wantEvenCv.wait(lk);
            }                   
            std::cout<<std::this_thread::get_id()<<": "<<num<<std::endl;
            num++;              
            wantOddCv.notify_one();
            lk.unlock();        
        }
    }

    void start() {
        std::thread t1(std::bind(&PrintEvenOdd::printOdd, this));
        std::thread t2(std::bind(&PrintEvenOdd::printEven, this));
        t1.join();
        t2.join();
    }
private:
    unsigned int target;
    unsigned int num;
    std::mutex mtx;
    std::condition_variable wantOddCv;
    std::condition_variable wantEvenCv;
};

int main() {
    PrintEvenOdd   printObj(100);
    printObj.start();

    std::cout<<"done"<<std::endl;

    return 0;#include <iostream>
#include <thread>
#include <condition_variable>
#include <mutex>
#include <functional>

class PrintEvenOdd final{
public:
    explicit PrintEvenOdd(unsigned int number): target(number), num(1){
    }

    void printOdd() {
        while(num < target) {
            std::unique_lock<std::mutex> lk(mtx);
            while(!(num % 2)) {
                wantOddCv.wait(lk);
            }
            std::cout<<std::this_thread::get_id()<<": "<<num<<std::endl;
            num++;
            wantEvenCv.notify_one();
            lk.unlock();
        }
    }                           

    void printEven() {          
        while(num < target) {   
            std::unique_lock<std::mutex> lk(mtx);
            while(num % 2) {    
                wantEvenCv.wait(lk);
            }                   
            std::cout<<std::this_thread::get_id()<<": "<<num<<std::endl;
            num++;              
            wantOddCv.notify_one();
            lk.unlock();        
        }
    }

    void start() {
        std::thread t1(std::bind(&PrintEvenOdd::printOdd, this));
        std::thread t2(std::bind(&PrintEvenOdd::printEven, this));
        t1.join();
        t2.join();
    }
private:
    unsigned int target;
    unsigned int num;
    std::mutex mtx;
    std::condition_variable wantOddCv;
    std::condition_variable wantEvenCv;
};

int main() {
    PrintEvenOdd   printObj(100);
    printObj.start();

    std::cout<<"done"<<std::endl;

    return 0;
}
```



