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

# GCP — Cloud Security Best Practices for Enterprises

## 1 - Build your Resources and Structure into Resource Hierarchy

### 1.1 - What is the Google Cloud Resource Hierarchy?
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

### 1.2 Key Components of Resource Hierarchy

#### 1.3 - Organization
#### 1.4 - Folders






## 2 - Implement IAM [Identity and Access Management] as the standard
## 3 - Custom Network and Network Security —Think and Design Early
## 4 - Securing your Applications and Data
## 5 - Logging, Monitoring and Operations
## 6 - Billing and Management
