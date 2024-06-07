---

copyright:
  years: 2024, 2024
lastupdated: "2024-06-07"

subcollection: pattern-classic-edge-gateway

keywords: network, VPC, PowerVS, Classic edge gateway

---

{{site.data.keyword.attribute-definition-list}}

# Service management design
{: #servicemgmt-design}

The following are service management design considerations for the Classic edge gateway pattern.

## Monitoring
{: #monitoring}

Evaluate the level of monitoring required by considering the workload criticality and the metrics that need to be observed.

Basic instance monitoring deployed in {{site.data.keyword.cloud_notm}} Classic environments can be viewed from the cloud portal Health Dashboard (Health Dashboard, Device \> Usage).

More advanced monitoring capabilities such as troubleshooting, events and alerts, and custom dashboards can be leveraged by integrating the cloud resources with the cloud native service {{site.data.keyword.monitoringfull_notm}}.

Learn more about [{{site.data.keyword.monitoringfull_notm}}](/docs/monitoring?topic=monitoring-getting-started#getting-started)

## Log Analysis
{: #log-analysis}

Examine what logs are required for troubleshooting and auditing, as well as the retention policy necessary to meet the audit and compliance requirements.

{{site.data.keyword.loganalysislong_notm}} can be used to manage operating system logs, application logs, and platform logs in the {{site.data.keyword.cloud_notm}}. {{site.data.keyword.loganalysislong_notm}} offers administrators, DevOps teams, and developers advanced features to filter, search, and tail log data, define alerts, and design custom views to monitor application and system logs.

For more information, see [Getting started with {{site.data.keyword.loganalysislong_notm}}](/docs/log-analysis?topic=log-analysis-getting-started#getting-started).

{{site.data.keyword.loganalysislong_notm}} is deprecated and will no longer be supported as of 30 March 2025. The replacement service, {{site.data.keyword.logs_full_notm}} is planned to be generally available late second quarter 2024. For more information, see [Getting started with {{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-getting-started).
{: attention}

## Activity Tracking
{: #activity-tracker}

Consider the need to record and monitor the activities and changes made inside the {{site.data.keyword.cloud_notm}} account to assist in investigating abnormal activity, critical actions, and to meet regulatory audit requirements. {{site.data.keyword.cloudaccesstraillong_notm}} monitors and manages activities in the {{site.data.keyword.cloud_notm}}. It provides a dashboard and notification for real-time monitoring. For more information, see [Getting started with {{site.data.keyword.cloudaccesstraillong_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started).

* Explore details on [activity tracker](/docs/virtual-servers?topic=virtual-servers-at_events) events for virtual servers.
* Exploredetails on [activity tracker](/docs/bare-metal?topic=bare-metal-bm-at-events) for bare metal.

Alternatively, third party software such as Splunk and Datadog can be integrated with {{site.data.keyword.cloud_notm}} to provide security monitoring, compliance reporting, and operational intelligence.

{{site.data.keyword.cloudaccesstraillong_notm}} services are deprecated and will no longer be supported as of 30 March 2025. The replacement service, {{site.data.keyword.logs_full_notm}} is planned to be generally available late second quarter 2024. For more information, see [Getting started with {{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-getting-started).
{: attention}
