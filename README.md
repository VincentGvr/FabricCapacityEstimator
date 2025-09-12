# Azure Analysis Services compute usage projection to Microsoft Fabric

## Plan 

1. [Introduction](#Introduction)
2. [Scenario](#Scenario)
3. [The outcomes](#The-outcomes)
4. [How to use it](#How-to-use-it)
5. [Upcoming Improvements](#Upcoming-Improvements)

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

## The outcomes

AAS Real Usage : 
<img width="1343" height="757" alt="image" src="https://github.com/user-attachments/assets/31338871-d4fb-4fca-b3b6-512f5e6b12b6" />

Projected Smoothing :  
<img width="2688" height="1509" alt="image" src="https://github.com/user-attachments/assets/4372407d-f7a9-4fdb-a0f2-23cc4739fda1" />

Projected Bursting : 
<img width="2687" height="1504" alt="image" src="https://github.com/user-attachments/assets/3afdce7f-f266-4e05-85f3-809f3fe2014d" />

## How to use it 

Download the .pbit from the repository.

Open the .pbit using Power BI desktop and fill in the parameters respecting the notes. All parameters can be updated after first refresh : 

<img width="1448" height="1175" alt="image" src="https://github.com/user-attachments/assets/c8449122-b073-44e7-a164-c10ba56dc3b1" />

1. Log Analytics Workspace Id is found on the Log Analytics Overview page : 

<img width="1401" height="372" alt="image" src="https://github.com/user-attachments/assets/20fbc91f-12ec-4280-88b3-09521cbb2fea" />

2. serverName is the name of the Azure Analysis Services server
3. databaseName is the name of the model deployed on the Azure Analysis Services server
4. capacitySize is the projected size of the capacity.
5. ratio is the applied ratio coming from cpu to CU(s). May vary depending on the hardware, from best to worst scenario observed.
6. dateFrom is the start date of the Logs projected  
7. dateTo is the end date of the Logs projected

When filled in, click Load. 

Once asked for credentials, select **Professional Account** and Sign In using a user that is authorized reading Logs on the Azure Log Analytics Workspace. 

## Upcoming Improvements

All models of the server projected instead of one.
