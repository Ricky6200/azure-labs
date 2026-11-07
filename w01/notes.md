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
