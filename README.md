# Azure Analysis Services compute usage projection to Microsoft Fabric

##  Introduction 

**What is the goal of this Github repository ?**

The goal of this repository is to provide a **Power BI template** that will connect to **Azure Analysis Services Logs**, and reproduce Microsoft Fabric Smoothing and Bursting logics to : 
- **Project sizing of the target capacity** based on compute and not only on model size to avoid frictions once deployed. 
- **Anticipate overuse** of Microsoft Fabric Capacity and **simulate impact of debt mechanism** based on true usage
- Visualize the **aspiring improvement** of **service availability** thanks to background and interactive smoothing simulation. 

**Requirements :**
- An Azure Analysis Services instance
- Diagnostic Settings enabled on Azure Log Analytics
- Access to this Azure Analytics workspace 

## Scenario

**Is there a way to anticipate the consumption in Microsoft Fabric of a Semantic Model (or Tabular Model) that is deployed in Azure Analysis Services (AAS) ?**

A single model can be deployed in both services, and can be adressed with the same processes and queries. We can compare their consumptions, as there is a way to track all queries sent to an AAS Tabular model [^1] and and queries sent to a Fabric Semantic model [^2]. Both logs speak the same langage, called [Trace Events](https://learn.microsoft.com/fr-fr/analysis-services/trace-events/analysis-services-trace-events?view=sql-analysis-services-2025). 

If I execute the same queries on both services, and compare the results and the results comes even, and if both workloads are equivalent in terms of CPU Usage and duration, then both engines produce the same compute usage. 

Now, knowing that I can monitor in depth the usage of a Microsoft Fabric Semantic Model, I can reproduce the logic of Smoothing and Bursting applied by Microsoft Fabric capacities to compute usage, based on the Fabric Capacity Metrics App and the Log Analytics queries. 

If Microsoft Fabric & AAS produce the same compute usage, it means that I can simulate the same Smoothing and Bursting logic on the real compute usage of an AAS Instance to project what could be the usage of a Tabular model if deployed in Fabric, in anticipation of a migration. 

[^1]:thanks to Diagnostic Settings Logs sent to Azure Log Analytics, 
[^2]:thanks to Log Analytics Workspace link. 
