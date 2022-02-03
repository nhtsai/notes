---
layout: post
title: "Azure Fundamentals"
description: "Course notes on ExamPro's AZ-900 certification course."
toc: true
comments: false
hide: false
search_exclude: false
show_tags: true
categories: [course-notes, cloud]
permalink: /azure-fundamentals
---

# AZ-900: Microsoft Azure Fundamentals Certification Course
* [Course Video](https://www.youtube.com/watch?v=NKEFWyqJ5XA)
* Content Outline
    * 15-25%: Cloud Concepts
    * 30-35%: Azure Core Services
    * 25-30%: Security, Privacy, Compliance, and Trust
    * 20-25%: Pricing and Support
* Need to get around 700/1000 or 70% to pass.
* Exam has 40-60 questions, can get 12-18 questions wrong.
* Question types: multiple choice, multiple answer, drag and drop, hot area (multiple drop-down menus)
* Exam time is 60 min, seat time is 90 min.
* Valid for 24 months/2 years before re-certification.

## Cloud Concepts

* **Cloud Computing**: practice of using a network of remote servers hosted on Internet to store, manage, and process data, rather than a local server or a personal computer
    * On-Premise: you own the server, manage the infrastructure, own all the risk
    * Cloud Providers: servers are owned and managed by other parties
* **Dedicated Servers**: one physical machine dedicated to a single business
    * Runs a single website or application
    * Very expensive, high maintenance, high security
* **Virtual Private Server**: one physical machine dedicated to a single business
    * virtualized into sub-machines to run multiple websites or applications
* **Shared Hosting**: one physical machine shared by hundreds of businesses
    * relies on most tenants under-utilizing their resources
    * very cheap, very limited
* **Cloud Hosting**: multiple physical machines that act as one system
    * abstracted into multiple cloud services
    * flexible, scalable, secure, cost-effective, high configurability
* Most Common **Infrastructure as a Service (IaaS)** Cloud Services
    * Compute: a virtual machine that can run applications, programs, and code
    * Storage: a virtual hard-drive that can store files
    * Networking: a virtual network that can define internet connections or network isolations
    * Databases: a virtual database for storing reporting data or general purpose application use

* Benefits of Cloud Computing
    * Cost Effective: pay for what you consume, no upfront cost
    * Global: launch workloads anywhere in the world
    * Secure: cloud services can be secure by default, handled by provider
    * Reliable: data backup, disaster recover, replication, fault tolerance
    * Scalable: adapt resources and services based on demand
    * Elastic: automate scaling
    * Current: underlying software is upgraded by provider

* Types of Cloud Computing
    * Software as a Service (SaaS): product that is run and managed by a service provider, for consumers
        * Salesforce, Gmail, Office 365, other software in the cloud
    * Platform as a Service (PaaS): focus on deployment and management of your apps, for developers
        * AWS Elastic Beanstalk, Heroku, Google App Engine
    * Infrastructure as a Service (IaaS): cloud IT, provides access to networking features, computers, and data storage space, for administrators
        * Microsoft Azure, AWS, Oracle Cloud

* Types of Cloud Computing Responsibilities
    * **Customer is responsible**

    | On-Premise | IaaS | PaaS | SaaS |
    | ---------- | ---- | ---- | ---- |
    | **Applications** | **Applications** | ****Applications**** | Applications |
    | **Data** | **Data** | **Data** | Data |
    | **Runtime** | **Runtime** | Runtime | Runtime |
    | **Middleware** | **Middleware** | Middleware | Middleware |
    | **OS** | **OS** | OS | OS |
    | **Virtualization** | Virtualization | Virtualization | Virtualization |
    | **Servers** | Servers | Servers | Servers |
    | **Storage** | Storage | Storage | Storage |

    * For on-premise, customer is responsible for everything!


## Microsoft Azure
* Tech company based in Redmond, Washington that makes software, devices, cloud services, search engines.
    * Most known for Windows Operating System
* Azure is the **cloud service provider (CSP)** from Microsoft
    * Means "bright blue color of the cloudless sky"

* Azure Deployment Models

    |  | Cost | Security | Configuration | Technical Knowledge |
    | -- | -- | -- | -- | -- |
    | Public Cloud | everything built on cloud provider, cloud native | most cost-effective | security controls by default, may not meet your security requirements | configuration limited by provider | no in-depth knowledge needed |
    | Private Cloud | everything built on company's data centers, on-premise | most-expensive | no security guarantee, can meet your security requirements | configurable anyway | in-depth knowledge needed to configure private infrastructure |
    | Hybrid Cloud | using both on-premise and cloud-native from cloud service provider | depends on cloud usage | need to secure, can meet your security requirements | best of both words | in-depth knowledge needed to configure private infrastructure and of cloud services |
    | Cross Cloud | using multiple cloud providers, multi-cloud | security controls spread across providers | configurations limited across providers | in-depth knowledge needed of multiple cloud providers |

* Total Cost of Ownership (TCO)
    * On-premise (CAPEX): software license fees, implementation, configuration, training, physical security, hardware, IT personnel, maintenance
    * Azure/Cloud (OPEX): subscription fees, implementation, configuration, training
    * Usually save 75% by moving to cloud services

* Capital vs. Operational Expenditure
    * **Capital Expenditure (CAPEX)**: spending money upfront on physical infrastructure, deduct that expense from tax bill over time, on-premise business model
        * Server costs
        * Storage costs
        * Network costs
        * Backup and Archive costs
        * Disaster recovery costs
        * Data center costs
        * Technical personnel costs
        * Need to guess upfront what you plan to spend
    * **Operational Expenditure (OPEX)**: physical costs are shifted to cloud service provider, customer handles non-physical costs
        * Software lease and customization costs
        * Cloud training costs
        * Cloud support costs
        * Cloud metric billing costs (compute, storage)
        * Can try a product or service without investing in equipment

* **Availability**: ability to ensure service remains available
    * **High Availability**: ensure no single point of failure and/or ensure a certain level of performance
    * Having multiple data centers in multiple **availability zones (AZs)** ensures availability
    * **Load Balancer**: evenly distributes traffic to multiple servers in a data center, rerouting traffic in case of server failure
    * Azure Load Balancer
* **Scalability**: ability to grow rapidly or unimpeded
    * **High Scalability**: increase capacity based on increased demand of traffic, memory, and compute power
    * **Vertical Scaling**: upgrading a single machine, scaling up
    * **Horizontal Scaling**: adding additional servers of the same size, scaling out
* **Elasticity**: ability to shrink and grow to meet demand
    * **High Elasticity**: automatically change capacity based on current demand of traffic, memory, and compute power
    * Use *horizontal scaling* to scale in or scale out machines
    * Generally, high elasticity is hard to accomplish with vertical scaling
    * Azure VM Scale Sets, SQL Server Stretch Database
* **Fault Tolerance**: ability to prevent or handle failures
* **Disaster Recovery**: ability to recover from failures, high durability
    * **High Durability**: ability to recover from a disaster and prevent loss of data
    * Backup archives, restoring backups, backup functionality, ensure live data is not corrupt

## Evolution of Computing
* **Dedicated**: physical server wholly utilized by a single customer
    * Need to guess total capacity needed when buying a server, might be underutilized
    * Upgrading is slow and expensive
    * Limited by operating system
    * Multiple apps can fight over shared resources
    * Guaranteed security, privacy, and full utility of underlying resources
* **Virtual Machines (VMs)**: multiple virtual machines run on one machine
    * **Hypervisor**: software layer to run virtual machines
    * Physical server is shared by multiple customers
    * Need to guess total capacity, might be underutilized, pay for a fraction of the server
    * Limited by guest operating system
    * Multiple apps can fight over shared resources if run on a single VM
* **Containers**: multiple containers run on one virtual machine
    * **Docker Daemon**: software layer that lets you run multiple containers
    * Can maximize capacity used by application to be more cost-effective
    * Containers share same underlying OS, but each container can run a different OS for more flexibility
    * Multiple apps can run side by side in containers without fighting over shared resources
* **Functions**: managed VMs running managed containers, apps divided up into functions, *serverless compute*
    * Upload code and choose amount of memory and duration, only responsibility for code
    * Very cost-effective, only pay for code execution time, VMs only execute when needed
    * **Cold Starts**: overhead time needed to start a server affects execution time

## Global Infrastructure
* **Region**: grouping of multiple data centers, or availability zones
* **Geography**: discrete market of 2+ regions that preserves *data residency and compliance boundaries*
    * United States, US Government, Canada, Brazil, Mexico
    * Used to guarantee data will remain within a country's boundaries
* **Paired Regions**: each region is paired with another region 300 miles away
    * only one region is updated a a time to ensure no outages, also used for disaster recovery
    * Azure Geo-Redundant Storage: replicates data to a secondary region automatically, ensuring data is durable even in the event that the primary region isn't recoverable.
* Not all Azure cloud services are available in every region.
* **Recommended Region**: a region that provides the broadest range of service capabilities and is designed to support availability zones now or in the future
* **Alternate Region**: a region that extends Azure's footprint within a data residency boundary where a recommended region also exists, not designed to support availability regions.
* **General Availability (GA)**: when a service is considered ready to be used publicly by everyone
* Azure Cloud services are grouped into 3 categories by when services are available
    * Foundational: available immediately or within 12 months in recommended and alternate regions
    * Mainstream: available immediately or within 12 months in recommended regions, available in alternate regions based on customer demand
    * Specialized: available in recommended or alternate regions based on customer demand
* **Special Regions**: regions to meet compliance or legal reason
    * US Government 
    * Chinese Government via partnership with 21Vianet for data centers
* **Availability Zones (AZs)**: physical location made up of 1+ data centers
    * **Data Center**: secured building that contains hundreds of thousands of computers
    * *A region will generally contain 3 AZs*
    * Data centers within a region are isolated from each other but will be close enough for low-latency
    * Common practice to run workloads in 3 AZs to ensures high availability
* AZ Supported Regions: not every region has support for availability zones, i.e. Alternate or Other
    * Recommended regions have at least 3 AZs
* Fault and Update Domains
    * An AZ in an Azure region is a combination of a fault domain and an update domain
    * **Fault Domain**: logical grouping of hardware to avoid a single point of failure within an AZ
        * Group of VMs that share a common power source and network switch
        * If part of data center fails, other servers won't be taken down.
    * **Update Domain**: ensure resources don't go offline when underlying hardware and software is updated
* **Availability Set**: logical grouping that ensures VMs are in different fault and update domains to avoid downtime
    * Each VM in an availability set is assigned a fault domain and an update domain.
    * Fault Domain e.g. server rack
    * Update Domain e.g. a group of specific servers across racks
    * Higher number of domains means more isolated VMs.
        * Update domain must be 1 when fault domain is 1.

## Azure Services Overview

* Computing Services
    * **Azure Virtual Machines**: Windows or Linux VMs, most common type of compute
        * You choose OS, Memory, CPU, and Storage, share hardware with other customers
    * **Azure Container Instances**: run containerized apps on Azure without provisioning servers or VMs, Docker as a Service
    * **Azure Kubernetes Service (AKS)**: easy to deploy, manage, and scale containerized applications, Kubernetes (K8) as a Service
    * **Azure Service Fabric**: distributed systems platform for easy to package, deploy, and manage scalable and reliable microservices, Tier-1 Enterprise Containers as a Service
    * **Azure Functions**: event-driven, serverless compute (functions) run code without provisioning or managing servers, pay for compute time, Functions as a Service
    * **Azure Batch**: Plans, schedules, and executes batch compute workloads across running 100+ jobs in parallel, can use Spot VMs to save money (using low-priority VMs to save money)

* Storage Services
    * **Azure Blob Storage**: store very large files and large amounts of unstructured files, pay for what you store, unlimited storage, no-resizing volumes, filesystem protocols, Object Serverless Storage
    * **Azure Disk Storage**: a virtual SSD or HDD volume, encryption by default, attached to VMs
    * **Azure File Storage:** shared volume that you can access and manage like a file server
    * **Azure Queue Storage**: data store for queueing and reliably delivering messages between applications, Messaging Queue
    * **Azure Table Storage**: Wide-column NoSQL database that hosts unstructured data independent of any schema
    * **Azure Databox (Heavy)**: rugged briefcase computer and storage designed to move TB or PB data, ship data in a physical device
    * **Azure Archive Storage**: long-term cold storage for when you need to hold onto files for years
    * **Azure Data Lake Storage**: centralized repository to store structured and unstructured data at any scale

* Database Services
    * **Azure Cosmos DB**: fully managed NoSQL database designed for scale with 99.999% availability
    * **Azure SQL DB**: fully managed MS SQL database with auto-scale, integral intelligence, and robust security
    * **Azure Database**: fully managed and scalable MySQL, PostgreSQL, MariaDB database with high availability and security
    * **SQL Server on VMs**: host enterprise SQL Server apps in cloud, convert MS SQL on-premise to Azure Cloud
    * **Azure Synapse Analytics**: fully managed data warehouse with integral security at every level of scale at no extra cost
    * **Azure Database Migration Service**: migrates databases to cloud with no application code changes
    * **Azure Cache for Redis**: caches frequently used and static data to reduce data and application latency
    * **Azure Table Storage**: wide-column NoSQL database that hosts unstructured data independent of any schema

* Application Integration Services
    * **Azure Notifications Hub**: send push notifications to any platform from any backend, *Pub/Sub*
    * **Azure API Apps**: quickly build and consume APIs in the cloud, *API Gateway to Azure Services*
    * **Azure Service Bus**: reliable cloud messaging as a service and simple hybrid integration
    * **Azure Stream Analytics**: serverless *real-time analytics*, from cloud to edge
    * **Azure Logic Apps**: schedule, automate, and orchestrate tasks, business processes, and workflows, integrate with Enterprise SaaS and Enterprise applications
    * **Azure API Management**: hybrid, multi-cloud management platform for for APIs across all environments, put in front of existing APIs for additional functionality
    * **Azure Queue Storage**: data store for queueing and reliably delivering messages between applications, *Messaging Queue*

* Developer and Mobile Tools
    * **Azure SignalR Service**: easily add real-time web functionality to applications, *Real-Time Messaging*, like Pusher for Azure
    * **Azure App Service**: easy to use service for deploying and scaling web applications, like Heroku for Azure
    * **Visual Studio**: IDE designed for creating powerful, scalable applications for Azure
    * **Xamarin**: create powerful and scalable native mobile apps with .NET and Azure, *Mobile-App Framework*

* DevOps Services
    * **Azure Boards**: proven agile tools to plan, track, and discuss work across teams, *Kanban Boards or Github Projects*
    * **Azure Pipelines**: build, test, and deploy with *CI/CD* that works with any language, platform, and cloud
    * **Azure Repos**: unlimited, cloud-hosted, private *Git repos* to collaborate and build code with pull requests and advanced file management
    * **Azure Test Plans**: test and ship with confidence using *manual and exploratory testing tools*
    * **Azure Artifacts**: create, host, and share packages with your team, add artifacts to CI/CD pipelines
    * **Azure DevTest Labs**: fast, easy, and lean dev-test environments
    * **Azure Resource Manager (ARM)**: programmatically create Azure resources via JSON template
        * **Infrastructure as Code (IaC)**: process of managing and provisioning data centers through scripts, rather than physical hardware configuration or interactive configuration tools
    * **Azure Quickstart Templates**: library of pre-made ARM templates to quickly launch new projects, use scripts to quickly set up a project
        * E.g. Deploy a Django App, Deploy Ubuntu VM with Docker Engine, Web App on Linux with PostgreSQL, etc.

* Cloud-Native Networking Services
    * **Azure DNS**: ultra-fast DNS responses and ultra-high domain availability
    * **Azure Virtual Network (vNet)**: logically isolated section of Azure network where you launch your Azure resources, choose *range of IP addresses using CIDR Range*
        * Create a virtual network within an Azure network with public and private subnets
        * CIDR Range of 10.0.0.0/16 = 65,536 IP Addresses
            * lower number (/16) means more IP addresses
        * **Subnets**: logical partition of IP network, breaking up vNet IP range into smaller CIDR ranges, i.e. subnet CIDR Range 10.0.0.0/24 = 256 IP Addresses
            * **Public Subnet**: can reach the internet, e.g. web app
            * **Private Subnet**: cannot reach internet, e.g. database
    * **Azure Load Balancer**: OSI Level 4 (Transport, lower-level) load balancer
    * **Azure Application Gateway**: OSI Level 7 (HTTP, app-level) load balancer, can apply Web Application Firewall service
    * **Networking Security Groups**: virtual firewall at subnet level to secure ports

* Enterprise/Hybrid Networking Services
    * **Azure Front Door**: scalable, secure entry point for fast delivery of global applications
    * **Azure Express Route**: connection between on-premise to Azure Cloud, 50 MB/s to 10 GB/s
    * **Virtual WAN**: networking service that brings networking, security, and routing functionalities together to provide single operational interface
    * **Azure Connection**: VPN connection securely connecting two Azure local networks via IPSec
    * **Virtual Network Gateway**: site-to-site VPN connection between Azure virtual network and local network.

* **Azure Traffic Manager**: operates at DNS layer to direct incoming DNS requests based on routing method of your choice, i.e. weighted, performance, priority, geographic, multivalue, subnet
    * Route traffic to servers geographically near to reduce latency
    * Fail-over to redundant systems in case primary systems failure
    * Route to random VM to simulate A/B testing

* **Azure DNS**: host domain names on Azure, create DNS Zones, manage DNS records

* **Azure Load Balancer**: evenly distribute incoming network traffic across a group of backend resources or servers, operates on OSI Layer 4 (Transport), does not understand HTTP requests
    * Public Load Balancer: routes incoming traffic from internet to public-facing servers (Public IPs)
    * Private Load Balancer: routes incoming traffic from internet to private-facing servers (Private IPs)

* **Scale Sets**: group identical VMs and automatically increase or decrease amount of servers based on:
    * change in CPU, memory, disk, network performance
    * on a predefined schedule
    * used for *Elasticity*

* IoT Services
    * **Internet of Things (IoT)**: network of internet connected objects able to collect and exchange data
    * **IoT Central**: connects IoT devices to cloud
    * **IoT Hub**: secure and reliable communication between IoT app and managed devices
    * **IoT Edge**: fully managed IoT Hub service, data processing and analysis nearest the IoT devices
        * **Edge Computing**: offload compute from cloud to local computing hardware, IoT devices
    * **Windows 10 IoT Core Services**: subscription for long-term OS support and services for Windows 10 IoT Core devices

* Big Data and Analytics Services
    * **Big Dat**a: massive volumes of structured and unstructured data that are difficult to move and process using traditional techniques
    * **Azure Synapse Analytics:** enterprise *data warehousing and big data analytics*, run SQL queries against large databases for *reporting*
    * **HDInsight**: open-source analytics software for Hadoop, Kafka, Spark
    * **Azure Databricks**: Spark-based analytics platform on Azure cloud
    * **Data Lake Analytics**: on-demand analytics job service that simplifies big data
        * **Data Lake**: storage repo that holds large amounts of raw data

* AI/ML Services
    * **Artificial Intelligence (AI)**: machines perform jobs that mimic human behavior
    * **Machine Learning (ML)**: machines that get better at a task without explicit programming
    * **Deep Learning (DL)**: machines have artificial neural network to solve complex problems
    * **Azure Machine Learning Service**: service to run AI/ML workloads to build flexible pipelines to automate workflows
    * **Personalizer**: personalized experiences for every user
    * **Translator**: real-time translation
    * **Anomaly Detector**: detect anomalies to quickly identify and troubleshoot issues
    * **Azure Bot Service**: serverless bot services that scales on demand
    * **Form Recogniser**: automate extraction text, key/value pairs, and tables
    * **Computer Vision**: customize computer vision models
    * **Language Understanding**: build natural language understanding into apps
    * **QnA Maker**: create conversational QnA bot from existing content
    * **Text Analytics**: extract sentiment, key phrases, named entities, language from text
    * **Content Moderator**: moderate text and images for safer user experience
    * **Face**: detect and identify people and emotions in images
    * **Ink Recogniser**: recognize digital handwritten content

* Serverless Services
    * **Serverless**: underlying servers, infrastructure, and OS fully managed by service provider, highly available, scalable, cost-effective
        * **Event Driven Scale**: serverless function triggered by or trigger other events to compose complex, scaling applications
        * **Abstraction of Servers**: code described as functions, running on different compute instances
        * **Micro-Billing**: bill in micro-seconds of compute time
    * **Azure Functions**: run small amounts of code, serverless functions
    * **Blog Storage**: serverless object storage
    * **Logic Apps**: build serverless workflows, state machines for serverless compute
    * **Event Grid**: use Pub/Sub messaging system to react and trigger events using Azure Cloud

## Management Tools
* **Azure Portal**: web-based, unified console to access Azure Cloud services
* **Azure Preview Portal**: portal to utilize new features that are not fully released
* **Azure Powershell**: manage Azure resources directly from Powershell CLI
    * **Powershell**: command-line shell and scripting language for task automation and configuration management framework, built on .NET Common Language Runtime (CLR)
* **Visual Studio Code**: source-code editor that can be run on Azure Cloud
* **Azure Cloud Shell**: interactive, authenticated, browser-accessible shell for managing Azure resources using PowerShell or Bash
* **Azure CLI**: type `az` followed by other commands to manage Azure resources
    * **Command Line Interface (CLI)**: process commands to computer program using lines of text, implemented using a shell or terminal

## Security
* **Azure Trust Center**: website portal to provide information on privacy, security, and regulatory compliance on Azure
* **Compliance Programs**: enterprise companies may specify compliance programs required for applications built on Azure
* **Azure Active Directory (Azure AD)**: *identity and access management service*, helps sign-in and access internal and external resources, *Single-Sign On (SSO)*
* **Multi-Factor Authentication (MFA)**: security control that uses a second device to confirm user logins, protect against stolen passwords
* **Azure Security Center**: unified infrastructure security management system to evaluate security of data centers
* **Key Vault**: safeguard cryptographic keys by cloud apps and services
    * **Secrets Management**: control access to tokens, passwords, certificates, API keys, and other secrets
    * **Key Management**: control encryption keys
    * **Certificate Management**: manage public and private SSL certificates
    * **Hardware Security Module (HSM)**: piece of hardware designed to store encryption keys, data stored in memory not disk and deleted upon device failure
        * Multi-tenant (multiple customers virtually isolated on single HSM) HSMs are FIPS 140-2 Compliant
        * Single-tenant (single customer on dedicated HSM) HSMs are FIPS 140-3 Compliant
    * **Azure DDoS Protection**: free basic protection and paid standard protection ($3k/month) with metrics, alerts, reporting, support, and SLAs
        * **Distributed Denial of Service (DDoS) Attack**: malicious attack by flooding website with large amounts of fake traffic
    * **Azure Firewall**: managed network security service to protect Azure virtual network resources
        * create, enforce, and log application and network connectivity policies *across subscriptions* and virtual networks
        * *static public IP address* for virtual network resources to allow outside firewalls to identify traffic originating from your virtual network
        * high availability, no additional load balancers are required
        * configure deployment to span multiple AZs for increased availability
        * no additional cost for firewall in Availability Zone (AZ)
        * additional costs for inbound and outbound data transfers in AZs
    * **Azure Information Protection (AIP)**: protect sensitive information with encryption restricted access and rights, and integrated security in Office apps
    * **Azure Application Gateway**: web-traffic load balancer (OSI Layer 7 HTTP) to reroute traffic based on set of rules
        * Web Application Firewall (WAF) can be attached for additional protection on OSI Layer 7
    * **Azure Advanced Threat Protection (ATP)**: cloud-based security that leverages on-premises Active Directory to *identify, detect, and investigate advanced threats, compromised identities, and malicious insider actions*
        * **Intrusion Detection System (IDS)/Intrusion Protection System (IPS)**: device or application that monitors network or systems for malicious activity or policy violations
    * **Microsoft Security Development Lifecycle (SDL)**: software security assurance process
        * Training, Requirements, Design, Implementation, Verification, Release, Response
    * **Azure Policy**: create, assign, and manage policies to enforce or control properties of a resource
        * evaluates resources by comparing resource properties to *Policy Definitions*, or  business rules in JSON
    * **Azure Role-Based Access Control (RBAC)**: manage access to Azure resources and permissions using role assignments
        * **Role Assignments**: security principal, role definition, scope
        * **Security Principal**: access identities
            * User: individual profile in Azure Active Directory
            * Group: set of users in Azure Active Directory
            * Service Principal: security identity used by applications or services to access specific Azure resources
            * Managed Identity: identity automatically managed by Azure
        * **Role Definition**: collection of permissions (e.g. read, write, delete), can be high-level or specific
            * Use built-in roles (Owner, Contributor, Reader, User Access Administrator) or custom roles
        * **Scope**: resources available to be accessed by role, controls resource access at Management, Subscription, or Resource Group level
    * **Lock Resources**: prevent other users from accidentally deleting or modifying critical resources of a subscription, resource group, or resource
        * **CanNotDelete**: users can read and modify but not delete resource
        * **ReadOnly**: users can read but not modify or delete resource
    * **Management Groups**: manage multiple subscriptions/accounts in hierarchical structure
        * Each group has top level root and subscriptions inherit conditions of their management group
    * **Azure Monitor**: collect, analyze, and act on telemetry from cloud and on-premises environments
    * **Azure Service Health**: information about current and upcoming issues, e.g. service impacting events, planned maintenance, other availability changes
        * Azure Status: service outage information in Azure
        * Azure Service Health: personalized view of Azure service and region health
        * Azure Resource Health: health of individual cloud resources
    * **Azure Advisor**: personalized cloud consultant for best practices on Azure 
        * provides recommendations in High Availability, Security, Performance, Cost, Operational Excellence

## Billing and Pricing
* **Service Level Agreement (SLA)**: Azure's commitments for uptime and connectivity
    * individualized per Azure service
    * performance targets in uptime and connectivity are in percentages
        * 99% (two nines), 99.9% (three nines), ..., 99.9999999% (nine nines)
    * not available for Free Tier or shared tiers
* **Service Credits**: discount applied as compensation for under-performing Azure product or service based on SLA
* **Composite SLA**: combined SLAs across different service offerings, improved with fallback systems
    * Web App + SQL Database = 99.95% * 99.99% = 99.94% composite
    * Web App + (SQL Database or Queue Fallback) = 99.95% * (99.99999%) = 99.95% composite
* **TCO Calculator**: estimate cost savings of migrating to Azure
* **Azure Marketplace**: apps and services from third-party publishers categorized as Free, Free-Trial, Pay-As-You-Go, or Bring-Your-Own-License (BYOL)
* **Azure Support**
    * Basic: email support for billing and account, Azure Advisor, Health Status, Community Support, Documentation
    * Developer: Basic support, email tech support, third-party software support, minimal business impact response time < 8 hrs, Architecture General Guidance
    * Standard: Developer support, moderate business impact response time < 4 hrs, critical business impact response time < 1 hr
    * Professional Direct: Standard support, minimal business impact response time < 4 hrs, moderate business impact response time < 2 hrs, Architecture, Operational Support, Proactive Guidance from ProDirect delivery managers, Webinars by Azure engineers
    * Enterprise: Professional Direct support, more things
* **Azure Licensing**
    * **Azure Hybrid Use Benefit (HUB)**: Repurpose investment of Window Server licenses to use Azure virtual machines (Windows Servers, SQL Servers), Bring your own license (BYOL)
* **Azure Subscriptions**
    * Student: no credit card required, $100 USD credits for 12 months
    * Free: credit card required, $200 USD credits for 30 days, certain Azure products free for 12 months
    * Pay-as-you-go (PAYG): credit card required, charged monthly based on resource consumption
    * Enterprise Agreement: receive discounted price for licenses and cloud services
* **Azure Pricing Calculator**: configure and estimate costs for Azure products
* **Azure Cost Management**
    * perform cost-analysis, visualize spending on Azure cloud services
    * create budgets, set budget threshold and alerts

## References
[^1]: Footnot