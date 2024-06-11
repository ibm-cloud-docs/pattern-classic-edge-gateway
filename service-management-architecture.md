---

copyright:
  years: 2024, 2024
lastupdated: "2024-06-11"

subcollection: pattern-classic-edge-gateway

keywords: network, VPC, PowerVS, Classic edge gateway

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for service management
{: #AD-servicemgmt}

The following are service management architecture decisions for the Classic edge gateway pattern.

## Architecture decisions for monitoring
{: #AD-monitoring}

| Architecture decision                                   | Requirement                                                                                          | Options                                                                          | Decision         | Rationale                                                        |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|----------------------|----------------------------------------------------------------------|
| Operational monitoring of cloud infrastructure and services | Monitor system health to detect issues that might impact the availability of the system and application. | - {{site.data.keyword.cloud_notm}} Health Dashboard  \n - IBM Cloud Monitoring (VPC)  \n - Bring Your Own monitoring tool | IBM Cloud Monitoring | IBM Cloud Monitoring provides a robust cloud native monitoring tool. |
{: caption="Table 1. Classic edge gateway service management monitoring architecture decisions"}

## Architecture decisions for logging
{: #AD-logging}

| Architecture decision                           | Requirement                                                                                             | Options                                                           | Decision           | Rationale                                                                                              |
|-----------------------------------------------------|-------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|------------------------|------------------------------------------------------------------------------------------------------------|
| Log monitoring of cloud infrastructure and services | Monitor operational logs to detect issues that might impact the availability of the system and application. | - IBM Cloud Log Analysis VPC IBM Cloud Logs  \n - Bring Your Own logging tool | IBM Cloud Log Analysis | IBM Cloud Logging provides a robust cloud native logging tool. IBM Cloud Logs is preferred when available. |
{: caption="Table 2. Classic edge gateway service management logging architecture decisions"}

## Architecture decisions for auditing
{: #AD-auditing}

| Architecture decision | Requirement                                                                                | Options                                                                              | Decision               | Rationale                                                                                                     |
|---------------------------|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------|-------------------------------------------------------------------------------------------------------------------|
| Audit Logging             | Monitor audit logs to track changes to cloud resources and detect potential security problems. | - IBM Cloud Activity Tracker (VPC)  \n - IBM Cloud Logs  \n - Bring Your Own Activity Tracker software | IBM Cloud Activity Tracker | IBM Cloud Activity Tracker provides a robust cloud native audit tool. IBM Cloud Logs is preferred when available. |
{: caption="Table 3. Classic edge gateway service management auditing architecture decisions"}
