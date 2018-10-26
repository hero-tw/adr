 # Data Persistence for KFE
 
* Status: [under investigation]
* Deciders: [EFX Architects and Managers]
* Date: [2018-10-22 when the decision was last updated]
 
Technical Story: [[ADR for Persistence](https://trello.com/c/85PvOlRY/51-spike-adr-for-persistence-rds-dynamo-cassandra)]

## Context and Problem Statement

Currently, Cassandra is the data persistence technology implemented in KFE, but due to its basic use-case in KFE and the upcoming migration to AWS, other database options are being considered as a replacement to simplify the migration and maintenance.
Here, we assess the pros and cons of each database that the team has investigated so far.

## Decision Drivers

* Performance: must keep up with frequency and size of record storage
* High Availability: scaling and data replication ensure accessibility of data
* Ease of Use: dev-friendliness and learning curve
* Infrastructure: ease of integration with public cloud platform
* Cloud Agnostic: client favors cloud agnosticism for potential future optimization / vendor or client agreements
* Cost: depends on usage

## Considered Options

* Amazon RDS with Postgres (AWS managed)
* Amazon DynamoDB (AWS managed)
* Cassandra (Apache)
* MongoDB

## Decision Outcome (TBD)

Chosen option: MongoDB. Although Cassandra is currently being used, given the migration to public cloud infrastructure and the need for basic data persistence, a simpler and more easily maintained technology is considered more desirable. 
MongoDB is a cloud-agnostic technology with scaling benefits like Cassandra's and proven products for integrating with the major public cloud platforms.

### Positive Consequences
MongoDB scaling and high availability benefits similar to Cassandra but has simpler implementation and management. Schema-less and JSON-like documents allows for flexible data structure. MongoDB Atlas, MongoDB Stitch (for serverless), and MongoDB Cloud manager simplify integration and management for any of the major cloud platforms.

### Negative consequences
While MongoDB has built-in replication with auto-elections so you can set up a secondary database that can be auto-elected if the primary database becomes unavailable, it requires some setup to do replication. Reads and writes are committed to the primary replica first and then replicated to the secondary replicas.

MongoDB has a single master. While the auto-elect process happens automatically, it can take 10 to 40 seconds for it to occur. While this is happening, you can not write to the replica set.

## Pros and Cons of the Options

### Amazon RDS with Postgres

* [Performance] Neutral, depends on data structure but relative to the other options for writing large amounts of data, it lags behind
* [High Availability] Neutral, 
* [Ease of Use] Good, generally developer-friendly SQL database
* [Infrastructure] Good, easily integrated with AWS (cloud native product)
* [Cloud Agnostic] Bad, AWS-specific product
* [Cost] TBD; depends on usage beyond free tier allowance and pricing contract (hourly rates, per request rates, and bulk duration rates)

### Amazon DynamoDB

* [Performance] Good, meant for large data quantities
* [High Availability] Good, automatic horizontal scaling and data replication supported
* [Ease of Use] Good, NoSQL has a learning curve but is manageable thereafter
* [Infrastructure] Good, easily integrated with AWS (cloud native product)
* [Cloud Agnostic] Bad, AWS-specific product
* [Cost] TBD; depends on quantity of read/write units and which regions

### Cassandra

* [Performance] Good, depends on configuration (configured with queries in mind) but meant for large data quantities
* [High Availability] Good, automatic horizontal scaling and data replication supported
* [Ease of Use] Bad, relatively steep learning curve and data modelling must be configured with queries in mind
* [Infrastructure] Bad, relative to the other options, a lot of management work will be put into integration and upkeep
* [Cloud Agnostic] Good, isn't tied to any public cloud platform
* [Cost] TBD; depends on usage where you pay for infrastructure where Cassandra is hosted (more regions can multiply cost)

###MongoDB

* [Performance] Good, meant for large data quantities and secondary indexes are a first-class construct in MongoDB so you can index on any property 
* [High Availability] Good, automatic horizontal scaling and data replication supported; another node takes over when master node goes down (but there is downtime during this process)
* [Ease of Use] Good, NoSQL has a learning curve but has expressive object model that makes it developer-friendly
* [Infrastructure] Good, multiple products that simplify integration and management for any of the major cloud platforms
* [Cloud Agnostic] Good, isn't tied to any public cloud platform and has products for integrating with major public cloud vendors in multiple fashions
* [Cost] TBD; depends on usage with multiple plans and tiers depending on needs

## Links
https://aws.amazon.com/rds/

https://aws.amazon.com/dynamodb/

https://aws.amazon.com/blogs/big-data/best-practices-for-running-apache-cassandra-on-amazon-ec2/

https://www.mongodb.com/

https://blog.panoply.io/cassandra-vs-mongodb