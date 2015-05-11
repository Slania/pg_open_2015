# Postgres in Paxos land

Modern patterns for storing data in the cloud includes replicating data across geographically distributed data centers for availability, redundancy and (locally, per-user) optimized latencies.
 
An important class of such data stores involves the use of directories to track the location of individual data objects to achieve per-user latency optimization. Directory-based datastores allow flexible data placement, and the ability to adapt placement in response to changing workload dynamics. However, a key challenge is maintaining and updating the directory state when replica placement changes.
 
## Postgres as a directory service store
 
I would like to present my experience of implementing a distributed system using Postgres as the underlying database to maintain these directories, which are in essence replicated state-based logs. My implementation is the first known open-source implementation of such a directory-lookup system built on a strong-consistency aware protocol like Paxos. The results of testing also happen to be the first openly published results on the performance of the Paxos protocol in a WAN setting. 
The salient features of the implemented service include:
 
 * 1) Fault tolerant
 * 2) Distributed, Low-Latency
 * 3) and most importantly, Correct (Complete and Consistent)

## Paxos as a Multi-Master read/write solution (strict consistency)

When we start talking about Multi-Master writes, the topic of consistency almost goes hand-in-hand. Consistency has traditionally been a widely researched computer science problem with distributed systems, and we have recently come to define shades of gray for consistency for different classes of systems. I want to highlight upon the implementation aspects of the system in terms of strict-consistency when applied to a Multi-Master solution, and the overheads (when deployed in a WAN environment) involved.
