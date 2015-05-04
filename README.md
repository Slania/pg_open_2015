# Postgres in Paxos land

Modern patterns for storing data in the cloud includes replicating data across geographically distributed data centers for availability, redundancy and (locally, per-user) optimized latencies.
 
An important class of such data stores involves the use of directories to track the location of individual data objects to achieve per-user latency optimization. Directory-based datastores allow flexible data placement, and the ability to adapt placement in response to changing workload dynamics. However, a key challenge is maintaining and updating the directory state when replica placement changes.
 
## Postgres as a directory service store
 
I would like to present my experience of implementing a distributed system using Postgres as the underlying database to maintain these directories. They are essentially implementations of replicated state-based logs. Since directory lookups lie in the critical path of data fetch, low latency lookups become critical. The salient features of the directory service include:
 
 * 1) Low-latency
 * 2) Fault tolerant
 * 3) Distributed
 * and most importantly, Correct (Complete and Consistent)

## Paxos as a Multi-Master read/write solution (strict consistency)

When we start talking about Multi-Master writes, the topic of consistency almost goes hand-in-hand. Consistency has traditionally been a widely researched computer science problem with distributed systems, and we have recently come to defines shades of gray of consistency. I will also discuss what the implementation of strict-consistency for a Multi-Master solution looks like, what the overheads are (when deployed in a WAN environment) and what genre of systems it makes sense to have Multi-Master write solutions on.
