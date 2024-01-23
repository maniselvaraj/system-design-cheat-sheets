# SQL vs NoSQL in 2022


<table>
  <tr>
   <td><strong>Capability</strong>
   </td>
   <td><strong>RDBMS</strong>
   </td>
   <td><strong>NoSQL</strong>
   </td>
  </tr>


  <tr>
   <td>CAP
   </td>
   <td>Always ACID (strong consistency). 
   </td>
   <td>Generally BASE (eventual consistency)
   </td>
  </tr>

  <tr>
   <td>Schema
   </td>
   <td>
      <li><strong>Schema-on-write</strong></li>
      <li>RDBMS data model focuses on the structure of data and its relationships with other data. (source: AWS docs)</li>


   <td>
<ul>

<li><strong>Schema-on-read</strong> is similar to dynamic type checking in programming languages.

<li>Schema-on-read is good if the collection does not all have the same structure for some reason i.e., data is heterogeneous.
</li>
<li>NoSQL data modeling focuses on access patterns, or how the application is going to consume the data, so it stores the data in a way that supports straightforward query operations. (source: AWS docs)</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>
<ul>

<li>Each record conforms to a fixed schema, meaning the columns must be decided and chosen before data entry

<li>Each row must have data for each column. 

<li>The schema can be altered later, but it involves modifying the whole database and going offline.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Schemas are dynamic. 

<li>Columns can be added on the fly, and each ‘row’ (or equivalent) doesn’t have to contain data for each ‘column.’
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Schema flexibility
   </td>
   <td>Rigid
   </td>
   <td>
<ul>

<li>Elastic.

<li>Upgrade is simpler. 
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Logical Schema
   </td>
   <td>
<ul>

<li>Relational - data is related. 

<li>Each row represents an entity. 

<li>Each column is a data point.
</li>
</ul>
   </td>
   <td>
    Different model:
<ul>

<li>Key/value

<li>Document 

<li>Object store (blob)

<li>Graph store

<li>Column family
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Physical schema instance
   </td>
   <td>Row major
   </td>
   <td>Columnar or column major (different columns need different treatment)
   </td>
  </tr>
  <tr>
   <td>Indexing
   </td>
   <td>Hash index, B-tree index
   </td>
   <td>Hash, B-tree, hybrid 
   </td>
  </tr>
  <tr>
   <td>Transaction
   </td>
   <td>Native, strong support, ACID. 
   </td>
   <td>No. Gives up ACID for scalability and performance
   </td>
  </tr>
  <tr>
   <td>Locking
   </td>
   <td>Row level, deadlocks are possible, deadlock detection is expensive.
   </td>
   <td>Cell or field level locking. New concept called leasing - avoids deadlock.
   </td>
  </tr>
  <tr>
   <td>Interface/Query
   </td>
   <td>SQL is powerful
   </td>
   <td>
<ul>

<li>APIs, SQL. 

<li>Queries are focused on a collection of documents. 

<li>UnQL (Unstructured Query Language). Different databases have different syntax for using UnQL.

<li>Support for specialized query operations. Non-restrictive schema
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Data joins
   </td>
   <td>Well supported. 
   </td>
   <td>No joins. Handle at upper levels
   </td>
  </tr>
  <tr>
   <td>Data relation
   </td>
   <td>Ideal for <strong>many-to-one</strong> and <strong>many-to-many</strong> through the use of normalization and through the use of foreign key relations.
   </td>
   <td>
<ul>

<li>Ideal for <strong>one-to-many</strong> relations

<li>Leads to locality which is better performant

<li>Goes against normalization.

<li>Many-to-x is harder due to lack of joins

<li>Document DBs target data that comes in self-contained documents and relationships between one document and another are rare
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Many-2-many
   </td>
   <td>Harder using mapping table
   </td>
   <td>Graph database. Graph DBs target data where anything is potentially related to everything.
   </td>
  </tr>
  <tr>
   <td>Sharding & Replication
   </td>
   <td>Limited support, layered, not native, and expensive
   </td>
   <td>Native. 
   </td>
  </tr>
  <tr>
   <td>Large data (> 1 TB)
   </td>
   <td>Good for &lt; 1 TB. Degradation in capabilities for > 1 TB
   </td>
   <td>
<ul>

<li>Native.

<li>Greater scalability, large datasets, high write throughput
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Throughput (read and write)
   </td>
   <td>Good for low volumes. Degrade for high volumes and expensive
   </td>
   <td>Excellent. 
   </td>
  </tr>
  <tr>
   <td>Applications
   </td>
   <td>
   </td>
   <td>Use KV store in applications only when the application knows the K. Else this is a waste as the application needs to scan all the keys K. For ex. DynamoDB
   </td>
  </tr>
 
 <tr>
   <td>Availability
   </td>
   <td>   
   </td>
   <td>
   <ul>
   <li>Optimistic Replication: Dont wait for ack from all. Reduces latency and allows continued availability. Introuces temporary data inconsistency.
   <li>Sloppy Quorum and Last Write Wins: Sloppy - A subset of replicas ack. Relaxed quorum leading to inconsistency. Inconsistency resolved by LWW.  
   <li>Hinted Handoff: Temporary store write as hint. Deliver hint later to appropriate node. Prevents data  loss.
   </li>
   </ul>
   </td>
  </tr>
 

  <tr>
   <td>Security
   </td>
   <td>Native. 
   </td>
   <td>Non-existent. Left to higher layers to implement
   </td>
  </tr>
 
  <tr>
   <td>Data integrity
   </td>
   <td>Native. 
   </td>
   <td>No
   </td>
  </tr>
  <tr>
   <td rowspan="3" >Other
   </td>
   <td rowspan="3" >
   </td>
   <td rowspan="3" >
<ul>

<li>Originally a twitter hashtag #NoSQL

<li>Free and open source
</li>
</ul>
   </td>
  </tr>
  <tr>
  </tr>
  <tr>
  </tr>
</table>

