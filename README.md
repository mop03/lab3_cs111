# Hash Hash Hash

One line description of this code.
This lab is meant to show how to use mutex locks to achieve concurrency in a hashtable. 

## Building

Explain briefly how to build your program.
To build the program, simply type 'make'.


## Running

Show an example run of your (completed) program on using the `-t` and `-s` flags
of a run where the base hash table completes in between 1-2 seconds.

## First Implementation

Describe your first implementation strategy here (the one with a single mutex).
Argue why your strategy is correct.

The first implementation strategy, for v1 with the single mutex, I declared my lock near the top of my code, then I initialized the lock after initializing the hashtable entries. The crucial line to lock and unlock the lock was right before and after the SLIST INSERT within the add_entry function. 
This is correct because it is after the calloc, and the only place where the code makes a critical section. 


### Performance

Run the tester such that the base hash table completes in 1-2 seconds.
Report the relative speedup (or slow down) with a low number of threads and a
high number of threads. Note that the amount of work (`-t` times `-s`) should
remain constant. Explain any differences between the two.


## Second Implementation

Describe your second implementation strategy here (the one with a multiple
mutexes). Argue why your strategy is correct.

The second strategy I used the amount of indexes in the hashtable, or the capacity of the hashtable, for the number of locks I would use. I made a lock for each entry list in the hash-table. This is correct because it would result in better performance due to finer granularity.  
### Performance

Run the tester such that the base hash table completes in 1-2 seconds.
Report the relative speedup with number of threads equal to the number of
physical cores on your machine (at least 4). Note again that the amount of work
(`-t` times `-s`) should remain constant. Report the speedup relative to the
base hash table implementation. Explain the difference between this
implementation and your first, which respect why you get a performance increase.

## Cleaning up

Explain briefly how to clean up all binary files.
To clean up the binary files, simple type 'make clean'.
