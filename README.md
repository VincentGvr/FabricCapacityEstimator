# Azure Analysis Services compute usage projection to Microsoft Fabric

##  Introduction 

**What is the goal of this Github repository ?**

The goal of this repository is to provide a **Power BI template** that will connect to **Azure Analysis Services Logs**, and reproduce Microsoft Fabric Smoothing and Bursting logics to : 
- **Project sizing of the target capacity** based on compute and not only on model size to avoid frictions once deployed. 
- **Anticipate overuse** of Microsoft Fabric Capacity and **simulate impact of debt mechanism** based on true usage
- Visualize the **aspiring improvement** of **service availability** thanks to Background and Interactive smoothing simulation. 

**Requirements :**
- An Azure Analysis Services Instance
- Diagnostic Settings Enabled on Azure Log Analytics
- Access to this Azure Analytics workspace 

## Scenario

**Is there a way to anticipate the consumption in Microsoft Fabric of a Semantic Model (or Tabular Model) that is deployed in Azure Analysis Services (AAS) ?**

A single model can be deployed in both services, and can be adressed with the same processes and queries. We can compare their consumptions, as there is a way to track all queries sent to an AAS Tabular model [^1] and and queries sent to a Fabric Semantic model [^2]. Both logs speak the same langage, called [Trace Events](https://learn.microsoft.com/fr-fr/analysis-services/trace-events/analysis-services-trace-events?view=sql-analysis-services-2025). 

If I execute the same queries on both services, and compare the results and the results comes even, and if both workloads are equivalent in terms of CPU Usage and duration, then both engines produce the same compute usage. 
Now, knowing that I can monitor in depth the usage of a Microsoft Fabric Semantic Model, I can reproduce the logic of Smoothing and Bursting applied by Microsoft Fabric capacities to compute usage, based on the Fabric Capacity Metrics App and the Log Analytics queries. 
If Microsoft Fabric & AAS produce the same compute usage, it means that I can simulate the same Smoothing and Bursting logic on the real compute usage of an AAS Instance to project what could be the usage of a Tabular model if deployed in Fabric, in anticipation of a migration. 

[^1]:thanks to Diagnostic Settings Logs sent to Azure Log Analytics, 
[^2]:thanks to Log Analytics Workspace link. 


## Full Logic : 

AAS : S0 server
<img width="1312" height="485" alt="image" src="https://github.com/user-attachments/assets/e46e702a-0256-4c8f-85cc-2073827c27b8" />

Linked to Log Analytics : 
<img width="1077" height="416" alt="image" src="https://github.com/user-attachments/assets/1047a967-e90d-4d99-b0c2-887eca0639a9" />

Fabric : F8 Capacity
<img width="1303" height="319" alt="image" src="https://github.com/user-attachments/assets/0680c9f6-9bba-4849-811d-35fd190eed80" />

Fabric Workspace : 
2 reports pointing towards 2 semantic models :
- AAS to LiveConnected AAS Semantic Model, deployed on Azure Analysis Services 
- Fabric to Imported Fabric Semantic Model 
<img width="1782" height="602" alt="image" src="https://github.com/user-attachments/assets/1fa64b83-0ab0-437f-be94-0668af5394c3" />
<img width="606" height="479" alt="image" src="https://github.com/user-attachments/assets/a62eb409-c838-4976-a510-fc598bd52fec" />

Linked to Log Analytics
<img width="606" height="317" alt="image" src="https://github.com/user-attachments/assets/4c3e248c-42c3-45c5-9280-8d3a49261f4a" />

Before : 

<img width="1440" height="508" alt="image" src="https://github.com/user-attachments/assets/76adddde-c9a4-4ed0-8078-66b979bc1958" />

Génération de requête sur le rapport : 

<img width="1453" height="813" alt="image" src="https://github.com/user-attachments/assets/fdc99ca2-5808-4a57-960d-d1ac9df96989" />

Fabric Capacity Metrics : 

<img width="500" height="298" alt="image" src="https://github.com/user-attachments/assets/1886db8a-a474-435b-a6bf-8af39ac0e79e" />
Detail :

<img width="1437" height="336" alt="image" src="https://github.com/user-attachments/assets/eb672229-95c6-490f-8068-60ccfdeff32e" />
<img width="657" height="726" alt="image" src="https://github.com/user-attachments/assets/b44c3851-be4d-4ccf-a406-f920ce402194" />

In Log Analytics with Request : 

<img width="484" height="259" alt="image" src="https://github.com/user-attachments/assets/b026dd5f-9a75-4733-aaa6-5cf344beea8f" />

41 queries vs 42 ==> 1 missing query = 23.5040 CU(s) ==> 986.1203 + 23.5040 = 1,009.6243


----------------------------------------------

Refreshes : 

<img width="817" height="230" alt="image" src="https://github.com/user-attachments/assets/9f330487-0902-49a4-89ce-181c7d9fd632" />


----------------------------------------------
AAS QPU : 
<img width="1458" height="517" alt="image" src="https://github.com/user-attachments/assets/f0965af2-3192-4069-968f-a24ae1a1e19e" />
