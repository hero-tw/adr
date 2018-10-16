[short title of solved problem and solution]
Status: [accepted | superseeded by ADR-0005 | deprecated | …]
Deciders: [list everyone involved in the decision]
Date: [YYYY-MM-DD when the decision was last updated]
Technical Story: [description | ticket/issue URL]

Context and Problem Statement
[Describe the context and problem statement, e.g., in free form using two to three sentences. You may want to articulate the problem in form of a question.]

Decision Drivers
[driver 1, e.g., a force, facing concern, …]
[driver 2, e.g., a force, facing concern, …]
…
Considered Options
[option 1]
[option 2]
[option 3]
…
Decision Outcome
Chosen option: "[option 1]", because [justification. e.g., only option, which meets k.o. criterion decision driver | which resolves force force | … | comes out best (see below)].

Positive Consequences
[e.g., improvement of quality attribute satisfaction, follow-up decisions required, …]
…
Negative consequences
[e.g., compromising quality attribute, follow-up decisions required, …]
…
Pros and Cons of the Options
[option 1]
[example | description | pointer to more information | …]

Good, because [argument a]
Good, because [argument b]
Bad, because [argument c]
…
[option 2]
[example | description | pointer to more information | …]

Good, because [argument a]
Good, because [argument b]
Bad, because [argument c]
…
[option 3]
[example | description | pointer to more information | …]

Good, because [argument a]
Good, because [argument b]
Bad, because [argument c]
…
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