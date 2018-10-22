 # Container ochestration options on AWS
 
* Status: [under investigation]
* Deciders: [EFX Architects and Managers]
* Date: [2018-10-22]
 
Technical Story: [description | ticket/issue URL]

## Context and Problem Statement

With migrating applications to AWS we will need to decide on a deployment target.
What is known is that we will for sure be deploying docker containers therefore the two options available are ECS or EKS.
We will see what the pros/cons of working with each container orchestrator.

## Decision Drivers

* Cloud Agnostic, some platforms are only available on AWS
* Cost, unknown cost of potential options when run on the cloud
* Ease of Use, how easy is it to use each orchestrator
* Infrastructure, how easy is it to get the deployment target to get up and running
* Adoptability, how available is information about adopting each container orchestrator

## Considered Options

* Elastic Container Service
* Elastic Kubernetes Service

## Decision Outcome (TBD)

Chosen option: "[option 1]", because [justification. e.g., only option, which meets k.o. criterion decision driver | which resolves force force | … | comes out best (see below)].

### Positive Consequences
[e.g., improvement of quality attribute satisfaction, follow-up decisions required, …]
…
### Negative consequences
[e.g., compromising quality attribute, follow-up decisions required, …]
…

## Features

### EKS

* Kubernetes usage is consistent across cloud providers
* It isn't a fully managed service
  * Manual deployment of worker nodes
  * Maintence of worker nodes falls on us, rather than on AWS
  * users for the cluster aren't automatically added, users must be given access
* Cost of service is $0.20 an hour for cluster plus the cost of the EC2 instances being used for workers

### ECS

* It soley exists in the AWS ecosystem
* It's a fully managed service
* Price based on vCPU and memory used by container ($0.0506 per vCPU per hour, $0.0127 per GB per hour)
  * Price can also be based on EC2 compute being used. It's the user's decision

## Evaluating based on Decision Drivers

### EKS

* [Cloud Agnostic] Good, How kubernetes is used is the same no matter where the cluster is living (ie AWS, GCP or bare-metal servers)
* [Cost] Bad, Cost comes out a bit more than ECS due to not being able to utilize 100% of the EC2 instance compute
* [Ease of Use] Good, Using kubernetes is fairly standard, deploying to any cluster (AWS or not) is the same
* [Infrastructure] Neutral, With it not being a fully managed service it leaves a lot of room for user error (ie User cluster access)
* [Adoptability] Neutral, There is a steep learning curve on how to use Kubernetes. The positive is that there is a ton of resources to help with this.

### ECS

* [Cloud Agnostic] Bad, Due to this being an AWS only offering, it is not available on any other infrastructure platform
* [Cost] Good, You only pay for the compute and memory used by the container
* [Ease of Use] Good, Deployments to ECS seemed pretty straight forward.
* [Infrastructure] Good, All the underlying infrastructure is taken care of, all the user needs to worry about is supplying the docker image
* [Adoptability] Neutral, While using ECS seemed pretty straight forward, there are not as many resources for ECS. More of the industry is using kubernetes

## Links
[Link type] [Link to ADR]
