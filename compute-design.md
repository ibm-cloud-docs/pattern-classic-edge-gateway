---

copyright:
  years: 2024, 2024
lastupdated: "2024-06-17"

subcollection: pattern-classic-edge-gateway

keywords: network, VPC, PowerVS, Classic edge gateway

---

{{site.data.keyword.attribute-definition-list}}

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

Infrastructure requirements for Linux and Windows jump servers and bastion hosts can vary based on factors such as the number of concurrent users, the specific use case, and the applications or services that run on the server. A typical jump server with 8 CPU and 16 GB of RAM, can support 25 concurrent sessions of any type (100 serial-over-LAN sessions, 200 Telnet, or SSH sessions). More sessions can be supported by larger server specifications.

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
