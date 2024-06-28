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
    fullname: Your Name Here
    organization: Your Organization Here
    email: your.email@example.com

normative:

informative:


--- abstract

TODO Abstract


--- middle

# Introduction

This document presents a comprehensive framework for the scheduled use of
resources within network environments, utilizing the IETF YANG models
designed for scheduling of network resources.

The framework aims to show how the emerging IETF data models can streamline
the management and  orchestration of network events, policies, services,
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

 * Ensuring synchronization of scheduled resources across distributed systems

 * Establishing a scalable and resilient scheduling system.

 * How to ensure a scheduling system can scale to accommodate complex networks
   with numerous resources and scheduling requests?

 * How to ensure the scheduling system remains operational and can recover from
   failures or disruptions, providing redundancy, failover mechanisms, and
   robust error handling.

 * Security scheduled resources

 * What mechanisms and policies could be applied to protect scheduling data
   and operations from unauthorized access and ensuring that only authorized
   entities can create, modify, or view schedules?

This document presents a comprehensive framework that elucidates various
scheduled resource scenarios and identifies the entities involved in
requesting resource schedule changes. It also addresses additional
challenges such as conflict resolution, priority handling, and
synchronization of scheduled tasks, ultimately enhancing the
reliability and performance of network services.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

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

More recently, the development of {{?I-D.ietf-netmod-schedule-yang}}, which
provides a common schedule YANG module is designed to be applicable for
scheduling purposes such as event, policy, services, or resources based on date
and time.

Along with with the common schedule YANG module, the document
{{?I-D.ietf-opsawg-ucl-acl}} introduces a YANG data model for policy-based network
access control. This model ensures consistent and efficient enforcement of
network access control policies based on group identity.

Combined, {{?I-D.ietf-netmod-schedule-yang}} and {{?I-D.ietf-opsawg-ucl-acl}} provide
powerful tools for the control and scheduling of network resources for several
use cases, including key examples outlined in this document.

# Functional components

##Scheduled Service Requester

The entity requesting a resource schedule change can vary widely. For example,
a network administrator may seek to restrict or limit access to specific network
resources based on day or time to optimize performance and enhance security.

Additionally, higher-layer OSS (Operations Support System) components may impose
restrictions on network resources in response to changing network conditions,
ensuring service continuity and compliance with operational policies. Automated
systems and AI-driven components can also request dynamic adjustments based on
real-time data, facilitating predictive maintenance and optimizing resource
usage to maintain peak network efficiency.

##Scheduled Service Responder

The function of this central component is responsibility for managing and coordinating
all network scheduling activities. There are several sub-components within this entity,
including:

 * Resource Manager: Manages the network resources that are subject to scheduling.
 * Schedule Manager: Handles creation, modification, and deletion of schedules.
 * Conflict Resolver: Detects and resolves scheduling conflicts based on predefined
   policies and priorities.
 * Policy Engine: Enforces scheduling policies and rules, ensuring compliance with
   organizational requirements.

Examples of a schedule service responder may be a network controller, network
management system or the network device itself.

# Architecture

~~~~
          +-------------------------------------------------+
          |            Schedule Service Requester           |
          +-----+------------------------------------+------+
                |                                    |
                |Request                             |Response
                |                                    |
          +-----+------------------------------------+------+
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
  +------------------------------+---------------------------------+
  |              Network Resources and Inventory                   |
  +----------------------------------------------------------------+
~~~~

## Functional Interfaces

To support the scheduling of network resources effectively, several
functional interfaces are are required. These interfaces facilitate
communication between different components of the network scheduling
system, ensuring seamless integration and operation, these include:

 * Schedule Service Requestor API: Schedule resource creation,
   modification, and deletion requests and responses. Querying of
   current and upcoming schedules, conflict and alert notifications.

 * Schedule Service Responder to Network API: Manages interactions with
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
   resources that are not currently in service, but may be availble if
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


## Synchronization

It will be critical to ensure all network schedule entaties, including
controllers and management systems are synchronized to a common time
reference. Several methods are availible to achieve this.


## State Management

## Policy and Enforcement

[Qiufang] Need some text. Actually, in a future version of the document
we could also blend in a discussion on Event Condition Action (ECA).
Actual, I'm [Dan] also happy to contribute here too.

# Applicable Models, Interfaces and Dependencies

## YANG Models

[Qiufang] Need to outline the applicable YANG models and dependencies.

## Other Dependencies

[Qiufang] and [Dan]

# Use Cases with detailed Code Examples

## Scheduling OAM Tests (Attestation)

Operations, Administration, and Maintenance (OAM) are important networking functions
for network monitoring and troubleshooting, as well as service-level agreements
and performance monitoring. Some actions might need to be executed repeatedly or
at a specific future time to carry out diagnostics. Scheduling-based OAM tools
are able to invoke a specific OAM unitary test or a sequence of OAM tests based
on some time-varying parameters, e.g., at a precise future time or repeatedly
occcur at specific intervals.

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

[Dan] Tidal Example
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

Applicable YANG Models[TBD]

# Manageability Considerations

[Dan] Basic management and operational considerations for 00 version.

# Security Considerations

[Dan] Basic security text for 00 version.

# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
