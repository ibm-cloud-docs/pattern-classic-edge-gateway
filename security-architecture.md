---

copyright:
  years: 2024, 2024
lastupdated: "2024-06-07"

subcollection: pattern-classic-edge-gateway

keywords: network, VPC, PowerVS, Classic edge gateway

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for security
{: #AD-security}

The following are security architecture decisions for the Classic edge gateway pattern.

## Architecture decisions for identity and access management
{: #AD-IAM}

| Architecture decision               | Requirement                                                                                                                                                                                                            | Options                                                                                             | Decision                    | Rationale                                                                                                                                                                                                                                                                                      |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Privileged Access Management            | Ensure that all operator actions are run securely through a bastion host  \n Implement session recording to track all activities and note any potential threats  \n Manage access to resources and track commands issued | - Bring Your Own bastion host  \n - Jump server  \n - Bring Your Own bastion host with Privileged Access Management (PAM) software | Bring Your Own bastion host or jump server | The bastion host or jump server is a Virtual Server instance that is provisioned through SSH over a private network to securely access resources within the {{site.data.keyword.Bluemix_notm}} private network.  \n \n Using PAM software is recommended when session recording, tracking, and managing all access is required. |
| Identity Access & Role Management (IAM) | Securely authenticate users for platform services and control access to resources consistently across {{site.data.keyword.Bluemix_notm}}                                                                                                            | {{site.data.keyword.iamshort}}                                                                                           | {{site.data.keyword.iamshort}}                   | Use IAM access policies to assign users, service IDs, and trusted profiles access to resources within the {{site.data.keyword.Bluemix_notm}} account.                                                                                                                                                                       |
{: caption="Table 1. Classic edge gateway security identity and access management architecture decisions"}
