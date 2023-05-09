<!-- Yay, no errors, warnings, or alerts! -->


# Distributed Transactions


## Cons of distributed transactions



1. Locks
    1. Read commit needs locks
    2. 2 phase commit needs to lock all the time
2. Coordinator failure
    1. Coordinator is itself a DB
    2. Single point of failure if not replicated
    3. Stateless apps become stateful due to coordinator
3. 2 phase commit / 3 phase commit needs additional network roundtrip


## Solutions for “A” in ACID
A = All or nothing. Atomicity or abortability
<table>
  <tr>
   <td colspan="2" ><strong>Two Atomic Commit Algorithms</strong>
   </td>
  </tr>
  <tr>
   <td><strong>2 PC</strong>
   </td>
   <td><strong>3 PC</strong>
   </td>
  </tr>
  <tr>
   <td>Coordinator plays big role
<ul>

<li>Prepare phase

<li>Commit or abort phase
</li>
</ul>
   </td>
   <td>3 PC  = 2 PC + timeout
<ul>

<li>Timeouts for cohorts on both sides so they can proceed with commit or abort
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Similar to a Western marriage: "I do" from both is needed
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td><strong>Sequence:</strong>
<ol>

<li>Node proposes transaction
<li>Node gets TransactionID from coordinator
<li>Coordinator sends “prepare” to nodes. Nodes can reply with yes/no
<li>Coordinator sends commit/abort to nodes
</li>
</ol>
   </td>
   <td><strong>Sequence:</strong>
<ol>

<li>Propose: propose and collect values. Coordinator failure is allowed
<li>Prepare: share vote results
<li>Commit/Abort
</li>
</ol>
   </td>
  </tr>
  <tr>
   <td><strong>Pros</strong>
<ul>

<li>Timeout in 3PC is not reliable. So 2PC is popular
</li>
</ul>
   </td>
   <td><strong>Pros</strong>
<ul>

<li>Even if coordinator fails, nodes can proceed with deterministic decision
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td><strong>Cons</strong>
<ul>

<li>Coordinator failure is blocking as nodes will not know what to do
<li>Hence, 2 PC is blocking due to coordinator failure
<li>May need server reboot and manual abort of transactions
<li>N/W round trip cost
</li>
</ul>
   </td>
   <td><strong>Cons</strong>
<ul>

<li>In N/W partition some will abort and some will commit resulting in split brain
<li>More message overhead than 2PC
<li>N/W round trip cost
</li>
</ul>
   </td>
  </tr>
</table>



## Distributed Transactions in Practice



* Solution for A&C in ACID
* XA or X/open Architecture
    * XA/extended architecture for 2PC across heterogeneous tech
    * XA is ‘C’ API. 
    * XA is not a protocol
    * Uses 2PC
* Calvin
    * Reduces time held on locks
    * Agree of execution order and transaction boundary before getting locks
    * Uses Paxos
* Spanner
    * 2PC over consensus group per partition
    * Truetime API for ordering
    * Paxos for translation log replication
* Percolator
    * Snapshot isolation
* Ramp
* SAGA (application level)


## Distributed Transactions in Application level: SAGAS

* SAGAS is application level distributed transaction
* Only supports A,C, and D. No concurrency
* Sequence of local transactions coordinated by async messages
* SAGA = T1 + T2 + T3  …Tn  //upon failure abort all
* Cons
    * Lack of isolation due to local transactions
    * Lost updates
    * Dirty reads
* Solutions for Cons
    * Semantic lock - application level lock
    * Commutative updates design
        * Like REST idempotency
        * Update in any order
    * Read your writes
    * Version file to turn non-commutative to commutative

<table>
  <tr>
   <td colspan="2" >
<strong>Types of SAGA</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Choreography </strong>
   </td>
   <td><strong>Orchestration</strong>
   </td>
  </tr>
  <tr>
   <td>
<ul>

<li>Distributed responsibility

<li>Event based
</li>
</ul>
   </td>
   <td>
<ul>

<li>Central coordination logic

<li>Orchestrator tells what to do
</li>
</ul>
   </td>
  </tr>
</table>

[Replication](replication.md) | [Partitioning](partitioning.md) | [Transactions](transaction.md) | [Isolation](isolation.md) | [Consistency](consistency.md) | [Distributed Transactions](distributed_transactions.md) | [Consensus](consensus.md)
