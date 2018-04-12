# Reliability subsystem

The **Reliability subsystem** provides the mechanism to make the state of a Service Fabric service highly available through the use of the Replicator, Failover Manager, and Resource Balancer.  

[The Replicator](https://github.com/Microsoft/service-fabric/tree/master/src/prod/src/Reliability/Replication) ensures that state changes in the primary service replica will automatically be replicated to secondary replicas, maintaining consistency between the primary and secondary replicas in a service replica set. The replicator is responsible for quorum management among the replicas in the replica set. It interacts with the failover unit to get the list of operations to replicate, and the reconfiguration agent provides it with the configuration of the replica set. That configuration indicates which replicas the operations need to be replicated. Service Fabric provides a default replicator called Fabric Replicator, which can be used by the programming model API to make the service state highly available and reliable.  

The [Failover Manager](https://github.com/Microsoft/service-fabric/tree/master/src/prod/src/Reliability/Failover/fm) ensures that when nodes are added to or removed from the cluster, the load is automatically redistributed across the available nodes. If a node in the cluster fails, the cluster will automatically reconfigure the service replicas to maintain availability.  

The [Resource Manager](https://github.com/Microsoft/service-fabric/tree/master/src/prod/src/Reliability/Failover/ra) places service replicas across failure domains in the cluster and ensures that all failover units are operational. The Resource Manager also [balances service resources across the underlying shared pool of cluster nodes to achieve optimal uniform load distribution](https://github.com/Microsoft/service-fabric/tree/f258f7579af9643dac6b1c75c93db9a3bcd28fdd/src/prod/src/Reliability/LoadBalancing). This is where you can see how we [implement the simulated annealing algorithm](https://github.com/Microsoft/service-fabric/blob/2ae6e44d1dce5c4f4f615cc46e6ed19190176e4c/src/prod/src/Reliability/LoadBalancing/Searcher.cpp#L1453) you may have read about [here](https://blogs.msdn.microsoft.com/azureservicefabric/2016/01/14/service-fabric-under-the-hood-the-cluster-resource-manager-part-2/).  