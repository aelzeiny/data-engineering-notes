# Log Structure Merge Trees
WOW! I Finally get them.

## Problem trying to be solved
* Secondary indexes slow down writes.
* It's random access to a Disk; which is very slow!
* If you use RDBMS's then you feel this.
* B+ Trees are optimized to read blocks of data from disk, but they must be very structured on writes!

## Solution #1: Say "NO"
* You don't have to solve 2ndary indexes if you don't have them.
* Avoiding the problem is actually the best solution
* HBase & DynamoDB were like... nah fam.

## Solution #2: In Memory
* MongoDB was like we can store this randomly in memory
* In 2020, random access to memory is about equal to sequential access to disk.
* What if I have more index than memory?
* Long server start-up times because index must be rebuilt

## Solution #3: Log-Structured Merge Trees
* I split my logs into multiple small files
* I have a metadata store about which keys lie in which files
* This shifts the problem from reads to writes, but you can optimize reads more!
* (A) I also have a bloom-filter that tells me probablistically if I need to read a file
* (B) CPU sequential reads are optimized on OS level, CPU cache level, and disk buffering level.