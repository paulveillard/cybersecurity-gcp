# GCP Network Design: Things to Consider Before Starting GCP Network Design

# Table of Contents
- [GCP Virtual Networking](#)
- [GCP Networking Services and Components at a Glance](#)
- [VPC Main Components](#)
- [Network Service Tiers](#)
- [A Virtual Private Cloud (VPC) Network](#)
- [6 - Levels of Google Cloud Networking](#)
  - 6.2.2 Network and Subnet Terminology
  - 6.2.3 CIDR Network Notation
  - 6.2.4 A Basic Cross-Region VPC Network
  - 6.2.5 Legacy Networks
  - 6.2.6 Listing Networks
  - 6.2.5 Projects and VPC Relationship
  - 6.2.6 VPC Specifications
  - 6.2.5 Types of VPC Networks
  - 6.2.6 VPC Specifications 
- [Firewalls](#)
- [Creating a custom network](#)
- [Adding firewall rules](#)
- [VPC Network Example:](#)

## `1 - GCP Virtual Networking`
- Networking services provide connectivity between cloud-based VMs, on-premises servers, and other cloud services. Google Cloud treats networking as a global feature that spans all its services.
> GCP networking is based on Google’s Andromeda architecture, which allows cloud administrators to create and use software-defined networking elements, such as firewalls, routing tables, and VMs.

## `2 - GCP Networking Services and Components at a Glance`

<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/network-1.png?raw=true" alt="Sublime's custom image"/>
</p>

## `3 - VPC Main Components`
<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/network-2.png?raw=true" alt="Sublime's custom image"/>
</p>

## `4 - Network Service Tiers`
GCP offers two Network Service Tiers

- **Standard Tier, and**
  - The Standard Networking Tier is a lower-cost networking capability with network performance comparable to other public clouds (according to Google’s documentation)
- **Premium Tier (the default)**
  - The default, Premium Tier, leverages Google’s global network, which brings high throughput and reliability.




## `5 - A Virtual Private Cloud (VPC) Network`
A **Virtual Private Cloud (VPC) network** is a virtualized layer on top of the physical network used by Google Cloud. 

- A VPC provides the following services:
  - Network connectivity services for your
    - Compute Engine VM instances,
      - Your VM instance can have more than one interface, each interface, however, must be connected to a different network
    - Google Kubernetes Engine (GKE) clusters,
    - App Engine flexible environment instances, and
    - Other Google Cloud products built with Compute Engine VMs
  - Native Internal TCP/UDP Load Balancing and proxy systems for Internal HTTP(S) Load Balancing
  - Connectivity to on-prem networks using Cloud VPN tunnels and Cloud Interconnect attachments


Virtual Private Cloud Network or simply network is a virtual version of a physical network. In Google Cloud Networking, networks provide data connections into and out of cloud resources – mostly Compute Engine instances. 
> Securing the Networks is critical to securing the data and controlling access to the resources.

- Google Cloud Networking achieves flexible and logical isolation of unrelated resources through its different levels.



## `6 - Levels of Google Cloud Networking`

### 6.1 Projects
- Projects are known to be the outermost container and are used to group resources that share the same trust boundaries. A lot of developers map Projects to teams since every Project has its own access policy (IAM) and member list. Projects serve as a collector of billing and quota details reflecting resource consumption as well. Projects comprise of Networks which contain Subnetworks, Firewall rules, and Routes.

### 6.2 Networks
- Networks directly connect our resources to each other and to the outside world. Networks which use Firewalls house the access policies for incoming and outgoing connections as well. Networks could be Global – which offers horizontal scalability across multiple Regions or Regional – which low-latency within a single Region. Virtual Private Cloud networks consist of one or more IP range partitions called subnetworks or subnets. Each subnet or subnetwork is associated with a region. VPC networks do not have any IP ranges associated with them. IP ranges are defined for the subnetworks. A network must have at least one subnet then only we can use it.

### 6.2.1 Subnetworks
- Subnetworks allow us to group related resources (Compute Engine instances) into RFC1918 private address spaces. Subnetworks are regional resources. Each subnetwork defines a range of IP addresses.

<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/subnetworks-1.png?raw=true" alt="Sublime's custom image"/>
</p>


- A Subnetwork can be in two modes:
   - Auto mode network
   - Custom mode network



### 6.2.2 Network and Subnet Terminology
### 6.2.3 CIDR Network Notation
### 6.2.4 A Basic Cross-Region VPC Network
### 6.2.5 Legacy Networks
### 6.2.6 Listing Networks
### 6.2.5 Projects and VPC Relationship
### 6.2.6 VPC Specifications 
### 6.2.5 Types of VPC Networks
### 6.2.6 VPC Specifications 

### 4.2.5 Considerations for Auto-mode VPC Networks

#### Auto mode network:

An auto mode network has one subnet per region, each with a predetermined IP range which fit within the 10.128.0.0/9 CIDR block. These subnets are created automatically when the auto mode network has created, and each subnet has the same name as the overall network. When any new GCP regions become available, new subnets for those regions are automatically added to the auto mode networks using an IP range from that block. We can add more subnets manually to auto mode networks in addition to the automatically created subnets.

### 4.2.6 Considerations for Custom-mode VPC Networks

#### Custom mode network:

A custom mode network has no subnets at creation giving us full control over subnet creation. In order to create an instance in a custom mode network, we must first create a subnetwork in that region and specify its IP range. A custom mode network has the possibilities of having zero, one, or many subnets per region.

It is possible to switch a network from auto mode to custom mode. But this conversion is one-way, custom mode networks cannot be changed to auto mode networks.

When a new project is created, a **default network** configuration provides each region with an auto mode network with pre-populated firewall rules.

We can create up to four additional networks in a project. Additional networks can be either auto subnet networks or custom subnet networks.

Each instance which is created within a subnetwork gets assigned to an IPv4 address from that subnetwork range.

> Note: Since the default Network allows relatively open access, it is a recommended best practice that you delete it. The default Network cannot be deleted unless another Network is present. Please make sure you delete all the firewall rules for the associated VPC before deleting the VPC network.



## `Firewalls`


## Creating a custom network


## Adding firewall rules


## VPC Network Example:


References:
- https://www.webagesolutions.com/blog/google-cloud-virtual-networking
- https://tudip.com/blog-post/google-cloud-networking/













https://medium.com/google-cloud/gcp-network-design-part-1-things-to-consider-before-starting-gcp-network-design-ef1b3191c2f1
