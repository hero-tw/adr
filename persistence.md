 # Data Persistence for KFE
 
* Status: [under investigation]
* Deciders: [EFX Architects and Managers]
* Date: [2018-10-22 when the decision was last updated]
 
Technical Story: [[ADR for Persistence](https://trello.com/c/85PvOlRY/51-spike-adr-for-persistence-rds-dynamo-cassandra)]

## Context and Problem Statement

Currently, Cassandra is the data persistence technology implemented in KFE, but given the use-case of a data persistence technology in KFE and the upcoming migration to AWS, other database options are being considered to replace the current Cassandra database.
Here we assess the pros and cons of each database that the team has investigated so far.
## Decision Drivers

* Performance; How many writes/reads per second each database can perform considering size of entries
* Object Modeling
* Ease of Implementation
* Ease of Integrating with AWS
* Documentation on the Database
* Cost


## Considered Options

* Amazon RDS (AWS managed)
* Amazon DynamoDB (AWS managed)
* Cassandra (Apache)


## Decision Outcome (TBD)

Chosen option: "[DynamoDB]", because [DynamoDB allows a sufficient amount of reads and writes to be made on a machine per second, while also scaling horizontally. This optimizes the writing of data, which 
also is beneficial because this database will not be queried often for records unlike Amazon RDS (its role is to simply hold a copy of the data that will be placed in the elastic search). 
There is enough documentation on the technology to allow developers to use it more easily than Cassandra, and it is also a product of Amazon which will ease the integration of DynamoDB into other Amazon services.
].

### Positive Consequences
[e.g., improvement of quality attribute satisfaction, follow-up decisions required, …]
[DynamoDB focuses on storing data that isn't to be queried too much or manipulated, which is exactly what is desired. Horizontal scaling optimizes the speed of writing to the database]
…
### Negative consequences
[The database is not too efficient when it comes to querying the data. You are not able to manipulate the data the way that you are in relational databases, such as joining tables.]
…

## Pros and Cons of the Options

### Amazon RDS

* Neutral, message size due to avro serialization is not an issue
* Good, because setup is simple (manual or terraform)
* Bad, only on AWS, limited options to run locally
* TBD, cost dependent on usage across environments, pay for usage rather than for infra
* Bad, schema management will need to be custom built
* Bad, retention limited to 7 days, default is 24 hours

### Amazon DynamoDB

* Neutral, unchanged from current process
* Bad, self managed setup and upgrades, many operational tasks required
* Good, can be run on any cloud or locally
* TBD, cost dependent on usage across environments, pay for compute rather than by message
* Good, schema management available in Kafka library
* Good, retention only limited by storage size

### Cassandra

*Neutral, unchanged from current process
*Good, managed by Kafka experts
*Good, can be accessed from any cloud or locally
*TBD, cost dependent on usage across environments, pay for compute rather than by message
*Good, schema management available in Kafka library
*Good, retention only limited by storage size


## Links
[Link type] [Link to ADR]
…

---

| Description                    | Kafka EC2/S  | KaaS                 | Kinesis             |
|----------------------------|-----------------|-------------------|------------------|
| Message size(Avro)       |  Y                    | Y                       |  Y                     | 
| Infrastructure setup       | Medium          | Easy                 | Easy                 |         
| Agnosticism                  | Y*                    | Y                       | N                     |
| Schema Registry          | Y                      | Y                      | N                      |
| Retention .                    | Unlimited         | Unlimited         | 7 days max**   |
| Perf/Cost ***,****           | ~30k msg/sec | ~30k msg/sec | ~20k msg/sec  |

1. While Kafka is agnostic, EC2 is not.  But it's not that complicated to deploy to other servers.
1. Unless Kinesis VCR
1. Performance #s obtained from https://blog.insightdatascience.com/ingestion-comparison-kafka-vs-kinesis-4c7f5193a7cd
1. Decision should be made based on actual needs.  Kinesis' cost goes up significantly based on size/number of messages.  This however should be reconciled with the costs of self maintaining a Kafka cluster, including but not limited to human resources, load balancing, AZ replication etc.  No costs estimate are available for KaaS (Confluent) at this time since usage is not specified.