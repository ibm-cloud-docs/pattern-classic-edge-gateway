---

copyright:
  years: 2024, 2025
lastupdated: "2025-01-25"

subcollection: pattern-classic-edge-gateway

keywords: network, VPC, PowerVS, Classic edge gateway

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for service management
{: #AD-servicemgmt}

The following are the architecture decisions for service management for the classic edge gateway pattern.

## Architecture decisions for monitoring
{: #AD-monitoring}

| Architecture decision                                   | Requirement                                                                                          | Options                                                                          | Decision         | Rationale                                                        |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|----------------------|----------------------------------------------------------------------|
| Operational monitoring of cloud infrastructure and services | Monitor system health to detect issues that might impact the availability of the system and application. | - {{site.data.keyword.cloud_notm}} Health Dashboard  \n - IBM Cloud Monitoring  \n - Bring Your Own monitoring tool | IBM Cloud Monitoring | IBM Cloud Monitoring provides a robust cloud native monitoring tool. |
{: caption="Classic edge gateway service management monitoring architecture decisions"}

## Architecture decisions for logging
{: #AD-logging}

| Architecture decision                           | Requirement                                                                                             | Options                                                           | Decision           | Rationale                                                                                              |
|-----------------------------------------------------|-------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|------------------------|------------------------------------------------------------------------------------------------------------|
| Log monitoring of cloud infrastructure and services | Monitor operational logs to detect issues that might impact the availability of the system and application. | - {{site.data.keyword.logs_full_notm}}  \n - Bring Your Own logging tool | {{site.data.keyword.logs_full_notm}} | IBM Cloud Logging provides a robust cloud native logging tool. {{site.data.keyword.logs_full_notm}} are preferred when available. |
{: caption="Classic edge gateway service management logging architecture decisions"}

## Architecture decisions for auditing
{: #AD-auditing}

| Architecture decision | Requirement                                                                                | Options                                                                              | Decision               | Rationale                                                                                                     |
|---------------------------|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------|-------------------------------------------------------------------------------------------------------------------|
| Audit Logging             | Monitor audit logs to track changes to cloud resources and detect potential security problems. | - {{site.data.keyword.logs_full_notm}}  \n - Bring Your Own Activity Tracker software | {{site.data.keyword.logs_full_notm}} | {{site.data.keyword.logs_full_notm}} provides a robust cloud native audit tool. {{site.data.keyword.logs_full_notm}} are preferred when available. |
{: caption="Classic edge gateway service management auditing architecture decisions"}
