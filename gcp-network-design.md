# GCP Network Design: Things to Consider Before Starting GCP Network Design

# Table of Contents
- [GCP Virtual Networking](#)
- [GCP Networking Services and Components at a Glance](#)
- [VPC Main Components](#)
- [Network Service Tiers](#)
- [A Virtual Private Cloud (VPC) Network](#)
- [Levels of Google Cloud Networking](#)
- [Firewalls](#)
- [Creating a custom network](#)
- [Adding firewall rules](#)
- [VPC Network Example:](#)

## `GCP Virtual Networking`
- Networking services provide connectivity between cloud-based VMs, on-premises servers, and other cloud services. Google Cloud treats networking as a global feature that spans all its services.
> GCP networking is based on Google’s Andromeda architecture, which allows cloud administrators to create and use software-defined networking elements, such as firewalls, routing tables, and VMs.

## `GCP Networking Services and Components at a Glance`

<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/network-1.png?raw=true" alt="Sublime's custom image"/>
</p>

## `VPC Main Components`
<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/network-2.png?raw=true" alt="Sublime's custom image"/>
</p>

## `Network Service Tiers`
GCP offers two Network Service Tiers

- **Standard Tier, and**
  - The Standard Networking Tier is a lower-cost networking capability with network performance comparable to other public clouds (according to Google’s documentation)
- **Premium Tier (the default)**
  - The default, Premium Tier, leverages Google’s global network, which brings high throughput and reliability.




## `A Virtual Private Cloud (VPC) Network`
Virtual Private Cloud Network or simply network is a virtual version of a physical network. In Google Cloud Networking, networks provide data connections into and out of cloud resources – mostly Compute Engine instances. 
> Securing the Networks is critical to securing the data and controlling access to the resources.

- Google Cloud Networking achieves flexible and logical isolation of unrelated resources through its different levels.



## Levels of Google Cloud Networking

### Projects
- Projects are known to be the outermost container and are used to group resources that share the same trust boundaries. A lot of developers map Projects to teams since every Project has its own access policy (IAM) and member list. Projects serve as a collector of billing and quota details reflecting resource consumption as well. Projects comprise of Networks which contain Subnetworks, Firewall rules, and Routes.

### Networks
- Networks directly connect our resources to each other and to the outside world. Networks which use Firewalls house the access policies for incoming and outgoing connections as well. Networks could be Global – which offers horizontal scalability across multiple Regions or Regional – which low-latency within a single Region. Virtual Private Cloud networks consist of one or more IP range partitions called subnetworks or subnets. Each subnet or subnetwork is associated with a region. VPC networks do not have any IP ranges associated with them. IP ranges are defined for the subnetworks. A network must have at least one subnet then only we can use it.

### Subnetworks
- Subnetworks allow us to group related resources (Compute Engine instances) into RFC1918 private address spaces. Subnetworks are regional resources. Each subnetwork defines a range of IP addresses.

## Firewalls


## Creating a custom network


## Adding firewall rules


## VPC Network Example:


References:
- https://www.webagesolutions.com/blog/google-cloud-virtual-networking
- https://tudip.com/blog-post/google-cloud-networking/













https://medium.com/google-cloud/gcp-network-design-part-1-things-to-consider-before-starting-gcp-network-design-ef1b3191c2f1
