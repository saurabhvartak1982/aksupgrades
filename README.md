# AKS Updates and Upgrades
AKS related upgrades need to be triggered by the users. AKS also needs to be in the supported range of versions. 

## 1. Types of upgrades and updates for AKS
Upgrades and updates related to AKS are of 3 types: <br />

1. **AKS cluster upgrades** (Upgrading of the AKS versions in control plane and nodepools) -- New minor update available almost every 3 - 4 months. Patch versions usually come in the frequency of 1 month or a few times within a month. Control plane upgrade includes the System Pods running on Nodes as well.<br />
Considering the semanting versioning covention of Major (X).Minor(Y).Patch(Z) -- <br/>
Minor versions supported are the latest and the previous 2 -- Y, Y-1, Y-2. 
Each Minor version supports 2 of the latest stable patch versions -- Z, Z-1. <br /><br /> 

2. **AKS node image upgrades** (OS patching of the AKS nodes) -- New OS image available almost every 1 week for the Linux OS and almost every 1 month for the Windows OS. Data plane upgrades including kubelet, containerd etc. and the same happen using OS re-imaging.<br /><br />

3. **AKS control plane maintenance** (Updates related to the patching of the control plane and related pods) -- Can occur any time <br /><br />
 
For all the upgrade types, the lower environments (non-production) can be upgraded first and the applications can be sanity checked after the upgrade. If the sanity check goes okay, the same upgrade can be then applied to the production environment. <br /><br />

## 2.	AKS cluster upgrades flow (point 1.1)
Frequency at which this upgrade needs to be performed: At least once in every month, considering a new patch version is released almost every month. <br /><br />
 
Two ways in which one comes to know if an action needs to be taken as far as AKS cluster upgrade is concerned is as follows:<br />
1.	Availability of the new release on the AKS’ GitHub release page - https://github.com/Azure/AKS/releases <br />
2.	Notification from the Azure Security Center that the AKS cluster should be upgraded to a supported version <br />
3.	In case automation is desired, AKS upgrades can be checked using the command: <br />
**az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table** <br /><br />
 
AKS cluster upgrades can be done manually as well as in an automated way. <br /><br />
 
More information on performing the AKS cluster upgrades here - https://docs.microsoft.com/en-us/azure/aks/upgrade-cluster <br /><br />
 
If auto-upgrade of the AKS cluster is desired, then there is a feature called **Auto-Upgrade Channel**. Setting the **Auto-Upgrade Channel** enables auto-upgrade of the entire AKS cluster (using channels **patch/stable/rapid**). Please note that upgrade of the AKS cluster also automatically involves performing the AKS node image upgrade (point 1.2).<br /><br />
 
More information on the AKS cluster auto-upgrades here - https://learn.microsoft.com/en-us/azure/aks/auto-upgrade-cluster <br /><br />
 
There is also a feature in preview called Planned Maintenance. It allows the AKS operator to mention the time window for the planned maintenance of the AKS control plane. However, if the Planned Maintenance is enabled along with Auto-Upgrade Channel, then the AKS cluster gets auto-upgraded during the mentioned Planned Maintenance window. <br /><br />
 
More information on the Planned Maintenance here - https://docs.microsoft.com/en-us/azure/aks/planned-maintenance <br /><br />
 
It is recommended to set the optimum value for max node surge for each nodepool and also the appropriate value for Pod Disruption Budgets. <br /><br />

## 3.	Node image upgrades flow (point 1.2)
Frequency at which node image upgrade needs to be done :  Every week if possible for Linux and every month if possible for Windows <br /><br />
 
Available upgrades for the node image can be checked using the command: <br />
**az aks nodepool get-upgrades \ <br />
    --nodepool-name mynodepool \ <br />
    --cluster-name myAKSCluster \ <br />
    --resource-group myResourceGroup** <br /><br />
 
More information on the AKS node image upgrades here - https://docs.microsoft.com/en-us/azure/aks/node-image-upgrade <br /><br />
 
If auto-upgrade of the AKS node image is desired, then the below options can be used: <br />
a.	Automation using a Linux based Cron Job or GitHub Actions. More information here --  https://docs.microsoft.com/en-us/azure/aks/node-upgrade-github-actions  <br />
b.	There is a feature called **Auto-Upgrade Channel (node-image channel)**. More information here -- https://learn.microsoft.com/en-us/azure/aks/auto-upgrade-cluster#using-auto-upgrade . <br />
There is also a feature in preview called **Planned Maintenance**. It allows the AKS operator to mention the time window for the planned maintenance of the AKS control plane. <br /> However, if the **Planned Maintenance** is enabled along with **Auto-Upgrade Channel (node-image channel)**, then the AKS node image gets auto-upgraded during the mentioned Planned Maintenance window. <br /><br />
 
It is recommended to set the optimum value for max node surge for each nodepool and also the appropriate value for Pod Disruption Budgets. <br />

Kindly note that the node image upgrades are not Kubernetes version upgrades but OS level upgrades for the Nodes. New Node Images may also include the newer versions of System Pods. <br />

Data plane Kubernetes version can stay behind the Control plane version by about 2 versions which is defined in the Kubernetes version skew policy -- https://kubernetes.io/releases/version-skew-policy/#supported-version-skew. <br />

Another important point is related to the Linux Nightly patch (done by Ubuntu or other OS). These are not Kubernetes related patches, but the OS level patches. Some of the patches like Kernel updates may need a reboot. <br />
For such patch updates requiring reboots, the reboot doesnt happen automatically but is indicated by a file which gets generated -- **/var/run/reboot-required**.
**KURED** helps in detecting this file and triggering a reboot. 
However a better way would be to have Node Image upgrade since it would include the newer patches any-way.

## 4.	AKS control plane maintenance (point 1.3)
AKS control plane maintenance occurs automatically. However, a maintenance window can be provided for the same using a feature - Planned Maintenance. <br />
The feature is currently in preview. More information here -- https://docs.microsoft.com/en-us/azure/aks/planned-maintenance <br /><br />
 
In absence of this feature, currently the maintenance happens automatically. During this time – some alerts and metrics may show downtime as well as some HPA/operations may not work.<br /><br />


## **Some of the views are personal. Reader's discretion advised. 




