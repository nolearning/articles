# Print Odd/Even Numbers

## prerequisite
* std::this_thread::yield

-------
* [std::this_thread::yield](https://en.cppreference.com/w/cpp/thread/yield)
* [std::this_thread::yield() vs std::this_thread::sleep_for()?](https://stackoverflow.com/questions/11048946/stdthis-threadyield-vs-stdthis-threadsleep-for)
* [pthread_yield() — Release the processor to other threads](https://www.ibm.com/support/knowledgecenter/en/SSLTBW_2.3.0/com.ibm.zos.v2r3.bpxbd00/ptyield.htm)
* [Windows SwitchToThread function](https://docs.microsoft.com/zh-cn/windows/desktop/api/processthreadsapi/nf-processthreadsapi-switchtothread)
* [WIN32: Yielding execution to another (given) thread](https://stackoverflow.com/questions/2022774/win32-yielding-execution-to-another-given-thread)
* [std::condition_variable::wait](https://en.cppreference.com/w/cpp/thread/condition_variable/wait)



## Problem: print odd even numbers using two different threads
Solutions:
* using mutex/conditions
```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <functional>
#include <string_view>

struct OddEven {
    int x = 1;
    std::mutex m;
    std::condition_variable cond;
};

void printTask(OddEven &oe, const std::string_view &label, bool odd)
{
    for (bool running{true}; running; ) {
        std::unique_lock<std::mutex> mlock(oe.m);
        oe.cond.wait(mlock, [&oe, odd] {
            return (oe.x & 1) == odd;
        });
        std::cout << label << oe.x << std::endl;
        oe.x++;
        running = oe.x < 10;
        oe.cond.notify_all();
    }
}

int main()
{
    OddEven oe;

    std::thread t1(printTask, std::ref(oe), "Odd Print", true);
    std::thread t2(printTask, std::ref(oe), "Even Print", false);
    t1.join();
    t2.join();
}
```
* using Object.wait/Object.notify/notifyall in Java
```java
class TaskEvenOdd implements Runnable {
    private int max;
    private Printer print;
    private boolean isEvenNumber;
 
    // standard constructors
 
    @Override
    public void run() {
        int number = isEvenNumber ? 2 : 1;
        while (number <= max) {
            if (isEvenNumber) {
                print.printEven(number);
            } else {
                print.printOdd(number);
            }
            number += 2;
        }
    }
}
class Printer {
    private volatile boolean isOdd;
 
    synchronized void printEven(int number) {
        while (!isOdd) {
            try {
                wait();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        System.out.println(Thread.currentThread().getName() + ":" + number);
        isOdd = false;
        notify();
    }
 
    synchronized void printOdd(int number) {
        while (isOdd) {
            try {
                wait();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        System.out.println(Thread.currentThread().getName() + ":" + number);
        isOdd = true;
        notify();
    }
}
public static void main(String... args) {
    Printer print = new Printer();
    Thread t1 = new Thread(new TaskEvenOdd(print, 10, false),"Odd");
    Thread t2 = new Thread(new TaskEvenOdd(print, 10, true),"Even");
    t1.start();
    t2.start();
}
```
* using BlockingQueue in Java
```java
//OddAndEvenSignalling.java
package com.practice.java.threads.oddeven;

import java.util.concurrent.BlockingQueue;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.LinkedBlockingQueue;

public class OddAndEvenSignalling
{
     
    private static final int MAX_NUMBER = 10;
     
    public static void main(String[] args)
    {
        BlockingQueue<Integer> odds = new LinkedBlockingQueue<Integer>();
        BlockingQueue<Integer> evens = new LinkedBlockingQueue<Integer>();
        ExecutorService executorService = Executors.newFixedThreadPool(2);
         
        //odd numbers are removed from the "from" queue --> printed --> incremented --> placed on the "to" queue by one thread
        //"from" queue is "odd number" queue and "to" queue is "even number" queue
        executorService.submit(new TakeAndOfferNext(odds, evens, MAX_NUMBER));
        System.out.println("================================");
        //even numbers are removed from the "from" queue --> printed --> incremented --> placed on the "to" queue by another thread
        //"from" queue is "even number" queue and "to" queue is "odd number" queue
        executorService.submit(new TakeAndOfferNext(evens, odds, MAX_NUMBER));
//        System.out.println("============offering items to consumer thread============");
        odds.offer(1);
        System.out.println("============Printing the numbers============");
                
    }
}

// TakeAndOfferNext.java
package com.practice.java.threads.oddeven;

import java.util.concurrent.BlockingQueue;
import java.util.concurrent.Callable;

public class TakeAndOfferNext implements Callable<Object>
{
     
    BlockingQueue<Integer> takeFrom;
    BlockingQueue<Integer> offerTo;
    int maxNumber;
     
    public TakeAndOfferNext(BlockingQueue<Integer> takeFrom, BlockingQueue<Integer> offerTo, int maxNumber)
    {
        this.takeFrom = takeFrom;
        this.offerTo = offerTo;
        this.maxNumber = maxNumber;
        System.out.println("Initialized");
    }
     
    public Object call() throws Exception
    {
    	System.out.println("Call back method called");
    	print();
        return null;
    }
     
    public void print()
    {
        while (true)
        {
            try
            {
                int i = takeFrom.take(); //removes the value in the "from" queue
                System.out.println(Thread.currentThread().getName() + " --> " + i);
                offerTo.offer(i + 1);    //increments the value by 1 and puts it in the "to" queue.
                if (i >= (maxNumber - 1))
                {
                    System.exit(0);
                }
            }
            catch (InterruptedException e)
            {
                throw new IllegalStateException("Unexpected interrupt", e);
            }
        }
    }
     
}
```

* using semaphores
```java
public static void main(String[] args) {
    SharedPrinter sp = new SharedPrinter();
    Thread odd = new Thread(new Odd(sp, 10),"Odd");
    Thread even = new Thread(new Even(sp, 10),"Even");
    odd.start();
    even.start();
}
class SharedPrinter {
 
    private Semaphore semEven = new Semaphore(0);
    private Semaphore semOdd = new Semaphore(1);
 
    void printEvenNum(int num) {
        try {
            semEven.acquire();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        System.out.println(Thread.currentThread().getName() + num);
        semOdd.release();
    }
 
    void printOddNum(int num) {
        try {
            semOdd.acquire();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        System.out.println(Thread.currentThread().getName() + num);
        semEven.release();
 
    }
}

class Even implements Runnable {
    private SharedPrinter sp;
    private int max;
 
    // standard constructor
 
    @Override
    public void run() {
        for (int i = 2; i <= max; i = i + 2) {
            sp.printEvenNum(i);
        }
    }
}
 
class Odd implements Runnable {
    private SharedPrinter sp;
    private int max;
 
    // standard constructors 
    @Override
    public void run() {
        for (int i = 1; i <= max; i = i + 2) {
            sp.printOddNum(i);
        }
    }
}
```
-------------
* [Java Thread IV: BlockingQueue to Print Odd/Even Numbers](http://javaoholic.blogspot.com/2014/11/java-thread-iv-blockingqueue-to-print.html)
* [Print Even and Odd Numbers Using 2 Threads](https://www.baeldung.com/java-even-odd-numbers-with-2-threads)
* [Multithread to print odd and even numbers](https://codereview.stackexchange.com/questions/200997/multithread-to-print-odd-and-even-numbers)
* [Core Java multi-thread coding -- printing odd and even numbers with two threads and a blocking queue](http://saralsaxena.blogspot.com/2014/11/core-java-multi-thread-coding-printing.html)
* [java: Printing odd even numbers using 2 threads](https://stackoverflow.com/questions/15182418/java-printing-odd-even-numbers-using-2-threads)
* [你会这道阿里多线程面试题吗？](https://mp.weixin.qq.com/s/Qdzbyow8AWCDLn6RABwo9w)
