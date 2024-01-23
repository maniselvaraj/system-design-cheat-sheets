
# Replication


<table>
  <tr>
   <td><strong>Pros</strong>
   </td>
   <td><strong>Cons</strong>
   </td>
  </tr>
  <tr>
   <td rowspan="13" >
<ul>

<li>Latency
<ul>
<li>Geolocated read (latency)
</li> 
</ul>

<li>Availability
<ul>
<li>Failover is good for availability
<li>2 datacenters (availability), DR and data resiliency 
</li> 
</ul>

<li>Scalability and Performance
<ul>
<li>Async replication is good for latency and throughput
</li>
</ul>

<li>Eventual consistency in:
<ul>
 <li>master/slave
 <li>Multi-master
</li> 
</ul>

<li>Strong consistency using:
<ul>
<li>2 phased commit
<li>Paxos
</li> 
</ul>

<li>Single master is popular in financial institutions

</li>
</ul>
   </td>
   <td rowspan="13" >
<ul>

<li>Replication lag affects mainly "consistency" 
<ul>
<li>Replication lag = leader - follower
<li>C in ACID is affected; Eventual consistency or loose consistency 
</li> 
</ul>


<li>Write heavy apps will bog down slaves
<li>Read heavy apps require more slaves resulting in more replication lag
<li>Latency and availability are hit if synchronous replication is used
<li>Need for serialization, ordering, timestamp
<li>No global transaction possible

<li>Single Master
    <ul>
        <li>mostly async replication - so potential data loss
        <li>inconsistent data upon failure
        </li>
    </ul>
<li>Multi-master and leaderless setup: 
<ul>
 <li>Could lead to Write conflict
<li>Latency affected due to synchronization
<li>Multi-master setup needs more hardware like loadbalancer
</li> 
</ul>


<li>Failover could lead to: 
<ul>
 
<li>Stale data
 
<li>Discarded writes
 
<li>Split brain
</li> 
</ul>
</li> 
</ul>
   </td>
  </tr>
</table>



## How is replication achieved?



* Statement based
* Write ahead logs (WAL)
    * storage engine logs
* Logical (row based)
    * Copy all insert/delete/update statements
* Trigger based
    * Custom code


## Situations and Solutions for Replication Lag Issues



1. Read-after-write consistency (RAW)
    1. Read your writes to ensure your write is OK
    2. Not for other users i.e., A writes and B cannot read
    3. Some techniques
        1. Always read from leader
        2. Read from leader for some initial minutes i.e., 1 minute
        3. Client uses timestamp and so replica honors it
    4. Cross device RAW is harder
        1. Ex: Google docs?
        2. Solution?
3. Monotonic Reads (MR)
    1. Read going backwards
    2. Strong consistency &lt; MR &lt; Eventual Consistency
    3. Solution: User to read from same replica
4. Consistent prefix reads (CPR)
    1. Maintain write order
    2. Solution:
        1. Use causal order
        2. Use happens-before and concurrency


## Leader based Replication



* Single leader
* Multi-master
* Sync vs Async
    * Async: Non-blocking
    * Sync: Guaranteed. Blocking.

<table>
  <tr>
   <td><strong>Pros</strong></td>
   <td><strong>Cons</strong></td>
  </tr>
  <tr>
   <td>
<ul>

<li>Better performance (latency, availability, throughput)

<li>Network tolerance

<li>High availability

<li>Suited for offline apps

<li>Suited for collaborative editing 
<ul>
 
<li>Can be single master
 
<li>Multi-master for faster operation
</li> 
</ul>

<li>Weak consistency: Suitable for VOIP, Video, Gaming
<li>Eventual consistency: Suitable for search engine, Amazon S3
<li>Strong consistency: Suitable for file system, RDBMS
</li>
</ul>
   </td>
   <td>
<ul>

<li>Write conflict

<li>Auto incrementing keys will be problematic

<li>Triggers

<li>Constraints (unique key)
</li>
</ul>
   </td>
  </tr>
</table>



### Solutions for Multi-master write conflicts

1. Use synchronous write - same as single master
2. Avoid write conflict
    1. Use home datacenter for writes
    2. Send user to same datacenter and use same master (single) within datacenter
3. Converge
    1. Use last write wins (causes data loss)
        1. Use UUID (time stamp, random text, uuid on hash)
        2. Largest UUID wins
    2. Use custom merge logic
    3. Record conflict in another store and let the user decide. Ex: confluence
    4. On-write / on-read custom conflict handlers
    5. Automatic conflict resolution
        1. CRDT
        2. Git like 3-way merge
        3. Operational transformation




## Leaderless Replication


* No leader election needed
* No failover needed. Write to many and read from many

### General approaches/pointers for leaderless replication:


1. Quorum
    1. W +  R > N
        1. Write (W) to many nodes
        2. Read (R)  from many nodes
        3. Use version number
        4. W & R must overlap
        5. N must be odd
    2. Cons of Quorum
        1. If W and R do not overlap, it results in Sloppy Quorum
        2. Concurrent write conflicts
        3. Concurrent write and read conflicts
        4. Failed write in some  W and not rolled back in some W
        5. Quorum condition may break if the node in quorum goes down.
3. Sloppy Quorum
    1. Not really a quorum 
    2. Writes go to W. But some nodes in W are not part of N home nodes
        1. Solution: Use hinted handoff after some nodes recover
    3. Good for write availability
    4. Assurance of durability
4. Staleness
    1. Need to monitor staleness
        1. DB metrics in single or multi-master can be used to monitor
        2. Hard in leaderless environment
    2. Solutions for staleness
        1. Read repair. Ex: Client repairs bad data
        2. Anti-entropy process. Ex: background process copies missing data

### Solutions for Leaderless write conflicts

1. Use last write wins (LWW)
    1. Force natural order using UUID, timestamp, random. Pick the largest UUID.
    2. Results in data loss (durability is affected)
2. Use happens before
    1. Use version for every key
    2. Upon write merge all lower version, but keep all higher version. Milk, eggs example.
3. Merge
    1. Use client side merge
    2. Use deletion marker (tomestone)
    3. Use CRDT
4. Version vector
    1. Version across replicas
    2. Version number per replica as well as per key



[Replication](replication.md) | [Partitioning](partitioning.md) | [Transactions](transaction.md) | [Isolation](isolation.md) | [Consistency](consistency.md) | [Distributed Transactions](distributed_transactions.md) | [Consensus](consensus.md)