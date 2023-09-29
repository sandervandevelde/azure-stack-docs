---
title: AKS hybrid data collection
description: Learn about the data exchanged between Kubernetes clusters and Azure.
author: sethmanheim
ms.topic: conceptual
ms.date: 09/12/2023
ms.author: sethm 
ms.reviewer: baziwane

# Intent: As an IT Pro, I want to learn about the data exchanged between Kubernetes clusters and Azure.
# Keyword: security concepts infrastructure security

---

# AKS hybrid data collection

[AKS hybrid](overview.md) is a service that enables you to run Kubernetes clusters in your own infrastructure, using Azure Arc to connect and manage them. AKS hybrid collects data from the clusters and connected machines to provide you with features such as monitoring, policy enforcement, and security updates. This article explains what data is collected, how it's classified, and how you can control it.

During the deployment of AKS hybrid, you must furnish a subscription and an Azure region in which data is stored. The Azure region is a virtual representation of your on-premises resources and doesn't correspond to the actual physical on-premises location. It represents the region with Microsoft-operated datacenters that store this data.

> [!IMPORTANT]
> Microsoft does not collect any sensitive information that may be classified as [Personally Identifiable Information](https://www.microsoft.com/microsoft-365-life-hacks/privacy-and-safety/what-is-pii) (PII). For more information, see the following [data collection section](#data-collection-and-residency).

There are three separate tiers to consider when curating data collection and exchange for on-premises deployments. This article describes the data exchanged between Kubernetes clusters (Tier 2) and Azure. See the public documentation for descriptions of data collection and
exchange between [tier 1](/azure/azure-arc/kubernetes/conceptual-data-exchange) and [tier 3](/azure-stack/hci/concepts/data-collection).

- Tier-1: Azure Arc-enabled services such as Azure Monitor, Azure Defender, Event Grid, etc.
- Tier-2: Kubernetes clusters - AKS hybrid.
- Tier-3: Physical host, such as Windows Server or Azure Stack HCI.

## Data collection and residency

AKS hybrid data is sent in JSON format, and is stored in a secure Microsoft-operated datacenter as follows:

- Billing data is sent to the respective resource of that region in which the customer has registered the device.
- Diagnostic data (classified as support data) is stored within the US or the EU, based on what the customer has opted for at the time of deployment.
- Telemetry data (classified as OII data) is always stored within the US.
- AKS hybrid data is not sent to Microsoft until the cluster is registered with Azure.

For information about how Microsoft stores diagnostic data in Azure, see [Data residency in Azure](https://azure.microsoft.com/global-infrastructure/data-residency/).

## Data retention

AKS hybrid does not store data outside the region selected during installation. After AKS hybrid collects this data, it's retained for 28
days. Aggregated, de-identified data may be kept longer.

## What data is collected?

AKS hybrid collects the following types of data:

- **Events related to the Hyper-V host operating systems**: Details such as the operating system name, version, and model. Identifiers     include event names and event dates for precise event tracking. Various flags, both integer and Boolean, denote specific conditions or statuses, device, and operating system attributes. These flags include the name, device ID, and ISO country code. The data schema for these events incorporates a range of data types, including strings, integers, datetimes, and Booleans.
- **Events associated with Kubernetes clusters control plane**: Specific metrics include cluster creation timestamps, pods, and node counts, and resource metrics including vCore counts. This data is used for monitoring and management of the Azure Kubernetes Service (AKS) hybrid cluster. The data schema for these events includes a range of data types, including Boolean, string, integer, and double.
- **Events pertaining to Hyper-V host operating system**: Emitted errors are captured for diagnostic and monitoring purposes. The predominant data schema used is the string format to encapsulate both the error message and the associated stack trace. Support is currently extended exclusively to Windows Server and Azure Stack HCI platforms.
- **Events pertaining to Mariner Linux VMs**: Including r, system boot and shutdown, service status changes, kernel messages, application errors, and user authentication activities only for system namespaces.
- **Billing events**: Events related to metering or billing of core usage. This set of events includes the event datetime and the quantity of cores. The data types include datetime for the event timing, and a floating-point number for the quantity.
- **Security events**: Aggregated events related to renewal of digital certificates and the functioning of the Key Management Service (KMS) plugin. These events are crucial for tracking of certificate lifecycles, encryption key statuses, revocations, and renewals. The underlying data schema exclusively employs string data types to encapsulate this vital information.

  > [!NOTE]
  > All events use either the Windows Universal Telemetry Client (UTC) or the Mariner Azure Device Health Service (ADHS).

For more information about Azure data collection and privacy policies, see the [Microsoft Privacy Statement](https://privacy.microsoft.com/privacystatement).

## Next steps

[Security concepts in AKS hybrid](concepts-security.md)