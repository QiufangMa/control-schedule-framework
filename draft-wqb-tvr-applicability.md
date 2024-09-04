---
title: "Applicability of YANG Data Models for Scheduling of Network Resources"
abbrev: "Schedule Framework"
category: info

docname: draft-wqb-tvr-applicability-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: AREA
workgroup: "TVR"
keyword:
 - schedule
 - framework
 - YANG module
 - applicability

author:
-
  fullname: Li Zhang
  organization: Huawei
  email: zhangli344@huawei.com
-
   fullname: Qiufang Ma
   organization: Huawei
   street: 101 Software Avenue, Yuhua District
   city: Nanjing, Jiangsu
   code: 210012
   country: China
   email: maqiufang1@huawei.com
-
   fullname: Qin Wu
   organization: Huawei
   street: 101 Software Avenue, Yuhua District
   city: Nanjing, Jiangsu
   code: 210012
   country: China
   email: bill.wu@huawei.com
-
   fullname: Mohamed Boucadair
   organization: Orange
   email: mohamed.boucadair@orange.com

contributor:
-
  fullname: Daniel King
  organization: Lancaster University
  country: United Kingdom
  email: d.king@lancaster.ac.uk
-
  fullname: Charalampos (Haris) Rotsos
  organization: Lancaster University
  email: c.rotsos@lancaster.ac.uk
-
  fullname: Peng Liu
  organization: China Mobile
  email: liupengyjy@chinamobile.com
-
  fullname: Tony Li
  organization: Juniper Networks
  email: tony.li@tony.li

normative:

informative:


--- abstract

This document describes the applicability of various network management tools
and data models that may be used for scheduling, to meet the requirements of a set
of representative use cases described in the "TVR (Time-Variant Routing) Requirements" (I-D.ietf-tvr-requirements).

It also presents a framework that elucidates various scheduling scenarios
and identifies the entities involved in requesting scheduled changes of
network resources.


--- middle

# Introduction

Scheduling-related tasks are usually considered for efficient and deterministic
network management. Such scheduling may be characterized as simple recurrent tasks
such as planned activation/deactivation of some network accesses, or can be
more complex such as scheduling placement of requests and planned invocation
of resources to satisfy service demands or adhere to local networks policies.
Many common building blocks are required for all these cases for adequate
management of schedules and related scheduled actions.

This document describes the applicability of various network management tools
and data models that may be used for scheduling, to meet the requirements of a set
of representative Time-Variant Routing (TVR) use cases described in {{?I-D.ietf-tvr-use-cases}}. By leveraging
a reference framework presented in this document, it shows how IETF data models in
{{?I-D.ietf-tvr-schedule-yang}} can fit into this framework and streamline the management and
orchestration of network resources based on precise date and time parameters.

The document also provides guidelines for implementing
scheduling capabilities across diverse network architectures, ensuring
that resources are allocated and utilized in a timely and effective manner.

In addition, the document outlines several challenges that must be considered when
deploying control mechanisms for scheduling network resources, including:

 * Discover where and what scheduling state is held.

 * Specify to apply schedule policies and detect and resolve conflicts.

 * Ensure synchronization of scheduled resources across distributed systems.

 * Establish a scalable and resilient scheduling system.

 * Ensure that a scheduling system can scale to accommodate complex networks
   with numerous resources and scheduling requests.

 * Ensure that the a scheduling system remains operational and can recover from
   failures or disruptions, providing redundancy, failover mechanisms, and
   robust error handling.

 * Secure scheduled resources.

 * Identify what mechanisms and policies could be applied to protect scheduling data
   and operations from unauthorized access and ensuring that only authorized
   entities can create, modify, or retrieve schedules.

> Editors Note: pointers to sections where each of these items is described will
> be provided in future version.

Key use cases highlight how the proposed framework can be used for scheduling
scenarios. The applicable IETF YANG modules are described, as well as
other dependencies that are needed.

> Editors Note: In future versions of this document, an appendix will also
provide JSON examples.

<!--
## Problem Statement

Efficient and coordinated use of resources
is paramount for maintaining optimal performance and reliability of many network environments. Networks are
increasingly complex, supporting a wide variety of services and applications that
require precise timing and resource allocation. Despite advancements in networking
techniques and the richness of protocols machinery, there is still a lack of reference architectures for scheduling
resources based on specific date and time parameters. This gap often leads to
inefficiencies, conflicts, and suboptimal utilization of network capabilities.

Typically, network administrators rely upon disparate and often proprietary tools
to manage scheduling for various network activities such as maintenance events,
policy enforcement, and resource allocation. These tools genrally operate in
isolation, lacking interoperability and integration with other network management
systems. As a result, administrators face significant challenges in coordinating
schedules, resolving conflicts, and ensuring that critical tasks are executed
without disruption. The absence of a unified scheduling framework exacerbates
these issues, leading to increased operational overhead and potential service
degradation (e.g., lack of integration with service impact analysis).

Furthermore, the dynamic nature of some networks demands a flexible and adaptive
scheduling solution that can respond to changing conditions and requirements.
Existing scheduling approaches are often rigid and static, unable to accommodate
the fluid demands of network services and applications. This rigidity hinders the
ability to optimize resource use and respond proactively to network events,
impacting the overall efficiency and effectiveness of network
operations.

To address these challenges, there is a clear need for a standardized framework
that can provide a cohesive approach to scheduling network resources. Such a
framework leverages existing YANG models to ensure compatibility and ease
of integration with current network management practices. By standardizing the
scheduling process, network operators can achieve greater predictability,
reduce conflicts, and optimize the use of network resources, thereby
enhancing the performance and reliability of their networks.

## Background

There is an existing framework {{?RFC8413}} for the use of scheduled resources.
This document outlines a framework detailing the architecture for supporting
the scheduled reservation of Traffic Engineering (TE) resources. It focuses
on the conceptual and architectural aspects, without delving into
specific protocols or protocol extensions required to implement this
service.

More recently, several specifications include a provision for scheduling.
Examples of such specifications are (but not limited to) {{?I-D.ietf-opsawg-ucl-acl}},
{{?I-D.contreras-opsawg-scheduling-oam-tests}}, and {{?I-D.ietf-tvr-schedule-yang}}.
The development of {{?I-D.ietf-netmod-schedule-yang}} is intended to be served
as common building blocks and provides a common schedule YANG module that can be
reused in scheduling contexts such as event, policy, services, or resources based
on date and time.

Combined, {{?I-D.ietf-netmod-schedule-yang}} and other documents built on top of
it (e.g., {{?I-D.ietf-opsawg-ucl-acl}}, {{?I-D.contreras-opsawg-scheduling-oam-tests}},
and {{?I-D.ietf-tvr-schedule-yang}}) provide powerful capabilities for the control and
scheduling of network resources for several use cases, including key examples
outlined in this document.
-->

# Conventions and Definitions

{::boilerplate bcp14-tagged}

The following terms are used in this document:

* Schedule Service Requester:
: An entity that issues a scheduling request of network events, policies, services,
  and resources.

* Schedule Service Responder:
: An entity that handles scheduling requests from a Schedule Service Requester.

* Schedule Manager:
: A functional entity that is managing the scheduling operations.

* Policy Engine:
: A functional entity that is responsible for enforcing a set of polices or rules
  in the network.

# An Reference Architecture {#architecture}

{{arch}} presents a reference architecture for the control scheduling of
network resources.

~~~~
          +-------------------------------------------------+
          |            Schedule Service Requester           |
          +-----+------------------------------------^------+
                |                                    |
                |Request                             |Response
                |                                    |
          +-----v------------------------------------+------+
          |            Schedule Service Responder           |
          |                                                 |
          |   +---------+                     +---------+   |
          |   |         |                     |         |   |
          |   | Schedule|                     | Conflict|   |
          |   | Manager |                     | Resolver|   |
          |   |         |                     |         |   |
          |   +---------+                     +---------+   |
          |                                                 |
          |   +---------+                     +---------+   |
          |   |         |                     |         |   |
          |   | Resource|                     | Policy  |   |
          |   | Manager |                     | Engine  |   |
          |   |         |                     |         |   |
          |   +---------+                     +---------+   |
          |                                                 |
          +----------------------+--------------------------+
                                 |
                                 |
                                 |
                                 |
     +---------------------------+-----------------------------+
     |              Network Resources and Inventory            |
     +---------------------------------------------------------+
~~~~
{: #arch title="An Architecture for the Scheduled Network Scenarios" artwork-align="center"}

## Functional Components

### Scheduled Service Requester

The entity requesting a resource schedule change can vary widely. For example,
a network administrator may seek to restrict or limit access to specific network
resources based on day or time to optimize performance and enhance security.

Additionally, higher-layer Operations Support System (OSS) components may impose
restrictions on network resources in response to changing network conditions,
ensuring service continuity and compliance with operational policies. Automated
systems and AI-driven components can also request dynamic adjustments based on
real-time data, facilitating predictive maintenance and optimizing resource
usage to maintain peak network efficiency.

### Scheduled Service Responder

This component is responsibile handling scheduling orders. That is, this entity manages and coordinates
all network scheduling activities. The extact internal structure of this entity is deployment-specific; however this
document describes an example internla decomposition of this entity:

 * Resource Manager:
 : Manages the network resources that are subject to scheduling.

 * Schedule Manager:
 : Handles creation, modification, deletion, and querying of schedules.

 * Conflict Resolver:
 : Detects and resolves scheduling conflicts based on predefined
   policies and priorities.

 * Policy Engine:
   : Enforces scheduling policies and rules, ensuring compliance with
   organizational requirements.

Examples of a schedule service responder may be a network controller, network
management system or the network device itself.

## Functional Interfaces

To support the scheduling of network resources effectively, several
functional interfaces are required. These interfaces interconnect
different components of the network scheduling
system, ensuring seamless integration and operation, these include:

 * Schedule Service Requester and Responder API:
 : Schedule resource order creation,
   order modification, and deletion requests and responses. Querying of
   current and upcoming schedules, conflict and alert notifications.

 * Schedule Service Responder and Network API:
 : Manages interactions with
   the network resources, inventory systems, planning systems, etc. Capable of querying
   available resources, allocating and deallocating resources based on
   current schedule plan, and monitoring resource utilization.

## Data Sources

When scheduling network resources, a variety of data sources are
required to accurately assess the network state and make informed
scheduling decisions. Here are some example data sources that will be
required:

 * Network Topology Information:
 : Connection details about the physical
   and logical layout of the network, including nodes, ports/links,
   and interconnections.

 * Network Resource Inventory:
 : A comprehensive list of deployed network
   resources that are not currently in service, but may be available if
   enabled.

 * Current Network Utilization:
 : Real-time data on the current usage of
   network resources, including bandwidth consumption, CPU load,
   memory usage, and power consumption.

 * Historic Network Utilization:
 : Past data on the current usage of network
   resources, including bandwidth consumption, CPU load, memory usage,
   and power consumption.

 * Scheduled Maintenance and Planned Outages:
 : Information on planned
   maintenance activities, scheduled downtimes, and service windows.

It is critical to leverage these diverse data sources, so network
administrators and automated systems can make well-informed scheduling
decisions that optimize resource utilization, maintain network
performance, and ensure service reliability.

## State Management

The scheduling state is maintained in the schedule manager, which is responsible
for the creation, deletion, modification, and query of scheduling information.

Groupings "schedule-status" and "schedule-status-with-name" in the "ietf-schedule"
YANG module {{?I-D.ietf-netmod-schedule-yang}} define common parameters for scheduling
management, including status exposure.

## Policy and Enforcement

Policies are a set of rules to administer, manage, and control access to network
resources. For example, the following shows an example of a scheduled ACL policy:

~~~~
drop TCP traffic source-port 16384 time 2025-12-01T08:00:00Z
/2025-12-15T18:00:00Z
~~~~

A set of scheduling policies and rules are maintained by the policy engine,
which is responsible for the policy enforcement. Policies are triggered to execute
at a certain time based on the scheduling parameters. Each policy might be executed
multiple times, depending on the its scheduling type (one-shot vs. recurrence).

## Synchronization

It is critical to ensure all network schedule entities, including
controllers and management systems are synchronized to a common time
reference. System instability and unpredictability might be caused if
there is any time inconsistencies between entities that request/respond to
policies or events based on time-varying parameters. Several methods are
available to achieve this.

<!--
# Procedures and Other Dependencies


## YANG Data Models {#schedule-modules}

This document is not intended to define any YANG modules, but shows how existing
schedule-related YANG modules could fit into this framework. The following provides
a list of applicable YANG modules that can be used to exchange data between schedule
service requester and responder:


  * {{?I-D.ietf-netmod-schedule-yang}} defines "ietf-schedule" YANG
    module for scheduling that works as common building blocks for YANG modules
    described in this section. The module doesn't define any
    protocol-accessible nodes but a set of reusable groupings applicable to be used
    in any scheduling contexts.

 * The "ietf-ucl-acl" YANG module defined in {{?I-D.ietf-opsawg-ucl-acl}} augments
   the IETF Access Ccontrol List (ACL) model {{?RFC8519}} with schedule parameters to allow each Access
   Control Entry (ACE) to be activated based on date and time conditions.

 * The "ietf-tvr-node" YANG module in {{?I-D.ietf-tvr-schedule-yang}} which is
   a device model, is designed to manage a single node with scheduled
   attributes (e.g., powered on/off).

 * The "ietf-tvr-topology" YANG module in {{?I-D.ietf-tvr-schedule-yang}} is used
   to manage the network topology with time-varying attributes
   (e.g., node/link availability, link bandwidth, or delay).

 * The "ietf-oam-unitary-test" in {{?I-D.contreras-opsawg-scheduling-oam-tests}}
   defines how to perform scheduled network diagnosis using
   (Operations, Administration, and Maintenance) OAM unitary test.

 * The "ietf-oam-test-sequence" in {{?I-D.contreras-opsawg-scheduling-oam-tests}}
   is defined to perform a collection of OAM unitary tests based on date and time
   constraints.

## Procedures

### Generating Schedule

TODO

### Distributing Schedule

Schedules distribution means that network schedules are distributed to the execution
devices by YANG model. Schedules distribution is not mandatory. This depends on the
location where the schedules are executed. If the schedules are generated and executed
on the same device, schedules distribution is not required. If schedules are generated
and executed on different devices, the schedules distribution is needed. Note that if
a schedule affects topology and a distributed routing protocol is used, then the schedule
needs to be distributed to all the nodes in the topology, so that other nodes can consider
the impact of the schedule when calculating routes.

### Executing Schedule

Schedules execution means that a component (e.g., device) undertakes an action (e.g., allocates and deallocates resources) at specified
time points. The schedule executor should fully understand the consequences of the schedule execution. For example, when a schedule's execution affects the network topology, the addition
and deletion of the topology need to be considered carefully.

A link coming up or a node joining a topology should not have any functional change until the
change is proven to be fully operational. The routing tables or paths could be pre-computed
but should not be installed before all of the topology changes are confired to be operational.
The benefits of this pre-computation appear to be very small. The network may choose to not do
any pre-installation or pre-computation in reaction to topological additions, at a small cost
of some operational efficiency.

Topological deletions are an entirely different matter. If a link or node is to be removed from
the topology, then the network should act before the anticipated change to route traffic around
the expected topological loss. Specifically, at some point before the topology change, the routing
tables or paths should be pre-computated and installed before the topology change take place.
The time necessary will vary depending on the exact network and configuration. When using IGP or other
distributed routing protocols, the affected links may be set to a high metric to direct traffic
to alternate paths. This type of change does require some time to propagate through the network,
so the metric change should be initiated far enough in advance that the network converges before the
actual topological change.

## Other Dependencies

This sections presents some outstanding dependencies that need to be considered
when deploying the scheduling mechanism. This may not be exhaustive.

### Access Control

Access control ensures only authorized control entities can have access to schedule
information, including querying, creation, modification, and deletion of schedules.
Unauthorized access may lead to unintended consequences.

The Network Access Control Model (NACM) {{?RFC8341}} provides standard mechanisms
to restrict access for particular uses to a preconfigured subset of all available
NETCONF or RESTCONF protocol operations and content.

### Atomic Operations

Atomic operations are guaranteed to be either executed completely or not executed
at all. Deployments based on scheduling must ensure schedule changes based on
recurrence rules are applied as atomic transactions. Either all changes are
successfully applied, or none at all. For example, a network policy may be
scheduled to be active every Tuesday in January of 2025. If the schedule is changed
to every Wednesday in January 2025, the recurrence set is changed from
January 7, 14, 21, 28 to January 1, 8, 15, 22, 29. If some occurrences can
not be applied successfully (e.g., January 1 cannot be scheduled because of conflict),
the others in the recurrence set will not be applied as well.

In addition, the scheduling management of network events, policies, services, and
resources may involve operations that are performed at particular future time(s).
Multiple operations might be involved for each instance in the recurrence set,
either all operations are successfully performed, or none at all.

### Rollback Mechanism

Rollback mechanism is useful to ensure that in case of an error, the system can
revert back to its previous state. Deployments are required to save the
checkpoints (manually or automatically) of network scheduling activities that
can be used for rollback when necessary, to maintain network stability.

### Inter-dependency

Enfrocement of some secheduled actions may depend on other schedules actions.
Means to identify such dependency are needed.
-->

# TVR Use Cases with Code Example {#uc}

## Tidal Network

Tidal network is a typical scenario of Energy Efficient case {{Section 3 of ?I-D.ietf-tvr-use-cases}}. The tidal network
means that the volume of traffic in the network changes
periodically like the ocean tide. This changes are mainly affected by
human activities. Therefore, this tidal effect is obvious in human-populated
areas, such as campuses and airports.

In the context of a tidal network, if the network maintains all the devices
up to guarantee a maximum throughput all the time, a lot of power will be
wasted. The energy-saving methods may include the deactivation of some or all
components of network nodes. These activities have the potential to alter
network topology and impact data routing/forwarding in a variety of ways.  Interfaces on
network nodes can be selectively disabled or enabled based on traffic patterns,
thereby reducing the energy consumption of nodes during periods of low network
traffic.

## Architecture Example

As described in {{Section 3.1.2 of ?I-D.ietf-tvr-requirements}}, the locality of
schedule generation can be centralized or distributed. Both
of these two generation methods are applicable in Tidal Network.

### Centralized Generation

In the centralized schedule generation, the Schedule Service Requester in {{arch}}
can be a network administrator, and the Scheduled Service Responder can be a network
controller or network management system. After generating schedules, the controller
(or management system) needs to determine whether to distribute these schedules based
on the schedule Execution Locality defined in {{Section 3.1.3 of ?I-D.ietf-tvr-requirements}}.

### Distributed Generation

In the distributed schedule generation，the Schedule Service Requester in {{arch}}
can be a network controller, and the Scheduled Service Responders are the network
devices, the relationship between network controllers and network devices is shown in
{{arch-example}}. In this mode, the generation and execution of schedules are both
on the same devices, so it does not involve the schedule distribution process.

~~~~
          +-----------------------------------------------------+
          |              Network Controller(s)                  |
          +-+--------------^------------------+---------------^-+
            |              |                  |               |
            |Request       |Response          |Request        |Response
            |              |                  |               |
    +-------v--------------+-------+    +-----v---------------+--------+
    |       Network Device A       |    |       Network Device B       |
    |                              |    |                              |
    |   +---------+  +---------+   |    |   +---------+  +---------+   |
    |   | Schedule|  | Conflict|   |    |   |         |  |         |   |
    |   | Manager |  | Resolver|   |    |   | Manager |  | Resolver|   |
    |   +---------+  +---------+   |    |   +---------+  +---------+   |  …
    |                              |    |                              |
    |   +---------+  +---------+   |    |   +---------+  +---------+   |
    |   | Resource|  | Policy  |   |    |   | Resource|  | Policy  |   |
    |   | Manager |  | Engine  |   |    |   | Manager |  | Engine  |   |
    |   +---------+  +---------+   |    |   +---------+  +---------+   |
    |                              |    |                              |
    +------------------------------+    +------------------------------+
~~~~
{: #arch-example title="An Architecture Example for Distributed Schedule Generation Scenario" artwork-align="center"}

### Example Procedures


#### Building Traffic Profiles

The first step to perform schedules in a tidal network is to analysis the traffic patterns at different
network devices and links comprehensively and then establish clear tidal points for lower and upper
network traffic. It should be noted that the change regularity of traffic may be different at different time
(for example, the traffic regularity in workday and weekend may totally be different), the selection of tidal
points should take full count of these factors. How to analyze the traffic patterns and determine the
tidal points are outside the scope of this document.

#### Estabilish Minimum and Peak Topology

An algorithm is required to calculate the minimum and peak topology to service expected demand at different
time slot. Such calculation algorithm for the topology is outside the scope of this document.

#### Generating Schedule

The schedule request is generated by the Schedule Service Requester according to the switching regularity
of the minimum and peak topology. For example, the minimum topology enables from 1 AM to 7 AM everyday, then
the network administrator need to shutdown some links or devices from 1 AM to 7 AM.

When the Scheduled Service Responder receives this schedule request, it will generate
a schedule with the following procedures:

-	The Conflict Resolver checks whether the current schedule request conflicts with other
schedules. If there is no conflict, then go to the next step. Otherwise, a message is
returned to the Schedule Service Requester, indicating that the conflict check fails.
A typical failure scenario is that the resource triggered by the current schedule is
occupied by another schedule. For example, there is an existing schedule
that requests the same links up from 5 AM to 10 AM every day, then there is a conflict with
this request and the existing schedule.

-	The Schedule Manager creates one schedule according to the request, and then store
it in the schedule database. The Schedule Manager
allocates a unique identifier for this schedule and returns that identifier to the
Schedule Service Requester after creating this schedule successfully. The Schedule
Service Requester may update or delete the schedules by this identifier in the future.

-	The Resource Manager allocates network resources (in this example the resources are
the detail interfaces related to the links) based on the schedule and stores the
allocated resources and its corresponding schedule identifier in a resource database.
The allocation of network resources requires a variety of data resources, such as
network topology information，network resource inventory，current network utilization, etc.

-	The Policy Engine’s responsibility needs to be future discussed.


#### Distributing Schedule

Schedules distribution means that network schedules are distributed to the execution
devices via dedicated management interfaces. Schedules distribution is not mandatory. This depends on the
location where the schedules are executed. If the schedules are generated and executed
on the same device, schedules distribution is not required. If schedules are generated
and executed on different devices, the schedules distribution is then needed. Note that if
a schedule affects topology and a distributed routing protocol is used, then the schedule
needs to be distributed to all the nodes in the topology, so that other nodes can consider
the impact of the schedule when calculating and selecting routes.

#### Executing Schedule

Schedules execution means that a component (e.g., device) undertakes an action
(e.g., allocates and deallocates resources) at specified time points. In a tidal network,
the schedule execution indicates to power on/off specific network components
(such as interfaces or entire network devices) directly or by commands.

The schedule executor should understand the consequences of the schedule execution.
The power on/off of network components usually affects the network topology, the
addition and deletion of the topology need to be considered separately.

A link coming up or a node joining a topology should not have any functional change until the
change is proven to be fully operational. The routing paths may be pre-computed
but should not be installed before all of the topology changes are confired to be operational.
The benefits of this pre-computation appear to be very small. The network may choose to not do
any pre-installation or pre-computation in reaction to topological additions, at a small cost
of some operational efficiency.

Topological deletions are an entirely different matter. If a link or node is to be removed from
the topology, then the network should act before the anticipated change to route traffic around
the expected topological change. Specifically, at some point before the planned topology change, the routing
paths should be pre-computated and installed before the topology change takes place.
The required time to perform such planned action will vary depending on the exact network and configuration. When using an IGP or other
distributed routing protocols, the affected links may be set to a high metric to direct traffic
to alternate paths. This type of change does require some time to propagate through the network,
so the metric change should be initiated far enough in advance that the network converges before the
actual topological change.

### Applicable Models

The following provides a list of applicable YANG modules that can be used to
exchange data between schedule service requester and responder specified in {{architecture}}:

 * The "ietf-tvr-node" YANG module in {{?I-D.ietf-tvr-schedule-yang}} which is
   a device model, is designed to manage a single node with scheduled
   attributes (e.g., powered on/off).

 * {{?I-D.ietf-netmod-schedule-yang}} defines "ietf-schedule" YANG
   module for scheduling that works as common building blocks for YANG modules
   described in this section. The module doesn't define any
   protocol-accessible nodes but a set of reusable groupings applicable to be used
   in any scheduling contexts.

### Code Examples

{{ex-inf}} indicates the example of a scheduling node that is powered on from 12 AM, December 1, 2025 to 12 AM, December 1, 2026 in UTC and its interface named "interface1" is scheduled to be enabled at 7:00 AM and disabled at 1:00 AM, every day, from December 1, 2025 to December 1, 2026 in UTC.
The JSON encoding is used only for illustration purposes.

~~~~
{
   "ietf-tvr-node:node-schedule":[
      {
         "node-id":12345678,
         "node-power-schedule":{
            "power-default":false,
            "schedules":[
               {
                  "schedule-id":111111,
                  "period-start":"2025-12-01T00:00:00Z",
                  "period-end":"2026-12-01T00:00:00Z",
                  "attr-value":{
                     "power-state":true
                  }
               }
            ]
         },
         "interface-schedule":[
            {
               "name":"interace1",
               "default-available":false,
               "default-bandwidth":1000000000,
               "attribute-schedule":{
                  "schedules":[
                     {
                        "schedule-id":222222,
                        "recurrence-first":{
                           "utc-start-time":"2025-12-01T07:00:00Z",
                           "duration":64800
                        },
                        "utc-until":"2026-12-01T00:00:00Z",
                        "frequency":"ietf-schedule:daily",
                        "interval":1,
                        "attr-value":{
                           "available":true
                        }
                     }
                  ]
               }
            }
         ]
      }
   ]
}
~~~~
{: #ex-inf title="An Example of Interface Activation Scheduling" artwork-align="center"}

# Other Dependencies

This sections presents some outstanding dependencies that need to be considered
when deploying the scheduling mechanism.

## Access Control

Access control ensures only authorized control entities can have access to schedule
information, including querying, creation, modification, and deletion of schedules.
Unauthorized access may lead to unintended consequences.

The Network Access Control Model (NACM) {{?RFC8341}} provides standard mechanisms
to restrict access for particular uses to a preconfigured subset of all available
NETCONF or RESTCONF protocol operations and content.

## Atomic Operations

Atomic operations are guaranteed to be either executed completely or not executed
at all. Deployments based on scheduling must ensure schedule changes based on
recurrence rules are applied as atomic transactions. Either all changes are
successfully applied, or none at all. For example, a network policy may be
scheduled to be active every Tuesday in January of 2025. If the schedule is changed
to every Wednesday in January 2025, the recurrence set is changed from
January 7, 14, 21, 28 to January 1, 8, 15, 22, 29. If some occurrences can
not be applied successfully (e.g., January 1 cannot be scheduled because of conflict),
the others in the recurrence set will not be applied as well.

In addition, the scheduling management of network events, policies, services, and
resources may involve operations that are performed at particular future time(s).
Multiple operations might be involved for each instance in the recurrence set,
either all operations are successfully performed, or none at all.

## Rollback Mechanism

Rollback mechanism is useful to ensure that in case of an error, the system can
revert back to its previous state. Deployments are required to save the
checkpoints (manually or automatically) of network scheduling activities that
can be used for rollback when necessary, to maintain network stability.

## Inter-dependency

Enfrocement of some secheduled actions may depend on other schedules actions.
Means to identify such dependency are needed.

# Manageability Considerations

## Multiple Schedule Service Requesters

This document does not make any assumption about the number of schedule service
requester entities that interact with schedule service responder. This means that
multiple schedule service requesters may send requests to the responder to schedule
the same network resources, which may lead to conflicts. If scheduling conflicts
occur, some predefined policies or priorities may be useful to reflect how
schedules from different sources should be prioritized.

# Security Considerations

Time synchronization may potentially lead to security threats, e.g., attackers
may modify the system time and it thus causes time inconsistencies and affects the
normal functionalities for managing and coordinating network scheduling activities.
In addition, care must be taken when defining recurrences occurring very often and
frequent that can be an additional source of attacks by keeping the
system permanently busy with the management of scheduling.

# IANA Considerations

This document has no IANA actions.


--- back

<!--
# Use Cases with detailed Code Examples

## Scheduling OAM Tests (Attestation)

Operations, Administration, and Maintenance (OAM) are important networking functions
for network monitoring and troubleshooting, as well as service-level agreements
and performance monitoring. Some actions might need to be executed repeatedly or
at a specific future time to carry out diagnostics. Scheduling-based OAM tools
are able to invoke a specific OAM unitary test or a sequence of OAM tests based
on some time-varying parameters, e.g., at a precise future time or repeatedly
occur at specific intervals.

Suppose the following fictional module is used:

~~~~
module example-oam-tests {
  yang-version 1.1;
  namespace "urn:example:oam-tests";
  prefix "ex-oam";

  import ietf-schedule {
    prefix "schedule";
  }
  list oam-test {
    key "test-name";
    leaf test-name {
      type string;
    }
    uses schedule:recurrence-utc;
  }
}
~~~~

The following indicates the example of a scheduling OAM traceroute test that is
executed at 3:00 AM, every other day, from December 1, 2025 in UTC.
The JSON encoding is used only for illustration purposes.

~~~~
{
    "example-oam-tests:oam-test": [
        {
            "test-name": "traceroute",
            "recurrence-first": {
                "utc-start-time": "2025-12-01T03:00:00Z"
            },
            "frequency": "ietf-schedule:daily",
            "interval": 2
        }
    ]
}
~~~~

## Time Variant Networking (Energy Efficient)

Tidal network is a typical scenario of Energy Efficient case. The tidal network
means the volume of traffic in the network changes
periodically like the ocean tide. This changes are mainly affected by
human activities. Therefore, this tidal effect is obvious in human-populated
areas, such as campuses and airport.

In the context of a tidal network, If the network maintains all the devices
up to guarantee the maximum throughput all the time, a lot of power will be
wasted. The energy-saving methods may include the deactivation of some or all
components of network nodes. These activities have the potential to alter
network topology and impact data routing in a variety of ways.  Interfaces on
network nodes can be selectively disabled or enabled based on traffic patterns,
thereby reducing the energy consumption of nodes during periods of low network
traffic.

The controlling of the interface states of each node can be achieved by using the ietf-tvr-node YANG module defined in {{?I-D.ietf-tvr-schedule-yang}}.

The following indicates the example of a scheduling node that is powered on from 12 AM, December 1, 2025 to 12 AM, December 1, 2026 in UTC and its interface named interface1 is scheduled to enable at 7:00 AM and disabled at 1:00 AM, every day, from December 1, 2025 to December 1, 2026 in UTC.
The JSON encoding is used only for illustration purposes.

~~~~
{
   "ietf-tvr-node:node-schedule":[
      {
         "node-id":12345678,
         "node-power-schedule":{
            "power-default":false,
            "schedules":[
               {
                  "schedule-id":111111,
                  "period-start":"2025-12-01T00:00:00Z",
                  "period-end":"2026-12-01T00:00:00Z",
                  "attr-value":{
                     "power-state":true
                  }
               }
            ]
         },
         "interface-schedule":[
            {
               "name":"interace1",
               "default-available":false,
               "default-bandwidth":1000000000,
               "attribute-schedule":{
                  "schedules":[
                     {
                        "schedule-id":222222,
                        "recurrence-first":{
                           "utc-start-time":"2025-12-01T07:00:00Z",
                           "duration":64800
                        },
                        "utc-until":"2026-12-01T00:00:00Z",
                        "frequency":"ietf-schedule:daily",
                        "interval":1,
                        "attr-value":{
                           "available":true
                        }
                     }
                  ]
               }
            }
         ]
      }
   ]
}
~~~~
-->

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
