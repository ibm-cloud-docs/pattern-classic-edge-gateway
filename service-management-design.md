---

copyright:
  years: 2024, 2024
lastupdated: "2024-07-26"

subcollection: pattern-classic-edge-gateway

keywords: network, VPC, PowerVS, Classic edge gateway

---

{{site.data.keyword.attribute-definition-list}}

# Service management design
{: #servicemgmt-design}

The following are the design considerations for service management for the classic edge gateway pattern.

## Monitoring
{: #monitoring}

Evaluate the level of monitoring required by considering the workload criticality and the metrics that need to be observed. Basic instance monitoring that is deployed in {{site.data.keyword.Bluemix}} classic environments can be viewed from the cloud portal health dashboard. To locate this information from the health dashboard, click Device, and then click Usage.

More advanced monitoring capabilities such as troubleshooting, events and alerts, and custom dashboards can be used by integrating the cloud resources with the cloud native service {{site.data.keyword.monitoringfull_notm}}. For more information, see [{{site.data.keyword.monitoringfull_notm}}](/docs/monitoring?topic=monitoring-getting-started#getting-started).

## Log Analysis
{: #log-analysis}

{{site.data.keyword.loganalysislong_notm}} is deprecated and will no longer be supported as of 30 March 2025. The replacement service, {{site.data.keyword.logs_full_notm}} is planned to be generally available in late 2024. For more information, see [Getting started with {{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-getting-started).
{: attention}

Examine what logs are required for troubleshooting and auditing, as well as the retention policy necessary to meet the audit and compliance requirements.

{{site.data.keyword.loganalysislong_notm}} can be used to manage to operate system logs, application logs, and platform logs in the {{site.data.keyword.cloud_notm}}. {{site.data.keyword.loganalysislong_notm}} offers administrators, DevOps teams, and developers advanced features to filter, search, and tail log data, define alerts, and design custom views to monitor application and system logs.

For more information, see [Getting started with {{site.data.keyword.loganalysislong_notm}}](/docs/log-analysis?topic=log-analysis-getting-started#getting-started).

## Activity Tracking
{: #activity-tracker}

{{site.data.keyword.cloudaccesstraillong_notm}} services are deprecated and will no longer be supported as of 30 March 2025. The replacement service, {{site.data.keyword.logs_full_notm}} is planned to be generally available in late 2024. For more information, see [Getting started with {{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-getting-started).
{: attention}

Consider the need to record and monitor the activities and changes made inside the {{site.data.keyword.cloud_notm}} account to help investigating abnormal activity, critical actions, and to meet regulatory audit requirements. {{site.data.keyword.cloudaccesstraillong_notm}} monitors and manages activities in the {{site.data.keyword.cloud_notm}}. It provides a dashboard and notification for real-time monitoring. For more information, see [Getting started with {{site.data.keyword.cloudaccesstraillong_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started).

Alternatively, third-party software such as Splunk and Datadog can be integrated with {{site.data.keyword.cloud_notm}} to provide security monitoring, compliance reporting, and operational intelligence.
