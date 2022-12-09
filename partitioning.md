

# Partitioning


<table>
  <tr>
   <td><strong>Pros</strong>
   </td>
   <td><strong>Cons</strong>
   </td>
  </tr>
  <tr>
   <td>
<ul>

<li>Availability, Performance, and Scalability

<li>Better availability when combined with replication
<li>Performance
    <ul>
        <li>Read less, write less, replicate less. 
        <li>More cache hits resulting in better performance
        <li>Reduced index size = faster query time
        </li>
    </ul>
<li>Scalability 
    <ul>
        <li>More throughput due to faster writes 
        <li>Horizontal scalability is cheaper
        </li>
    </ul>
</li>
</ul>
   </td>
   <td>
<ul>

<li>Complex joins
<li>Complex application logic in terms of queries and scatter/gather
<li>Referential integrity is broken (constraints, foreign key)
<li>Rebalancing is costly
<li>Hot spotting
</li>
</ul>
   </td>
  </tr>
</table>



### Types of Partitioning


<table>
  <tr>
   <td><strong>Horizontal aka Range/row based</strong>
   </td>
   <td><strong>Key or Hash based</strong>
   </td>
   <td><strong>Other </strong>
   </td>
  </tr>
  <tr>
   <td>

<table>
  <tr>
   <td>Pro
   </td>
   <td>Con
   </td>
  </tr>
  <tr>
   <td>
    <ul>
    <li>Sorted keys for range query
    </li>
    </ul>
   </td>
   <td>
    <ul>
    <li>Hotspotting. Ex: timestamp. 
    <li>Unbalanced servers.
    </li>
    </ul>
   </td>
  </tr>
</table>


   </td>
   <td>

<table>
  <tr>
   <td>Pro
   </td>
   <td>Con
   </td>
  </tr>
  <tr>
   <td>Better distribution
   </td>
   <td>
    <ul>
    <li>No range query.
    <li>Scatter gather.
    </li>
    </ul>
   </td>
  </tr>
</table>


   </td>
   <td>


* Directory based lookup
* Round robin
* Composite
   </td>
  </tr>
</table>




### Partitioning Criteria

* Tenant
* Geo
* Entity id
* Graph
* Time
* Industry practices
    * B2B: customer ID based partition (salesforce)
    * B2C: Multidimension, multisource (Amazon)
    * B2C2C: Geo based (Instacart)


### Techniques to reduce hotspots



* Use a hash - not great for celebrity
* Add random number to celebrity record alone
    * Results in scatter-gather
    * Extra bookkeeping is needed as random number is needed for celebrity alone




## Rebalancing (partition vs dataset size)



1. In general, do not do Hash Mod N as most keys will have to be moved
2. Fixed number of partitions
    1. size of each partition is proportional to the size of the dataset
    2. Create more logical partitions than nodes i.e., several parts in one node
    3. Pro: mismatched hardware. More partitions on powerful machine
    4. Con: Hard to find the right partition size. 
        1. Small partition: overhead
        2. Large partition: expensive to rebalance
3. Dynamic Partition
    1. number of partitions is proportional to the size of dataset
    2. Popular in range partitioned Db like HBase
    3. Partition split automatically when size limit is reached
    4. Partition is merged automatically if data is deleted.
    5. Can be used in hash based as well 
    6. Mongo 2.4 supports 
4. Partition proportional to nodes
    1. Closer to consistent hashing
    2. Number of partitions proportional to the number of nodes i.e., fixed number of partitions per node
    3. Size of each partition grows proportional to dataset size while number of nodes is consistent
    4. New node randomly chooses fixed number of partitions to split and take ownership of one half


## Request Routing



* Purpose: Find the node that has the data
* Contact any node and if not found, forward the request
* Use routing tier (reverse proxy)
* Make clients partition aware
* In practice, use coordination service like Zookeeper to keep cluster metadata. Zookeeper maintains mapping
* Cassandra/Riak uses gossip protocol to share info
* Request routing degrades to a worst case scenario of scatter/gather or fan out.

## Secondary Partition

Primary key is used to uniquely identify a record. Secondary key is used for search. It is harder to map multiple keys to a partition.

### Type of secondary partition

<table>
  <tr>
   <td>Document based 
   </td>
   <td>Term based
   </td>
  </tr>
  <tr>
   <td>
<ul>

<li>Each partition has its own local index
<li><strong>Pro</strong>: only need to update the partition that contains the ID
<li><strong>Con</strong>: scatter-gather
</li>
</ul>
   </td>
   <td>
<ul>
<li>Global index covering all parts
<li>Term-partitioned
<li><strong>Pro</strong>: fast read
<li><strong>Con</strong>: slow write
</li>
</ul>
   </td>
  </tr>
</table>



[Replication](replication.md) | [Partitioning](partitioning.md) | [Transactions](transaction.md) | [Isolation](isolation.md) | [Consistency](consistency.md) | [Distributed Transactions](distributed_transactions.md) | [Consensus](consensus.md)