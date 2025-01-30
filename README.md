# Security in Google Cloud Platform (GCP): Theory, Testing, Techniques, and Tools


An ongoing & curated collection of awesome software best practices and techniques, libraries and frameworks, E-books and videos, websites, blog posts, links to github Repositories, technical guidelines and important resources about securing Google Cloud Platform (GCP) in Cybersecurity.
> Thanks to all contributors, you're awesome and wouldn't be possible without you! Our goal is to build a categorized community-driven collection of very well-known resources.


## `Table of Contents`
- [GCP Landing Zone Best Practices](#)
- [GCP — Cloud Security Best Practices for Enterprises](#)
  - [1 - Build your Resources and Structure into Resource Hierarchy](#)
  - [2 - Implement IAM (Identity and Access Management) as the standard](#)
  - [3 - Design Custom Network and Network Security —Think and Design Early](#)
  - [4 - Securing your Applications and Data](#)
  - [5 - Deploy Logging, Monitoring and Operations](#)
  - [6 - Implement Billing and Management](#)
- 

# `GCP — Cloud Security Best Practices for Enterprises`

<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/gcp-original.png?raw=true" alt="Sublime's custom image"/>
</p>



## `1 - Build your Resources and Structure into Resource Hierarchy`

### `1.1 - What is the Google Cloud Resource Hierarchy?`
- At its core, Google Cloud Resource Hierarchy is a structured way of organizing resources within the Google Cloud Platform (GCP). This hierarchical structure reflects relationships and dependencies, allowing for better management and control over various cloud assets. The primary components of this hierarchy include organizations, folders, projects, and individual resources.


<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/gcp-1.png?raw=true" alt="Sublime's custom image"/>
</p>

- Google Cloud resources are organized hierarchically.
> All resources except for the highest resource in a hierarchy have exactly one parent. At the lowest level, service resources are the fundamental components that make up all Google Cloud services. Examples of service resources include Compute Engine Virtual Machines (VMs), Pub/Sub topics, Cloud Storage buckets, App Engine instances. All these lower level resources have project resources as their parents, which represent the first grouping mechanism of the Google Cloud resource hierarchy.

<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/gcp-2.png?raw=true" alt="Sublime's custom image"/>
</p>


- The Org Node aka **‘Organisation’** helps to provide central visibility and control over all the resources under that Organisation node.
- **Folders** help to create isolation between different teams or departments under the organisation. Folders can be further split in to sub-folders based on your business needs and there is no restriction on folder levels.
- The bottom layer is the ‘Project’ level. **Projects** help contain GCP resources which help constitute your application/s. Projects can contain resources spread across multiple regions and geographies. Best Practice is to have one project per application per environment.
> For eg., if you’ve 2 applications ‘A’ and ‘B’, with a development, staging and production environment, you would have 6 projects in total such as A-dev, A-stage, A-prod and B-dev, B-stage, B-prod. This segregation helps in isolating the environments from each other so that changes in one project/env do not accidentally impact other project/env and also helps provide better access control too.

### `1.2 Key Components of Resource Hierarchy`

#### 1.2.1 - Organization

- An organization in Google Cloud is the top-level container for resources. It serves as the anchor point for managing billing, policies, and permissions. Organizations provide a global context for all resources within them.

  <p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/organisation.png?raw=true" alt="Sublime's custom image"/>
</p>


##### Purpose and Significance:
**Anchor Point:** The organization serves as the highest-level anchor point in the resource hierarchy. It provides a global context for all resources within it, acting as the umbrella under which various projects and resources are organized.

**Billing and Policies:** One of the key roles of the organization is to manage billing and set overarching policies for all associated resources. This centralized control allows for streamlined financial management and policy enforcement.

##### Characteristics and Functionality:
Hierarchy Root: The organization is the root of the resource hierarchy. All resources and projects within the Google Cloud Platform are organized under an organization.

**Management and Permission:** It facilitates centralized management, allowing administrators to set permissions and policies that apply across the entire organization. This includes access control and defining roles for users and groups.

**Resource Aggregation:** Resources within an organization can span multiple projects and folders. This aggregation makes it easier to manage and oversee resources at scale.

##### Creating and Managing an Organization:
**Google Cloud Console:** Organizations are typically created and managed through the Google Cloud Console. The console provides a user-friendly interface for administrators to configure organizational settings, manage billing, and set policies.

**IAM Policies:** Identity and Access Management (IAM) policies are crucial within an organization. They determine who has access to what resources and what actions they can perform. IAM policies are set at the organization level and cascade down through the hierarchy.

##### Best Practices
Reflecting Business Structure: Align the organization’s structure with the business’s organizational structure. This ensures that the resource hierarchy accurately represents the relationships and dependencies within the organization.

Billing and Cost Tracking: Leverage the organization’s billing features to track and manage costs effectively. This includes setting budgets and alerts and optimizing spending across projects.

The following code shows the structure of the organization’s resources:

````
{
  "creationTime": "2020-01-07T21:59:43.314Z",
  "displayName": "my-organization",
  "lifecycleState": "ACTIVE",
  "name": "organizations/34739118321",
  "owner": {
    "directoryCustomerId": "C012ba234"
  }
}
`````

#### 1.2.2 - Folders
#### 1.2.3 - Projects
#### 1.2.3 - Resources






## 2 - Implement IAM [Identity and Access Management] as the standard
In a nut shell, most common issues found are like using ‘individual’ accounts instead of ‘groups’ to grant access to specific Google Cloud resources and fully ignoring the basic security principle of ‘least privilege’

 <p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/iam-1.png?raw=true" alt="Sublime's custom image"/>
</p>

To avoid this, **follow the recommended IAM best practices** such as:

- Always use fully managed google accounts which are tied to your corporate domain name through Cloud Identity. This forms a management layer through which enabling or disabling the access to your google cloud resources is easily possible
- Synchronise your user directory to Cloud Identity which lets your users to access the google cloud resources with their existing credentials. This leads to have your identity platform as the ‘single source of truth’ whereas Cloud Identity helps you to control the ‘how’ access to your google cloud resources.
- Control access to resources using google groups instead of individual accounts and use Service Accounts where applicable. For all the server-to-server interactions, use service accounts. For example:
> Avoid Automatic IAM permissions for default service accounts and Ability to create service accounts in a production environment.
- Define Organization Policy/Policies which helps to set restrictions on specific resources to determine how those can be configured and used. For example, you can define a constraint to restrict any VM to not to use default service account.

## `3 - Custom Network and Network Security —Think and Design Early`
To be frank, the issues I noticed under this section are like eye-openers as I couldn’t believe my eyes when I first looked at it.

You know what — People are using ‘Default’ Networks. Just not that but also ‘Default’ firewall rules which are unbelievably true from my perspective. You cannot start worse than this ;>(

<p align="center">
  <img src="https://github.com/paulveillard/cybersecurity-gcp/blob/main/img/network-1.png?raw=true" alt="Sublime's custom image"/>
</p>

Let me simplify the bare minimum things to start considering from the beginning

- Avoid using ‘Default’ VPC Network especially in Google Cloud rather use ‘Custom’ VPC Network. There is a massive difference between other CSPs vs about GCP how the default VPC has been set up. Spend little time to understand that before you start. Google recommends enterprises to use custom mode VPC Networks for some valid reasons. Refer here for more information.

- Avoid using ‘Default’ firewall rules instead create your own ‘custom’ firewall rules which brings loads of control about how you can group and isolate related resources with combination of the network and firewall rules. These firewall rules allow you to specify the type of traffic like ports and protocols and the source or destination of the traffic. Default firewall rules open up ports for a wide range of IP addresses for some protocols which is a potential security risk and not a recommended best practice.

  
- Enable Firewall Rule Logging which helps to audit, verify and analyse the effectiveness of the firewall rules in action. It’s a recommended practice to enable these logs for audit and analysis purpose.

  
- Use VPC Flow Logs and create a process to enable flow logs when needed. Theses logs can be used for network monitoring, forensics, real-time security analysis and also helps in expense optimisation too.

  
- Avoid or Limit External Access to the internet to only those google cloud resources that need it. Resources within a VPC network can communicate among themselves anyhow through internal IP addresses and this can be further limited using Firewall rules if needed. Use google cloud services such as ‘Cloud NAT’ and ‘Private Google Access’ based on the use case to avoid simply opening up your resources to the internet. Common mistakes noted are like Every VM has an external IP address attached to it, GCS buckets are freely accessible apart from not using the earlier mentioned services.

  
- Centralise and Control the Network using Shared VPC or VPC Peering. VPC Network Peering helps to connect different VPC networks and to communicate internally without traversing through the internet whereas Shared VPC allows an organisation to connect resources from multiple projects to a common network to communicate securely and efficiently using the internal IP from that network. In case if you are trying to connect from on-premises to Google Cloud, use Cloud VPN or Interconnect services depending on the business use case.

## `4 - Securing your Applications and Data`
## `5 - Logging, Monitoring and Operations`
## `6 - Billing and Management`
