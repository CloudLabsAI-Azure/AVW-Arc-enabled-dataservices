# Azure Arc Enabled Data Services

## Scenario

Contoso Inc runs databases on different hardware, across on-premises datacenters, and multiple public clouds, with Microsoft Azure being the primary cloud.
Contoso Inc’s R&D teams are well-invested in containerized databases for their modernized applications. As a result, they are using Kubernetes as their container orchestration platform. Kubernetes is deployed both as self-managed Kubernetes clusters in their on-premises environments and managed Kubernetes deployments in the cloud.
Contoso Inc wants to manage their databases independently of where they run, so they have a single control plane.

## Lab Context

Contoso Inc is looking to implement and utilize Azure Arc to manage their databases

## Overview

### What is Azure Arc?

For customers who want to simplify complex and distributed environments across on-premises, edge and multi-cloud, Azure Arc enables deployment of Azure services anywhere and extends Azure management to any infrastructure.

-	Organize and govern across environments - Get databases, Kubernetes clusters, and servers sprawling across on-premises, edge, and multi-cloud environments under control by centrally organizing and governing from a single place.
-	Manage Kubernetes Apps at scale - Deploy and manage Kubernetes applications across environments using DevOps techniques. Ensure that applications are deployed and configured from source control consistently.
-	Run data services anywhere - Get automated patching, upgrades, security and scale on-demand across on-premises, edge, and multi-cloud environments for your data estate.

With Arc, customers can bring existing infrastructure running on-premises or other public cloud environments into Azure’s fold. Once registered, they can manage virtual machines and Kubernetes clusters deployed in Azure, on-premises environments, and even non-Azure cloud platforms. 

One of Arc’s key capabilities is running a subset of Azure data services in Kubernetes clusters registered with Arc. Azure Arc enabled data services, the service supports running Azure SQL Managed Instance and Azure PostgreSQL Hyperscale in on-premises data centers, multi-cloud, and the edge. 

### How Azure Arc Enabled Data Services adds value?

Azure Arc enabled data services component brings the managed data services such as SQL managed instance and PostgreSQL Hyperscale to hybrid and multi-cloud environments based on Kubernetes clusters. The value proposition of Azure Arc enabled data services articulates around:

-	Always current
-	Elastic scale
-	Self-service provisioning
-	Unified management
-	Disconnected scenario support

Since they are managed services, they receive periodic updates, patches, and new features from Microsoft. With this, On-premises databases can stay up to date while ensuring that customers maintain control. Because Azure Arc enabled data services are a subscription service, customers will no longer face end-of-support situations databases.
PostgreSQL Hyperscale deployed as a part of Arc delivers a Postgres database engine running in Kubernetes clusters in on-premises, at the edge and in public clouds. It supports self-service provisioning, elastic scale, unified management, and disconnected scenarios. 

Azure Arc enabled SQL Managed Instance has near 100% compatibility with the latest SQL Server database engine. It allows existing SQL Server customers to lift and shift applications to Azure Arc data services with minimal application and database changes while maintaining data sovereignty. 

