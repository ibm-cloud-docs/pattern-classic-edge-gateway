---

copyright:
  years: 2024, 2024
lastupdated: "2024-06-19"

subcollection: pattern-classic-edge-gateway

keywords: network, VPC, PowerVS, Classic edge gateway

---

{{site.data.keyword.attribute-definition-list}}

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
- High availability: Implement redundant connections or failover mechanisms to ensure connectivity even during outages.
- Compliance: If your organization operates under specific compliance regulations, ensure that the VPN solution meets those requirements.
- Management: Managing the VPN infrastructure can require dedicated personnel or managed services, adding to the cost.
- Technical expertise: Implementing and maintaining a VPN might require technical expertise. Assess your internal IT capabilities or consider external support options.

{{site.data.keyword.IBM_notm}} classic data centers offer three options for implementing a virtual private network connection from a remote site into {{site.data.keyword.cloud_notm}}, including SSL VPN, IPsec VPN, and VPN gateway appliance on classic. This pattern supports VPN gateway appliance on classic.

For more information, see [Virtual Private Network options](/docs/iaas-vpn/set-up-ipsec-vpn.html?topic=iaas-vpn-getting-started#use-case-scenarios) available in {{site.data.keyword.cloud_notm}}.

## Gateway appliance and firewall
{: #gateway-appliance}

Choosing the right firewall is crucial for safeguarding your network. Review the key consideration guidance.

- Basic versus next-generation: Basic firewalls offer packet filtering, while next–generation firewall (NGFW) provides deeper inspection, application control, intrusion prevention, and more. Choose based on your security needs and complexity.
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
- Single versus high availability: Consider single points of failure and service level requirements.

{{site.data.keyword.IBM_notm}} classic data centers support four gateway appliance and firewall options including Juniper vSRX, Virtual Router Appliance, FortiGate (FAS 10 Gbps and vFSA), and bring your own gateway appliance (BYOG - Checkpoint, Cisco, and Palo Alto).

This pattern supports personal choice based on appliance functional requirements and operational expertise.
{: note}

Explore and compare [gateway options](/docs/fortigate-10g?topic=fortigate-10g-exploring-firewalls) available in {{site.data.keyword.cloud_notm}}.

## Generic Routing Encapsulation (GRE) tunnels
{: #GRE}

GRE tunnels support the Bring Your Own IP (BYOIP) requirement.

![Illustrates the details of GRE for a Classic edge gateway solution architecture](image/GRE.svg){: caption="Figure 1. Classic edge gateway GRE Encapsulation" caption-side="bottom"}

1.  Client network connectivity from on-premises is accomplished through Direct Link access.
2.  A gateway is deployed in classic, which provides routing and security functions.
3.  A GRE tunnel is created between the gateway and a customer router.
4.  192.168.xx.xx (BYOIP) packets are encapsulated by 10.1.x.x, {{site.data.keyword.IBM_notm}} assigned IP, which is routed through the BCR over the GRE and unencapsulated on the other side.

{{site.data.keyword.BluDirectLink}} does not offer BYOIP on the private network. The {{site.data.keyword.Bluemix_notm}} backbone advertises the customer’s available routes that are assigned by {{site.data.keyword.IBM_notm}} to their remote networks. Traffic with a destination IP address that is not assigned by {{site.data.keyword.cloud_notm}} (10.0.0.0/8) is dropped by {{site.data.keyword.cloud_notm}}. Traffic can be encapsulated between the remote client network and the {{site.data.keyword.cloud_notm}} network for nonassigned {{site.data.keyword.cloud_notm}} addresses by establishing GRE tunnels between the gateway and the transit gateway. This allows non-{{site.data.keyword.IBM_notm}} assigned IP addresses to be passed back to the on-premises environment.

A second GRE is required between the classic gateway and the transit gateway when nonassigned {{site.data.keyword.cloud_notm}} addresses are used in the VPC or {{site.data.keyword.powerSys_notm}} environment.

A third GRE is used to share nonassigned {{site.data.keyword.cloud_notm}} addresses between classic gateways in separate regions. When resiliency is required, GREs can be configured on two devices in a high availability pair to eliminate single points of failure. For more information, see [High availability and disaster recovery](/docs/transit-gateway?topic=transit-gateway-ha-dr#high-availability) and [{{site.data.keyword.cloud_notm}} public and private IP ranges](/docs/cloud-infrastructure?topic=cloud-infrastructure-ibm-cloud-ip-ranges).

When you configure a GRE connection on a transit gateway, you must specify the availability zone. For a robust high availability solution, configure multiple GRE connections that use different availability zones. Learn how to create a [redundant GRE](/docs/transit-gateway?topic=transit-gateway-redundant-gre-connection&interface=ui).
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
- Organizations that need fine-grained control over routing and traffic management. {{site.data.keyword.dl_short}} Dedicated is ideal for moderate to high-bandwidth needs.

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

## Load balancing
{: #load-balancing}

There are three load balancing options load balancing options that differ based on your use case.

### Local Load balancing
{: #local-load-balancing}

Load balancing is the process of distributing network traffic efficiently among multiple servers within a single region to optimize and ensure application availability to meet specific service level requirements.

Considerations include:

- Layer-4 load balancing for HTTP, HTTPS, and TCP traffic: Distributes traffic based on IP addresses, ports, and basic metrics like packet arrival time. Works well for simple traffic but lacks deeper application understanding.
- Advanced Layer-7 load balancing for HTTP and HTTPS traffic: Analyzes the deeper aspects like URLs, headers, and user data. Offers granular routing based on the content, user needs, and server capabilities. Ideal for complex applications.
- Server health checks: Regularly ping servers to ensure that they are healthy and can handle traffic. Unhealthy servers are removed from the pool until they recover.
- SSL offload: Takes over the decryption and encryption of HTTPS traffic, freeing up server resources for application processing. Improves performance and security.
- Public internet-facing to private load balancing: Makes the internal applications accessible from the internet through a public IP address and NAT gateway. Provides an extra layer of security by keeping application servers hidden on a private network.
- Public to public internet-facing load balancing: Distributes traffic among multiple public-facing servers for high availability and scalability of websites or services.
- Private internal load balancing: Distributes traffic among internal servers on a private network. Improves performance and scalability for internal applications without internet exposure.

{{site.data.keyword.cloud_notm}} offers two load balancer options for classic infrastructure, which include {{site.data.keyword.loadbalancer_full}} and {{site.data.keyword.vpx_full}} appliance. Review the following documentation to learn more about local load balancing: 

* [Load balancer feature options](/docs/citrix-netscaler-vpx?topic=citrix-netscaler-vpx-explore).
* [{{site.data.keyword.loadbalancer_full}}](/docs/loadbalancer-service?topic=loadbalancer-service-about-ibm-cloud-load-balancer)
* [{{site.data.keyword.vpx_full}} Load Balancer](/docs/citrix-netscaler-vpx?topic=citrix-netscaler-vpx-about-citrix-netscaler-vpx)

{{site.data.keyword.vpc_full}} (VPC) offers three load balancer options, which include: {{site.data.keyword.nlb_full}}, {{site.data.keyword.alb_full}}, and the F5 BIG-IP Virtual Edition for VPC.

* Explore [VPC load balancer options](/docs/vpc?topic=vpc-nlb-vs-elb#load-balancer-types).
* Refer to the VPC [load balancer comparison chart](/docs/vpc?topic=vpc-nlb-vs-elb#lb-comparison-chart) for more information.
* Learn more about the [{{site.data.keyword.alb_full}}](/docs/vpc?topic=vpc-load-balancers-about&interface=ui).
* Learn more about the [{{site.data.keyword.nlb_full}}](/docs/vpc?topic=vpc-nlb-vs-elb#load-balancer-nlb).
* Learn more about the [F5 BIG-IP Virtual Edition for VPC](https://clouddocs.f5.com/cloud/public/v1/ibm_index.html){: external}.

This pattern uses the {{site.data.keyword.cloud_notm}} {{site.data.keyword.alb_full}} (ALB) to distribute traffic among multiple server instances within the region.
{: note}

### Global load balancing
{: #gslb}

Global server load balancing (GSLB) is a technique for distributing internet traffic across geographically dispersed servers. It aims to optimize user experience and application performance by directing users to the nearest or most appropriate server based on various factors like latency, server load, and user location. GSLB is a valuable component of a disaster recovery strategy.

If global server load balancing is required, {{site.data.keyword.cloud_notm}} offers a global load balancer service through {{site.data.keyword.cis_full_notm}}, which routes traffic to servers across multiple geographic locations to ensure application availability.

Other third-party providers such as Akamai, network appliances from vendors like F5 and Citrix, and Domain Name Service can also be used to meet the global load balancing requirement.

When a user tries to access a website or application, the request goes to the GSLB device. The GSLB device uses various algorithms and real-time data to determine the best server to route the request to. Factors that are considered include:

- User location: Sending users to the closest server minimizes latency.
- Server load: Distributing traffic among available servers prevents overloading any single server.
- Server health: Avoiding unresponsive or overloaded servers.
- Application-specific criteria: Specific services might require routing based on user type, content availability, and so on.

For more information, see [Global server load balancing and other features of {{site.data.keyword.cis_full_notm}}](/docs/cis?topic=cis-about-ibm-cloud-internet-services-cis).

## Private access to cloud services
{: #cloud-services}

With the presence of both the classic infrastructure and Virtual Private Cloud (VPC) environments, there are two paths to access cloud services privately. Moving these workloads from the public network to the private network offers two advantages:

- Enhanced security: Cloud services are no longer served on an internet routable IP address.
- Cost-effective: The private network does not incur billable or metered bandwidth charges.

### Service endpoints
{: #service-endpoints}

With {{site.data.keyword.cloud_notm}} service endpoints, you can connect to {{site.data.keyword.cloud_notm}} services over the {{site.data.keyword.cloud_notm}} private network instead of the default public network.

In {{site.data.keyword.cloud_notm}} classic, virtual route forwarding (VRF) must be enabled on the account to move routing to a dedicated routing table. When VRF is enabled on the {{site.data.keyword.cloud_notm}} account, Private Service Endpoints can be created. Cloud service resources can then be accessed by the {{site.data.keyword.cloud_notm}} private network from an {{site.data.keyword.IBM_notm}} assigned IP address in the classic account. For more information, see [{{site.data.keyword.cloud_notm}} VRF](/docs/dl?topic=dl-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud).

When the source address is not an {{site.data.keyword.IBM_notm}} assigned IP address, a proxy server in classic is used as an intermediary allowing access to cloud services.

Verify that [Private service endpoints](/docs/account?topic=account-vrf-service-endpoint&interface=ui) are available for the cloud services.

### Virtual Private Endpoint for VPC
{: #VPE}

{{site.data.keyword.cloud_notm}} {{site.data.keyword.vpe_full}} enables you to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choosing, which is allocated from a subnet within your VPC. This pattern uses the VPE for VPC to access cloud services privately. For more information, see [{{site.data.keyword.vpe_full}}](/docs/vpc?topic=vpc-about-vpe).

Verify that cloud services are VPE for VPC [enabled](/docs/vpc?topic=vpc-vpe-supported-services#vpe-enabled-supported-services).
{: note}

![Illustrates SE versus {{site.data.keyword.vpe_short}} for Classic edge gateway solution architecture](image/SE-vs-VPE.svg){: caption="Figure 2. Classic edge gateway service endpoint access" caption-side="bottom"}

## Cloud Internet Services (CIS)
{: #CIS}

{{site.data.keyword.cis_full_notm}} (CIS) provides global server load balancing, public domain name services, and public network security features. For more information, see [CIS](/docs/cis?topic=cis-about-ibm-cloud-internet-services-cis)

## Domain Name Services (DNS)
{: #DNS}

Domain Name Services (DNS) provides access to your systems and services through user-friendly domain names rather than IP addresses.

Key considerations are:

- Public versus private DNS name resolution: The ability to provide DNS-based access to users on the public Internet instead of internal DNS-based communication within your private environment.
- Integration with other existing DNS systems: Integrating your on-premises DNS service with your cloud environment.

{{site.data.keyword.cloud_notm}} provides a flexible approach to DNS name resolution.

- Public DNS name resolution: Converts human-friendly names into computer-readable addresses.
    - Use the [DNS interface](/docs/dns?topic=dns-how-to-use-the-dns-interface) in the {{site.data.keyword.cloud_notm}} portal to manage your zones and records. The domain can be hosted by any third party provider. Set the name server (NS) record to the provided {{site.data.keyword.IBM_notm}} name servers.
    - Use the DNS function provided with {{site.data.keyword.cis_full_notm}}. Domain name control must be delegated to {{site.data.keyword.cis_full_notm}} (CIS).
    - Use DNS services to provide public name resolution for on-premises, classic, VPC, and PowerVS resources by deploying [Custom Resolvers](/docs/dns-svcs?topic=dns-svcs-custom-resolver). For more information, see [{{site.data.keyword.dns_full_notm}}](/docs/dns-svcs?topic=dns-svcs-getting-started).
- Private DNS name resolution: Allows integration with your on-premises DNS server over private connectivity.
    - Configure your own DNS services in the classic environment, typical options are:
        - Install your own custom DNS service on a virtual server.
        - Configure DNS on your gateway device.
    - Use {{site.data.keyword.dns_full_notm}} for VPC.
        By using this service, you can provide versatile private name resolution between classic, on-premises, and VPC resources by deploying [Custom Resolvers](/docs/dns-svcs?topic=dns-svcs-custom-resolver). For more information, see [{{site.data.keyword.dns_full_notm}}](/docs/dns-svcs?topic=dns-svcs-getting-started).

        This pattern uses {{site.data.keyword.dns_full_notm}} to provide FQDN resolution for on-premises, Classic, VPC, and PowerVS workload environments.
        {: note}
