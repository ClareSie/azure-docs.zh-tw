---
title: 跨區域移動 Azure 資源的支援
description: 列出可跨 Azure 區域移動的 Azure 資源類型
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 01/20/2020
ms.author: raynew
ms.openlocfilehash: 9bc7dc66ccf3049ac878f7871c816e5ade1afde5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "76760703"
---
# <a name="support-for-moving-azure-resources-across-regions"></a>跨區域移動 Azure 資源的支援

本文會確認是否支援將 Azure 資源類型移至另一個 Azure 區域。 

跳至資源提供者命名空間：
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft。授權](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.BatchAI](#microsoftbatchai)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.classiccompute](#microsoftclassiccompute)
> - [Microsoft.classicnetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Container](#microsoftcontainer)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataConnect](#microsoftdataconnect)
> - [Microsoft.DataExchange](#microsoftdataexchange)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLake](#microsoftdatalake)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft.MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft.MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningOperationalization](#microsoftmachinelearningoperationalization)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.Media](#microsoftmedia)
> - [Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.operationsmanagement](#microsoftoperationsmanagement)
> - [Microsoft 對等互連](#microsoftpeering)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PortalSdk](#microsoftportalsdk)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.ProjectOxford](#microsoftprojectoxford)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Scheduler](#microsoftscheduler)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft .Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.SqlVM](#microsoftsqlvm)
> - [Microsoft.Storage](#microsoftstorage)
> - [StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft.TerraformOSS](#microsoftterraformoss)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft Token](#microsofttoken)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [WindowsVirtualDesktop](#microsoftwindowsvirtualdesktop)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- | 
> | domainservices | 否 | 
> | domainservices/replicasets | 否 | 

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | tenants | 否 |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | actionrules | 否 | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | servers | 否 |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 服務 |  是 | 

## <a name="microsoftappconfiguration"></a>AppConfiguration

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | configurationstores | 否 | 

## <a name="microsoftappservice"></a>Microsoft.AppService

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | apiapps | 否 | 
> | appidentities | 否 | 
> | gateways | 否 | 


## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | policyassignments | 否 |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | automationaccounts | 否 | 
> | automationaccounts/設定 | 否 | 
> | automationaccounts/runbook | 否 | 



## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | b2cdirectories | 否 | 

## <a name="microsoftazuredata"></a>AzureData

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | sqlserverregistrations | 否 |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | registrations | 否 | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | batchaccounts | 否 |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | clusters | 否 | 
> | fileservers | 否 | 
> | jobs | 否 | 
> | workspaces | 否 | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | mapapis | 否 | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | biztalk | 否 | 

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | blockchainmembers | 否 |
> | 監看員 | 否 | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | blueprintassignments | 否 | 

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | botservices | 否 | 

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | redis | 否 | 


## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | cdnwebapplicationfirewallpolicies | 否 |
> | profiles | 否 | 
> | 設定檔/端點 | 否 | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | certificateorders | 否 | 


## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | domainnames | 否 |  
> | virtualmachines | 否 | 



## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | networksecuritygroups | 否 |
> | reservedips | 否 | 
> | virtualnetworks | 否 | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | storageaccounts | 是 |  


## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 | 否 | 

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | availabilitysets | 否 | 
> | diskencryptionsets | 否 | 
> | disks | 否 | 
> | galleries | 否 | 
> | 資源庫/影像 | 否 | 
> | 資源庫/影像/版本 | 否 | 
> | hostgroups | 否 | 
> | hostgroups/主機 | 否 | 
> | images | 否 | 
> | proximityplacementgroups | 否 | 
> | restorepointcollections | 否 | 
> | sharedvmimages | 否 | 
> | sharedvmimages/版本 | 否 | 
> | snapshots | 否 | 
> | virtualmachines | 是 | 
> | virtualmachines/extensions | 否 | 
> | virtualmachinescalesets | 否 | 

## <a name="microsoftcontainer"></a>Microsoft.Container

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | containergroups | 否 | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | containergroups | 否 | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | registries | 否 |  
> | 登錄/buildtasks | 否 |  
> | 登錄/複寫 | 否 | 
> | 登錄/工作 | 否 |  
> | 登錄/webhook | 否 | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | containerservices | 否 | 
> | managedclusters | 否 | 
> | openshiftmanagedclusters | 否 | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 應用程式所需 | 否 | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 | 否 | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 連接器 | 否 |  

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | hubs | 否 |  

## <a name="microsoftcustomproviders"></a>CustomProviders

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | resourceproviders | 否 | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | jobs | 否 | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | databoxedgedevices | 否 | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | workspaces | 否 | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | catalogs | 否 | 
> | datacatalogs | 否 | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | connectionmanagers | 否 | 

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | packages | 否 | 
> | plans | 否 | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | datafactories | 否 | 
> | factories | 否 |  

## <a name="microsoftdatalake"></a>Microsoft.DataLake

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | datalakeaccounts | 否 | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 | 否 | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 | 否 | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 服務 | 否 | 
> | 服務/專案 | 否 | 
> | slots | 否 | 

## <a name="microsoftdatashare"></a>DataShare

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 | 否 | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | servers | 否 |  

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | servers | 否 |  

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | servergroups | 否 | 
> | servers | 否 |  
> | serversv2 | 否 | 

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | artifactsources | 否 | 
> | rollouts | 否 |  
> | servicetopologies | 否 | 
> | servicetopologies/服務 | 否 |  
> | servicetopologies/services/serviceunits | 否 | 
> | steps | 否 | 

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | elasticpools | 否 | 
> | elasticpools / iothubtenants | 否 | 
> | iothubs | 是 | 
> | provisioningservices | 否 | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | controllers | 否 | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | labcenters | 否 | 
> | labs | 否 | 
> | 實驗室/環境 | 否 |  
> | 實驗室/servicerunners | 否 | 
> | 實驗室/virtualmachines | 否 |  
> | schedules | 否 |  

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | databaseaccounts | 否 | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | domains | 否 | 

## <a name="microsoftenterpriseknowledgegraph"></a>EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 服務 | 否 |  

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | domains | 否 |  
> | topics | 否 | 

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | clusters | 否 |  
> | 命名空間 | 否 | 

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 | 否 | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | hanainstances | 否 | 
> | sapmonitors | 否 |  

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | clusters | 否 | 

## <a name="microsofthealthcareapis"></a>HealthcareApis

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 服務 | 否 |  

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 機時 | 否 | 

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | datamanagers |  否 | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | jobs |  否 | 

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 | 否 | 
> | actiongroups |  否 | 
> | activitylogalerts | 否 | 
> | alertrules |  否 | 
> | autoscalesettings |  否 | 
> | components |  否 |  
> | guestdiagnosticsettings | 否 | 
> | metricalerts | 否 | 
> | notificationgroups | 否 | 
> | notificationrules | 否 | 
> | scheduledqueryrules |  否 | 
> | webtests |  否 | 
> | workbooks |  否 |  


## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | iotapps |  否 |  

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | checknameavailability |  否 |  
> | graph |  否 | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | hsmpools | 否 | 
> | vaults |  否 | 


## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | clusters |  否 |  

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | labaccounts | 否 | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 | 否 | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 | 否 | 

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | hostingenvironments | 否 | 
> | integrationaccounts |  否 |  
> | integrationserviceenvironments | 否 | 
> | isolatedenvironments | 否 | 
> | workflows |  否 |  

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | commitmentplans |  否 | 
> | webservices |  否 | 
> | workspaces |  否 | 

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | operationalizationclusters |  否 | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 | 否 | 
> | 帳戶/工作區 | 否 | 
> | 帳戶/工作區/專案 | 否 | 
> | teamaccounts | 否 | 
> | teamaccounts/工作區 | 否 | 
> | teamaccounts/工作區/專案 | 否 | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 | 否 | 

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft.MachineLearningOperationalization

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | hostingaccounts | 否 | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | workspaces | 否 | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | userassignedidentities | 否 | 

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 |  否 |  

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | classicdevservices | 否 | 

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | mediaservices |  否 | 
> | windowsazure.mediaservices.extensions/liveevent |  否 | 
> | windowsazure.mediaservices.extensions/streamingendpoint |  否 | 

## <a name="microsoftmicroservices4spring"></a>Microservices4Spring

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | appclusters | 否 | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | assessmentprojects | 否 | 
> | migrateprojects | 否 | 
> | projects | 否 | 

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | netappaccounts | 否 | 
> | netappaccounts / capacitypools | 否 | 
> | netappaccounts/capacitypools/磁片區 | 否 | 
> | netappaccounts/capacitypools/磁片區/mounttargets | 否 | 
> | netappaccounts/capacitypools/磁片區/快照集 | 否 | 

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | applicationgateways | 否 | 
> | applicationgatewaywebapplicationfirewallpolicies | 否 | 
> | applicationsecuritygroups |  否 |  
> | azurefirewalls |  否 |  
> | bastionhosts | 否 | 
> | connections |  否 | 
> | ddoscustompolicies |  否 | 
> | ddosprotectionplans | 否 | 
> | dnszones |  否 | 
> | expressroutecircuits | 否 | 
> | expressroutecrossconnections | 否 | 
> | expressroutegateways | 否 | 
> | expressrouteports | 否 | 
> | frontdoors | 否 | 
> | frontdoorwebapplicationfirewallpolicies | 否 | 
> | loadbalancers | 是-基本 SKU<br>無標準 SKU | 是-基本 SKU<br> -是標準 SKU |
> | localnetworkgateways |  否 | 
> | natgateways |  否 | 
> | networkintentpolicies |  否 | 
> | networkinterfaces | 是 | 
> | networkprofiles | 否 | 
> | networksecuritygroups | 是 | 
> | networkwatchers |  否 |  
> | networkwatchers / connectionmonitors |  否 | 
> | networkwatchers/鏡頭 |  否 | 
> | networkwatchers / pingmeshes |  否 | 
> | p2svpngateways | 否 | 
> | privatednszones |  否 |  
> | privatednszones / virtualnetworklinks |  否 |  
> | privateendpoints | 否 | 
> | privatelinkservices | 否 | 
> | publicipaddresses | 是-基本 SKU<br>無標準 SKU | 是-基本 SKU<br>無標準 SKU |
> | publicipprefixes | 否 | 
> | routefilters | 否 | 
> | routetables |  否 | 
> | serviceendpointpolicies |  否 | 
> | trafficmanagerprofiles |  否 | 
> | virtualhubs | 否 | 
> | virtualnetworkgateways |  否 |  
> | virtualnetworks |  否 | 
> | virtualnetworktaps | 否 | 
> | virtualwans | 否 | 
> | vpngateways （虛擬 WAN） | 否 | 
> | vpnsites （虛擬 WAN） | 否 | 
> | webapplicationfirewallpolicies |  否 | 


## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 命名空間 |  否 | 
> | 命名空間/notificationhubs |  否 |  

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | workspaces |  否 | 



## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | managementconfigurations |  否 | 
> | 檢視 |  否 | 

## <a name="microsoftpeering"></a>Microsoft 對等互連

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 對等互連 | 否 | 

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | dashboards | 否 | 

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | rootresources | 否 | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | workspacecollections |  否 | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | capacities |  否 | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 帳戶 | 否 | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | vaults | 否。 [停用保存庫並重新](https://docs.microsoft.com/azure/site-recovery/move-vaults-across-regions)建立 Site Recovery  | 


## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 命名空間 |  否 | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 查詢 |  否 |  

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 應用程式所需 |  否 | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | flows |  否 |  
> | jobcollections |  否 | 

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | searchservices |  否 | 


## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | iotsecuritysolutions |  否 | 
> | playbookconfigurations | 否 | 

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | gateways | 否 | 
> | nodes | 否 | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 命名空間 |  否 | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 應用程式所需 | 否 | 
> | clusters |  否 | 
> | 叢集/應用程式 | 否 | 
> | containergroups | 否 | 
> | containergroupsets | 否 | 
> | edgeclusters | 否 | 
> | networks | 否 | 
> | secretstores | 否 | 
> | 磁碟區 | 否 | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 應用程式所需 |  否 | 
> | containergroups | 否 | 
> | gateways |  否 | 
> | networks |  否 | 
> | 密碼 |  否 | 
> | 磁碟區 |  否 |  

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | signalr |  否 |  

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | appliancedefinitions | 否 | 
> | appliances | 否 | 
> | applicationdefinitions | 否 | 
> | 應用程式所需 | 否 | 
> | jitrequests | 否 | 

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | instancepools | 否 | 
> | managedinstances | 是 | 
> | managedinstances/資料庫 | 是 | 
> | servers | 是 | 
> | 伺服器/資料庫 | 是 | 
> | 伺服器/elasticpools | 是 | 
> | virtualclusters | 是 | 

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | sqlvirtualmachinegroups |  否 |  
> | sqlvirtualmachines |  否 |  

## <a name="microsoftsqlvm"></a>Microsoft.SqlVM

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | dwvm | 否 | 

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | storageaccounts | 是 | 

## <a name="microsoftstoragecache"></a>StorageCache

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | 緩衝區 | 否 | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | storagesyncservices |  否 | 

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | storagesyncservices | 否 | 

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | storagesyncservices | 否 | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | managers | 否 | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | streamingjobs |  否 |  


## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | environments | 否 | 
> | 環境/eventsources | 否 | 
> | 執行個體 | 否 | 
> | 實例/環境 | 否 | 
> | 實例/環境/eventsources | 否 | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | providerregistrations | 否 | 
> | resources | 否 | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | environments |  否 | 
> | 環境/eventsources |  否 |  
> | 環境/referencedatasets |  否 | 

## <a name="microsofttoken"></a>Microsoft Token

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | stores | 否 | 

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | imagetemplates | 否 | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | account |  否 | 
> | 帳戶/擴充功能 |  否 | 
> | 帳戶/專案 |  否 | 



## <a name="microsoftvmwarecloudsimple"></a>VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | dedicatedcloudnodes | 否 | 
> | dedicatedcloudservices | 否 | 
> | virtualmachines | 否 | 

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | certificates | 否 | 
> | connectiongateways |  否 |  
> | connections |  否 |  
> | customapis |  否 | 
> | hostingenvironments | 否 | 
> | serverfarms |  否 |  
> | sites |  否 | 
> | sites/premieraddons |  否 |  
> | 網站/位置 |  否 |  


## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | deviceservices | 否 | 

## <a name="microsoftwindowsvirtualdesktop"></a>WindowsVirtualDesktop

> [!div class="mx-tableFixed"]
> | 資源類型 | 區域移動 | 
> | ------------- | ----------- |
> | applicationgroups | 否 | 
> | hostpools | 否 | 
> | workspaces | 否 | 

## <a name="third-party-services"></a>協力廠商服務

協力廠商服務目前不支援移動作業。
