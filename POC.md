Proof of Concept: Setting Up Azure ARC on Windows and Linux Servers
Introduction
This document outlines the process of setting up Azure ARC on Windows and Linux servers with the following items. 

- Azure subscription – with proper permissions
- Supported systems and local access.
- Network Connectivity 
- Security Hardening
**Azure Subscription Setup**
![image](https://github.com/MSJosh/ARC/assets/120500937/e76b2683-ad89-4233-8878-78f79fd54bc4)

Azure landing zones were designed with hybrid and multi-cloud in mind. To support hybrid and multi-cloud, the reference architecture requires two additions:
- Hybrid and multi-cloud connectivity: Understand key network design considerations and recommendations for working with Azure Arc.
- Unified operations: Include Azure Arc-enabled resources to extend your governance and operations support with consistent tooling.
-  [Azure Arc landing zone accelerator for hybrid and multicloud](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/scenarios/hybrid/enterprise-scale-landing-zone)


**Permissions required to deploy Azure Arc.**
- Azure Connected Machine Onboarding Role: This role is required to onboard servers to Azure. This role can only be used to use onboard servers and cannot re-onboard or delete the server resource.
- Azure Connected Machine Resource Administrator Role: This role is required to read, modify, and delete a machine.
- Local Administrator or Root Access: To install and configure the Azure Connected Machine agent, you must have an account with elevated privileges (that is, an administrator or as root) on the machines. [Connect hybrid machines to Azure using a deployment script](https://learn.microsoft.com/en-us/azure/azure-arc/servers/onboard-portal)
- When connecting a large set of machines, it is recommended to utilize a service principal to handle onboarding without requiring manual interaction. [Connect hybrid machines to Azure at scale](https://learn.microsoft.com/en-us/azure/azure-arc/servers/onboard-service-principal)

**Supported Operating Systems**
Azure Arc supports the following Windows and Linux operating systems. Only x86-64 (64-bit) architectures are supported. The Azure Connected Machine agent does not run on x86 (32-bit) or ARM-based architecture.
- Amazon Linux 2 and 2023
- Azure Linux (CBL-Mariner) 1.0, 2.0
- Azure Stack HCI
- CentOS Linux 7 and 8
- Debian 10, 11, and 12
- Oracle Linux 7 and 8
- Red Hat Enterprise Linux (RHEL) 7, 8 and 9
- Rocky Linux 8 and 9
- SUSE Linux Enterprise Server (SLES) 12 SP3-SP5 and 15
- Ubuntu 16.04, 18.04, 20.04, and 22.04 LTS
- Windows 10, 11 [see client operating system guidance](https://learn.microsoft.com/en-us/azure/azure-arc/servers/prerequisites#client-operating-system-guidance)
- Windows IoT Enterprise
- Windows Server 2008 R2 SP1, 2012, 2012 R2, 2016, 2019, and 2022
  - Both Desktop and Server Core experiences are supported
  - Azure Editions are supported on Azure Stack HCI
    
[Supported operating systems](https://learn.microsoft.com/en-us/azure/azure-arc/servers/prerequisites#supported-operating-systems)

**Network Connectivity**

![image](https://github.com/MSJosh/ARC/assets/120500937/164c9343-385b-4d10-b239-6942decc40c5)

[Network topology and connectivity for Azure Arc-enabled servers](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/scenarios/hybrid/arc-enabled-servers/eslz-arc-servers-connectivity)

•	All connections are TCP unless otherwise specified.
•	All HTTP connections use HTTPS and SSL/TLS with officially signed and verifiable certificates.
•	All connections are outbound unless otherwise specified.

**Azure ARC Connectivity **

| Agent resource  |Description | When required  |Endpoint used with private link|
| ------------- | ------------- | ------------- | ------------- |
|aka.ms | Used to resolve the download script during installation | At installation time, only | Public |
| download.microsoft.com  | Used to download the Windows installation package  | At installation time, only  | Public  |
| packages.microsoft.com | Used to download the Linux installation package | At installation time, only | Public  |
| login.windows.net  | Microsoft Entra ID  | Always  | Public |
| login.microsoftonline.com  | Microsoft Entra ID | Always  | Public  |
| pas.windows.net  | Microsoft Entra ID  | Always  | Public  |
| management.azure.com  | Azure Resource Manager - to create or delete the Arc server resource  | When connecting or disconnecting a server, only  | [Public, unless a resource management private link is also configured](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/create-private-link-access-portal)  |
| *.his.arc.azure.com | Metadata and hybrid identity services  | Always  | Private |
| *.guestconfiguration.azure.com |Notification service for extension and connectivity scenarios  | Always  | Public  |
| *.guestnotificationservice.azure.com | Notification service for extension and connectivity scenarios  | If using SSH or Windows Admin Center from Azure    | Public  |
| azgn*.servicebus.windows.net | Notification service for extension and connectivity scenarios | Always | Public  |
| *.servicebus.windows.net  | For Windows Admin Center and SSH scenarios  | CIf using SSH or Windows Admin Center from Azure  | Public |
| *.waconazure.com  | Content Cell  | Content Cell  | Public  |
| *.blob.core.windows.net| Download source for Azure Arc-enabled servers’ extensions | Always, except when using private endpoints  |Not used when private link is configured  |
| dc.services.visualstudio.com | Agent telemetry  | Optional, not used in agent versions 1.24+  | Public  |
|san-af-<region>-prod.azurewebsites.net  | Azure Arc data processing service  | For SQL Server enabled by Azure Arc. The Azure Extension for SQL Server uploads inventory and billing information to the data processing service. | Public  |






| Content Cell  | Content Cell  | Content Cell  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  | Content Cell  |
