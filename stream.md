Streaming options on AWS
Status: [under investigation]
Deciders: [EFX Architects and Managers]
Date: [YYYY-MM-DD when the decision was last updated]
Technical Story: [description | ticket/issue URL]

Context and Problem Statement
The migration of KFE to AWS requires a streaming platform for the application to function.  
The current version uses Kafka, however there are options available on AWS.  We would like to assess
the pros/cons of potential choices

Decision Drivers
Performance, message size may be an issue given the usage of avro
Infrastructure, setup of a stream platform on AWS can be difficult
Cloud Agnostic, some platforms are only available on AWS
Cost, unknown cost of potential options when run on the cloud
Schema management, application uses Avro, need to understand support by platform
Retention, what is the need, and what do the platforms support

Considered Options
Kinesis (AWS managed)
Kafka (EC2 - self managed)
Kafka (PAAS - confluent cloud)

Decision Outcome (TBD)
Chosen option: "[option 1]", because [justification. e.g., only option, which meets k.o. criterion decision driver | which resolves force force | … | comes out best (see below)].

Positive Consequences
[e.g., improvement of quality attribute satisfaction, follow-up decisions required, …]
…
Negative consequences
[e.g., compromising quality attribute, follow-up decisions required, …]
…
Pros and Cons of the Options

Kinesis

Neutral, message size due to avro serialization is not an issue
Good, because setup is simple (manual or terraform)
Bad, only on AWS, limited options to run locally
TBD, cost dependent on usage across environments, pay for usage rather than for infra
Bad, schema management will need to be custom built
Bad, retention limited to 7 days, default is 24 hours

Kafka - EC2

Neutral, unchanged from current process
Bad, self managed setup and upgrades, many operational tasks required
Good, can be run on any cloud or locally
TBD, cost dependent on usage across environments, pay for compute rather than by message
Good, schema management available in Kafka library
Good, retention only limited by storage size

Kafka - Confluent Cloud

Neutral, unchanged from current process
Good, managed by Kafka experts
Good, can be accessed from any cloud or locally
TBD, cost dependent on usage across environments, pay for compute rather than by message
Good, schema management available in Kafka library
Good, retention only limited by storage size


Links
[Link type] [Link to ADR]
…


| Description                    | Kafka EC2/S  | KaaS                 | Kinesis             |
|----------------------------|-----------------|-------------------|------------------|
| Message size(Avro)       |  Y                    | Y                       |  Y                     | 
| Infrastructure setup       | Medium          | Easy                 | Easy                 |         
| Agnosticism                  | Y*                    | Y                       | N                     |
| Schema Registry          | Y                      | Y                      | N                      |
| Retention .                    | Unlimited         | Unlimited         | 7 days max**   |
| Perf/Cost ***,****           | ~30k msg/sec | ~30k msg/sec | ~20k msg/sec  |

*While Kafka is agnostic, EC2 is not.  But it's not that complicated to deploy to other servers.
**Unless Kinesis VCR
***Performance #s obtained from https://blog.insightdatascience.com/ingestion-comparison-kafka-vs-kinesis-4c7f5193a7cd
****Decision should be made based on actual needs.  Kinesis' cost goes up significantly based on size/number of messages.  This however should be reconciled with the costs of self maintaining a Kafka cluster, including but not limited to human resources, load balancing, AZ replication etc.  No costs estimate are available for KaaS (Confluent) at this time since usage is not specified.