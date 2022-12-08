

# Consensus


## Consensus Algorithms



* Properties
    * Agreement: No 2 nodes decide differently
    * Validity: Node decided value proposed by someone else
    * Termination: all correct processes eventually reach the decision
* Algorithms
    * Need to broadcast
    * Need ordered broadcast
        * Atomic aka total order broadcast gives reliable delivery and total order
    * Broadcast leads to chicken-egg problem i.e., who is the first leader
        * So use epoch number: ballot number in Paxos, term number in RAFT, 
        * Epoch gives total order and monotonic increase
    * Use quorum

<table>
  <tr>
   <td>
<strong>Pros</strong>
   </td>
   <td><strong>Cons</strong>
   </td>
  </tr>
  <tr>
   <td>
<ol>

<li>For multipartition scenarios

<li>For Leader election

<li>For Atomic commit

<li>For Ordering events

<li>In general for  
<ol>
 
<li>Agreement
 
<li>Integrity
 
<li>Validity
</li> 
</ol>

<li>Fault tolerance

<li>Total order broadcast can implement linearizable atomic operations
</li>
</ol>
   </td>
   <td>
<ol>

<li>Need strict majority

<li>Hard to add/remove nodes due to voting

<li>Timeouts could lead to frequent leader election

<li>Network problems could lead to leader resignation in RAFT

<li>Voting is synchronous while DB replication is usually asynchronous
</li>
</ol>
   </td>
  </tr>
</table>



## Consensus Implementations



1. ZAB (Zookeeper atomic broadcast)
    1. Elect leader
    2. Leader proposes new epoch
    3. Followers respond to proposal
    4. Leader receives client message and broadcasts
    5. Leader waits for quorum to acknowledge
    6. Final commit
2. Paxos
    1. Proposers, acceptors, learners
    2. Quorum based
    3. Sequence
        1. Voting phase to elect leader
        2. Replication phase to broadcast value
    4. Variations: Multi-paxos, fast paxos
3. RAFT
    1. Newer since 2013
    2. Sequence
        1. Leader election
        2. Periodic heartbeat from leader
        3. Leader log replication / broadcast
4. Byzantine Consensus - PBFT
    1. Practical byzantine fault tolerance


[Replication](replication.md) | [Partitioning](partitioning.md) | [Transactions](transaction.md) | [Isolation](isolation.md) | [Consistency](consistency.md) | [Distributed Transactions](distributed_transactions.md) | [Consensus](consensus.md)