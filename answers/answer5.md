# Describe a deadlock. What causes it? What are the effects?
A deadlock occurs when two or more threads are waiting for one another to release their mutual locks. For example, thread 1 might create lock A, and is waiting for lock B so that it can finish its work. Thread 2 might create lock B, and is waiting for lock A. The two threads end up waiting indefinitely, waiting for the other to release the lock. The sequence of events is usually something like this:

1. Thread 1 makes lock A
2. Thread 2 makes lock B
3. Thread 1 awaits lock B
4. Thread 2 awaits lock A

The way to resolve it in the short term is to find a way to safely shut down one of the threads and release a lock, allowing the other to finish. The long term resolution is to refactor multithreaded code so that this condition is avoided.