# AKS Updates and Upgrades

## 1. Types of upgrades and updates for AKS
Upgrades and updates related to AKS are of 3 types: <br />

1. **AKS cluster upgrades** (Upgrading of the AKS versions in control plane and nodepools) -- New minor update available almost every 3 months <br />
2. **AKS node image upgrades** (OS patching of the AKS nodes) -- New OS image available almost every 1 week <br />
3. **AKS control plane maintenance** (Updates related to the patching of the control plane and related pods) -- Can occur any time <br /><br />
 
For all the upgrade types, the lower environments (non-production) can be upgraded first and the applications can be sanity checked after the upgrade. If the sanity check goes okay, the same upgrade can be then applied to the production environment. <br /><br />

## 2.	AKS cluster upgrades flow (point 1.1)
Frequency at which this upgrade needs to be performed: At least once in every 3 months <br /><br />
 
Two ways in which one comes to know if an action needs to be taken as far as AKS cluster upgrade is concerned is as follows:<br />
1.	Availability of the new release on the AKS’ GitHub release page - https://github.com/Azure/AKS/releases <br />
2.	Notification from the Azure Security Center that the AKS cluster should be upgraded to a supported version <br />
3.	In case automation is desired, AKS upgrades can be checked using the command: <br />
**az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table** <br /><br />
 
AKS cluster upgrades can be done manually as well as in an automated way. <br /><br />
 
More information on performing the AKS cluster upgrades here - https://docs.microsoft.com/en-us/azure/aks/upgrade-cluster <br /><br />
 
If auto-upgrade of the AKS cluster is desired, then there is a feature called **Auto-Upgrade Channel** in preview. Setting the **Auto-Upgrade Channel** enables auto-upgrade of the entire AKS cluster (using channels **patch/stable/rapid**). Please note that upgrade of the AKS cluster also automatically involves performing the AKS node image upgrade (point 1.2).<br /><br />
 
More information on the AKS cluster auto-upgrades here - https://docs.microsoft.com/en-us/azure/aks/upgrade-cluster#set-auto-upgrade-channel <br /><br />
 
There is also a feature in preview called Planned Maintenance. It allows the AKS operator to mention the time window for the planned maintenance of the AKS control plane. However, if the Planned Maintenance is enabled along with Auto-Upgrade Channel, then the AKS cluster gets auto-upgraded during the mentioned Planned Maintenance window. <br /><br />
 
More information on the Planned Maintenance here - https://docs.microsoft.com/en-us/azure/aks/planned-maintenance <br /><br />
 
It is recommended to set the optimum value for max node surge for each nodepool. <br /><br />

## 3.	Node image upgrades flow (point 1.2)
Frequency at which node image upgrade needs to be done :  Every week if possible <br /><br />
 
Available upgrades for the node image can be checked using the command: <br />
**az aks nodepool get-upgrades \ <br />
    --nodepool-name mynodepool \ <br />
    --cluster-name myAKSCluster \ <br />
    --resource-group myResourceGroup** <br /><br />
 
More information on the AKS node image upgrades here - https://docs.microsoft.com/en-us/azure/aks/node-image-upgrade <br /><br />
 
If auto-upgrade of the AKS node image is desired, then the below options can be used: <br />
a.	Automation using a Linux based Cron Job or GitHub Actions. More information here --  https://docs.microsoft.com/en-us/azure/aks/node-upgrade-github-actions  <br />
b.	There is a feature called **Auto-Upgrade Channel (node-image channel)** in preview. More information here -- https://docs.microsoft.com/en-us/azure/aks/upgrade-cluster#set-auto-upgrade-channel . <br />
There is also a feature in preview called **Planned Maintenance**. It allows the AKS operator to mention the time window for the planned maintenance of the AKS control plane. <br /> However, if the **Planned Maintenance** is enabled along with **Auto-Upgrade Channel (node-image channel)**, then the AKS node image gets auto-upgraded during the mentioned Planned Maintenance window. <br /><br />
 
It is recommended to set the optimum value for max node surge for each nodepool. <br /><br />

## 4.	AKS control plane maintenance (point 1.3)
AKS control plane maintenance occurs automatically. However, a maintenance window can be provided for the same using a feature - Planned Maintenance. <br />
The feature is currently in preview. More information here -- https://docs.microsoft.com/en-us/azure/aks/planned-maintenance <br /><br />
 
In absence of this feature, currently the maintenance happens automatically. During this time – some alerts and metrics may show downtime as well as some HPA/operations may not work.<br /><br />


## **Some of the views are personal. Reader's discretion advised. 




