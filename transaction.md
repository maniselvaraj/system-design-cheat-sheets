
# Transaction

**Pros**

* Works when hardware or application fails
* Handles network issues
* Handles clients writing to DB at the same time
* Handles race conditions between clients

### BASE vs ACID
* BASE: basically available, soft state, and eventual consistent.
   - Basically available: database should always ensure some level of availability, even in the face of network partitions, hardware failures, or other issues. This means that the database system is designed to respond to client requests even if it means sacrificing some level of consistency in the data
   - Soft state Soft state implies that the state of the system can change over time due to various factors, such as node failures, network delays, or concurrent updates
   - Eventually consistent
* ACID: Atomicity, Consistency, Isolation, Durability


<table>
  <tr>
   <td><strong>A - atomicity</strong>
   </td>
   <td><strong>C - consistency</strong>
   </td>
   <td><strong>I - isolation or concurrency</strong>
   </td>
   <td><strong>D - durability</strong>
   </td>
  </tr>
  <tr>
   <td>

<ul>
<li>Should have been called abortability
<li>Each transaction is all or nothing
<li>Steps are indivisible - sequence of operations is treated as a single logical unit of work
<li>Either commit or abort
</li>
</ul>
Techniques:
<ul>
<li>Implemented using a log for crash recovery
<li>DBs provide complex atomic operations
<li>Compare and set operation
</li>
</ul>
   </td>
   <td>
<ul>

<li>Being in a good state
<li>Any transaction will bring the DB from one valid state to another and not intermediate
<li>Application specific guarantee.
<li>AID of ACID are properties of the DB
<li>C of ACID is the user-controlled property of application i.e., application must define the valid rules and constraints that lead to a consistent final state.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Concurrency of transactions
<li>2 transactions concurrently has the same effect as executed serially - aka serialization
<li>Defines when the changes to Db become visible and what changes may become visible
<li>Isolation levels, such as Read Uncommitted, Read Committed, Repeatable Read, and Serializable, define the degree of isolation provided by the database
</li>
</ul>
   </td>
   <td>
<ul>

<li>No loss. Once transaction is committed, it will remain permanent
<li>Promise that data will not be forgotten
</li>
</ul>
   </td>
  </tr>
</table>

[Replication](replication.md) | [Partitioning](partitioning.md) | [Transactions](transaction.md) | [Isolation](isolation.md) | [Consistency](consistency.md) | [Distributed Transactions](distributed_transactions.md) | [Consensus](consensus.md)