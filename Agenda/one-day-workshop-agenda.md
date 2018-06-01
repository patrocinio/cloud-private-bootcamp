One Day IBM Cloud Private Workshop (Draft)
================================================

The one-day workshop is able to touch on a relatively wide array of topics.  
We do not intend to provide hands-on exercises due to the logistics of trying to do that in a one-day workshop.

We do intend to have numerous demonstrations.  The general approach should be to present by demonstration rather than presentation.  Obviously there are times where a good diagram will be useful to communicating important concepts.

# Docker Fundamentals (60 minutes)
Combination presentation, demo

- Overview
- Images
- Containers
- Dockerfile
- Build, Tag, Push
- Running a container

# Kubernetes Fundamentals Part I (60 minutes)
Combination presentation, demo

- Overview
- kubectl
- YAML by example
- Kubernetes objects
  - Deployments
  - Stateful Sets
  - Replicas
  - Daemonsets
  - Services (Cluster, NodePort)
  - Storage (PV, PVCs, StorageClass)
- ICP Workloads and Service UI
- ConfigMaps, Secrets
- Ingress

# Helm (30 min)
- Overview
- PPA
- Helm command line for ICP

# IBM Cloud Private Part I (45 min)
- Overview
- Introduction to the user interface
- Deploying/upgrading Helm charts
- Monitoring/logging
- Application logging
- RBAC, namespaces,
- LDAP directory integration (ICP)
- External service integration (databases & rest services)

# Storage (45 minutes)
- Storage options with ICP
- Trade-offs for each option
- At a minimum cover NFS, GlusterFS & Heketi, IBM Spectrum (GPFS)
- Other storage options

# Microservices Part I (60 minutes)
- Application architecture
  - common architecture patterns (overview)
- Concerns, trade-offs, pitfalls
- Development process overview

# Microservices Part II (60 minutes)
- IBM microservices reference architecture
- Walk through of microservices application implementation
- API Connect overview
- Datapower oveview (optional) *TBD* is this relevant here?

# Database options (60 minutes)
- Databases and microservice architecture
- ACID vs BASE
  - Transactional 2PC
  - Event sourcing pattern
- SQL vs No SQL
- Relational vs Object vs Document
- Compare and contrast:
  - MySQL/MariaDB
  - Cloudant
  - MongoDB
  - Oracle, DB2
  - Others - *TBD*

# Microservices application logging and monitoring (30 minutes)
This may be a separate topic.  Or it may be folded into one of the
above microservices units.
- Kabana
- Logging framework

# DevOps with ICP (60 minutes)
Assumes we can demonstration a CI/CD pipeline with some application
- CI/CD pipeline integration
- Jenkins integration
- Urban Code Deploy integration

# Resource monitoring in ICP (60 minutes)
- Prometheous
- Grafana
- Integration with other monitoring tools (e.g, APM, NetCool) *TBD* This may require a separate unit.

# Log monitoring in ICP (60 minutes)
- ELK Stack
  - Elastic Search
  - Logstash
  - Kabana
- Integration with Splunk

# Other Topics
The topics below are expected to be for a specialized audience.

## IBM Cloud Private Production Deployment (60 minutes)
For the operations team
- HA topology
- Docker storage driver
- Vulnerability Advisor
- Resiliency considerations
- Multi-site considerations
- Backup/restore
- Disaster Recovery


## Application modernization Part I (45 minutes)
For audiences with legacy Java EE applications
- Typical application modernization concerns
  - modernization approaches
  - application cloud readiness assessment
  - estimating level of effort
  - creating a modernization roadmap

## Application Modernization Part II (45 minutes)
For audiences with legacy Java EE applications
- Transformation Advisor (TA)
- Demo of TA

## Deployment automation (45 minutes)
For operations audiences interested in a deployment automation.
*TBD* - Assumes we have a place to demonstrate some of this stuff.
- Terraform
- Ansible
- Cloud Automation Manager *TBD* May need its own unit.  Demo could be difficult, but would be excellent, e.g., deployment to IBM Cloud.

## Deploying ICP to public clouds (30 minutes)
For audiences interested in deploying ICP to public clouds
*TBD* - Is this ready for prime time?
- Terraform for AWS, IBM Cloud (SoftLayer), Azure
