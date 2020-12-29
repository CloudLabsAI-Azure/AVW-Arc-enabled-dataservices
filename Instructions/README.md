# Azure Arc Enabled Data Services

## Overview

### What is Azure Arc?

For customers who want to simplify complex and distributed environments across on-premises, edge, and multi-cloud, [Azure Arc](https://azure.microsoft.com/services/azure-arc/) enables deployment of Azure services anywhere and extends Azure management to any infrastructure.

  - Organize and govern across environments - Get sprawling Windows and Linux servers and Kubernetes clusters under control by centrally organizing and governing from Azure-across clouds, data centers, and edge.
  - Manage Kubernetes Apps at scale - Deploy and manage Kubernetes applications across environments using DevOps techniques. Ensure that applications are consistently deployed and configured at scale from source control.
  - Run Azure data services anywhere - Get the latest cloud innovation and automation, elastic scale, and unified management for data workloads that are running across hybrid infrastructure. Ensure consistency in data governance and security and manage costs efficiently.

### How Azure Arc Enabled Data Services add value?

Azure Arc enabled data services component brings the managed data services such as SQL managed instance and PostgreSQL Hyperscale to hybrid and multi-cloud environments based on Kubernetes clusters. The value proposition of Azure Arc enabled data services articulates around:

-	Always current
-	Elastic scale
-	Self-service provisioning
-	Unified management
-	Disconnected scenario support

Since they are managed services, they receive periodic updates, patches, and new features from Microsoft. With this, On-premises databases can stay up to date while ensuring that customers maintain control. Because Azure Arc enabled data services are a subscription service, customers will no longer face end-of-support situations databases.
PostgreSQL Hyperscale deployed as a part of Arc delivers a Postgres database engine running in Kubernetes clusters on-premises, at the edge, and in public clouds. It supports self-service provisioning, elastic scale, unified management, and disconnected scenarios. 

Azure Arc enabled SQL Managed Instance has near 100% compatibility with the latest SQL Server database engine. It allows existing SQL Server customers to lift and shift applications to Azure Arc data services with minimal application and database changes while maintaining data sovereignty. 

## Hands-on Labs Scenario

The following labs provide you a quick and easy way to get started with Azure Arc through virtual environments that do not require any complex set-up or installations. For the purposes of these exercises, let’s consider Contoso, a large manufacturing organization. Their IT systems run Windows, Linux, Kubernetes, and SQL Servers across multiple locations, including on-premises data centers, distribution centers, and multiple public clouds. This poses operational challenges for Contoso. They’d like a consistent way to govern and operate across these disparate environments, ensure security across the entire organization, and enable innovation and developer agility (especially with their investments in cloud-native practices), all while meeting regulatory and compliance requirements.

With Azure Arc, Contoso can unify inventory, governance, and compliance ensuring consistency for their entire IT landscape. They already take advantage of the core management capabilities such as tagging, update management, governance with Azure Policy, monitoring with Azure Monitor, security with Azure Security Center and Azure Sentinel, and more provided by the Azure Resource Manager for their Azure workloads but would like to extend these same capabilities to their resources outside Azure. By onboarding their servers and Kubernetes clusters running outside Azure to Azure Arc, Contoso can take advantage of all the ARM capabilities mentioned above. In addition, with Azure Arc enabled Kubernetes, Contoso can guarantee Kubernetes deployments and app consistency through GitOps-based configuration management for their Kubernetes clusters in Azure, other clouds, and on-premises.


## Lab Context

Leveraging Azure Arc enabled data services, Contoso is interested in implementing cloud-native, evergreen versions of SQL and PostgreSQL Hyperscale to reduce the management overhead and deploy their applications and databases anywhere with elastic scale.

Let’s take the journey together with Contoso and see how easy it is to accomplish all the above with Azure Arc.


