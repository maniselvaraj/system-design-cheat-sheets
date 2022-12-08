
# Transaction

**Pros**



* Works when hardware or application fails
* Handles network issues
* Handles clients writing to DB at the same time
* Handles race conditions between clients

**BASE**: basically available, soft state, and eventual consistent

**ACID**: Atomicity, Consistency, Isolation, Durability


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

<li>Each txn is all or nothing

<li>Steps are indivisible

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

<li>Any txn will bring the DB from one valid state to another and not intermediate

<li>Application specific guarantee.

<li>AID are properties of the DB

<li>C is the property of application
</li>
</ul>
   </td>
   <td>
<ul>

<li>Concurrency of Txn

<li>2 txns concurrently has the same effect as executed serially - aka serialization

<li>Defines when the changes to Db become visible and what changes may become visible
</li>
</ul>
   </td>
   <td>
<ul>

<li>No loss

<li>Once Txn is committed, it will remain so

<li>Promise that data will not be forgotten
</li>
</ul>
   </td>
  </tr>
</table>

[Replication](replication.md) | [Partitioning](partitioning.md) | [Transactions](transaction.md) | [Isolation](isolation.md) | [Consistency](consistency.md) | [Distributed Transactions](distributed_transactions.md) | [Consensus](consensus.md)