[Title]

[Company]

# Overview
{: #overview}

The most common network scenario is to integrate on-premise access with Classic Infrastructure, Virtual Private Cloud (VPC), and PowerVS workloads within the {{site.data.keyword.cloud_notm}}. The objective of this pattern is to:

-   Illustrate on-premise network connectivity to a Classic edge gateway with a Virtual Private Cloud (VPC) and a PowerVS workspace in {{site.data.keyword.cloud_notm}}.
-   Provide an IBM Solution Design for the Network elements required when deploying in {{site.data.keyword.cloud_notm}}.
-   Securely connect your external locations to {{site.data.keyword.cloud_notm}} with {{site.data.keyword.dl_short}}, enabling access to Classic infrastructure, Virtual Private Cloud (VPC), and PowerVS resources.
-   Focus on {{site.data.keyword.cloud_notm}} Network elements, while ensuring requirements can be met from a performance, system availability, and connectivity perspective.

NOTE: This approach is referred to as Classic edge gateway because all traffic flows to the classic gateway for inspection before it is allowed to pass to any downstream workloads within {{site.data.keyword.cloud_notm}}.

This pattern is intended to:

-   Accelerate and simplify solution design by providing a standard {{site.data.keyword.cloud_notm}} deployment architecture reference following the [IBM {{site.data.keyword.arch_framework}}](/docs/architecture-framework).
-   Provide a prescriptive, end-to-end enterprise-class solution design, with diagrams, component architecture decisions along with rationale for cloud component selection to meet enterprise requirements.

# Reference Architecture
{: #reference-architecture}

This reference architecture is used to force all network traffic to flow through the classic firewall or gateway appliance for inspection before it is allowed to the downstream workloads within {{site.data.keyword.cloud_notm}}.

## Architecture diagram
{: #architecture-diagram}

This architecture will describe on-premises data center(s) connectivity into {{site.data.keyword.cloud_notm}} Classic, with workloads in classic, PowerVS, and VPC. The diagram includes examples to show where workload compute instances, proxy servers and jump servers would reside. Within the diagram, there are identifying numbers indicating key components in the description below.

![Illustrates Classic edge gateway solution architecture](image/classic edge.svg){: caption="Figure 1. Classic edge gateway solution architecture" caption-side="bottom"}


1.  Client network connectivity from on-premises using redundant Direct Links.
2.  Gateway provides routing and security functions.
3.  Optional network path is accomplished through site-to-site VPN terminated on Classic Gateway.
4.  Power Virtual Server workspace with Power Edge Router (PER), subnets, and resources
5.  GREa tunnel allows BYOIP to be advertised between Classic and on-premises.
6.  GREb tunnel allows BYOIP to be advertised between Classic and the PowerVS and VPC environments.
7.  GREc tunnel allows BYOIP to be advertised between Classic environments in separate regions.
8.  Virtual Bastion host for remote administrative access.
9.  DNS services
10. Proxy Server as an intermediary between on-prem and cloud services.
11. Cloud Internet Services (CIS) to enhance the security, performance, and reliability of internet-facing applications and websites.
12. Compute Instance in Virtual Private Cloud
13. Virtual Private Endpoint for VPC
14. Application load balancer

## Design scope
{: #design-scope}

Following the [{{site.data.keyword.arch_framework}}](/docs/architecture-framework?topic=architecture-framework-taxonomy), the Classic edge gateway network pattern covers design considerations and architecture decisions for the following aspects and domains:

- **Compute:** Virtual Servers
- **Networking:** Enterprise Connectivity, BYOIP/Edge Gateways, Network Segmentation, Cloud Native Connectivity, Load Balancing, and DNS
- **Security:** Identity and Access Management (IAM)
- **Resiliency:** High Availability, Disaster Recovery
- **Service management:** Monitoring, Logging, Auditing, Alerting, Event Management

![Illustrates a detailed network and component architecture for a
Classic edge gateway solution architecture](image/heat map.svg){: caption="Figure 2. Classic edge gateway design scope" caption-side="bottom"}

The Architecture Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. See [Introduction to the {{site.data.keyword.arch_framework}}](/docs/architecture-framework?topic=architecture-framework-intro) for more details.

## Requirements
{: #requirements}

The following represents a baseline set of requirements that are applicable to most clients and critical to a successful classic edge gateway network deployment.

| Aspect         | Requirement                                                                                                                                                                                                                                                                                      |
|--------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute            | Secure remote administrative support of all devices within the {{site.data.keyword.cloud_notm}} environment.                                                                                                                                                                                                                |
| Network            | Private enterprise connectivity from customer data centers to {{site.data.keyword.cloud_notm}} for access to applications, data, and services.                                                                                                                                                                              |
|                    | Private administrative and management connectivity.                                                                                                                                                                                                                                                  |
|                    | Provide network isolation with the ability to separate applications based on attributes such as data classification, public versus private traffic flows, and internal application function.                                                                                                         |
|                    | Provide the ability to use Bring Your Own IP (BYOIP)                                                                                                                                                                                                                                                 |
| Security           | Firewalls must be restrictively configured to provide advanced security features and prevent all traffic, both inbound and outbound, except that which is required, documented, and approved and optionally include Intrusion Protection System (IPS) and Intrusion Detection System (IDS) services. |
|                    | Distributed Denial of Service (DDoS) and Web Application Firewall (WAF) security capabilities required                                                                                                                                                                                               |
|                    | Secure access for administration and management of the environment                                                                                                                                                                                                                                   |
| Resiliency         | Multi-region capability to support a disaster recovery strategy and solution that allows all production applications to be included by using cloud infrastructure disaster recovery strategies.                                                                                                      |
| Service management | Provide health and system monitoring with ability to monitor and correlate performance metrics and events and provide alerting across applications and infrastructure.                                                                                                                               |
|                    | Ability to diagnose issues and exceptions and identify error source                                                                                                                                                                                                                                  |
{: caption="Table 1. Classic edge gateway requirements"}

## Components
{: #components}

| Aspect             | Component                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | How the component is used                                                                                                                   |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute                | [Virtual Server on Classic](/docs/virtual-servers?topic=virtual-servers-about-virtual-servers)  \n Bastion Host                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | The bastion host is deployed on a virtual server instance within classic and is used for remote administrative support.                         |
|                        | [Virtual Server on Classic](/docs/virtual-servers?topic=virtual-servers-about-virtual-servers)  \n Proxy Server                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Acts as an intermediary between the on-premises network and {{site.data.keyword.cloud_notm}} services.                                                                 |
| Networking             | Virtual Private Network (VPN)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Provides a secured connection into {{site.data.keyword.cloud_notm}} over the Internet. VPN can be used for migrations, administrative access, and backup connectivity. |
|                        | [Gateway Appliance in Classic](/docs/gateway-appliance?topic=gateway-appliance-getting-started-ga)  \n - [{{site.data.keyword.vsrx}}](/docs/vsrx?topic=vsrx-getting-started)  \n -  [{{site.data.keyword.vra}}](/docs/virtual-router-appliance?topic=virtual-router-appliance-getting-started-vra)  \n - [FortiGate FSA 10 Gbps](/docs/fortigate-10g?topic=fortigate-10g-getting-started)  \n - [FortiGate vFSA](/docs/vfsa?topic=vfsa-getting-started)  \n - Bring Your Own Gateway ([BYOG](/docs/gateway-appliance?topic=gateway-appliance-order-byoa))  \n Bare Metal (including Checkpoint, Cisco, Palo Alto) | Provides router, firewall, and VPN gateway functions for secure and reliable connectivity to cloud resources.                                   |
|                        | Generic Routing Encapsulation (GRE) tunnels                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Supports Bring Your Own IP (BYOIP) communication between on-premise, Classic Infrastructure, and PowerVS.                                       |
|                        | [{{site.data.keyword.dl_short}}](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl)  \n - Direct Link Connect                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Connect on-premise networks to the {{site.data.keyword.cloud_notm}} using physical telco connections or virtual exchange services.                                     |
|                        | [Load Balancers](/docs/vpc?topic=vpc-nlb-vs-elb)  \n - Network load balancer  \n - Application load balancer  \n - [F5 Big IP](https://clouddocs.f5.com/cloud/public/v1/ibm_index.html){: external}.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Application Load Balancing for web servers, app servers, and database servers                                                                   |
|                        | [Private Service Endpoints](/docs/account?topic=account-vrf-service-endpoint&interface=ui#use-service-endpoint)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Connect directly to cloud services without using the public network                                                                             |
|                        | [{{site.data.keyword.cis_short}} (CIS)](/docs/cis?topic=cis-getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Public Load balancing of web server traffic across regions                                                                                      |
|                        | [DNS Services](/docs/dns-svcs?topic=dns-svcs-about-dns-services)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | The Domain Name System (DNS) to associate human-friendly domain names with IP addresses                                                         |
| Security           | [IAM](/docs/account?topic=account-cloudaccess)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | IBM Cloud Identity & Access Management                                                                                                          |
|                        | [{{site.data.keyword.cis_short}} (CIS)](/docs/cis?topic=cis-getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | DDoS protection and Web Application Firewall (WAF) for public connectivity                                                                      |
|                        | [Gateway Appliance in Classic](/docs/gateway-appliance?topic=gateway-appliance-getting-started-ga)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Advanced firewall capabilities such as Intrusion Detection System (IDS) and Intrusion Protection System (IPS) services.                         |
|                        | Bastion Host                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | The Bastion Host is deployed on a virtual server instance within classic and will be used for remote administrative support                     |
| Resiliency         | Multi-Region Deployment                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Allows for disaster recovery in secondary region                                                                                                |
|                        | Multiple {{site.data.keyword.dl_short}} connections                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Allows for network resiliency for failover & recovery                                                                                           |
| Service management | Health Dashboard                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Apps and operational monitoring                                                                                                                 |
|                        | [IBM Cloud monitoring](/docs/monitoring?topic=monitoring-getting-started#getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Used to gain operational visibility into the performance and health of your applications, services, and platforms.                              |
|                        | [IBM Cloud Log Analysis](/docs/log-analysis?topic=log-analysis-getting-started#getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Used to manage operating system logs, application logs, and platform logs in the {{site.data.keyword.cloud_notm}}                                                      |
|                        | [IBM Cloud Activity Tracker](/docs/activity-tracker?topic=activity-tracker-getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Used to capture and monitor activities in your {{site.data.keyword.cloud_notm}} account                                                                                |
|                        | [IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Logging service providing users with capabilities for querying, tailing, and visualizing logs.                                                  |
{: caption="Table 2. Classic edge gateway solution components"}

# Compute design
{: #compute-design}

The following are compute design considerations for the Classic edge gateway pattern.

## Workload Hosts (Classic, VPC, and PowerVS)
{: #workload-hosts}

Selecting the appropriate workload host in this architecture includes but is not limited to the following considerations regarding:

-   Performance: CPU, Memory, storage options, and network speed.
-   Virtualization and tenancy: container, virtual or bare metal, multitenant or dedicated single tenant.
-   Security, availability, and cost considerations.
-   Application-specific certification or optimization requirements.

Based on workload and specific requirements, select from a range of Virtual Servers, Bare Metal servers, containers and VMware-based solutions.

* Explore [{{site.data.keyword.baremetal_short}}](/docs/bare-metal?topic=bare-metal-about-bm)s for Classic.
* Explore [{{site.data.keyword.virtualmachinesshort}}](/docs/virtual-servers?topic=virtual-servers-about-virtual-servers) for Classic.
* Explore [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cluster-create-classic&interface=ui) and [{{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-getting-started) for classic environments.
* Explore [{{site.data.keyword.vmwaresolutions_full_notm}}](/docs/vmwaresolutions?topic=vmwaresolutions-getting-started).
* Explore [{{site.data.keyword.powerSys_notm}}](/docs/power-iaas?topic=power-iaas-getting-started).
* Explore [{{site.data.keyword.vsi_is_short}}](/docs/vpc?topic=vpc-about-advanced-virtual-servers) for VPC.
* Explore [{{site.data.keyword.bm_is_short}}](/docs/vpc?topic=vpc-about-bare-metal-servers) for VPC.

## Jump Server and Bastion Hosts
{: #jump-bastion}

Infrastructure requirements for Linux and Windows jump servers and bastion hosts can vary based on factors such as the number of concurrent users, the specific use case, and the applications or services that run on the server. An average jump server with 8 CPU and 16 GB of RAM, can support 25 concurrent sessions of any type (100 serial-over-LAN sessions, 200 Telnet, or SSH sessions). More sessions can be supported by larger server specifications.

Table 1 contains general jump server and bastion host sizing guidelines for Ubuntu LTS, CentOS, or Debian Bastion Hosts:

| Traffic | Concurrent Users | Server Size               |
|-------------|----------------------|-------------------------------|
| Low         | 10-20                | 1-2 Cores CPU; 2-4 GB Memory  |
| Moderate    | 30-50                | 2-4 Cores CPU; 4-8 GB Memory  |
| High        | 50+                  | 4-8 Cores CPU; 8-16 GB Memory |
{: caption="Table 1. Classic edge gateway Bastion Server sizing"}

The jump server or bastion host can be deployed on either a {{site.data.keyword.baremetal_short_sing}} or {{site.data.keyword.BluVirtServers_short}} Instance (VSI) within the classic environment. This pattern uses a VSI to deploy a bastion host.

## Proxy server
{: #proxy-server}

The proxy server acts as an intermediary between the on-premises network and the {{site.data.keyword.cloud_notm}} services.

Some key design considerations for a proxy server include:

- Proxy type: Choose between a forward proxy where clients access the internet through it or a reverse proxy that sits in front of web servers for load balancing or security.
- Protocol support: Ensure that the proxy supports the protocols that are used by the clients and the servers it communicates with HTTP, HTTPS, FTP, and so on.
- Content transformation: Decide whether the proxy needs to modify content, for example, compression and encryption before sending it to clients.
- Caching: Determine how aggressively the proxy caches content to improve performance and reduce bandwidth usage.
- Security: Consider features like access control, encryption (SSL/TLS), and content filtering to protect your network.
- Logging and monitoring: Decide what data to log for troubleshooting, security audits, and usage analysis.
- Hardware: Select hardware with sufficient processing power, memory, and network bandwidth to handle expected traffic.
- Scalability: Design the architecture to accommodate future growth in users and traffic volume.
- High Availability: Consider redundancy measures to ensure that the proxy remains operational if there is a failure.

Sizing considerations include:

- Number of users: More users require a more powerful processor and more memory to handle concurrent connections.
- Traffic volume: Higher traffic volume demands faster processors, more RAM, and potentially higher network bandwidth.
- Proxy functionality: Features like caching, security (encryption), and content filtering can increase resource consumption.

General sizing guidelines include:

- CPU: A multi-core processor (4 or more cores) is recommended for efficient handling of multiple connections.
- Memory: 4 GB or more is a good starting point, but more memory might be needed for very demanding scenarios. A rule of thumb is 32 MB of RAM for every 1 GB of disk space for caching purposes.
- Disk space: Enough space to store the operating system, proxy server software, and potentially cached data. The amount of cache storage depends on your caching strategy.
- Network interface: A high-bandwidth network interface card (NIC) is crucial for handling incoming and outgoing traffic efficiently.

The proxy server can be deployed on either a Bare Metal Server or Virtual Servers Instance (VSI) within the classic environment. This pattern uses a VSI to deploy the proxy server.

# Network design
{: #network-design}

The following are network design considerations for the Classic edge gateway pattern.

## Virtual Private Network (VPN)
{: #vpn}

Implementing a site-to-site VPN offers secure connectivity between the on-premises networks and remote locations but comes with important considerations before deployment.

- Bandwidth: Analyze expected data transfer needs and choose a VPN solution with sufficient bandwidth capacity.
- Bandwidth consumption: Depending on the service provider, data transfer through the VPN might incur usage charges.
- Latency: Consider the physical distance between connected sites and the potential latency impact on performance-sensitive applications.
- Encryption overhead: Encryption adds processing overhead. Evaluate the impact on performance, especially for real-time applications.
- Hardware and software: Factor in the cost of VPN hardware, software licenses, and any additional infrastructure needed.
- Scalability: Ensure that the chosen VPN solution can accommodate future growth in network size and data transfer needs.
- High Availability: Implement redundant connections or failover mechanisms to ensure connectivity even during outages.
- Compliance: If your organization operates under specific compliance regulations, ensure that the VPN solution meets those requirements.
- Management: Managing the VPN infrastructure can require dedicated personnel or managed services, adding to the cost.
- Technical expertise: Implementing and maintaining a VPN might require technical expertise. Assess your internal IT capabilities or consider external support options.

IBM classic data centers offer three options for implementing a virtual private network connection from a remote site into {{site.data.keyword.cloud_notm}}, including SSL VPN, IPsec VPN, and VPN gateway appliance on classic. This pattern supports VPN gateway appliance on classic.

For more information, see [Virtual Private Network options](/docs/iaas-vpn/set-up-ipsec-vpn.html?topic=iaas-vpn-getting-started#use-case-scenarios) available in {{site.data.keyword.cloud_notm}}.

## Gateway Appliance and Firewall
{: #gateway-appliance}

Choosing the right firewall is crucial for safeguarding your network. Review the key consideration guidance.

- Basic versus Next-Generation: Basic firewalls offer packet filtering, while Next generation Firewalls (NGFWs) provide deeper inspection, application control, intrusion prevention, and more. Choose based on your security needs and complexity.
- Threat detection and prevention: Consider features like malware detection, intrusion prevention, and sandboxing to address evolving threats.
- VPN capabilities: If secure remote access is needed, ensure that the firewall supports VPN protocols suitable to meet the requirements.
- Content filtering: Control access to specific websites or categories based on user groups or policies.
- Throughput: Ensure that the firewall can meet the internet traffic volume without bottlenecks.
- Latency: Evaluate potential latency impact, especially for real-time applications like video conferencing.
- Scalability: Consider future network growth and choose a firewall that can accommodate increased traffic and users.
- Management interface: Choose a web-based, command-line, or physical interface that aligns with your IT team's expertise and preference.
- Logging and reporting: Robust logging and reporting capabilities are essential for monitoring activity and detecting security incidents.
- Updates and support: Opt for a firewall with regular software updates and reliable technical support from the vendor.
- Budget: Firewalls range in price based on features and capabilities. Determine a budget and prioritize.
- Compatibility: Ensure that the firewall is compatible with existing network hardware and software.
- Security certifications: Look for firewalls that comply with recognized security standards like NSS labs or Common Criteria.
- Brand reputation: Choose a reputable brand with a proven track record and positive customer reviews.
- Virtual appliance versus hardware firewalls: Evaluate cloud-based options for flexibility and scalability but consider throughput and port sizes for latency and bottlenecks.
- Single versus High Availability: Consider single points of failure and service level requirements.

{{site.data.keyword.IBM_notm}} classic data centers support four gateway appliance and firewall options including Juniper vSRX, Virtual Router Appliance, FortiGate (FAS 10 Gbps and vFSA), and bring your own gateway appliance (BYOG - Checkpoint, Cisco, and Palo Alto). This pattern supports personal choice based on appliance functional requirements and operational expertise.

Explore and compare [gateway options](/docs/fortigate-10g?topic=fortigate-10g-exploring-firewalls) available in {{site.data.keyword.cloud_notm}}.

## Generic Routing Encapsulation (GRE) tunnels
{: #GRE}

GRE tunnels support the Bring Your Own IP (BYOIP) requirement.

![Illustrates the details of GRE for a Classic edge gateway solution architecture](image/GRE.svg){: caption="Figure 1. Classic edge gateway GRE Encapsulation" caption-side="bottom"}

1.  Client network connectivity from on-premises is accomplished through Direct Link access.
2.  A gateway is deployed in classic, which provides routing and security functions.
3.  A GRE tunnel is created between the gateway and a customer router.
4.  192.168.xx.xx (BYOIP) packets are encapsulated by 10.1.x.x, IBM assigned IP, which is routed through the BCR over the GRE and unencapsulated on the other side.

{{site.data.keyword.BluDirectLink}} does not offer BYOIP on the private network. The {{site.data.keyword.IBM_notm}} Cloud backbone advertises the customer’s available routes that are assigned by {{site.data.keyword.IBM_notm}} to their remote networks. Traffic with a destination IP address that is not assigned by {{site.data.keyword.cloud_notm}} (10.0.0.0/8) is dropped by {{site.data.keyword.cloud_notm}}. Traffic can be encapsulated between the remote client network and the {{site.data.keyword.cloud_notm}} network for nonassigned {{site.data.keyword.cloud_notm}} addresses by establishing GRE tunnels between the gateway and a customer router. This allows non-IBM assigned IP addresses to be passed back to the on-premises environment.

A second GRE is required between the classic gateway and the transit gateway when nonassigned {{site.data.keyword.cloud_notm}} addresses are used in the VPC or {{site.data.keyword.powerSys_notm}} environment.

A third GRE is used to share nonassigned {{site.data.keyword.cloud_notm}} addresses between classic gateways in separate regions. When resiliency is required, GREs can be configured on two devices in a high availability pair to eliminate single points of failure. For more information, see [High availability and disaster recovery](/docs/transit-gateway?topic=transit-gateway-ha-dr#high-availability) and [{{site.data.keyword.cloud_notm}} public and private IP ranges](/docs/cloud-infrastructure?topic=cloud-infrastructure-ibm-cloud-ip-ranges).

When you configure a GRE connection on a transit gateway, you must specify the availability zone. For a robust high availability solution, configure multiple GRE connections that use different availability zones.
{: important}

## Enterprise Connectivity
{: #enterprise-connectivity}

The two {{site.data.keyword.dl_short}} options available are {{site.data.keyword.dl_short}} Dedicated and {{site.data.keyword.dl_short}} Connect.

Enterprise connectivity considerations include:

- Bandwidth requirements: There are various bandwidth options available in [{{site.data.keyword.dl_full_notm}}](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl). {{site.data.keyword.dl_short}} Dedicated offers higher bandwidth potential.
- Security needs: If isolation is needed, {{site.data.keyword.dl_short}} Dedicated provides the highest isolation for sensitive data.
- Location and existing infrastructure: Dedicated requires colocation or dedicated circuits. Consider the [Provider location](/docs/dl?topic=dl-locations#connect-locations).
- Cost: {{site.data.keyword.dl_short}} Connect is more cost effective, especially for smaller bandwidth needs.
- Deployment time: {{site.data.keyword.dl_short}} Connect can be deployed faster due to pre-established connections.

To avoid IP address conflicts for classic connections to a Direct Link, avoid IP address ranges in the 10.0.0.0/14, 10.200.0.0/14, 10.198.0.0/15, and 10.254.0.0/16 blocks for on-premises networks. On-premises routes that overlap are dropped.
{: note}

### {{site.data.keyword.dl_short}} Dedicated
{: #direct-link-dedicated}

{{site.data.keyword.dl_short}} Dedicated is ideal for:

- Organizations with colocation facilities near {{site.data.keyword.cloud_notm}} PoPs or data centers.
- Network service providers that deliver circuits to customers or other data centers.
- Highly sensitive data or mission-critical applications that require maximum security and performance.
- Organizations that need fine-grained control over routing and traffic management. {{site.data.keyword.dl_short}} dedicated is ideal for moderate to high-bandwidth needs.

{{site.data.keyword.dl_short}} Dedicated use cases include:

- Banking and finance: Transferring sensitive financial data or running critical trading applications with maximum security and performance.
- Healthcare: Managing protected health information (PHI) with strict compliance requirements.
- Government and defense: Handling classified data or mission-critical systems that demand the highest security levels.
- Research and development: Transferring large data sets for research or running high-performance computing (HPC) workloads.
- Media and entertainment: Streaming high-quality video content or managing large media files with low latency and high bandwidth.

For more information, see [available {{site.data.keyword.dl_short}} Dedicated locations](/docs/dl?topic=dl-locations#dedicated-locations).

### {{site.data.keyword.dl_short}} Connect
{: #direct-link-connect}

{{site.data.keyword.dl_short}} Connect is ideal for:

- Organizations with diverse connectivity requirements for multi-cloud and hybrid cloud.
- Organizations that are not located near {{site.data.keyword.cloud_notm}} PoPs or need smaller bandwidths.
- Organizations seeking a more affordable private connection compared to {{site.data.keyword.dl_short}} Dedicated.
- Simple and quick deployment due to pre-established connections in data centers.

{{site.data.keyword.dl_short}} Connect use cases include:

- Hybrid cloud architectures: Connecting on-premises infrastructure to {{site.data.keyword.cloud_notm}} resources for seamless workload distribution and data sharing.
- Multi-cloud deployments: Linking {{site.data.keyword.cloud_notm}} with other cloud providers for workload flexibility and disaster recovery.
- Remote branch offices: Connecting geographically dispersed offices to {{site.data.keyword.cloud_notm}} for centralized applications and data access.
- Software-as-a-Service (SaaS) applications: Accessing SaaS applications that are hosted on {{site.data.keyword.cloud_notm}} with improved performance and security.

For more information, see [{{site.data.keyword.dl_short}} Connect locations and providers](/docs/dl?topic=dl-locations#connect-locations).

## Load Balancing
{: #load-balancing}

Review the three types of load balancing.

### Local Load balancing
{: #local-load-balancing}

Load balancing is the process of distributing network traffic efficiently among multiple servers within a single region to optimize and ensure application availability to meet specific service level requirements.

Considerations Include:

- Layer-4 load balancing for HTTP, HTTPS, and TCP traffic: Distributes traffic based on IP addresses, ports, and basic metrics like packet arrival time. Works well for simple traffic but lacks deeper application understanding.
- Advanced Layer-7 load balancing for HTTP and HTTPS traffic: Analyzes the deeper aspects like URLs, headers, and user data. Offers granular routing based on the content, user needs, and server capabilities. Ideal for complex applications.
- Server health checks: Regularly ping servers to ensure that they are healthy and can handle traffic. Unhealthy servers are removed from the pool until they recover.
- SSL offload: Takes over the decryption and encryption of HTTPS traffic, freeing up server resources for application processing. Improves performance and security.
- Public internet-facing to private load balancing: Makes the internal applications accessible from the internet through a public IP address and NAT gateway. Provides an extra layer of security by keeping application servers hidden on a private network.
- Public to public internet-facing load balancing: Distributes traffic among multiple public-facing servers for high availability and scalability of websites or services.
- Private internal load balancing: Distributes traffic among internal servers on a private network. Improves performance and scalability for internal applications without internet exposure.

{{site.data.keyword.cloud_notm}} offers two Load balancer options for classic infrastructure which include: {{site.data.keyword.loadbalancer_full}} and {{site.data.keyword.vpx_full}} appliance.

* Explore [load balancer feature options](/docs/citrix-netscaler-vpx?topic=citrix-netscaler-vpx-explore).
* Learn more about [{{site.data.keyword.loadbalancer_full}}](/docs/loadbalancer-service?topic=loadbalancer-service-about-ibm-cloud-load-balancer)
* Learn more about [{{site.data.keyword.vpx_full}} Load Balancer](/docs/citrix-netscaler-vpx?topic=citrix-netscaler-vpx-about-citrix-netscaler-vpx)

{{site.data.keyword.IBM_notm}} {{site.data.keyword.vpc_full}} (VPC) offers three load balancer options which include: {{site.data.keyword.nlb_full}}, {{site.data.keyword.alb_full}}, and the F5 BIG-IP Virtual Edition for VPC.

* Explore [VPC load balancer options](/docs/vpc?topic=vpc-nlb-vs-elb#load-balancer-types).
* Refer to the VPC [load balancer comparison chart](/docs/vpc?topic=vpc-nlb-vs-elb#lb-comparison-chart) for more information.
* Learn more about the [{{site.data.keyword.alb_full}}](/docs/vpc?topic=vpc-load-balancers-about&interface=ui).
* Learn more about the [{{site.data.keyword.nlb_full}}](/docs/vpc?topic=vpc-nlb-vs-elb#load-balancer-nlb).
* Learn more about the [F5 BIG-IP Virtual Edition for VPC](https://clouddocs.f5.com/cloud/public/v1/ibm_index.html){: external}.

This pattern uses the {{site.data.keyword.cloud_notm}} {{site.data.keyword.alb_full}} (ALB) to distribute traffic among multiple server instances within the region.
{: note}

### Global Load Balancing
{: #gslb}

Global Server Load Balancing (GSLB) is a technique for distributing internet traffic across geographically dispersed servers. It aims to optimize user experience and application performance by directing users to the nearest or most appropriate server based on various factors like latency, server load, and user location. GSLB is a valuable component of a disaster recovery strategy.

If Global Server Load Balancing (GSLB) is required, {{site.data.keyword.cloud_notm}} offers a global load balancer service through {{site.data.keyword.cis_full_notm}}, which routes traffic to servers across multiple geographic locations to ensure application availability.

Other third-party providers such as Akamai, network appliances from vendors like F5 and Citrix, and Domain Name Service can also be used to meet the global load balancing requirement.

When a user tries to access a website or application, the request goes to the GSLB device. The GSLB device uses various algorithms and real-time data to determine the best server to route the request to. Factors that are considered include:

- User location: Sending users to the closest server minimizes latency.
- Server load: Distributing traffic among available servers prevents overloading any single server.
- Server health: Avoiding unresponsive or overloaded servers.
- Application-specific criteria: Specific services might require routing based on user type, content availability, and so on.

For more information, see [Global Server Load Balancing and other features of {{site.data.keyword.cis_full_notm}}](/docs/cis?topic=cis-about-ibm-cloud-internet-services-cis).

## Private access to Cloud Services
{: #cloud-services}

With the presence of both the classic infrastructure and Virtual Private Cloud (VPC) environments, there are two paths to access cloud services privately.

Moving these workloads from the public network to the private network offers two advantages:

- Enhanced security: Cloud Services are no longer served on an internet routable IP address.
- Cost-effective: The private network does not incur billable or metered bandwidth charges.

### Service Endpoints
{: #service-endpoints}

With {{site.data.keyword.cloud_notm}} service endpoints, you can connect to {{site.data.keyword.cloud_notm}} services over the {{site.data.keyword.cloud_notm}} private network instead of the default public network.

In {{site.data.keyword.cloud_notm}} classic, virtual route forwarding (VRF) must be enabled on the account to move routing to a dedicated routing table. When VRF is enabled on the {{site.data.keyword.cloud_notm}} account, Private Service Endpoints can be created. Cloud service resources can then be accessed by the {{site.data.keyword.cloud_notm}} private network from an {{site.data.keyword.IBM_notm}} assigned IP address in the classic account. For more information, see [{{site.data.keyword.cloud_notm}} VRF](/docs/dl?topic=dl-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud).

When the source address is not an {{site.data.keyword.IBM_notm}} assigned IP address, a proxy server in classic is used as an intermediary allowing access to cloud services.

Verify that [Private Service endpoints are available for your cloud services](/docs/account?topic=account-vrf-service-endpoint&interface=ui).

### Virtual Private Endpoint for VPC
{: #VPE}

{{site.data.keyword.cloud_notm}} {{site.daata.keyword.vpe_full}} enables you to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choosing, allocated from a subnet within your VPC.

Learn more on [{{site.daata.keyword.vpe_full}}](/docs/vpc?topic=vpc-about-vpe).

Verify that cloud services are VPE for VPC [enabled](/docs/vpc?topic=vpc-vpe-supported-services#vpe-enabled-supported-services).

This pattern uses the VPE for VPC to access cloud services privately.
{: note}

![Illustrates SE versus {{site.data.keyword.vpe_short}} for Classic edge gateway solution architecture](image/SE-vs-VPE.svg){: caption="Figure 2. Classic edge gateway Service Endpoint access" caption-side="bottom"}

## Cloud Internet Services (CIS)
{: #CIS}

{{site.data.keyword.cis_full_notm}} (CIS) provides global server load balancing, public domain name services, and public network security features.

Learn more about [CIS](/docs/cis?topic=cis-about-ibm-cloud-internet-services-cis)

## Domain Name Services (DNS)
{: #DNS}

Domain Name Services provides access to your systems and services via user-friendly domain names rather than IP addresses.

Key considerations are:

- Public vs private DNS name resolution: ability to provide DNS based access to users on the public Internet vs internal DNS-based communication within your private environment.
- Integration with other existing DNS systems: integrating your on-premises DNS service with your cloud environment.

{{site.data.keyword.cloud_notm}} provides a flexible approach to DNS name resolution.

- Public DNS name resolution: translates human-friendly names into computer-readable addresses.
    - Use the [DNS interface](/docs/dns?topic=dns-how-to-use-the-dns-interface) in the {{site.data.keyword.cloud_notm}} Portal to manage your zones and records. The domain can be hosted by any 3rd party provider. Set the Nameserver (NS) record to the provided {{site.data.keyword.IBM_notm}} Name servers.
    - Use the DNS function provided with {{site.data.keyword.cis_full_notm}}. Domain name control must be delegated to {{site.data.keyword.cis_full_notm}} (CIS).
    - Use DNS services to provide public name resolution for on-premise, classic, VPC, and PowerVS resources by deploying [Custom Resolvers](/docs/dns-svcs?topic=dns-svcs-custom-resolver). Learn more about [{{site.data.keyword.dns_full_notm}}](/docs/dns-svcs?topic=dns-svcs-getting-started).
- Private DNS name resolution: allows integration with your on-premises DNS server over private connectivity.
    - Configure your own DNS services in the classic environment, typical options are:
        - Install your own custom DNS service on a virtual server.
        - Configure DNS on your gateway device.
    - Utilize {{site.data.keyword.dns_full_notm}} for VPC.
        This Service allows you to provide versatile private name resolution between classic, on-premises and VPC resources by deploying [Custom Resolvers](/docs/dns-svcs?topic=dns-svcs-custom-resolver). Learn more about [{{site.data.keyword.dns_full_notm}}](/docs/dns-svcs?topic=dns-svcs-getting-started).

        This pattern uses {{site.data.keyword.dns_full_notm}} to provide FQDN resolution for on-premise, Classic, VPC, and PowerVS workload environments.
        {: note}

# Security design
{: #security-design}

The following are security design considerations for the Classic edge gateway pattern.

## Identity and Access Management (IAM)
{: #IAM}

{{site.data.keyword.iamshort}} provides role-based access controls (RBAC) and is part of the zero trust strategy that allows for least privileged access to help support regulatory and compliancy requirements. {{site.data.keyword.IBM&reg}} Security Verify can be added to support multi-factor authentication. For more information, see [{{site.data.keyword.iamshort}} (IAM)](/docs/account?topic=account-iamoverview) and [{{site.data.keyword.IBM&reg}}  Security Verify](https://www.ibm.com/verify){: external}.

Click to learn more about [IAM Roles](/docs/account?topic=account-userroles).

## Cloud Internet Services (CIS)
{: #internet-services}

In addition to providing Global Server load balancing and domain name services, {{site.data.keyword.cis_full_notm}} (CIS) provides many security features to help meet compliance requirements as either a Pay-As-You-Go or bundled service package option. For more information, see [{{site.data.keyword.cis_full_notm}}](/docs/cis?topic=cis-about-ibm-cloud-internet-services-cis).

Consider using {{site.data.keyword.cis_full_notm}} or other third-party products such as Akamai, Cloudflare, Imperva, Barracuda, or F5 to meet security requirements:

- [DDoS protection](/docs/cis?topic=cis-distributed-denial-of-service-ddos-attack-concepts): Shields your website from malicious attacks that flood it with traffic.
- [Web Application Firewall (WAF)](/docs/cis?topic=cis-waf-q-and-a): Acts as a security guard for your website, filtering out suspicious traffic and blocking known threats like SQL injections and code injection attempts.
- Transport Layer Security (TLS): Encrypts communication between your website and visitors, protecting sensitive data like passwords and credit card information.
- Range: non-HTTP and HTTPS port protection Secures ports on your server beyond the standard web traffic ports (HTTP and HTTPS), protecting against attacks that target vulnerable services.

In this pattern IBM Cloud Internet Services DDoS and WAF features are used to meet security requirements.
{: note}

## Gateway Appliance – Firewall
{: #firewall}

{{site.data.keyword.cloud_notm}} classic firewalls offer various security functions to protect your cloud resources. Specific features and capabilities of {{site.data.keyword.cloud_notm}} classic firewalls vary depending on the vendor, hardware, software, licenses, add-on bundle, and configuration options selected.

Consider existing vendor relationships and operation teams’ technical expertise before you make an appliance choice.

In addition, consider the following needs:

- Number of users and devices: How many devices are connected to the network? A small business network has different needs than a large business network.
- Types of devices: Consider the variety of resources, data, and applications connected to the network.
- Bandwidth requirements: How much data traffic does the network typically handle?
- Scalability: Choose a firewall that can grow with your network needs.
- Type of firewall: Different firewall types offer different levels of protection. Consider stateful inspection firewalls, application-level firewalls, or next-generation firewalls (NGFWs) depending on the security need.
- Security features: Look for features like intrusion detection and prevention systems (IDS and IPS), deep packet inspection (DPI), malware protection, and content filtering.
- Threat intelligence: Choose a firewall that receives regular updates on the latest threats and vulnerabilities.

Consider the security level required based on the business need:

* Tier 1: Public Front-end Enterprise
* Tier 2: General Internal Enterprise
* Tier 3: Business Critical Enterprise
* Tier 4: Highly sensitive Enterprise
* Tier 5: Ultra-Secure Enterprise

| Security Function                                                    | Tier 1 | Tier 2 | Tier 3 | Tier 4 | Tier 5 |
|--------------------------------------------------------------------------|------------|------------|------------|------------|------------|
| Network Firewall                                                         | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Web Application Firewall (WAF)                                           | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Intrusion Detection System (IDS)                                         | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Antivirus/Antimalware Software                                           | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Network Segmentation                                                     |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Administrative Role-Based Access Controls (RBAC)                         |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Virtual Private Network (VPN)                                            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |            |            |
| Advanced Firewall with Deep Packet Inspection                            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Intrusion Prevention System (IPS)                                        |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Multifactor Authentication (MFA)                                         |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Integration with Security Incident and Event Management (SIEM) System    |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Network Segmentation with Strict Access Controls                         |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Administrative Privileged Access Management (PAM)                        |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Integration with Advanced Threat Intelligence and Threat Hunting systems |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Zero-Trust Architecture                                                  |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Micro-Segmentation                                                       |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Continuous Monitoring and Auditing                                       |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Integration with Advanced Threat Detection and Response (ATDR) Systems   |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Integration with Real-Time Incident Response Capabilities                |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
{: caption="Table 1: Classic edge gateway security features"}

{{site.data.keyword.cloud_notm}} classic data centers support four gateway appliance and firewall options including Juniper vSRX, Virtual Router Appliance, FortiGate (FSA 10 Gbps and vFSA), and bring your own gateway appliance (BYOG) for Checkpoint, Cisco, and Palo Alto. This pattern supports personal choice based on security requirements and operational expertise.

### Gateway references
{: #gateway-reference}

* Explore [{{site.data.keyword.vra}} (vRA)](/docs/virtual-router-appliance?topic=virtual-router-appliance-getting-started-vra).
* Explore [{{site.data.keyword.vsrx_full}} license features](/docs/vsrx?topic=vsrx-getting-started#choosing-license).
* Explore [FortiGate FSA 10 Gbps](/docs/fortigate-10g?topic=fortigate-10g-getting-started).
* Explore [Virtual FortiGate Security Appliance (vFSA)](/docs/vfsa?topic=vfsa-getting-started)
* Explore [Bring Your Own Gateway Appliance (BYOG) to support Checkpoint, Cisco, and Palo Alto](/docs/gateway-appliance?topic=gateway-appliance-order-byoa).

## VPN Security
{: #VPN-security}

VPN offers a valuable layer of security for public internet activities.

Key VPN Security Considerations include:

- Encryption strength: Choose a VPN with strong encryption algorithms like AES-256. Weak encryption can be cracked, exposing your data.
- Protocol choice: Protocols like OpenVPN, IKEv2, and IPsec offer strong security and performance. Avoid outdated or less secure protocols like PPTP.
- Authentication: Consider strong authentication methods like multi-factor authentication for secure data transmission.
- Access control: Granularly control access permissions to resources accessible through the VPN, limiting potential damage from breaches.
- Security of endpoints: Ensure that all connected devices comply with security policies and are kept up to date with patches.

{{site.data.keyword.cloud_notm}} classic data centers offer three options for implementing a virtual private network connection from a remote site into {{site.data.keyword.cloud_notm}}, including SSL VPN, IPsec VPN, and VPN gateway appliance on classic. This pattern supports the VPN gateway appliance on classic to meet the private administrative and management connectivity requirements.

## Service Endpoints
{: #SE}

With {{site.data.keyword.cloud_notm}} service endpoints and Virtual Private endpoints for VPC, you can connect to {{site.data.keyword.cloud_notm}} services over the {{site.data.keyword.cloud_notm}} private network instead of the default public network. Moving these workloads from the public network to the private network offers enhanced security as Cloud Services are no longer served on an internet routable IP address.

## Jump Server or Bastion Host
{: #jump-server}

Jump servers or bastion hosts can offer extra levels of security and control and can be deployed on bare metal or virtual server instances within {{site.data.keyword.cloud_notm}} classic.

Considerations include:

- Enhanced security required - All access to internal systems funnels through the jump server or bastion host, making it easier to monitor and enforce security policies. Access can be granted or revoked to specific users and systems with granularity. By keeping internal systems directly inaccessible from the outside, it minimizes potential entry points for attackers. Hackers need to compromise the jump server first, adding an extra layer of defense.
- Improved management and auditing - Manage access to all internal systems from a single point, streamlining the process and reduce errors. All activity on the jump server or Bastion host is logged, providing a centralized record of who accessed what and when. This helps with troubleshooting, security audits, and forensic investigations.
- Secure access to legacy systems - Jump servers and Bastion hosts can act as a bridge between modern tools and legacy systems that might not support secure protocols like SSH. You can use the jump server to tunnel secure connections to older systems.
- Multi-factor Authentication (MFA) - MFA can be implemented on the jump server or Bastion host itself, adding another layer of protection to internal system access.

The following table helps determine whether a bastion host or jumper server is needed:

| Feature    | Jump Server                                                       | Bastion Host                                                        |
|----------------|-----------------------------------------------------------------------|-------------------------------------------------------------------------|
| Location       | Internal Network                                                      | Network perimeter (DMZ and public subnet)                               |
| Purpose        | Manage internal systems                                               | Grant controlled access to specific internal systems for external users |
| Security Focus | Centralized access control, simplified administration                 | Secure entry point, isolation of internal systems                       |
| Attack Surface | Higher (internal systems directly exposed if jump server compromised) | Lower (internal systems protected even if bastion host compromised)     |
{: caption="Table 2. Classic edge gateway Jump Server vs Bastion Host Matrix"}

# Resiliency design
{: #resiliency-design}

The following are resiliency design considerations for the Classic edge gateway pattern.

## {{site.data.keyword.dl_short}} resiliency
{: #DL-resiliency}

{{site.data.keyword.dl_short}} resiliency can be designed into a deployment to ensure availability. Most network resiliency is enhanced with multiple paths and dynamic routing.

The primary consideration is the level of risk a company is willing to take vs the financial cost budgeted for the deployment. Resiliency increases as you layer on services in a single point of presence (PoP) from a {{site.data.keyword.dl_short}} Dedicated connection to two {{site.data.keyword.dl_short}} connect connections.

1.  {{site.data.keyword.dl_short}} Dedicated: A single physical cross-connect, port, and switch.
2.  {{site.data.keyword.dl_short}} Connect: A single virtual SDN connection from an exchange provider.
3.  (2) {{site.data.keyword.dl_short}} Dedicated: Provides two physical ports and cross connects, which can be deployed on separate switches to ensure resiliency.
4.  (2) {{site.data.keyword.dl_short}} Connects: Provides two virtual SDN connections from an exchange provider, which can be provisioned on separate switches to ensure resiliency.

Also, consider whether there is an existing relationship with a telecommunication or cloud exchange provider that can be used.

{{site.data.keyword.dl_short}} Connect considerations include:

- Provided by a network cloud exchange provider.
- Provides SDN over physical hardware for a more cost-effective solution with a short deployment timeline.
- A second path can be provisioned by ordering a second {{site.data.keyword.dl_short}} connect port that is provisioned on a separate switch from the exchange provider for an extra layer of resiliency.

{{site.data.keyword.dl_short}} Dedicated considerations include:

- Provided by a telecommunication provider.
- Physical hardware – allows for a single circuit to a meet-me room with a physical cross-connect cable into a designated port. Traffic will not continue over the physical cable if the cable, hardware, or port is unavailable.
- A second path can be provisioned using the same or different telecommunications provider with a higher BGP prepend allowing for dynamic routing if there is a physical failure or scheduled maintenance activity.

Bidirectional Forwarding Detection (BFD) can be enabled on both {{site.data.keyword.dl_short}} Dedicated and {{site.data.keyword.dl_short}} Connect to detect network failures quicker, effectively reducing the time that it takes to fail over to a secondary path. Consider the cost implications when ordering a {{site.data.keyword.dl_short}}. The secondary path and the connection to a second region can be ordered as metered instead of unmetered if the primary connection is going to be used for most of the user traffic, which reduces the monthly fee and allows for traffic diversity if there is a primary circuit failure. Resilience considerations should include gateway or firewall appliances and GRE tunnel configurations to eliminate as many single points of failure as possible.

This pattern includes two {{site.data.keyword.dl_short}} connect circuits to meet the resiliency requires with a cost effective, flexible, and quick deployable solution. For more information, see [Models for diversity in {{site.data.keyword.dl_short}}](/docs/dl?topic=dl-models-for-diversity-and-redundancy-in-direct-link).

## Multi-Region Deployment
{: #multi-region}

In the multi-region deployment pattern, a second region is chosen within {{site.data.keyword.cloud_notm}} for disaster recovery.

![Illustrates a detailed network and component architecture for a
multi-region Classic edge gateway solution architecture](image/cross region.svg){: caption="Figure 1. Classic edge gateway Multi Region View" caption-side="bottom"}

1.  Optional network path is accomplished through site-to-site VPN terminated on Classic Gateway.
2.  Client network connectivity from on-premises using Direct Link.
3.  Gateway provides routing and security functions.
4.  Virtual Bastion Host for remote administrative access.
5.  GREa tunnel allows BYOIP to be advertised between Classic and on-premises.
6.  GREb tunnel allows BYOIP to be advertised between Classic environments in separate regions.
7.  GREc tunnel allows BYOIP to be advertised between Classic, VPC, and PowerVS
8.  Proxy Server as an intermediary between on-prem and cloud services.
9.  Cloud Internet Services (CIS) to enhance the security, performance, and reliability of internet-facing applications and websites.
10. Virtual Private Endpoint (VPE for VPC) as an alternative to SE & Proxy server allow access to cloud services over the private network.
11. DNS service for VPC.
12. VPC workloads.
13. PowerVS workspace and virtual servers.
14. Application load balancer.
15. TGW1 advertises & routes local traffic between on-premise and Classic.
16. TGW2 advertises & routes local traffic between Classic, VPC, and PowerVS.
17. TGW3 advertises & routes Global traffic between regions for VPC and PowerVS.

The multi-region deployment provides for a second location in a separate geographical region that is used as a disaster recovery location, which provides more resiliency if there is a regional failure.

While deploying the application or infrastructure across multiple regions offers several advantages, including increased fault tolerance, improved latency for geographically distributed users, and potential cost optimization, it also introduces extra complexities that need careful consideration.

Key factors to consider when planning a multi-region deployment:

- Cost: While multi-region deployments can improve service availability, they also incur extra costs associated with managing infrastructure and data across multiple regions. The financial impact includes costs for resources (compute, storage, network) in each region, as well as potential egress charges for data transfer between regions. Closely consider the required number of network paths to each region and if metered or unmetered billing best meets the business need.
- Data management: Deciding where and how to replicate your data across regions is crucial. Consider factors like data consistency, latency, and regulatory compliance.
- Latency: Replicating data across large geographic distances can introduce latency and can impact performance.
- Compliance: Data privacy regulations might restrict where data can be stored and replicated.
- Network connectivity: Ensuring reliable and secure public and private communication between resources in different regions is essential and involves considerations like:
    - Network latency: Higher latency between regions can slow down communication and impact user experience.
    - Bandwidth: Provision adequate bandwidth to avoid bottlenecks based on the amount of data that is transfering between regions and the time allotment of transfer window.
    - Security: Implement robust security measures to protect data transfer between regions, including encryption and access controls.
- Disaster recovery: A multi-region deployment can significantly enhance disaster recovery capabilities by providing geographically separated environments. It is important to plan for how to fail over and synchronize data to a secondary region if there is an outage. Creating accurate run books and regularly testing failover action plans are critical to a successful recovery to meet recovery point objectives (RPO) and recovery time objectives (RTO).
- Deployment and management: Deploying and managing infrastructure and applications across multiple regions can be complex. Consider tools and automation to streamline the processes. Also, factor in the increased operational overhead associated with managing resources in different regions.

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

# Architecture Decisions
{: #architecture-decisions}

## Architecture decisions for compute
{: #AD-compute}

The following are compute architecture decisions for the Classic edge gateway pattern.

| Architecture decision | Requirement                                                            | Options                                 | Decision   | Rationale                                    |
|---------------------------|----------------------------------------------------------------------------|---------------------------------------------|----------------|--------------------------------------------------|
| Compute: Bastion host     | Secure connection to manage internal systems and centralize access control | \*\*·\*\*Bare Metal \*\*·\*\*Virtual Server | Virtual Server | flexible compute resources to meet compute needs |
| Compute: Proxy server     | Provide access to service endpoints from IPs not assigned by {{site.data.keyword.cloud_notm}}     | \*\*·\*\*Bare Metal \*\*·\*\*Virtual Server | Virtual Server | flexible compute resources to meet compute needs |
{: caption="Table 1. Classic edge gateway compute architecture decisions"}

## Architecture decisions for network
{: #AD-network}

The following are network architecture decisions for the Classic edge gateway pattern.

### Architecture decisions for enterprise connectivity
{: #AD-connectivity}

| Architecture decision | Requirement                                                                                | Options                                                                                   | Decision                                     | Rationale                                                                                       |
|---------------------------|------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Management connectivity   | Provide secure, encrypted connectivity to the cloud’s private network for management purposes. | \*\*·\*\*SSL VPN \*\*·\*\*IPsec VPN \*\*·\*\*Site-to-Site VPN on Gateway appliance in Classic | Site-to-Site VPN on Gateway appliance in Classic | Secure and suitable for production-level performance                                                |
| Enterprise connectivity   | Provide connectivity between client enterprise and {{site.data.keyword.cloud_notm}}.                                  | \*\*·\*\*Direct Link Connect \*\*·\*\*Direct Link Dedicated                                   | Direct Link Connect                              | cost effective, quicker deployment time, and supports hybrid and multi-cloud deployment strategies. |
{: caption="Table 1. Classic edge gateway network enterprise connectivity architecture decisions"}

### Architecture decisions for BYOIP and Edge Gateways
{: #AD-edge-gateway}

| Architecture decision          | Requirement                                                                          | Options                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Decision                                                                                                               | Rationale                        |
|------------------------------------|------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| Bring Your Own IP (BYOIP) approach | Provide capability for BYOIP to {{site.data.keyword.cloud_notm}}.                                               | Generic Routing Encapsulation (GRE) tunnel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | GRE Tunnel                                                                                                                 | Allows BYOIP routes to be advertised |
| Edge Gateways                      | Capability to provide edge routing services, firewall and tunnel (VPN, GRE) termination. | [Gateway Appliance in Classic](/docs/gateway-appliance?topic=gateway-appliance-getting-started-ga)   \n - [Juniper vSRX](/docs/vsrx?topic=vsrx-getting-started)  \n - [Virtual Router Appliance](/docs/virtual-router-appliance?topic=virtual-router-appliance-getting-started-vra)  \n - [FortiGate FSA 10 Gbps](/docs/fortigate-10g?topic=fortigate-10g-getting-started)  \n - [FortiGate vFSA](/docs/vfsa?topic=vfsa-getting-started)   \n - Bring Your Own Gateway ([BYOG](/docs/gateway-appliance?topic=gateway-appliance-order-byoa))  \n Bare Metal (including Checkpoint, Cisco, Palo Alto) | Select based on required [features](/docs/vsrx?topic=vsrx-exploring-firewalls) and client preferences | Client preference                    |
{: caption="Table 2. Classic edge gateway network BYOIP and Edge Gateway architecture decisions"}

### Architecture decisions for network segmentation and isolation
{: #AD-segmentation}

| Architecture decision                    | Requirement                                        | Options                             | Decision                            | Rationale                                 |
|----------------------------------------------|--------------------------------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------------|
| Network segmentation and isolation (classic) | Ability to provide network isolation across workloads. | VLANs, subnets, and security groups     | VLANs, subnets, and security groups     | Allows for segmentation and network isolation |
| Network segmentation and isolation (VPC)     | Ability to provide network isolation across workloads. | VPCs, subnets, ACLs and security groups | VPCs, subnets, ACLs and security groups | Allows for segmentation and network isolation |
{: caption="Table 3. Classic edge gateway network segmentation and isolation architecture decisions"}

### Architecture decisions for cloud native connectivity
{: #AD-cloud-native}

| Architecture decision                     | Requirement                             | Options                                                                      | Decision                    | Rationale                                                                                  |
|-----------------------------------------------|---------------------------------------------|----------------------------------------------------------------------------------|---------------------------------|------------------------------------------------------------------------------------------------|
| Cloud Native Connectivity (to cloud services) | Provide secure connection to Cloud Services | \*\*·\*\*Private Cloud Service endpoints \*\*·\*\*Public Cloud Service Endpoints | Private Cloud Service endpoints | Provides private connectivity to cloud services offering enhanced security and cost efficiency |
{: caption="Table 4. Classic edge gateway network cloud native connectivity architecture decisions"}

### Architecture decisions for load balancing
{: #AD-load-balancing}

| Architecture decision       | Requirement                                                                                                            | Options                                                                                                                      | Decision                  | Rationale                                                                                                  |
|---------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|-------------------------------|----------------------------------------------------------------------------------------------------------------|
| Global Load balancing           | Load balancing over the public network across two regions in the event of an outage (DR) for failover to the other region. | \*\*·\*\*Cloud Internet Service (CIS) \*\*·\*\*DNS services                                                                      | Cloud internet Services (CIS) | Provides a cost-effective solution while offering additional security features                                 |
| Load balancing (public/private) | Load balancing workloads across multiple workload instances or zones over the public network.                              | IBM Cloud Load Balancer Citrix Netscaler VPX Network Load Balancer (NLB) Application Load Balancer (ALB) F5 Big-IP Load Balancer | Application Load Balancer     | Cost effectively provides a wide range of layer 7 load balancing functions for both public and private traffic |
{: caption="Table 5. Classic edge gateway network load balancing architecture decisions"}

### Architecture decisions for domain name system
{: #AD-DNS}

| Architecture decision | Requirement                                                                                 | Options                                                                                                                                                  | Decision | Rationale                                                   |
|---------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|-----------------------------------------------------------------|
| Public DNS                | Provide DNS resolution to support the use of hostnames instead of IP addresses for applications | - DNS via cloud portal \*\*·\*\*Cloud Internet Services (CIS) \*\*·\*\*Third Party DNS provider \*\*·\*\*Custom DNS on Classic VSI \*\*·\*\*DNS services | DNS Services | Cost effective and reliable                                     |
| Private DNS               | Provide DNS resolution within {{site.data.keyword.cloud_notm}}'s private network                                       | \*\*·\*\*Custom DNS on VSI \*\*·\*\*DNS on Gateway appliance \*\*·\*\*DNS services in VPC                                                                    | DNS services | Cost effective, reliable, and supports complex DNS requirements |
{: caption="Table 6. Classic edge gateway network Domain Name System architecture decisions"}

# Architecture decisions for security
{: #AD-security}

The following are security architecture decisions for the Classic edge gateway pattern.

## Architecture decisions for identity and access management
{: #AD-IAM}

| Architecture decision               | Requirement                                                                                                                                                                                                  | Options                                                                                                          | Decision                         | Rationale                                                                                                                                                                                                                                                                                   |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Identity Access & Role Management (IAM) | Securely authenticate users for platform services and control access to resources consistently across {{site.data.keyword.cloud_notm}}                                                                                                  | Cloud Identity and Access Management                                                                                 | Cloud Identity and Access Management | Use IAM access policies to assign users, service IDs, and trusted profiles access to resources within the {{site.data.keyword.cloud_notm}} account.                                                                                                                                                                    |
| Privileged Access Management            | Ensure that all operator actions are run securely through a bastion host Implement session recording to track all activities and note any potential threats Manage access to resources and track commands issued | Bring Your Own bastion host Jump server Bring Your Own bastion host with Privileged Access Management (PAM) software | Bring Your Own Bastion host          | The bastion host or jump server is a Virtual Server instance that is provisioned through SSH over a private network to securely access resources within the {{site.data.keyword.cloud_notm}} private network. Using PAM software is recommended when session recording, tracking, and managing all access is required. |
{: caption="Table 1. Classic edge gateway security identity and access management architecture decisions"}

# Architecture decisions for resiliency
{: #AD-resiliency}

The following are resiliency architecture decisions for the Classic edge gateway pattern.

## Architecture decisions for Network Resiliency
{: #AD-network-resiliency}

| Architecture decision           | Requirement                                                                                                       | Options                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Decision                                                 | Rationale                                                                                                                                                                   |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| High Availability Network path      | Ensure availability of resources if outages occurs. Support SLA targets for availability                              | Single {{site.data.keyword.dl_full_notm}} Dedicated Single {{site.data.keyword.dl_full_notm}} Connect Two {{site.data.keyword.dl_full_notm}} Dedicated Two {{site.data.keyword.dl_full_notm}} Connect                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Two {{site.data.keyword.dl_full_notm}} Connect                            | Two {{site.data.keyword.dl_full_notm}} Connect provides a cost effective resilient solution with a short deployment interval and is flexible to meet both hybrid and multi-cloud strategies. |
| High Availability Gateway Appliance | Ensure availability of infrastructure resources if outages occur. Support SLA targets for infrastructure availability | [Gateway Appliance in Classic](/docs/gateway-appliance?topic=gateway-appliance-getting-started-ga)  \n - [Juniper vSRX](/docs/vsrx?topic=vsrx-getting-started)  \n - [Virtual Router Appliance](/docs/virtual-router-appliance?topic=virtual-router-appliance-getting-started-vra)  \n - [FortiGate FSA 10 Gbps](/docs/fortigate-10g?topic=fortigate-10g-getting-started)  \n - [FortiGate vFSA](/docs/vfsa?topic=vfsa-getting-started)  \n - Bring Your Own Gateway ([BYOG](/docs/gateway-appliance?topic=gateway-appliance-order-byoa))  \n Bare Metal (including Checkpoint, Cisco, Palo Alto) | Deploy Gateway Appliance of choice in high availability pair | Ensures if one appliance is unavailable access is still available through remaining gateway appliance.                                                                          |
| High availability path detection    | Ensure the quickest traffic path recovery                                                                             | Bidirectional Forwarding Detection (BFD)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Bidirectional Forwarding Detection (BFD)                     | Provides a much faster way of detecting link failures compared to the built-in mechanisms within routing protocols.                                                             |
{: caption="Table 1. Classic edge gateway resiliency architecture decisions"}

## Architecture decisions for disaster recovery
{: #AD-DR}

| Architecture decision | Requirement                                                                                                                            | Options                                                                                                                                                | Decision                         | Rationale                                                                                                       |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| Disaster recovery network | Network disaster recovery capability in secondary region to meet recovery time objective (RTO) recovery point objective (RPO) requirements | \*\*·\*\*Single {{site.data.keyword.dl_full_notm}} Dedicated Single {{site.data.keyword.dl_full_notm}} Connect Two {{site.data.keyword.dl_full_notm}} Dedicated Two {{site.data.keyword.dl_full_notm}} Connect | Single {{site.data.keyword.dl_full_notm}} Connect | Provides a cost effective and flexible connection into a second region, with metered and unmetered billing options. |
{: caption="Table 2. Classic edge gateway disaster recovery architecture decisions"}

# Architecture decisions for service management
{: #AD-servicemgmt}

The following are service management architecture decisions for the Classic edge gateway pattern.

## Architecture decisions for monitoring
{: #AD-monitoring}

| Architecture decision                                   | Requirement                                                                                          | Options                                                                          | Decision         | Rationale                                                        |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|----------------------|----------------------------------------------------------------------|
| Operational monitoring of cloud infrastructure and services | Monitor system health to detect issues that might impact the availability of the system and application. | {{site.data.keyword.cloud_notm}} Health Dashboard IBM Cloud Monitoring (VPC) Bring Your Own monitoring tool | IBM Cloud Monitoring | IBM Cloud Monitoring provides a robust cloud native monitoring tool. |
{: caption="Table 1. Classic edge gateway service management monitoring architecture decisions"}

## Architecture decisions for logging
{: #AD-logging}

| Architecture decision                           | Requirement                                                                                             | Options                                                           | Decision           | Rationale                                                                                              |
|-----------------------------------------------------|-------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|------------------------|------------------------------------------------------------------------------------------------------------|
| Log monitoring of cloud infrastructure and services | Monitor operational logs to detect issues that might impact the availability of the system and application. | IBM Cloud Log Analysis VPC IBM Cloud Logs Bring Your Own logging tool | IBM Cloud Log Analysis | IBM Cloud Logging provides a robust cloud native logging tool. IBM Cloud Logs is preferred when available. |
{: caption="Table 2. Classic edge gateway service management logging architecture decisions"}

## Architecture decisions for auditing
{: #AD-auditing}

| Architecture decision | Requirement                                                                                | Options                                                                              | Decision               | Rationale                                                                                                     |
|---------------------------|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------|-------------------------------------------------------------------------------------------------------------------|
| Audit Logging             | Monitor audit logs to track changes to cloud resources and detect potential security problems. | IBM Cloud Activity Tracker (VPC) IBM Cloud Logs Bring Your Own Activity Tracker software | IBM Cloud Activity Tracker | IBM Cloud Activity Tracker provides a robust cloud native audit tool. IBM Cloud Logs is preferred when available. |
{: caption="Table 3. Classic edge gateway service management auditing architecture decisions"}
