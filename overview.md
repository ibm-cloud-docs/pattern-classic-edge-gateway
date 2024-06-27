---

copyright:
  years: 2024, 2024
lastupdated: "2024-06-27"

subcollection: pattern-classic-edge-gateway

keywords: network, VPC, PowerVS, Classic edge gateway

---

{{site.data.keyword.attribute-definition-list}}

# Overview
{: #overview}

The most common network scenario is called Classic Edge gateway. It allows on-premise traffic to flow into a set of firewalls prior to routing traffic to IBM Cloud’s Virtual Private Cloud (VPC) and PowerVS environments. It is used to integrate on-premises access with classic infrastructure, Virtual Private Cloud (VPC), and Power Virtual Server workloads within the {{site.data.keyword.cloud_notm}}. The objective of this pattern is to:

-   Illustrate on-premises network connectivity to a classic edge gateway with a {{site.data.keyword.vpc_full}} (VPC) and a {{site.data.keyword.powerSysFull}} workspace in {{site.data.keyword.cloud_notm}}.
-   Provide an {{site.data.keyword.IBM_notm}} solution design for the network resources required when deploying in {{site.data.keyword.cloud_notm}}.
-   Allows for the implementation of security components within the edge. The edge will isolate on-premise from {{site.data.keyword.cloud_notm}}’s VPC and PowerVS environments.
-   Focus on {{site.data.keyword.cloud_notm}} network elements, while ensuring that requirements can be met from a performance, system availability, and connectivity perspective.

This approach is referred to as the classic edge gateway because all traffic flows to the classic gateway for inspection before it is allowed to pass to any downstream workloads within {{site.data.keyword.cloud_notm}}.
{: note}

This pattern is intended to:

-   Accelerate and simplify solution design by providing a standard {{site.data.keyword.cloud_notm}} deployment architecture reference following the [{{site.data.keyword.IBM_notm}} {{site.data.keyword.arch_framework}}](/docs/architecture-framework).
-   Provide a prescriptive, end-to-end enterprise-class solution design, with diagrams, component architecture decisions along with rationale for cloud component selection to meet enterprise requirements.
