# W01 - Cloud computing, shared responsibility, cloud models

## 1. Shared responsibility

**Runtime** = the Java/JDK version, Tomcat, the platform your code runs on.

### My prediction (before reading the module)

| Model   | OS patching | Runtime | App code | Data | Identity   |
|---------|-------------|---------|----------|------|------------|
| On-prem | You         | You     | You      | You  | You        |
| IaaS    | You         | You     | You      | You  | Shared/You |
| PaaS    | MS          | Shared  | You      | You  | Shared     |
| SaaS    | MS          | MS      | MS       | You  | Shared     |

### Corrected (after the module)

| Model   | OS patching | Runtime   | App code  | Data | Identity |
|---------|-------------|-----------|-----------|------|----------|
| On-prem | You         | You       | You       | You  | You      |
| IaaS    | You         | You       | You       | You  | **You**  |
| PaaS    | Microsoft   | **Microsoft** | You   | You  | Shared   |
| SaaS    | Microsoft   | Microsoft | Microsoft | You  | Shared   |

**What I got wrong:**
- **IaaS identity = You** - on IaaS I run the identity/directory infrastructure on my own VMs.
- **PaaS runtime = Microsoft** - App Service installs and patches the JDK/Tomcat; I only choose the version.

**Rules to remember:**
- **Always me:** data, devices, accounts/identities (who has access, passwords, MFA).
- **Always Microsoft:** physical datacenter, network and hosts.
- **Identity "Shared"** in PaaS/SaaS = Microsoft runs the identity platform (Microsoft Entra ID); I manage users and permissions.

### Question: who patches the OS for Azure App Service?

- **My prediction:** Microsoft
- **Module says:** Microsoft
- **Was I right? Why:** Yes. App Service is a PaaS service, so the underlying operating system is managed and patched by Microsoft.

## 2. Cloud models - use cases (Java + SQL stack)

### Public cloud

1. **Scenario:** A startup builds an e-commerce website using Java Spring Boot and SQL.
   - **Why public:** Lowest upfront cost, rapid deployment, automatic scaling.
   - **Azure services:** Azure App Service (Java app), Azure SQL Database, Azure Storage, Azure Front Door
2. **Scenario:** A company launches a new customer portal but doesn't know future traffic.
   - **Why public:** Scale from hundreds to millions of users without buying hardware.
   - **Azure services:** Azure Kubernetes Service (AKS), Azure SQL Database, Azure Load Balancer, Azure Monitor
3. **Scenario:** Development and testing environment for Java developers.
   - **Why public:** Environments can be created and destroyed on demand.
   - **Azure services:** Azure Virtual Machines, Azure DevTest Labs, Azure SQL Database, Azure Repos / GitHub

### Private cloud

1. **Scenario:** A bank runs core banking applications containing highly regulated customer data.
   - **Why private:** Strict regulatory and security requirements.
   - **Setup:** Company's own datacenter, VMware/OpenStack infrastructure, Java application servers, SQL Server/Oracle databases
2. **Scenario:** Defense or military systems handling classified information.
   - **Why private:** Data cannot leave organization-controlled infrastructure.
   - **Setup:** Isolated datacenter, dedicated networking, internal Java services, private database clusters
3. **Scenario:** Manufacturing plant with applications that must keep running even if internet connectivity is lost.
   - **Why private:** Local control and low latency.
   - **Setup:** On-prem servers, local SQL databases, internal Java services, private storage and networking

### Hybrid cloud

1. **Scenario:** Existing banking application cannot move customer database to cloud yet.
   - **On-prem:** SQL Server database
   - **Azure:** Java web/API layer
   - **Azure services:** Azure App Service, VPN Gateway, Azure Monitor
2. **Scenario:** A company uses on-prem applications but wants cloud backup and disaster recovery.
   - **On-prem:** Production Java application and SQL Server
   - **Azure:** Backup and DR copies
   - **Azure services:** Azure Backup, Azure Site Recovery, Azure Storage
3. **Scenario:** Seasonal workload (e.g., retail during Diwali/Black Friday).
   - **On-prem:** Normal workload
   - **Azure:** Extra capacity during peak periods (cloud bursting)
   - **Azure services:** Azure Virtual Machines, Azure Kubernetes Service (AKS), Azure Load Balancer, Azure SQL Managed Instance

## 3. 60-second explanations

Structure for each: **definition -> who controls the hardware -> one advantage + one disadvantage -> example**.

### Quick version

- **Public cloud:** Microsoft owns the hardware, shared by many customers; no upfront cost, pay for what you use (OpEx); less infrastructure control. Example: Java app + Azure SQL fully in Azure.
- **Private cloud:** Used by one organisation only (own datacenter or dedicated third-party hosting); most control, but high upfront cost (CapEx). Example: bank running Java and SQL in its datacenter.
- **Hybrid cloud:** Public + private connected so data and apps can move between them; flexible but more complex. Example: Java app in Azure, database on-premises.

### Public cloud

> Public cloud means the infrastructure is owned and run by a provider like Microsoft and shared by many customers, with each customer's data kept isolated. I don't buy any hardware; I pay only for what I use, so cost moves from CapEx to OpEx. The biggest advantages are speed and scale: I can create a server in minutes and scale up on busy days. The downside is less control over the hardware, and some strict regulations may not allow it. For example, a new Java Spring Boot e-commerce site on Azure App Service with Azure SQL Database: no upfront investment, and it scales automatically when traffic grows.

### Private cloud

> Private cloud means cloud resources used by one organisation only. It's usually in the company's own datacenter, but it can also be hosted by a third party as long as it's dedicated to that company. The advantage is full control over hardware, security and data location, which helps with strict compliance. The disadvantage is cost and effort: the company buys and maintains the hardware upfront (CapEx), needs its own IT team, and scales more slowly. For example, a bank runs its core Java applications and SQL Server databases in its own datacenter because regulations require tight control of customer data.

### Hybrid cloud

> Hybrid cloud combines public and private cloud, connected together so data and applications can move between them. The company keeps sensitive or regulated systems on-premises and uses Azure for everything that benefits from flexibility. The advantages are flexibility, a gradual migration path, and cloud bursting: using Azure's extra capacity at peak times. The disadvantage is complexity, because you manage two environments plus a secure connection like a VPN or ExpressRoute. For example, a company keeps its customer SQL Server database on-premises for compliance and moves the Java web/API layer to Azure App Service, connected through a VPN Gateway.
