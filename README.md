# Hash Hash Hash
This lab is meant to show how to use mutex locks to achieve concurrency in a hashtable. 

##Building 

To build the program, simply type 'make'.

## Running

Show an example run of your (completed) program on using the `-t` and `-s` flags
of a run where the base hash table completes in between 1-2 seconds.

$ ./hash-table-tester -t 5 -s 40000
Generation: 165,805 usec
Hash table base: 1,325,905 usec
  - 0 missing
Hash table v1: 514,915  usec
  - 0 missing
Hash table v2: 570,637 usec
  - 0 missing


## First Implementation

Describe your first implementation strategy here (the one with a single mutex).
Argue why your strategy is correct.

The first implementation strategy, for v1 with the single mutex, I declared my lock near the
top of my code, then I initialized the lock after initializing the hashtable entries. The crucial
line to lock and unlock the lock was right before and after the SLIST INSERT HEAD within 
the add_entry function. This is correct because it is after the calloc, and where the code has a critical section due to inserting. 
I destroyed the lock right before the free of the hash-table.
### Performance

Run the tester such that the base hash table completes in 1-2 seconds.
Report the relative speedup (or slow down) with a low number of threads and a
high number of threads. Note that the amount of work (`-t` times `-s`) should
remain constant. Explain any differences between the two.

VM with 5 cores:
Differences seen: the times for v1 and v2 are higher with the more threads, but base time seemed to go down.

low number of threads:
$ ./hash-table-tester -t 5 -s 40000
Generation: 165,805 usec
Hash table base: 1,325,905 usec
  - 0 missing
Hash table v1: 514,915  usec
  - 0 missing
Hash table v2: 570,637 usec
  - 0 missing

high number of threads:
$ ./hash-table-tester -t 10 -s 20000
Generation: 164,704 usec
Hash table base: 762,784 usec
  - 0 missing
Hash table v1: 668,353 usec
  - 0 missing
Hash table v2: 677,018 usec
  - 0 missing


## Second Implementation

Describe your second implementation strategy here (the one with a multiple
mutexes). Argue why your strategy is correct.

The second strategy I used the amount of indexes in the hashtable, or the 
capacity of the hashtable, for the number of locks I would use. I made a lock 
for each entry list in the hash-table.Then, the lock and unlock would occur around the same
SLIST_INSERT_HEAD line, as in v1 for each respective lock. Then,I created an extra checkpoint for locking, within the add_entry function consisting of code before, within, and after the if statement
(lock before, unlock and destroy before return, and an unlock right outside
 the if statement). This is correct because it would result in better performance due to finer granularity and checking places for possible concurrency issues. 
I destroyed the list of locks within the last for-loop before we free the hash-table. 
### Performance

Run the tester such that the base hash table completes in 1-2 seconds.
Report the relative speedup with number of threads equal to the number of
physical cores on your machine (at least 4). Note again that the amount of work
(`-t` times `-s`) should remain constant. Report the speedup relative to the
base hash table implementation. Explain the difference between this
implementation and your first, which respect why you get a performance increase.

Differences seen: 
The base hash table implementation when I changed the number of threads and cores
there were, from 5 to 6 cores and with it the thread number, I saw that the base table time was higher with the lower core number and thread number. I got a performance increase with the v2 implementation due to the 
power of concurrency and parallelism compared to v1 implementation. 

VM with 5 cores:
$ ./hash-table-tester -t 5 -s 50000
Generation: 199,918 usec
Hash table base: 1,722,348 usec
  - 0 missing
Hash table v1: 933,561 usec
  - 0 missing
Hash table v2: 731,392 usec
  - 0 missing

VM with 6 cores:
$ ./hash-table-tester -t 6 -s 41,667
Generation:  199,753 usec
Hash table base: 1,659,611 usec
  - 0 missing
Hash table v1: 994,293  usec
  - 0 missing
Hash table v2: 814,116  usec
  - 0 missing

## Cleaning up

To clean up the binary files, simple type 'make clean'.
