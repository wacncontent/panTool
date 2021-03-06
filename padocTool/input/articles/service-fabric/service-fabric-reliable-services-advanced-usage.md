<properties
   pageTitle="Advanced Usage of Service Fabric Reliable Service Programming Model"
   description="Learn about advanced usage of Service Fabric's Reliable Service programming model for added flexibility in your services."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.date="08/26/2015"
   wacn.date=""/>

# Advanced usage of the Reliable Services programming model
Service Fabric simplifies writing and managing reliable stateless and stateful services. This guide will talk about advanced usages of the Reliable Services programming model to gain more control and flexibility over your services. Prior to reading this guide, familiarize yourself with [the Reliable Services programming model](/documentation/articles/service-fabric-reliable-services-introduction).

## Stateless Service base classes
The StatelessService base class provides CreateCommunicationListener() and RunAsync(), which is sufficient for the majority of stateless services. The StatelessServiceBase class underlies StatelessService and exposes additional service lifecycle events. You can derive from StatelessServiceBase if you need additional control or flexibility. See the developer reference documentation on [StatelessService](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.services.statelessservice.aspx) and [StatelessServiceBase](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.services.statelessservicebase.aspx) for more information.

- `void OnInitialize(StatelessServiceInitializiationParameters)`
    OnInitialize is the first method called by Service Fabric. Service initialization information is provided such as service name, partition id, instance id, and code package information. No complex processing should be done here. Lengthy initialization should be done in OnOpenAsync.

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`
    OnOpenAsync is called when the stateless service instance is about to be used. Extended service initialization tasks can be started at this time.

- `Task OnCloseAsync(CancellationToken)`
    OnCloseAsync is called when the stateless service instance is going to be gracefully shutdown. This can occur when the service's code is being upgraded, the service instance is being moved due to load balancing, or a transient fault is detected. OnCloseAsync can be used to safely close any resources, stop any background processing, finish saving external state, or close down existing connections.

- `void OnAbort()`
    OnAbort is called when the stateless service instance is being forcefully shutdown. This is generally called when a permanent fault is detected on the node, or when Service Fabric cannot reliably manage the service instance's lifecycle due to internal failures.

## Stateful Service base classes
The StatefulService base class should be sufficient for most stateful services. Similar to stateless services, the StatefulServiceBase class underlies StatefulService and exposes additional service lifecycle events. Additionally, it allows you to provide a custom reliable state provider and optionally support communication listeners on Secondaries. You can derive from StatefulServiceBase if you need additional control or flexibility. See the developer reference documentation on [StatefulService](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.services.statefulservice.aspx) and [StatefulServiceBase](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.services.statefulservicebase.aspx) for more information.

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`
    OnChangeRoleAsync is called when the stateful service is changing roles, for example to Primary or Secondary. Primary replicas are given write status (are allowed to create and write to the reliable collections), while Secondary replicas are given read status (can only read from existing reliable collections). You can start or update the background tasks in response to role changes, such as performing read-only validation, report generation, or data mining on a Secondary.

- `IStateProviderReplica CreateStateProviderReplica()`
    A stateful service is expected to have a reliable state provider. StatefulService uses the ReliableStateManager class, which provides the reliable collections (e.g. dictionaries and queues). You may want to supply your own provider if you wish to manage the state yourself, or extend the functionality of one of the built-in state providers.

- `bool EnableCommunicationListenerOnSecondary { get; }`
    By default, communication listeners are only created on Primaries. Both StatefulService and StatefulServiceBase allow you to override this property to allow communication listeners to be created on Secondaries. You may want to allow your Secondaries to handle read-only requests, to improve the throughput on read-heavy workloads.

    > [AZURE.NOTE] You are responsible for ensuring your Secondaries do not attempt to create or write to reliable collections.  Attempts to write on a Secondary will cause an exception which, if unhandled, will cause the replica to be closed and reopened.

StatefulServiceBase also provides the same four lifecycle events as StatelessServiceBase, with the same semantics and use cases:

- `void OnInitialize(StatefulServiceInitializiationParameters)`
- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`

## Next steps
For more advanced topics related to Service Fabric, see the following articles.

- [Configuring Stateful Reliable Services](/documentation/articles/service-fabric-reliable-services-configuration)

- [Service Fabric Health Introduction](/documentation/articles/service-fabric-health-introduction.md)

- [Using System health reports for troubleshooting](/documentation/articles/service-fabric-understand-and-troubleshoot-with-system-health-reports)

- [Placement Constraints Overview](/documentation/articles/service-fabric-placement-constraint)

- [Secure Replication traffic of Stateful Services in Azure Service Fabric](/documentation/articles/service-fabric-replication-security)
 