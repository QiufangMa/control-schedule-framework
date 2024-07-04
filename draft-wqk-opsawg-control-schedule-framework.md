---
title: "A Framework for the Control Scheduling of Network Resources"
abbrev: "Schedule Framework"
category: info

docname: draft-wqk-opsawg-control-schedule-framework-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: AREA
workgroup: "OPSAWG"
keyword:
 - schedule
 - framework
 - YANG module

author:
-
   fullname: Qiufang Ma
   organization: Huawei
   role: editor
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
  fullname: Daniel King
  organization: Lancaster University
  role: editor
  country: United Kingdom
  email: d.king@lancaster.ac.uk
-
  fullname: Li Zhang
  organization: Huawei
  email: zhangli344@huawei.com

normative:

informative:


--- abstract

This document presents a comprehensive framework that elucidates various
scheduled resource scenarios and identifies the entities involved in
requesting resource schedule changes. It also addresses additional
challenges such as conflict resolution, priority handling, and
synchronization of scheduled tasks, ultimately enhancing the
reliability and performance of network services.

Key use cases highlight how the framework may be used for scheduled resource
scenarios. The document highlights the required data models and protocols to
provide function. The required IETF YANG model is described where possible,
and the specific code needed for function or to report information is also
provided. In future versions of this document, an appendix will also provide
JSON running code examples.

--- middle

# Introduction

This document presents a comprehensive framework for the scheduled use of
resources within network environments, utilizing the IETF YANG models
designed for scheduling of network resources.

The framework aims to show how the emerging IETF data models can streamline
the management and orchestration of network events, policies, services,
and resources based on precise date and time parameters. By leveraging the
defined YANG modules, this framework facilitates interoperable and
efficient scheduling mechanisms that enhance the predictability,
coordination, and optimization of network operations.

The document also provides guidelines and best practices for implementing
scheduling capabilities across diverse network architectures, ensuring
that resources are allocated and utilized in a timely and effective manner.

The document also outlines several challenges that must be considered when
deploying control mechanisms for scheduling of network resources, including:

 * Where and what scheduling state is held?

 * How to apply policy and detect and resolve conflicts?

 * Ensuring synchronization of scheduled resources across distributed systems.

 * Establishing a scalable and resilient scheduling system.

 * How to ensure a scheduling system can scale to accommodate complex networks
   with numerous resources and scheduling requests?

 * How to ensure the scheduling system remains operational and can recover from
   failures or disruptions, providing redundancy, failover mechanisms, and
   robust error handling?

 * How to secure scheduled resources?

 * What mechanisms and policies could be applied to protect scheduling data
   and operations from unauthorized access and ensuring that only authorized
   entities can create, modify, or retrieve schedules?

This document presents a comprehensive framework that elucidates various
scheduled resource scenarios and identifies the entities involved in
requesting resource schedule changes. It also addresses additional
challenges such as conflict resolution, priority handling, and
synchronization of scheduled tasks, ultimately enhancing the
reliability and performance of network services.

[Editors Note] In future versions of this document, an appendix will also
provide JSON running code examples.

## Problem Statement

In modern network environments, the efficient and coordinated use of resources
is paramount for maintaining optimal performance and reliability. Networks are
increasingly complex, supporting a wide array of services and applications that
require precise timing and resource allocation. Despite advancements in network
technology, there is a notable lack of standardized mechanisms for scheduling
resources based on specific date and time parameters. This gap often leads to
inefficiencies, conflicts, and suboptimal utilization of network capabilities.

Currently, network administrators rely on disparate and often proprietary tools
to manage scheduling for various network activities such as maintenance events,
policy enforcement, and resource allocation. These tools frequently operate in
isolation, lacking interoperability and integration with other network management
systems. As a result, administrators face significant challenges in coordinating
schedules, resolving conflicts, and ensuring that critical tasks are executed
without disruption. The absence of a unified scheduling framework exacerbates
these issues, leading to increased operational overhead and potential service
degradation.

Furthermore, the dynamic nature of modern networks demands a flexible and adaptive
scheduling solution that can respond to changing conditions and requirements.
Existing scheduling approaches are often rigid and static, unable to accommodate
the fluid demands of network services and applications. This rigidity hinders the
ability to optimize resource use and respond proactively to network events,
ultimately impacting the overall efficiency and effectiveness of network
operations.

To address these challenges, there is a clear need for a standardized framework
that can provide a cohesive approach to scheduling network resources. Such a
framework should leverage existing YANG models to ensure compatibility and ease
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
{{?I-D.contreras-opsawg-scheduling-oam-tests}}, and {{?I-D.united-tvr-schedule-yang}}.
The development of {{?I-D.ietf-netmod-schedule-yang}} is intended to be served
as common building blocks and provides a common schedule YANG module that can be
reused in scheduling contexts such as event, policy, services, or resources based
on date and time.

Combined, {{?I-D.ietf-netmod-schedule-yang}} and other documents built on top of
it (e.g., {{?I-D.ietf-opsawg-ucl-acl}}, {{?I-D.contreras-opsawg-scheduling-oam-tests}},
and {{?I-D.united-tvr-schedule-yang}}) provide powerful capabilities for the control and
scheduling of network resources for several use cases, including key examples
outlined in this document.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Terminology

The following terminology is used in this document. 

Schedule Service Requester: 
Schedule Service Responder:
Schedule Manager: 
Policy Engine: 

# Architecture

{{arch}} presents the referenced architecture for the control scheduling of
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

## Functional components

### Scheduled Service Requester

The entity requesting a resource schedule change can vary widely. For example,
a network administrator may seek to restrict or limit access to specific network
resources based on day or time to optimize performance and enhance security.

Additionally, higher-layer OSS (Operations Support System) components may impose
restrictions on network resources in response to changing network conditions,
ensuring service continuity and compliance with operational policies. Automated
systems and AI-driven components can also request dynamic adjustments based on
real-time data, facilitating predictive maintenance and optimizing resource
usage to maintain peak network efficiency.

### Scheduled Service Responder

The function of this central component is responsibility for managing and coordinating
all network scheduling activities. There are several sub-components within this entity,
including:

 * Resource Manager: Manages the network resources that are subject to scheduling.
 * Schedule Manager: Handles creation, modification, deletion, and querying of schedules.
 * Conflict Resolver: Detects and resolves scheduling conflicts based on predefined
   policies and priorities.
 * Policy Engine: Enforces scheduling policies and rules, ensuring compliance with
   organizational requirements.

Examples of a schedule service responder may be a network controller, network
management system or the network device itself.

## Functional Interfaces

To support the scheduling of network resources effectively, several
functional interfaces are required. These interfaces facilitate
communication between different components of the network scheduling
system, ensuring seamless integration and operation, these include:

 * Schedule Service Requestor and Responder API: Schedule resource creation,
   modification, and deletion requests and responses. Querying of
   current and upcoming schedules, conflict and alert notifications.

 * Schedule Service Responder and Network API: Manages interactions with
   the network resources and inventory systems. Capable of querying
   available resources, allocating and deallocating resources based on
   current schedule plan, and monitoring resource utilization.

## Data Sources

When scheduling network resources, a variety of data sources are
required to accurately assess the network state and make informed
scheduling decisions. Here are examples data sources that will be
required:

 * Network Topology Information: Connection details about the physical
   and logical layout of the network, including nodes, ports/links,
   and interconnections.

 * Network Resource Inventory: A comprehensive list of deployed network
   resources that are not currently in service, but may be available if
   enabled.

 * Current Network Utilization: Real-time data on the current usage of
   network resources, including bandwidth consumption, CPU load,
   memory usage, and power consumption.

 * Historic Network Utilization: Past data on the current usage of network
   resources, including bandwidth consumption, CPU load, memory usage,
   and power consumption.

 * Scheduled Maintenance and Planned Outages: Information on planned
   maintenance activities, scheduled downtimes, and service windows.

It is critical to leverage these diverse data sources, so network
administrators and automated systems can make well-informed scheduling
decisions that optimize resource utilization, maintain network
performance, and ensure service reliability.

## State Management

The scheduling state is maintained in the schedule manager, which is responsible
for the creation, deletion, modification, and query of scheduling information.
Groupings "schedule-status" and "schedule-status-with-name" in the "ietf-schedule"
yang module {{?I-D.ietf-netmod-schedule-yang}} define common parameters for scheduling
management/status exposure.

## Policy and Enforcement

Policies are a set of rules to administer, manage, and control access to network
resources. For example, the following shows an example of a scheduled ACL policy:

~~~~
drop TCP traffic source-port 16384 time 2025-12-01T08:00:00Z/2025-12-15T18:00:00Z
~~~~

A set of scheduling policies and rules are maintained in the policy engine,
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

# Applicable Models, Interfaces and Dependencies

## YANG Data Models {#schedule-modules}

This document is not intended to define any YANG modules, but shows how existing
schedule-related YANG modules could fit into this framework. The following provides
a list of applicable YANG modules that can be used to exchange data between schedule
service requester and responder:

 * The "ietf-ucl-acl" YANG module defined in {{?I-D.ietf-opsawg-ucl-acl}} augments
   the IETF ACL model {{?RFC8519}} with schedule parameters to allow each access
   control entry (ACE) to be activated based on date and time conditions.

 * The "ietf-tvr-node" YANG module in {{?I-D.ietf-tvr-schedule-yang}} which works
   as a device model, is designed to manage a single node with scheduled
   attributes (e.g., powered on/off).

 * The "ietf-tvr-topology" YANG module in {{?I-D.ietf-tvr-schedule-yang}} is designed
   to manage the network topology with time-varying attributes
   (e.g., node/link availability, link bandwidth or delay).

 * The "ietf-oam-unitary-test" in {{?I-D.contreras-opsawg-scheduling-oam-tests}}
   is defined to perform scheduled network diagnosis using
   (Operations, Administration, and Maintenance) OAM unitary test.

 * The "ietf-oam-test-sequence" in {{?I-D.contreras-opsawg-scheduling-oam-tests}}
   is defined to perform a collection of OAM unitary tests based on date and time
   constraints.

Additionally, {{?I-D.ietf-netmod-schedule-yang}} defines "ietf-schedule" YANG
module for scheduling that works as common building blocks for YANG modules
described in {{schedule-modules}}. The module doesn't define any
protocol-accessible nodes but a set of reusable groupings applicable to be used
in any scheduling contexts.

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

The tidal network means the volume of traffic in the network changes
periodically like the ocean tide. This changes are mainly affected by
human activities. Therefore, this tidal effect is obvious in human-populated
areas, such as campuses and airport.

In the context of a tidal network, If the network maintains all the devices
up to guarantee the maximum throughput all the time, a lot of power will be
wasted. The energy-saving methods may include the deactivation of some or all
components of network nodes. These activities have the potential to alter
network topology and impact data routing in a variety of ways.  Ports on
network nodes can be selectively disabled or enabled based on traffic patterns,
thereby reducing the energy consumption of nodes during periods of low network
traffic.

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

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
