---

copyright:
  years: 2024, 2024
lastupdated: "2024-06-07"

subcollection: pattern-classic-edge-gateway

keywords: network, VPC, PowerVS, Classic edge gateway

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for network
{: #AD-network}

The following are network architecture decisions for the Classic edge gateway pattern.

## Architecture decisions for enterprise connectivity
{: #AD-connectivity}

| Architecture decision | Requirement                                                                                | Options                                                                                   | Decision                                     | Rationale                                                                                       |
|---------------------------|------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Management connectivity   | Provide secure, encrypted connectivity to the cloud’s private network for management purposes. | \*\*·\*\*SSL VPN \*\*·\*\*IPsec VPN \*\*·\*\*Site-to-Site VPN on Gateway appliance in Classic | Site-to-Site VPN on Gateway appliance in Classic | Secure and suitable for production-level performance                                                |
| Enterprise connectivity   | Provide connectivity between client enterprise and {{site.data.keyword.cloud_notm}}.                                  | \*\*·\*\*Direct Link Connect \*\*·\*\*Direct Link Dedicated                                   | Direct Link Connect                              | cost effective, quicker deployment time, and supports hybrid and multi-cloud deployment strategies. |
{: caption="Table 1. Classic edge gateway network enterprise connectivity architecture decisions"}

## Architecture decisions for BYOIP and Edge Gateways
{: #AD-edge-gateway}

| Architecture decision          | Requirement                                                                          | Options                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Decision                                                                                                               | Rationale                        |
|------------------------------------|------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| Bring Your Own IP (BYOIP) approach | Provide capability for BYOIP to {{site.data.keyword.cloud_notm}}.                                               | Generic Routing Encapsulation (GRE) tunnel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | GRE Tunnel                                                                                                                 | Allows BYOIP routes to be advertised |
| Edge Gateways                      | Capability to provide edge routing services, firewall and tunnel (VPN, GRE) termination. | [Gateway Appliance in Classic](/docs/gateway-appliance?topic=gateway-appliance-getting-started-ga)   \n - [Juniper vSRX](/docs/vsrx?topic=vsrx-getting-started)  \n - [Virtual Router Appliance](/docs/virtual-router-appliance?topic=virtual-router-appliance-getting-started-vra)  \n - [FortiGate FSA 10 Gbps](/docs/fortigate-10g?topic=fortigate-10g-getting-started)  \n - [FortiGate vFSA](/docs/vfsa?topic=vfsa-getting-started)   \n - Bring Your Own Gateway ([BYOG](/docs/gateway-appliance?topic=gateway-appliance-order-byoa))  \n Bare Metal (including Checkpoint, Cisco, Palo Alto) | Select based on required [features](/docs/vsrx?topic=vsrx-exploring-firewalls) and client preferences | Client preference                    |
{: caption="Table 2. Classic edge gateway network BYOIP and Edge Gateway architecture decisions"}

## Architecture decisions for network segmentation and isolation
{: #AD-segmentation}

| Architecture decision                    | Requirement                                        | Options                             | Decision                            | Rationale                                 |
|----------------------------------------------|--------------------------------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------------|
| Network segmentation and isolation (classic) | Ability to provide network isolation across workloads. | VLANs, subnets, and security groups     | VLANs, subnets, and security groups     | Allows for segmentation and network isolation |
| Network segmentation and isolation (VPC)     | Ability to provide network isolation across workloads. | VPCs, subnets, ACLs and security groups | VPCs, subnets, ACLs and security groups | Allows for segmentation and network isolation |
{: caption="Table 3. Classic edge gateway network segmentation and isolation architecture decisions"}

## Architecture decisions for cloud native connectivity
{: #AD-cloud-native}

| Architecture decision                     | Requirement                             | Options                                                                      | Decision                    | Rationale                                                                                  |
|-----------------------------------------------|---------------------------------------------|----------------------------------------------------------------------------------|---------------------------------|------------------------------------------------------------------------------------------------|
| Cloud Native Connectivity (to cloud services) | Provide secure connection to Cloud Services | \*\*·\*\*Private Cloud Service endpoints \*\*·\*\*Public Cloud Service Endpoints | Private Cloud Service endpoints | Provides private connectivity to cloud services offering enhanced security and cost efficiency |
{: caption="Table 4. Classic edge gateway network cloud native connectivity architecture decisions"}

## Architecture decisions for load balancing
{: #AD-load-balancing}

| Architecture decision       | Requirement                                                                                                            | Options                                                                                                                      | Decision                  | Rationale                                                                                                  |
|---------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|-------------------------------|----------------------------------------------------------------------------------------------------------------|
| Global Load balancing           | Load balancing over the public network across two regions in the event of an outage (DR) for failover to the other region. | \*\*·\*\*Cloud Internet Service (CIS) \*\*·\*\*DNS services                                                                      | Cloud internet Services (CIS) | Provides a cost-effective solution while offering additional security features                                 |
| Load balancing (public/private) | Load balancing workloads across multiple workload instances or zones over the public network.                              | IBM Cloud Load Balancer Citrix Netscaler VPX Network Load Balancer (NLB) Application Load Balancer (ALB) F5 Big-IP Load Balancer | Application Load Balancer     | Cost effectively provides a wide range of layer 7 load balancing functions for both public and private traffic |
{: caption="Table 5. Classic edge gateway network load balancing architecture decisions"}

## Architecture decisions for domain name system
{: #AD-DNS}

| Architecture decision | Requirement                                                                                 | Options                                                                                                                                                  | Decision | Rationale                                                   |
|---------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|-----------------------------------------------------------------|
| Public DNS                | Provide DNS resolution to support the use of hostnames instead of IP addresses for applications | - DNS via cloud portal \*\*·\*\*Cloud Internet Services (CIS) \*\*·\*\*Third Party DNS provider \*\*·\*\*Custom DNS on Classic VSI \*\*·\*\*DNS services | DNS Services | Cost effective and reliable                                     |
| Private DNS               | Provide DNS resolution within {{site.data.keyword.cloud_notm}}'s private network                                       | \*\*·\*\*Custom DNS on VSI \*\*·\*\*DNS on Gateway appliance \*\*·\*\*DNS services in VPC                                                                    | DNS services | Cost effective, reliable, and supports complex DNS requirements |
{: caption="Table 6. Classic edge gateway network Domain Name System architecture decisions"}
