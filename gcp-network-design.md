# GCP Network Design: Things to Consider Before Starting GCP Network Design

# Table of Contents
- [1 - GCP Virtual Networking](#)
- [2 - GCP Networking Services and Components at a Glance](#)
- [3 - VPC Main Components](#)
- [4 - Network Service Tiers](#)
- [5 - A Virtual Private Cloud (VPC) Network](#)
- [6 - Levels of Google Cloud Networking](#)
  - [6.2.2 Network and Subnet Terminology](#)
  - [6.2.3 CIDR Network Notation](#)
  - [6.2.4 A Basic Cross-Region VPC Network](#)
  - [6.2.5 Legacy Networks](#)
  - [6.2.6 Listing Networks](#)
  - [6.2.5 Projects and VPC Relationship](#)
  - [6.2.6 VPC Specifications](#)
  - [6.2.5 Types of VPC Networks](#)
  - [6.2.6 VPC Specifications](#)
- [7 - Firewalls](#)
  - [7.5.1 Ingress (Inbound) Firewall Rules](#)
  - [7.5.2 Egress (Outbound) Rules](#)
  - [7.5.3 Authoring Firewall Rules](#)
  - [7.5.4 Setting a Default Compute Zone with gcloud](#)
  - [7.5.5 Protocol and Destination Port Specification Combinations](#)
  - [7.5.6 GKE Firewall Rules](#)
- [8 - Routes](#)
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
VPC networks and subnets are different types of objects in Google Cloud. 
- VPC networks are global objects spanning all the available regions.
  - Subnets are regional objects.

- A VPC network must have at least one subnet before you can use the network. Subnet creation mode defines the type of VPC network (more on that later in the module …). Subnets define IP range partitions within a VPC. Accordingly, a VPC network, as such, does not have any IP address ranges associated with them.  IP ranges are defined for the subnets. Each subnet is associated with a geographical region; you can create more than one subnet per region.


### 6.2.3 CIDR Network Notation

Google network documentation uses the CIDR network notation. 

> Classless Inter-Domain Routing (CIDR) is a method for allocating IP addresses and for IP routing. CIDR notation gives a compact representation of an IP address along with its associated network mask using this form:

 ```
 <IP_address>/<width (in bits) of the network prefix> 

  ```
For example,

 ```
192.168.100.14/24 
represents the IPv4 address 192.168.100.14 and its subnet 24-bit mask 255.255.255.0
 ```

Firewall rules designate the **0.0.0.0/0** CIDR address to represent any source (e.g. the Internet).

Notes:
It is good to know that there are several “non-routable” (not reachable from outside of the private networks) IP ranges:

- class A reserved space 10.0.0.0/8
- class B reserved space 172.16.0.0/12
- class C reserved space 192.168.0.0/1

> The above IP addresses are routable only within private networks.


### 6.2.4 A Basic Cross-Region VPC Network

[GCP Example](https://cloud.google.com/vpc/docs/shared-vpc?hl=en#shared_vpc_overview) of A host project with a Shared VPC network provides internal connectivity for two service projects, while a standalone project doesn't use Shared VPC (click to enlarge).

<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/network-3.png?raw=true" alt="Sublime's custom image"/>
</p>

- A Shared VPC Admin for the organization has created a host project and attached two service projects to it:
  - Service Project Admins in **Service project A** can be configured to access all or some of the subnets in the Shared VPC network. A Service Project Admin with at least subnet-level permissions to the 10.0.1.0/24 subnet has created Instance A in a zone of the us-west1 region. This instance receives its internal IP address, 10.0.1.3, from the 10.0.1.0/24 CIDR block.
  - Service Project Admins in **Service project B** can be configured to access all or some of the subnets in the Shared VPC network. A Service Project Admin with at least subnet-level permissions to the 10.15.2.0/24 subnet has created Instance B in a zone of the us-east1 region. This instance receives its internal IP address, 10.15.2.4, from the 10.15.2.0/24 CIDR block.

- The *Standalone Project* does not participate in the Shared VPC at all; it is neither a host nor a service project. Standalone instances are created by IAM principals who have at least the compute.InstanceAdmin role for the project.

### 6.2.5 Legacy Networks
Currently used Google Cloud VPC networks offer users more advanced features compared to older networks now downgraded to the legacy status. Legacy networks lack many of the capabilities found in modern VPCs. Legacy networks are associated with a single global IP range that cannot be subdivided into subnets; VPC networks are partitionable into subnets making it possible for each Google Cloud region to be associated with one or more subnets in a single VPC network. Legacy networks do not support Private Google Access (PGA is discussed later in the module). If you already have an older legacy network, there is a migration path to upgrade it to a VPC network; there is, however, no path to convert a VPC network into a legacy network.

> Notes: Google Cloud issued a warning: “Legacy networks are deprecated and will shut down on June 1, 2021 for any GCP project. After that date, you won’t be able to create legacy networks. However, existing legacy networks won’t be affected and will continue to operate normally. Until that date, the field will be available only for projects with existing legacy networks.”

- One limitation with legacy networks is that it is not possible to create regional subnets in a legacy network

### 6.2.6 Listing Networks & Viewing Network Details

A fast way to view the VPC (and legacy, if any) networks in your project is to use the gcloud compute networks list command. VPC networks will list their subnet modes; legacy networks will be marked with the LEGACY subnet creation mode.

<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/listnet.png?raw=true" alt="Sublime's custom image"/>
</p>

 ```
gcloud compute networks describe <NETWORK>
 ```

### 6.2.5 Projects and VPC Relationship
Projects can contain multiple VPC networks unless an organizational policy prohibits it.
- While you can create and add more networks to your project, networks, however, cannot be shared between projects. New projects start with a default network (an auto-mode VPC network) that has one subnetwork (subnet) in each region. The default network (an auto-mode VPC network) comes with pre-populated firewall rules that you can delete or modify.

### 6.2.6 VPC Specifications 
VPC networks, including their associated routes and firewall rules, are global resources. 
- They are not associated with any particular region or zone. Subnets are regional resources; each subnet defines a range of its IP addresses. Traffic to and from your VM instances is controlled with network firewall rules. Firewall rules are implemented on the VMs themselves, so traffic can only be controlled and logged as it leaves or arrives at a VM. Resources within a VPC network can communicate with one another by using internal IPv4 addresses, subject to applicable network firewall rules. Instances with internal IP addresses can communicate with Google APIs and services. IAM roles can be applied when administering network resources.

- Using a Shared VPC [https://cloud.google.com/vpc/docs/shared-vpc], an organization can keep a VPC network in a common host project. IAM-authorized members from other projects in the same organization are allowed to create resources that use subnets of that Shared VPC network. VPC networks from different projects or organizations can be connected to each other using VPC Network Peering. VPC networks can be securely connected in hybrid environments by using Cloud VPN or Cloud Interconnect.

### 6.2.5 Types of VPC Networks

Types of VPC networks are determined by their subnet creation mode:

- Auto-mode VPC networks
  - Unless you choose to disable this default, each new project is provisioned with a default auto-mode VPC network along with pre-populated firewall rules. These are built with one automatically provisioned subnet per region at creation time and automatically receive new subnets in new regions. Subnets are assigned a set of predefined IP ranges that fit within the 10.128.0.0/9 CIDR block.

- Custom-mode VPC networks
  - Give you full control over subnet configuration; you have to manually create subnets.

You can switch from an auto-mode VPC network to a custom-mode VPC; the reverse conversion is not supported (this is a one-way conversion path).



### 6.2.6 Considerations for Auto-mode VPC Networks

The most attractive feature of auto-mode VPC networks is the ease of setting them up. For production networks, Google recommends using custom-mode VPC networks. In the slide’s notes, you can find part of the table mapping regions to IP ranges and default gateway.

<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/regions.png?raw=true" alt="Sublime's custom image"/>
</p>


#### Auto mode network:

An auto mode network has one subnet per region, each with a predetermined IP range which fit within the 10.128.0.0/9 CIDR block. These subnets are created automatically when the auto mode network has created, and each subnet has the same name as the overall network. When any new GCP regions become available, new subnets for those regions are automatically added to the auto mode networks using an IP range from that block. We can add more subnets manually to auto mode networks in addition to the automatically created subnets.

### 6.2.7 Considerations for Custom-mode VPC Networks
Custom-mode VPC networks are more flexible than auto-mode ones and offer the following benefits:

- This mode will support your plans to connect VPC networks through VPC Network Peering or Cloud VPN.
  - This is not possible with auto-mode networks since the subnets of every auto-mode VPC network use the same predefined range of IP addresses (the 10.128.0.0/9 CIDR block) — in other words, you cannot connect auto-mode VPC networks to one another

#### Custom mode network:

A custom mode network has no subnets at creation giving us full control over subnet creation. In order to create an instance in a custom mode network, we must first create a subnetwork in that region and specify its IP range. A custom mode network has the possibilities of having zero, one, or many subnets per region.

It is possible to switch a network from auto mode to custom mode. But this conversion is one-way, custom mode networks cannot be changed to auto mode networks.

When a new project is created, a **default network** configuration provides each region with an auto mode network with pre-populated firewall rules.

We can create up to four additional networks in a project. Additional networks can be either auto subnet networks or custom subnet networks.

Each instance which is created within a subnetwork gets assigned to an IPv4 address from that subnetwork range.

> Note: Since the default Network allows relatively open access, it is a recommended best practice that you delete it. The default Network cannot be deleted unless another Network is present. Please make sure you delete all the firewall rules for the associated VPC before deleting the VPC network.



## `7 - Firewalls`
Each network has a default firewall rule that blocks all inbound traffic to instances (default-deny-all-ingress). 
- To allow traffic to an instance, we must create “allow” rules for the firewall. Also, the default firewall allows traffic from instances unless we configure it to block outbound connections using an “egress” firewall configuration (default-allow-all-egress).
- Therefore, by default we can create “allow” rules for traffic we wish to pass ingress, and “deny” rules for traffic which we wish to restrict egress. We can also create a default-deny policy for egress and prohibit external connections entirely.

In general, it is recommended to set the least permissive firewall rule that will support the kind of traffic which we are trying to pass. 

> For example, if we want to allow traffic to reach some instances, but restrict traffic from reaching others, create rules that allow traffic to the intended instances only. This configuration which is more restrictive happens to be more predictable than a large firewall rule that allows traffic to all of the instances. If we want to have “deny” rules to override certain “allow” rules, we can set priority levels on each rule and the rule with the lowest numbered priority will be evaluated first. It is not recommended to create large and complex sets of override rules as it can lead to allowing or blocking traffic that is not intended.

The default network has automatically created firewall rules. No manually created network has firewall rules which are automatically created. For all networks except the default network, we must create any firewall rules we need.

**The ingress firewall rules which are automatically created for the default network are:**

- **default-allow-internal:** Allows network connections of any protocol and port between instances on the network.
- **default-allow-ssh:** Allows SSH connections from any source to any instance on the network over TCP port 22.
- **default-allow-rdp:** Allows RDP connections from any source to any instance on the network over TCP port 3389.
- **default-allow-icmp:** Allows ICMP traffic from any source to any instance on the network.

  
**We cannot SSH into an instance without allowing SSH firewall rule.**

Each and every rule of firewall has a Priority value from 0-65535 which is inclusive. Relative priority values are used to determine the precedence of conflicting rules. Lower priority value implies higher precedence. When not specified, 1000 is used as a priority value. If a packet is matching with the conflicting rules with the same priority, the deny policy takes precedence.

We can review the default firewall rules using the console through this path: Navigation menu > VPC networks > Firewall rules.

> GCP Firewalls are stateful: for each initiated connection tracked by allowing rules in one direction, the return traffic is automatically allowed, regardless of any rules.

### `7.1 Virtual Firefalls`

VPC firewall rules control traffic coming in and out of VM instances on a network. The default network once provisioned, has a default set of firewall rules; you can create custom rules if more detailed network resource protection is needed. 
- Firewall rules are defined at the VPC network level. Using firewall rules configurations, you can dis/allow connections to or from instances. Connections are allowed or denied on a per-instance basis.
- VPC firewall rules can, essentially, control traffic between individual instances within the same network.

### `7.2 Firewall Rules`

- To create a VPC firewall rule, you need to specify the target VPC network along with the elements that define what the rule does.
- These elements lend you control over:
  - Traffic protocols, source and destination ports, etc.

#### 7.2.1 Adding firewall rules

###### Firewall Rules and IAM
> The privilege of creating, modifying, and deleting firewall rules has been reserved for the compute.securityAdmin role by IAM. Users assigned to the compute.networkAdmin roles are able to safely view and list firewall rules that might apply to their projects.

###### Allow/Ingress Rules
> Allow and ingress are values which we can set for the flags —action and –direction while creating the firewall rules. –action can have 2 values. ALLOW and DENY. ALLOW was the only action supported previously. DENY rules are generally simpler to understand and apply.

–direction can have 2 values. INGRESS and EGRESS. INGRESS refers to inbound and EGRESS refers to outbound connections.

With INGRESS, EGRESS, ALLOW, DENY, and PRIORITY we have maximum control and flexibility for determining connections in and out of our networks.

###### Network route
> All networks have routes created automatically to the Internet (default route) and to the IP ranges in the network. The route names are automatically generated and will look different for each project.

We can review the default routes using the console through this path: Navigation menu > VPC network > Routes.

### `7.3 Firewall Rule Elements (Components)`
Each firewall rule you create in Google Cloud consists of the following main configuration elements (called components in the Google Cloud documentation):

- The direction of the connection:
  - Inbound (ingress) rules applied to incoming connections, and
  - Outbound (egress) rules applied to connections going to specified destinations from targets
- An action on a matched connection:
  - Either allow or deny the matching connection
  - Rule status:
    - Enable and disable (suspend the rule without deleting)
- A target, which defines the instances (including GKE clusters and App Engine flexible environment instances) to which the rule applies
- A source for ingress rules or a destination for egress rules
- The connection’s protocol (tcp, udp, icmp, esp, ah, sctp, ipip) and the destination port

> For more information, visit https://cloud.google.com/vpc/docs/firewalls#firewall_rule_components.

> In addition to firewall rules that you create, Google Cloud has other rules that can affect incoming (ingress) or outgoing (egress) connections, for example, Google Cloud doesn’t allow certain IP protocols, such as egress traffic on TCP port 25 within a VPC network. For more information, see https://cloud.google.com/vpc/docs/firewalls#blockedtraffic

### `7.4 Creating a custom network`




### `7.5 Adding firewall rules`

To allow access to VM instances, we must apply firewall rules. We can use the instance tag to apply firewall rule to the VM instances. The firewall rule will apply to any VM using the same instance tag.

Instance Tags are used by networks and firewalls to apply certain firewall rules to tagged VM instances. For example, if there are several instances that perform the same task, such as serving a large website, we can tag these instances with a shared word or term and then use that tag to allow HTTP access to those instances with a firewall rule. Tags are also reflected in the metadata server, so we can use them for applications running on our instances.

Firewall rules can be created using either a console or cloud shell. Here we will create using cloud shell.

The following command will create a firewall rule to allow HTTP internet requests.


 ```
gcloud compute firewall-rules create allow-http \

 --allow tcp:80 --network custom-network \ 

--source-ranges 0.0.0.0/0 \ 

--target-tags http

 ```

The above command will create a firewall-rule which will allow HTTP internet requests to those instances which have the tag http because we are using the flag –target-tags.

Here is an example command to create an instance which has tags.

 ```
 gcloud compute instances create us-test-01 \

 --subnet subnet-us-central \ 

--zone us-central1-a \ 

--tags http
 ```

##### VPC Network Example:


<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/VPC-network.png?raw=true" alt="Sublime's custom image"/>
</p>


> In the above example:

- subnet1 is defined as 10.240.0.0/24 in us-west1 region. There are 2 VM instances in the us-west1-a zone in this subnet and their IP addresses are from the available range of addresses in subnet1.

- Subnet2 is defined as 192.168.1.0/24 in the us-east1 region. There are 2 VM instances in the us-east1-a zone in this subnet and their IP addresses are from the available range of addresses in subnet1.

- Subnet3 (mistakenly mentioned as subnet1 in the diagram) is defined as 10.2.0.0/16, in the us-east1 region. There is one VM instance in the us-east1-a zone and the second instance in us-east1-b zone in subnet3, each receiving an IP address from its available range. Because subnets are regional resources, instances can have their network interfaces associated with any subnet in the same region that contains their zones.


#### 7.5.1  Ingress (Inbound) Firewall Rules
<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/ingress.png?raw=true" alt="Sublime's custom image"/>
</p>

> Source: https://cloud.google.com/vpc/docs/firewalls#firewall_rule_components

#### 7.5.2 Egress (Outbound) Rules
<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/egress.png?raw=true" alt="Sublime's custom image"/>
</p>

> Source: https://cloud.google.com/vpc/docs/firewalls#firewall_rule_components

#### 7.5.3 Authoring Firewall Rules

You create or modify VPC firewall rules by using either

- Cloud Console, or
- The gcloud command-line tool, or
- REST API

#### 7.5.4 Setting a Default Compute Zone with gcloud
- Use this gcloud command to set default compute zone:

````
gcloud config set compute/zone <your default zone>, e.g.
gcloud config set compute/zone us-central1-a

````

#### 7.5.5 Protocol and Destination Port Specification Combinations
- The following example can help you understand how to identify the applicable rules to a connection between two VM instances (i1 and i2) running in the same network
Traffic from i1 to i2 can be controlled by using either of these firewall rules:
  - An ingress rule with a target of i2 and a source of i1
  - An egress rule with a target of i1 and a destination of i2
 
  <p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/protocol.png?raw=true" alt="Sublime's custom image"/>
</p>

> Source: https://cloud.google.com/vpc/docs/firewalls#firewall_rule_components

> Notes: Be aware of the configuration rule that a port cannot be specified by itself (e.g. 8080). Make sure you use it in combination F23 with the protocol (e.g. tcp:8080)

#### 7.5.6 GKE Firewall Rules

Google Kubernetes Engine creates firewall rules automatically when creating the following resources:

- GKE Clusters
- GKE Services
- GKE Ingresses

> For more information, see https://cloud.google.com/kubernetes-engine/docs/concepts/firewall-rules

## `8 - Routes`
A route is a virtual networking component that allows you to implement more advanced networking functions for your instances, such as creating VPNs.
- Routes define paths for packets leaving instances (egress traffic), in other words, a route controls how packets leaving an instance should be directed.

> For example, a route might specify that packets destined for a particular network range should be handled by a gateway VM instance that you configure and operate.

- For a complete overview of Google Cloud routes, visit https://cloud.google.com/vpc/docs/routes

References:
- https://www.webagesolutions.com/blog/google-cloud-virtual-networking
- https://tudip.com/blog-post/google-cloud-networking/

### - 8.1 Route Categories (Types)

- Google Cloud has two categories (types) of routes:
  - system-generated, and
  - custom
- Every new network is provisioned with two types of system-generated routes:
  - The default one that defines a path for traffic leaving the VPC network and provides internet access to VMs that need it, as well as the “typical path” for Private Google Access
  - A subnet route created for each of the IP ranges associated with a subnet
- Custom routes are either manually created static routes or dynamic routes maintained automatically by one or more of your Cloud Routers
  - For more information on custom routes, visit https://cloud.google.com/vpc/docs/routes#custom-routes

#### Notes

> Note A: If your VPC network is connected to an on-prem network through Cloud VPN or Cloud Interconnect, you need to make sure that your subnet IP address ranges do not conflict with those on-prem.

 

> Note B: Every subnet has at least one subnet route for its primary IP range; additional subnet routes are created for a subnet if you add secondary IP ranges to it. Subnet routes define paths for traffic to reach VMs that use the subnets.

### - 8.2  Configuring Private Google Access

- When you create a Compute Engine VM, it has, by default, no external IP address assigned to its network interface
  - This circumstance limits the instance to only being able to send IP packets to other internal IP addresses
- To allow connectivity from these VMs to external IP addresses used by Google Cloud Developer APIs and services, you need to enable Private Google Access (PGA) on the subnet used by the VM’s network interface
  - PGA also allows access to the external IP addresses used by App Engine
  - The list of the supported PGA services can be found here: https://cloud.google.com/vpc/docs/private-google-access#pga-supported
  - PGA configuration steps can be found here: https://cloud.google.com/vpc/docs/configure-private-google-access

> Notes: Currently, the following services are not supported by PGA:
 - App Engine Memcache
 - Filestore
 - Memorystore

### - 8.3 The Implementation of Private Google Access Diagram
 
  <p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/private.png?raw=true" alt="Sublime's custom image"/>
</p>

> Notes: In the above diagram (description is borrowed from https://cloud.google.com/vpc/docs/private-google-access#pga-supported):

````
The VPC network has been configured to meet the DNS, routing, and firewall network requirements for Google APIs and services. Private Google Access has been enabled on subnet-a, but not on subnet-b. 
VM A1 can access Google APIs and services, including Cloud Storage, because its network interface is located in subnet-a, which has Private Google Access enabled. Private Google Access applies to the instance because it only has an internal IP address. 
VM B1 cannot access Google APIs and services because it only has an internal IP address and Private Google Access is disabled for subnet-b.
VM A2 and VM B2 can both access Google APIs and services, including Cloud Storage, because they each have external IP addresses. Private Google Access has no effect on whether or not these instances can access Google APIs and services because both have external IP addresses.
````

### - 8.4 

## `License`
MIT License & [cc](https://creativecommons.org/licenses/by/4.0/) license

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

To the extent possible under law, [Paul Veillard](https://github.com/paulveillard/) has waived all copyright and related or neighboring rights to this work.










https://medium.com/google-cloud/gcp-network-design-part-1-things-to-consider-before-starting-gcp-network-design-ef1b3191c2f1
