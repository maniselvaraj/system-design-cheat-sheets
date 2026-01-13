# AWS Database Services Comparison: Aurora, DocumentDB, DynamoDB, and ElastiCache

## Overview
This document provides a comprehensive comparison of AWS managed database services based on workload patterns, scalability, consistency, availability, and multi-region deployment patterns.

---

## [1] Workload & Access Patterns

| Criteria | Aurora | DocumentDB | DynamoDB | ElastiCache |
|----------|---------|------------|----------|-------------|
| **Read vs Write Intensity** | Optimized for both; excellent read scaling with up to 15 read replicas | Optimized for read-heavy; supports up to 15 read replicas | Optimized for balanced and write-heavy workloads with auto-scaling | Primarily read-heavy; write-through/write-behind patterns for writes |
| **Read/Write Ratio** | Balanced to read-heavy (1:10 scale with read replicas) | Read-heavy (1:15 scale with read replicas) | Highly flexible; supports both read-heavy and write-heavy (unlimited throughput with on-demand mode) | Read-heavy (typical cache use case) |
| **Transactional Type** | OLTP (Online Transaction Processing) | OLTP with document model | OLTP with high velocity | N/A (caching layer) |
| **Query Patterns** | Complex relational joins, multi-table transactions, aggregations, full SQL support | Document queries, nested object access, aggregations, limited joins | Simple and complex key-value, secondary indexes, global secondary indexes, PartiQL queries | Simple key-value, sorted sets, hash operations, pub/sub |
| **Data Access Latency** | Single-digit milliseconds (read replicas: 10-20ms) | Single-digit milliseconds (10-20ms typical) | Single-digit milliseconds (can achieve sub-millisecond with DAX) | Sub-millisecond (microsecond latency for in-memory) |
| **Batch vs Real-time** | Both supported; batch via SQL, real-time via connection pooling | Both supported; batch operations via bulk writes | Optimized for real-time; supports batch via BatchGetItem/BatchWriteItem | Real-time access; cache-aside pattern for batch |
| **Event-driven/Streaming** | Supports via Aurora Database Activity Streams, Binlog replication | Supports via Change Streams (similar to MongoDB) | Native integration with DynamoDB Streams, Kinesis, Lambda triggers | Supports via Redis Streams or Valkey Streams |

**References:**
- [Aurora Global Database](https://aws.amazon.com/rds/aurora/global-database/)
- [DocumentDB Global Clusters](https://aws.amazon.com/documentdb/global-clusters/)
- [DynamoDB Global Tables](https://aws.amazon.com/dynamodb/global-tables/)
- [ElastiCache Global Datastore](https://aws.amazon.com/elasticache/features/global-datastore/)

---

## [2] Scalability Characteristics

| Criteria | Aurora | DocumentDB | DynamoDB | ElastiCache |
|----------|---------|------------|----------|-------------|
| **Vertical Scalability Limits** | Up to 128 TiB storage, instance types up to 244 GiB RAM (db.r6g.16xlarge and larger) | Up to 128 TiB storage per cluster, instances up to 768 GiB RAM (r6g.16xlarge) | No practical limits (serverless) | Instance-based: up to 317 GiB RAM (r7g.16xlarge) |
| **Horizontal Scalability** | Read scaling via up to 15 read replicas; write scaling limited to primary instance | Read scaling via up to 15 read replicas; write scaling limited to primary instance | Unlimited horizontal scaling via partitioning (adaptive capacity) | Horizontal scaling via cluster mode with up to 500 shards (Redis/Valkey) |
| **Auto-scaling Capabilities** | Auto-scaling for read replicas (1-15), Aurora Serverless v2 for compute and storage | Auto-scaling for read replicas; manual instance scaling | Full auto-scaling for read/write capacity (on-demand mode) or provisioned with auto-scaling policies | Auto-scaling for replicas; ElastiCache Serverless (preview) |
| **Sharding/Partitioning** | N/A (single-master architecture); application-level sharding possible | N/A (single-master); application-level sharding required for write scaling | Native partition key-based sharding with automatic distribution | Native sharding in cluster mode (up to 500 shards per cluster) |
| **Max Data Size** | 128 TiB per cluster (auto-expanding storage) | 128 TiB per cluster | Unlimited (400 KB per item limit) | Cluster mode: 500 nodes × instance memory; Non-cluster: single instance memory |

**References:**
- [Aurora User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database.html)
- [DocumentDB Replication](https://docs.aws.amazon.com/documentdb/latest/developerguide/replication.html)
- [DynamoDB Limits](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Limits.html)
- [ElastiCache Replication](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/Redis-Global-Datastore.html)

---

## [3] Consistency & Data Guarantees

| Criteria | Aurora | DocumentDB | DynamoDB | ElastiCache |
|----------|---------|------------|----------|-------------|
| **Consistency Model** | Strong consistency (synchronous within region); eventual across regions in Global Database | Strong consistency (synchronous within region); eventual consistency for global clusters | Tunable: eventual (default) or strong consistency per read; Multi-Region Strong Consistency (MRSC) in preview | Eventual consistency for cross-region; strong within cluster |
| **ACID Transaction Support** | Full ACID compliance (MySQL/PostgreSQL compatible) | ACID transactions within a replica set (similar to MongoDB 4.0) | ACID transactions across multiple items/tables within a single region (up to 100 items) | Limited; supports transactions in Redis/Valkey 5.0+ (multi/exec) but not ACID across keys |
| **Transaction Scope** | Multi-table, multi-row, cross-schema within single cluster | Multi-document within same replica set | Multi-item, multi-table within same region and account | Multi-command within single connection (optimistic locking) |
| **Isolation Levels** | READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, SERIALIZABLE (PostgreSQL); READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, SERIALIZABLE (MySQL) | Snapshot isolation (read committed default) | Serializable isolation for transactions | Optimistic locking with WATCH command |
| **Conflict Resolution** | N/A for single-region (strong consistency); last-write-wins for Global Database cross-region writes | Last-write-wins for global clusters | Last-write-wins (LWW) for MREC; coordinated writes for MRSC | Last-write-wins for Global Datastore |
| **Idempotency Support** | Application-level (supported via conditional updates, unique constraints) | Application-level (supported via unique indexes, conditional writes) | Built-in via conditional writes (ConditionExpression), client request tokens | Application-level (requires custom implementation) |
| **Read Replica Support** | Yes, up to 15 Aurora Replicas per cluster with <10ms lag | Yes, up to 15 replicas per cluster with <10ms lag | N/A (built-in replication across 3 AZs); Global Tables for cross-region | Yes, up to 5 replicas per shard in cluster mode |
| **Multi-tenant Scalability** | Excellent; supports thousands of databases per cluster via schema/database separation | Good; supports multiple databases per cluster | Excellent; partition key design enables isolated tenant data within single table or separate tables | Good; namespace separation via key prefixes; separate clusters recommended for isolation |

**References:**
- [Aurora Global Database for Multi-Region Applications](https://aws.amazon.com/blogs/database/use-amazon-aurora-global-database-to-build-resilient-multi-region-applications/)
- [DocumentDB Global Clusters Overview](https://docs.aws.amazon.com/documentdb/latest/developerguide/global-clusters.html)
- [DynamoDB Multi-Region Strong Consistency Preview](https://aws.amazon.com/about-aws/whats-new/2024/12/amazon-dynamodb-global-tables-previews-multi-region-strong-consistency/)
- [DynamoDB Global Tables - How It Works](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/V2globaltables_HowItWorks.html)

---

## [4] Availability & Resilience

| Criteria | Aurora | DocumentDB | DynamoDB | ElastiCache |
|----------|---------|------------|----------|-------------|
| **High Availability (Multi-AZ)** | Yes, automatic across 3 AZs with 6-way replication | Yes, automatic across 3 AZs with 6-way replication | Yes, automatic across 3 AZs (standard tables) | Yes, multi-AZ with automatic failover |
| **Automated Failover** | Yes, automatic failover to read replica in <30 seconds typically | Yes, automatic failover to replica in <30 seconds typically | Automatic (transparent to application) | Yes, <1 minute for multi-AZ; <1 minute for Global Datastore promotion |
| **RPO Guarantees** | Zero within region (synchronous); ~1 second for Global Database (typically <1s lag) | Zero within region; <1 second for global clusters (typically <1s lag) | Zero for single-region; typically <1-2 seconds for Global Tables MREC; zero for MRSC | N/A for single region cache; <1 second for Global Datastore |
| **RTO Guarantees** | <30 seconds for same-region failover; <1 minute for Global Database cross-region promotion | <30 seconds for same-region; <1 minute for global cluster promotion | Automatic (no downtime); <1 minute for Global Tables regional failover | <1 minute for multi-AZ; <1 minute for Global Datastore promotion |
| **Fault Tolerance** | Survives loss of 2 AZs for reads, 3 AZs for writes (6-way replication) | Survives loss of 2 AZs (6-way replication) | Survives loss of entire AZ transparently | Survives loss of AZ with multi-AZ configuration |
| **Backup & Restore** | Continuous incremental backups to S3; automated daily snapshots; retention 1-35 days | Continuous incremental backups; automated daily snapshots; retention 1-35 days | Continuous backups with point-in-time recovery (PITR) up to 35 days; on-demand backups | Redis/Valkey: automated snapshots; manual snapshots; Memcached: no persistence |
| **Point-in-Time Recovery** | Yes, any second within retention period (1-35 days) | Yes, any second within retention period (1-35 days) | Yes, any second within last 35 days (when PITR enabled) | Yes (Redis/Valkey only) within snapshot window |
| **Single-Region DR** | Multi-AZ automatic failover; read replicas for DR; cloning for fast recovery | Multi-AZ automatic failover; read replicas; snapshot restore | Multi-AZ automatic; on-demand backups; global tables within region | Multi-AZ automatic failover; backup/restore from snapshots |
| **Multi-Region DR** | Aurora Global Database: up to 5 secondary regions, <1 min RTO, <1s RPO; headless clusters for cost-effective DR | Global Clusters: up to 10 secondary regions (as of 2025), <1 min RTO, <1s RPO; headless clusters for cost optimization | Global Tables: up to 6 regions, automatic multi-region replication, Active-Active | Global Datastore: up to 2 secondary regions, <1 min RTO, <1s RPO |

**References:**
- [Aurora Global Database Disaster Recovery](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database-disaster-recovery.html)
- [Aurora Headless Clusters for Multi-Region Resiliency](https://aws.amazon.com/blogs/database/achieve-cost-effective-multi-region-resiliency-with-amazon-aurora-global-database-headless-clusters/)
- [Deploy Multi-Region Aurora Applications](https://aws.amazon.com/blogs/database/deploy-multi-region-amazon-aurora-applications-with-a-failover-blueprint/)
- [DocumentDB Global Clusters Disaster Recovery](https://docs.aws.amazon.com/documentdb/latest/developerguide/global-clusters-disaster-recovery.html)
- [DocumentDB Headless Clusters](https://aws.amazon.com/blogs/database/use-headless-clusters-in-amazon-documentdb-for-cost-effective-multi-region-resiliency/)
- [DynamoDB Multi-Region Strategy](https://aws.amazon.com/blogs/database/part-1-accelerate-your-multi-region-strategy-with-amazon-dynamodb/)
- [ElastiCache Global Datastore Disaster Recovery](https://disaster-recovery.workshop.aws/en/services/databases/elasticache/global-datastore.html)

---

## [5] Deployment Patterns

### Single Region Active-Active with Strong Consistency

| Database | Support | Mechanisms for Strong Consistency | References |
|----------|---------|-----------------------------------|------------|
| **Aurora** | Yes (within cluster) | - 6-way storage replication across 3 AZs with quorum-based writes (4/6 write quorum, 3/6 read quorum)<br>- Synchronous replication to storage layer<br>- Single writer instance ensures serializable consistency<br>- Read replicas have <10ms lag but serve eventually consistent reads<br>- Aurora PostgreSQL: Synchronous replication to specific replicas possible | [Aurora Global Database](https://aws.amazon.com/rds/aurora/global-database/)<br>[Aurora Architecture](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Overview.html) |
| **DocumentDB** | Yes (within replica set) | - 6-way storage replication across 3 AZs<br>- Quorum-based writes similar to Aurora<br>- Single primary node for writes ensures consistency<br>- Read replicas provide eventually consistent reads by default<br>- Read concern "majority" for strongly consistent reads possible | [DocumentDB High Availability](https://docs.aws.amazon.com/documentdb/latest/developerguide/replication.html)<br>[DocumentDB Architecture](https://docs.aws.amazon.com/documentdb/latest/developerguide/how-it-works.html) |
| **DynamoDB** | Yes | - Automatic 3-way synchronous replication across 3 AZs<br>- Strongly consistent reads via ConsistentRead=true parameter<br>- All writes are strongly consistent by default<br>- Leader-based replication ensures linearizability<br>- Conditional writes provide optimistic concurrency control | [DynamoDB Read Consistency](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html)<br>[DynamoDB Architecture](https://www.allthingsdistributed.com/2012/01/amazon-dynamodb.html) |
| **ElastiCache** | Limited | - Single primary node for writes within cluster<br>- Synchronous replication not available (Redis/Valkey uses asynchronous replication)<br>- Read replicas serve eventually consistent data<br>- Multi-AZ provides HA but not strong consistency for reads<br>- Application-level locking required for strong consistency guarantees | [ElastiCache Replication](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/Replication.html)<br>[ElastiCache Architecture](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/WhatIs.html) |

### Multi-Region Active-Active with Strong Consistency

| Database | Support | Mechanisms for Strong Consistency | References |
|----------|---------|-----------------------------------|------------|
| **Aurora** | Limited (Active-Passive with Write Forwarding) | **Default Active-Passive Architecture:**<br>- Aurora Global Database provides Active-Passive architecture<br>- Primary region accepts writes; secondary regions are read-only<br>- Managed planned failover (switchover) provides RPO=0 via synchronization before promotion<br>- Unplanned failover has RPO ~1 second (based on replication lag)<br>- Physical replication via dedicated infrastructure<br><br>**Write Forwarding with GLOBAL Consistency Mode (PostgreSQL 14.9+, 15.4+; MySQL 2.08.1+):**<br>- Enables writes from secondary regions to be forwarded to primary region<br>- GLOBAL consistency mode ensures read-after-write consistency across all regions<br>- Secondary region queries wait for committed changes from both primary and all other secondary regions<br>- Session sees its own writes plus all committed data globally when query begins<br>- Provides strongest consistency but with performance trade-offs due to cross-region synchronization<br>- Useful for multi-region applications requiring strong consistency guarantees<br>- Not true active-active (all writes still processed by primary), but enables multi-region write capability with strong read consistency | [Aurora Global Database Multi-Region](https://aws.amazon.com/blogs/database/use-amazon-aurora-global-database-to-build-resilient-multi-region-applications/)<br>[Aurora Failover Blueprint](https://aws.amazon.com/blogs/database/deploy-multi-region-amazon-aurora-applications-with-a-failover-blueprint/)<br>[Aurora Switchover](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database-disaster-recovery.html#aurora-global-database-failover)<br>[Aurora Write Forwarding](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database-write-forwarding.html)<br>[Write Forwarding with PostgreSQL](https://aws.amazon.com/blogs/database/using-write-forwarding-with-amazon-aurora-global-database-for-postgresql/)<br>[Implementing Write Synchronization](https://aws.amazon.com/blogs/database/implementing-run-time-write-synchronization-in-amazon-aurora-global-database/) |
| **DocumentDB** | No (Active-Passive only) | - Global Clusters provide Active-Passive architecture<br>- Primary region accepts writes; up to 10 secondary regions are read-only<br>- Asynchronous replication with <1 second lag typically<br>- Planned switchover and unplanned failover supported<br>- Headless clusters for cost-effective DR without read traffic<br>- Application-level active-active requires custom conflict resolution | [DocumentDB Global Clusters](https://aws.amazon.com/documentdb/global-clusters/)<br>[DocumentDB Headless Clusters](https://aws.amazon.com/blogs/database/use-headless-clusters-in-amazon-documentdb-for-cost-effective-multi-region-resiliency/)<br>[DocumentDB DR](https://docs.aws.amazon.com/documentdb/latest/developerguide/global-clusters-disaster-recovery.html) |
| **DynamoDB** | Yes (Preview for MRSC) | **Multi-Region Eventual Consistency (MREC) - Generally Available:**<br>- Active-Active writes to any region<br>- Last-write-wins (LWW) conflict resolution based on timestamp<br>- Asynchronous replication, typically 1-2 seconds<br>- Eventually consistent reads across regions<br><br>**Multi-Region Strong Consistency (MRSC) - Preview:**<br>- Active-Active writes with strong consistency guarantee<br>- Successfully acknowledged write in any region immediately available for reads in all regions<br>- Consensus-based protocol ensures linearizability across regions<br>- Higher latency for writes due to cross-region coordination<br>- Currently available in US East (N. Virginia), US East (Ohio), US West (Oregon) | [DynamoDB Global Tables](https://aws.amazon.com/dynamodb/global-tables/)<br>[DynamoDB MRSC Preview](https://aws.amazon.com/about-aws/whats-new/2024/12/amazon-dynamodb-global-tables-previews-multi-region-strong-consistency/)<br>[DynamoDB Multi-Region Strategy](https://aws.amazon.com/blogs/database/part-1-accelerate-your-multi-region-strategy-with-amazon-dynamodb/)<br>[DynamoDB Global Tables Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html) |
| **ElastiCache** | No (Active-Passive only) | - Global Datastore provides Active-Passive architecture<br>- Primary region accepts writes; up to 2 secondary regions are read-only<br>- Asynchronous replication with <1 second lag typically<br>- Promotion of secondary to primary in <1 minute<br>- Eventually consistent reads from secondary regions<br>- No built-in conflict resolution for multi-region writes | [ElastiCache Global Datastore](https://aws.amazon.com/elasticache/features/global-datastore/)<br>[ElastiCache Multi-Region Session Store](https://aws.amazon.com/blogs/database/build-a-multi-region-session-store-with-amazon-elasticache-for-valkey-global-datastore/)<br>[ElastiCache Global Datastore Documentation](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/Redis-Global-Datastore.html) |

---

## Summary Recommendations by Use Case

### Choose **Aurora** when:
- You need full ACID compliance with complex SQL queries
- Relational data model with joins and foreign keys
- Strong consistency within region is critical
- MySQL or PostgreSQL compatibility required
- Read-heavy workloads with occasional writes
- You need proven enterprise-grade relational database

### Choose **DocumentDB** when:
- You have document-based data models (JSON/BSON)
- MongoDB compatibility and migration path needed
- Flexible schema with nested documents
- Read-heavy workloads with occasional writes
- You need aggregation pipelines and rich queries
- Geospatial queries required

### Choose **DynamoDB** when:
- You need single-digit millisecond latency at any scale
- Massive scale with unpredictable or spiky traffic
- Serverless architecture with zero operations overhead
- Key-value or document access patterns
- Write-heavy or balanced read/write workloads
- Multi-region active-active with eventual consistency (or strong consistency in preview)
- Event-driven architectures with Streams integration

### Choose **ElastiCache** when:
- You need sub-millisecond latency (microseconds)
- Caching layer for database or API acceleration
- Session store, leaderboards, real-time analytics
- Pub/sub messaging patterns
- Temporary/ephemeral data
- Not primary data store (used alongside other databases)

---

## Additional References

### Aurora Resources
- [Aurora High Availability and Disaster Recovery Whitepaper](https://d1.awsstatic.com/Amazon%20Aurora%20High%20Availability%20and%20Disaster%20Recovery%20Features%20for%20Global%20Resilience%20Whitepaper.pdf)
- [Cross-Region DR with Aurora Global Database for PostgreSQL](https://aws.amazon.com/blogs/database/cross-region-disaster-recovery-using-amazon-aurora-global-database-for-amazon-aurora-postgresql/)
- [Scale Applications with Multi-Region EKS and Aurora](https://aws.amazon.com/blogs/database/part-1-scale-applications-using-multi-region-amazon-eks-and-amazon-aurora-global-database/)
- [Using Write Forwarding in Aurora Global Database](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database-write-forwarding.html)
- [Using Write Forwarding with Aurora PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database-write-forwarding-apg.html)
- [Using Write Forwarding with Aurora MySQL](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database-write-forwarding-ams.html)
- [Implementing Run-Time Write Synchronization in Aurora Global Database](https://aws.amazon.com/blogs/database/implementing-run-time-write-synchronization-in-amazon-aurora-global-database/)

### DynamoDB Resources
- [DynamoDB Write Modes with Global Tables](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-global-table-design.prescriptive-guidance.writemodes.html)
- [Create DynamoDB Global Tables with MRSC](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/example_dynamodb_Scenario_MRSCGlobalTables_section.html)

### DocumentDB Resources
- [DocumentDB 10 Secondary Regions Announcement](https://aws.amazon.com/about-aws/whats-new/2025/07/amazon-documentdb-mongodb-ccompatibility-support-secondary-region-clusters/)
- [Deploy Globally Available MongoDB-Compatible Clusters](https://pages.awscloud.com/Deploy-globally-available-MongoDB-compatible-clusters-with-Amazon-DocumentDB_2021_0002-DAT_OD.html)

### ElastiCache Resources
- [ElastiCache Database Caching Strategies Whitepaper](https://docs.aws.amazon.com/whitepapers/latest/database-caching-strategies-using-redis/amazon-elasticache-for-redis-global-datastore.html)
- [ElastiCache Global Datastore Disaster Recovery Workshop](https://disaster-recovery.workshop.aws/en/labs/basics/elasticache-global-datastore.html)

### DR Guides
- [AWS DB DR Strategies](https://docs.aws.amazon.com/prescriptive-guidance/latest/strategy-database-disaster-recovery/welcome.html)
- [AWS Multi-region Fundamentals](https://docs.aws.amazon.com/prescriptive-guidance/latest/aws-multi-region-fundamentals/introduction.html)

---

**Document Version:** 1.0
**Last Updated:** January 2026
**Author:** AWS Database Architect
