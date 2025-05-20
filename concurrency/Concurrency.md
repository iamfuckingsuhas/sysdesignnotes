
concurrency means system can execute multiple tasks (processes/tasks) at once, not necessarily in parallel. 

> Thread synchronization is the coordination of threads to ensure proper execution and prevent conflicts when accessing shared resources.

Critical Section: A code part where the shared resources are accessed. It is critical as multiple processes enter this section at same time leading to data corruption and errors

A mutex is like a lock that ensures only one process can access a resource at a time

race condition - two or more threads try to access same resource without coordination.

deadlock - two or more threads are waiting for each other to release resources.

shared resources should have synchronization and locking

semaphore and mutexes are used for process synchronizaton. semaphore is a variable that stores whether a resource is available or not
mutex is a semaphore that provides mutual exclusion to resources.

mutex - no two processes can exists in a critical section at any point of time.

semaphore uses a counter to allow for synchronization of multiple instances of resources. processes can try to access one instance, not availble, go to another instance of the resource.

semaphore ops - 
wait(v) - decreases value  by v - at start
signal - increases value by v - at end


if(wait > 0) - decrement and continue execution
else wait till signal is called and val > 0


Type - 
* binary semaphore - mutex - can have 0/1 and is initialized to 1. mutliple process, single resource
* counting semaphore - can have v value. for multiple resources with multiple instances. initialized to number of resources available.

Producer Consumer problem - https://www.geeksforgeeks.org/producer-consumer-problem-using-semaphores-set-1/?ref=lbp producer produces items and adds to buffer, consumer takes from buffer. we block producer till buffer has atleast one spot  and block consumer till buffer is more than 1.

producer - 

wait(empty) 
wait(mutex) // accessing resource
// add resource to buffer
signal(mutex) // release lock
signal(full) // increase full by 1


consumer - 
wait(full) // decrease full by 1
wait(mutex)
// get resource from buffer
signal(mutex) // release lock
signal(empty) // increase empty by 1 


Read Writer problem - https://www.geeksforgeeks.org/readers-writers-problem-set-1-introduction-and-readers-preference-solution/?ref=lbp
multiple readers can read but only one writer can write
since only one writer can write - mutex
since many readers can read - semaphore

for write - 
wait(mutex) 
// write to 
signal(mutex)

for read - 
wait(read)


Two ways to create thread in java

inherit Thread class & override run method
implement runnable interface& override run method and create obj using Thread t = new Thead(RunnableObj). 
Thread gives us yield & interrupt

join - 
t2.start()
t1.join()

t2 will wait till t1 is terminated.

Thread.start -> creates new thread and run is executed on new thread -> called once
Thread.run -> runs existing thread -> can be called multiple times 


Thread safety - 
* synchronization -  `   synchronized   ` keyword creates a critical section that only one thread can access at a time. atomic and immediately visible.
* volatile keyword - immediately visible to other threads but operations not atomic (multi threads can add).
* atomic variables - can be accessed by multiple threads
* final keyword - since they cant be modified, they are thread safe

ThreadPool - reuse threads
 // create threads - 
 Thread t=. new Thread();
 // create pool
 ExecutorService executor = Executors.newFixedThreadPool(200);
 // add task to pool
 executor.execute(t);
 // kill pool
 executor.shutdown();

Synchronize - 

synchronized void inc() {
	cnt++;
}

or for a block - 

void inc() {
synchronized(this) {
cnt++;
}
}


if synchronized is add to a static method, the monitor is attached to class (only one thread can access static methods at any time). 

Atomic variables faster than > Volative faster than > sync

LockFramework - locks can be used to control which part of code is threadsafe for which thread.
can have multiple locks for one method.
Lock t = new ReentrantLock();
t.lock();

t.unlock();



https://www.geeksforgeeks.org/reentrant-lock-java/

reentrant lock is an impl of lock framework. it tracks a hold count. when a thread acquires, hold-- when released, hold++;

tryLock - nonblocking, if other thread is using the lock



If We have a lot of writes more than reads - synchronized can be faster because of less overhead
if we have more reads - reentrantReadWriteLock can be faster
We can use exclusive lock - renetrantlock.lock() for short critical sections
ConcurrentHashMap - uses bucket level locks, no locks for reads

