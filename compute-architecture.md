---

copyright:
  years: 2024, 2024
lastupdated: "2024-06-10"

subcollection: pattern-classic-edge-gateway

keywords: network, VPC, PowerVS, Classic edge gateway

---

{{site.data.keyword.attribute-definition-list}}


# Architecture decisions for compute
{: #AD-compute}

The following are compute architecture decisions for the Classic edge gateway pattern.

| Architecture decision | Requirement                                                            | Options                                 | Decision   | Rationale                                    |
|---------------------------|----------------------------------------------------------------------------|---------------------------------------------|----------------|--------------------------------------------------|
| Compute: Bastion host     | Secure connection to manage internal systems and centralize access control | - Bare Metal  \n - Virtual Server | Virtual Server | flexible compute resources to meet compute needs |
| Compute: Proxy server     | Provide access to service endpoints from IPs not assigned by {{site.data.keyword.cloud_notm}}     | - Bare Metal  \n - Virtual Server | Virtual Server | flexible compute resources to meet compute needs. |
{: caption="Table 1. Classic edge gateway compute architecture decisions"}
