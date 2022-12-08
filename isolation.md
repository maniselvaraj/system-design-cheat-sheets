

# Isolation / Concurrency


### Isolation of transactions is needed to address below anomalies:



1. Read anomaly
    1. Dirty read: read uncommitted txns from another write
    2. Non-repeatable read (NRR): same query returns different results
    3. Phantom read: same as NRR, but for range query.
2. Write anomaly
    1. Lost update: simultaneous update of same key/value
    2. Dirty write: uses uncommitted value to modify
    3. Write skew: individual txn is good. But when combined goes bad


### Solutions for Lost Update problem



* Lost update = later write clobbers earlier write
* Atomic write (exclusive lock)
* App level explicit lock. Ex: SQL “for update” locks all rows
* Automatically detect and abort
* Atomic compare and set
* Replicated environment: locks or compare/set will not work. Usually last write wins (LWW) is used


## Concurrency Control (CC) Categories


<table>
  <tr>
   <td><strong>Optimistic CC</strong>
   </td>
   <td><strong>Multiversion CC (MVCC)</strong>
   </td>
   <td><strong>Pessimistic CC</strong>
   </td>
  </tr>
  <tr>
   <td>
<ul>

<li>Non-blocking

<li>Lossy

<li>Critical section based

<li>Reader-writer locks

<li>Upgradeable locks
</li>
</ul>
   </td>
   <td>
<ul>

<li>Lockless using timestamp ordering

<li>Lock based like 2 phased locking

<li>Snapshot isolation
</li>
</ul>
   </td>
   <td>
<ul>

<li>Record level locks (may cause deadlocks)

<li>Lock-free using timestamp ordering like last write wins (LWW)
</li>
</ul>
   </td>
  </tr>
</table>



## Isolation/Concurrency Levels


<table>
  <tr>
   <td><strong>Weak Isolation</strong>
   </td>
   <td><strong>Strong Isolation</strong>
   </td>
  </tr>
  <tr>
   <td>Non-serializable
   </td>
   <td>Serializability (strongest)
   </td>
  </tr>
  <tr>
   <td><strong>Read committed</strong>
<ul>

<li>No dirty read

<li>No dirty write

<li>Non-repeatable read allowed

<li>Phantom read allowed

<li>Dirty writes solved by row level locks

<li>Dirty reads solved by using the same lock

<li>Use read-write lock
</li>
</ul>
   </td>
   <td><strong>Actual serial execution</strong>
<ul>

<li>Use single thread like Redis

<li>Not good for write heavy apps

<li>Good for in-memory DB like Redis

<li>
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td><strong>Snapshot Isolation (SI)</strong>
<ul>

<li>Based on snapshot before start of Txn

<li>Txn is aborted if the value in the snapshot changes. Hence, no lost update

<li>Write skew is possible as local is good, but global is bad

<li>Allows repeatable read
</li>
</ul>

<p>
How is it achieved?
<ul>

<li>Need write locks

<li>Big diff vs 2PL: Due to snapshot readers don't block writers and writers don't block readers. 

<li>Needs MVCC due to snapshot

<li>Pro: Good for read heavy apps
</li>
</ul>

   </td>
   <td><strong>2 Phased Lock (2PL)</strong>
<ul>

<li>Stronger lock

<li>Reader block writers and writers block readers
</li>
</ul>
<p>
How is it achieved?
<ul>

<li>Read: shared mode lock

<li>Write: exclusive lock

<li>Phase 1: acquire lock

<li>Phase 2: release lock

<li>Cons: 2 PL is pessimistic. If anything goes wrong, then wait.
</li>
</ul>

   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td><strong>Serializable Snapshot Isolation (SSI)</strong>
<ul>

<li>Optimistic concurrency control

<li>SSI = SI + algorithm to detect serializable write conflict

<li>Writers don't block readers and vice versa

<li>Good for read heavy apps

<li>Only Txns that executed serializably are committed
</li>
</ul>
   </td>
  </tr>
</table>

[Next (Consistency)](consistency.md)