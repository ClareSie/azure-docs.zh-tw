---
title: 依資源類型區分的移動作業支援
description: 列出可移至新資源群組或訂用帳戶的 Azure 資源類型。
ms.topic: conceptual
ms.date: 07/06/2020
ms.openlocfilehash: 7f241e12200101e2f8f9efa7cf31e4483b2d4229
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86044527"
---
# <a name="move-operation-support-for-resources"></a>資源的移動作業支援

此文章列出 Azure 資源類型是否支援移動作業。 其也提供移動資源時要考慮之特殊條件的相關資訊。

跳至資源提供者命名空間：
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.Addons](#microsoftaddons)
> - [Microsoft.ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [MICROSOFT.APPSERVICE](#microsoftappservice)
> - [Microsoft.Attestation](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AVS](#microsoftavs)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.AzureStackHCI](#microsoftazurestackhci)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Billing](#microsoftbilling)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [BIZTALKSERVICES](#microsoftbiztalkservices)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.BlockchainTokens](#microsoftblockchaintokens)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Capacity](#microsoftcapacity)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ChangeAnalysis](#microsoftchangeanalysis)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.ClassicSubscription](#microsoftclassicsubscription)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Commerce](#microsoftcommerce)
> - [Microsoft.Compute](#microsoftcompute)
> - [ConnectedCache](#microsoftconnectedcache)
> - [Microsoft.Consumption](#microsoftconsumption)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [MICROSOFT.CONTENTMODERATOR](#microsoftcontentmoderator)
> - [MICROSOFT.CORTANAANALYTICS](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CostManagementExports](#microsoftcostmanagementexports)
> - [MICROSOFT.CUSTOMERINSIGHTS](#microsoftcustomerinsights)
> - [Microsoft.CustomerLockbox](#microsoftcustomerlockbox)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [MICROSOFT.DATACONNECT](#microsoftdataconnect)
> - [MICROSOFT.DATAEXCHANGE](#microsoftdataexchange)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [MICROSOFT.DATALAKE](#microsoftdatalake)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DataProtection](#microsoftdataprotection)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DevOps](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DigitalTwins](#microsoftdigitaltwins)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Experimentation](#microsoftexperimentation)
> - [Microsoft.Falcon](#microsoftfalcon)
> - [Microsoft.Features](#microsoftfeatures)
> - [MICROSOFT.GENOMICS](#microsoftgenomics)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [HybridNetwork](#microsofthybridnetwork)
> - [Microsoft.Hydra](#microsofthydra)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kubernetes](#microsoftkubernetes)
> - [Microsoft.KubernetesConfiguration](#microsoftkubernetesconfiguration)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [MICROSOFT.LOCATIONBASEDSERVICES](#microsoftlocationbasedservices)
> - [MICROSOFT.LOCATIONSERVICES.LOG](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [MICROSOFT.MACHINELEARNINGCOMPUTE](#microsoftmachinelearningcompute)
> - [MICROSOFT.MACHINELEARNINGEXPERIMENTATION](#microsoftmachinelearningexperimentation)
> - [MICROSOFT.MACHINELEARNINGMODELMANAGEMENT](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.Maintenance](#microsoftmaintenance)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [MICROSOFT.MANAGEDNETWORK](#microsoftmanagednetwork)
> - [Microsoft.ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Management](#microsoftmanagement)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.Marketplace](#microsoftmarketplace)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.MarketplaceOrdering](#microsoftmarketplaceordering)
> - [Microsoft.Media](#microsoftmedia)
> - [MICROSOFT.MICROSERVICES4SPRING](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.MixedReality](#microsoftmixedreality)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OffAzure](#microsoftoffazure)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [PowerPlatform](#microsoftpowerplatform)
> - [Microsoft.ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft.ProviderHub](#microsoftproviderhub)
> - [Microsoft.Quantum](#microsoftquantum)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.RedHatOpenShift](#microsoftredhatopenshift)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.SerialConsole](#microsoftserialconsole)
> - [MICROSOFT.SERVERMANAGEMENT](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [MICROSOFT.伺服器](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [MICROSOFT.STORAGECACHE](#microsoftstoragecache)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [MICROSOFT.STORAGESYNCDEV](#microsoftstoragesyncdev)
> - [MICROSOFT.STORAGESYNCINT](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [MICROSOFT.STREAMANALYTICSEXPLORER](#microsoftstreamanalyticsexplorer)
> - [Microsoft.Subscription](#microsoftsubscription)
> - [microsoft.support](#microsoftsupport)
> - [Microsoft.Synapse](#microsoftsynapse)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Token](#microsofttoken)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft.VMware](#microsoftvmware)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.VnfManager](#microsoftvnfmanager)
> - [Microsoft.VSOnline](#microsoftvsonline)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsESU](#microsoftwindowsesu)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [WorkloadBuilder](#microsoftworkloadbuilder)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | domainservices | 否 | 否 |
> | domainservices / oucontainer | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | azureadmetrics | 否 | 否 |
> | diagnosticsettings | 否 | 否 |
> | diagnosticsettingscategories | 否 | 否 |
> | 作業 | 否 | 否 |
> | privatelinkforazuread | 否 | 否 |
> | tenants | 否 | 否 |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | operationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | supportproviders | 否 | 否 |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | aadsupportcases | 否 | 否 |
> | addsservices | 否 | 否 |
> | agents | 否 | 否 |
> | anonymousapiusers | 否 | 否 |
> | 組態 | 否 | 否 |
> | 記錄 | 否 | 否 |
> | 作業 | 否 | 否 |
> | reports | 否 | 否 |
> | servicehealthmetrics | 否 | 否 |
> | 服務 | 否 | 否 |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 組態 | 否 | 否 |
> | generaterecommendations | 否 | 否 |
> | 中繼資料 | 否 | 否 |
> | 作業 | 否 | 否 |
> | Mahout | 否 | 否 |
> | suppressions | 否 | 否 |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | actionrules | 否 | 否 |
> | alerts | 否 | 否 |
> | alertslist | 否 | 否 |
> | alertsmetadata | 否 | 否 |
> | alertssummary | 否 | 否 |
> | alertssummarylist | 否 | 否 |
> | 作業 | 否 | 否 |
> | smartdetectoralertrules | 否 | 否 |
> | smartgroups | 否 | 否 |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |
> | servers | 否 | 否 |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checkfeedbackrequired | 否 | 否 |
> | checknameavailability | 否 | 否 |
> | checkservicenameavailability | 否 | 否 |
> | 作業 | 否 | 否 |
> | reportfeedback | 否 | 否 |
> | 服務 | 是 | 是 |
> | validateservicename | 否 | 否 |

> [!IMPORTANT]
> 無法移動設定為取用 SKU 的 API 管理服務。

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | configurationstores | 否 | 否 |
> | configurationstores / eventgridfilters | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operationsstatus | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operationstatus | 否 | 否 |
> | 作業 | 否 | 否 |
> | spring | 否 | 否 |
> | 春季/應用程式 | 否 | 否 |
> | 春季/apps/部署 | 否 | 否 |

## <a name="microsoftappservice"></a>MICROSOFT.APPSERVICE

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | apiapps | 否 | 否 |
> | appidentities | 否 | 否 |
> | gateways | 否 | 否 |

> [!IMPORTANT]
> 請參閱 [App Service 移動指引](./move-limitations/app-service-move-limitations.md)。

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | attestationproviders | 否 | 否 |
> | defaultproviders | 否 | 否 |
> | 位置 | 否 | 否 |
> | 位置/defaultprovider | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checkaccess | 否 | 否 |
> | classicadministrators | 否 | 否 |
> | dataaliases | 否 | 否 |
> | denyassignments | 否 | 否 |
> | elevateaccess | 否 | 否 |
> | findorphanroleassignments | 否 | 否 |
> | locks | 否 | 否 |
> | 作業 | 否 | 否 |
> | operationstatus | 否 | 否 |
> | 權限 | 否 | 否 |
> | policyassignments | 否 | 否 |
> | policydefinitions | 否 | 否 |
> | policysetdefinitions | 否 | 否 |
> | privatelinkassociations | 否 | 否 |
> | provideroperations | 否 | 否 |
> | resourcemanagementprivatelinks | 否 | 否 |
> | roleassignments | 否 | 否 |
> | roleassignmentsusagemetrics | 否 | 否 |
> | roledefinitions | 否 | 否 |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | automationaccounts | Yes | 是 |
> | automationaccounts / configurations | 否 | 否 |
> | automationaccounts / jobs | 否 | 否 |
> | automationaccounts / privateendpointconnectionproxies | 否 | 否 |
> | automationaccounts / privateendpointconnections | 否 | 否 |
> | automationaccounts / privatelinkresources | 否 | 否 |
> | automationaccounts / runbooks | 否 | 否 |
> | automationaccounts / softwareupdateconfigurations | 否 | 否 |
> | automationaccounts / webhooks | 否 | 否 |
> | 作業 | 否 | 否 |

> [!IMPORTANT]
> Runbook 必須存在於與自動化帳戶相同的資源群組中。
>
> 如需相關資訊，請參閱[將您的 Azure 自動化帳戶移至另一個訂用帳戶](../../automation/how-to/move-account.md?toc=/azure/azure-resource-manager/toc.json)。

## <a name="microsoftavs"></a>Microsoft.AVS

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / checkquotaavailability | 否 | 否 |
> | 位置/checktrialavailability | 否 | 否 |
> | 作業 | 否 | 否 |
> | privateclouds | 否 | 否 |
> | privateclouds / clusters | 否 | 否 |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | b2cdirectories | 是 | 是 |
> | b2ctenants | 否 | 否 |
> | checknameavailability | 否 | 否 |
> | guestusages | Yes | Yes |
> | 作業 | 否 | 否 |

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | datacontrollers | Yes | Yes |
> | hybriddatamanagers | Yes | Yes |
> | 作業 | 否 | 否 |
> | postgresinstances | Yes | Yes |
> | sqlinstances | Yes | Yes |
> | sqlmanagedinstances | Yes | Yes |
> | sqlserverinstances | Yes | Yes |
> | sqlserverregistrations | 是 | 是 |
> | sqlserverregistrations / sqlservers | 否 | 否 |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | cloudmanifestfiles | 否 | 否 |
> | edgesubscriptions | 否 | 否 |
> | 作業 | 否 | 否 |
> | registrations | 否 | 否 |
> | registrations / customersubscriptions | 否 | 否 |
> | registrations / products | 否 | 否 |

## <a name="microsoftazurestackhci"></a>Microsoft.AzureStackHCI

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | clusters | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | batchaccounts | 是 | 是 |
> | 位置 | 否 | 否 |
> | locations / accountoperationresults | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / quotas | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | billingaccounts | 否 | 否 |
> | billingaccounts / agreements | 否 | 否 |
> | billingaccounts / billingpermissions | 否 | 否 |
> | billingaccounts / billingprofiles | 否 | 否 |
> | billingaccounts / billingprofiles / availablebalance | 否 | 否 |
> | billingaccounts / billingprofiles / billingpermissions | 否 | 否 |
> | billingaccounts / billingprofiles / billingroleassignments | 否 | 否 |
> | billingaccounts / billingprofiles / billingroledefinitions | 否 | 否 |
> | billingaccounts / billingprofiles / billingsubscriptions | 否 | 否 |
> | billingaccounts / billingprofiles / createbillingroleassignment | 否 | 否 |
> | billingaccounts / billingprofiles / customers | 否 | 否 |
> | billingaccounts / billingprofiles / instructions | 否 | 否 |
> | billingaccounts / billingprofiles / invoices | 否 | 否 |
> | billingaccounts / billingprofiles / invoices / pricesheet | 否 | 否 |
> | billingaccounts / billingprofiles / invoices / transactions | 否 | 否 |
> | billingaccounts / billingprofiles / invoicesections | 否 | 否 |
> | billingaccounts / billingprofiles / invoicesections / billingpermissions | 否 | 否 |
> | billingaccounts / billingprofiles / invoicesections / billingroleassignments | 否 | 否 |
> | billingaccounts / billingprofiles / invoicesections / billingroledefinitions | 否 | 否 |
> | billingaccounts / billingprofiles / invoicesections / billingsubscriptions | 否 | 否 |
> | billingaccounts / billingprofiles / invoicesections / createbillingroleassignment | 否 | 否 |
> | billingaccounts / billingprofiles / invoicesections / initiatetransfer | 否 | 否 |
> | billingaccounts / billingprofiles / invoicesections / products | 否 | 否 |
> | billingaccounts / billingprofiles / invoicesections / products / transfer | 否 | 否 |
> | billingaccounts / billingprofiles / invoicesections / products / updateautorenew | 否 | 否 |
> | billingaccounts / billingprofiles / invoicesections / transactions | 否 | 否 |
> | billingaccounts / billingprofiles / invoicesections / transfers | 否 | 否 |
> | billingaccounts / billingprofiles / patchoperations | 否 | 否 |
> | billingaccounts / billingprofiles / paymentmethods | 否 | 否 |
> | billingaccounts / billingprofiles / policies | 否 | 否 |
> | billingaccounts / billingprofiles / pricesheet | 否 | 否 |
> | billingaccounts / billingprofiles / pricesheetdownloadoperations | 否 | 否 |
> | billingaccounts / billingprofiles / products | 否 | 否 |
> | billingaccounts / billingprofiles / transactions | 否 | 否 |
> | billingaccounts / billingroleassignments | 否 | 否 |
> | billingaccounts / billingroledefinitions | 否 | 否 |
> | billingaccounts / billingsubscriptions | 否 | 否 |
> | billingaccounts / billingsubscriptions / invoices | 否 | 否 |
> | billingaccounts / createbillingroleassignment | 否 | 否 |
> | billingaccounts / createinvoicesectionoperations | 否 | 否 |
> | billingaccounts / customers | 否 | 否 |
> | billingaccounts / customers / billingpermissions | 否 | 否 |
> | billingaccounts / customers / billingsubscriptions | 否 | 否 |
> | billingaccounts / customers / initiatetransfer | 否 | 否 |
> | billingaccounts / customers / policies | 否 | 否 |
> | billingaccounts / customers / products | 否 | 否 |
> | billingaccounts / customers / transactions | 否 | 否 |
> | billingaccounts / customers / transfers | 否 | 否 |
> | billingaccounts / departments | 否 | 否 |
> | billingaccounts/部門/billingpermissions | 否 | 否 |
> | billingaccounts/部門/billingroleassignments | 否 | 否 |
> | billingaccounts/部門/billingroledefinitions | 否 | 否 |
> | billingaccounts / enrollmentaccounts | 否 | 否 |
> | billingaccounts / enrollmentaccounts / billingpermissions | 否 | 否 |
> | billingaccounts / enrollmentaccounts / billingroleassignments | 否 | 否 |
> | billingaccounts / enrollmentaccounts / billingroledefinitions | 否 | 否 |
> | billingaccounts / invoices | 否 | 否 |
> | billingaccounts/發票/交易 | 否 | 否 |
> | billingaccounts / invoicesections | 否 | 否 |
> | billingaccounts / invoicesections / billingsubscriptionmoveoperations | 否 | 否 |
> | billingaccounts / invoicesections / billingsubscriptions | 否 | 否 |
> | billingaccounts / invoicesections / billingsubscriptions / transfer | 否 | 否 |
> | billingaccounts / invoicesections / elevate | 否 | 否 |
> | billingaccounts / invoicesections / initiatetransfer | 否 | 否 |
> | billingaccounts / invoicesections / patchoperations | 否 | 否 |
> | billingaccounts / invoicesections / productmoveoperations | 否 | 否 |
> | billingaccounts / invoicesections / products | 否 | 否 |
> | billingaccounts / invoicesections / products / transfer | 否 | 否 |
> | billingaccounts / invoicesections / products / updateautorenew | 否 | 否 |
> | billingaccounts / invoicesections / producttransfersresults | 否 | 否 |
> | billingaccounts / invoicesections / transactions | 否 | 否 |
> | billingaccounts / invoicesections / transfers | 否 | 否 |
> | billingaccounts / lineofcredit | 否 | 否 |
> | billingaccounts / listinvoicesectionswithcreatesubscriptionpermission | 否 | 否 |
> | billingaccounts / operationresults | 否 | 否 |
> | billingaccounts / patchoperations | 否 | 否 |
> | billingaccounts / paymentmethods | 否 | 否 |
> | billingaccounts / products | 否 | 否 |
> | billingaccounts / transactions | 否 | 否 |
> | billingperiods | 否 | 否 |
> | billingpermissions | 否 | 否 |
> | billingproperty | 否 | 否 |
> | billingroleassignments | 否 | 否 |
> | billingroledefinitions | 否 | 否 |
> | createbillingroleassignment | 否 | 否 |
> | departments | 否 | 否 |
> | enrollmentaccounts | 否 | 否 |
> | invoices | 否 | 否 |
> | operationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | operationstatus | 否 | 否 |
> | transfers | 否 | 否 |
> | transfers / accepttransfer | 否 | 否 |
> | transfers / declinetransfer | 否 | 否 |
> | transfers / operationstatus | 否 | 否 |
> | transfers / validatetransfer | 否 | 否 |
> | validateaddress | 否 | 否 |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | listcommunicationpreference | 否 | 否 |
> | mapapis | 否 | 否 |
> | 作業 | 否 | 否 |
> | updatecommunicationpreference | 否 | 否 |

## <a name="microsoftbiztalkservices"></a>BIZTALKSERVICES

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | biztalk | 否 | 否 |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | blockchainmembers | 否 | 否 |
> | cordamembers | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / blockchainmemberoperationresults | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / listconsortiums | 否 | 否 |
> | locations / watcheroperationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | watchers | 否 | 否 |

## <a name="microsoftblockchaintokens"></a>Microsoft.BlockchainTokens

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 作業 | 否 | 否 |
> | tokenservices | 否 | 否 |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | blueprintassignments | 否 | 否 |
> | blueprintassignments / assignmentoperations | 否 | 否 |
> | blueprintassignments / operations | 否 | 否 |
> | blueprints | 否 | 否 |
> | blueprints / artifacts | 否 | 否 |
> | blueprints / versions | 否 | 否 |
> | blueprints / versions / artifacts | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | botservices | 否 | 否 |
> | botservices / channels | 否 | 否 |
> | botservices / connections | 否 | 否 |
> | checknameavailability | 否 | 否 |
> | listauthserviceproviders | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operationsstatus | 否 | 否 |
> | 作業 | 否 | 否 |
> | redis | 是 | Yes |
> | redis/eventgridfilters | 否 | 否 |
> | redis / privatelinkresources | 否 | 否 |
> | redisenterprise | 是 | Yes |

> [!IMPORTANT]
> 如果 Azure Cache for Redis 執行個體是使用虛擬網路設定的，該執行個體便無法移至不同的訂用帳戶。 請參閱[網路移動限制](./move-limitations/networking-move-limitations.md)。

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | appliedreservations | 否 | 否 |
> | calculateexchange | 否 | 否 |
> | calculateprice | 否 | 否 |
> | calculatepurchaseprice | 否 | 否 |
> | catalogs | 否 | 否 |
> | checkoffers | 否 | 否 |
> | checkpurchasestatus | 否 | 否 |
> | checkscopes | 否 | 否 |
> | commercialreservationorders | 否 | 否 |
> | 兌換 | 否 | 否 |
> | listbenefits | 否 | 否 |
> | 作業 | 否 | 否 |
> | placepurchaseorder | 否 | 否 |
> | reservationorders | 否 | 否 |
> | reservationorders / availablescopes | 否 | 否 |
> | reservationorders / calculaterefund | 否 | 否 |
> | reservationorders / merge | 否 | 否 |
> | reservationorders / reservations | 否 | 否 |
> | reservationorders / reservations / availablescopes | 否 | 否 |
> | reservationorders / reservations / revisions | 否 | 否 |
> | reservationorders / return | 否 | 否 |
> | reservationorders / split | 否 | 否 |
> | reservationorders / swap | 否 | 否 |
> | reservations | 否 | 否 |
> | resources | 否 | 否 |
> | validatereservationorder | 否 | 否 |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | cdnwebapplicationfirewallmanagedrulesets | 否 | 否 |
> | cdnwebapplicationfirewallpolicies | 否 | 否 |
> | checknameavailability | 否 | 否 |
> | checkresourceusage | 否 | 否 |
> | edgenodes | 否 | 否 |
> | operationresults | 否 | 否 |
> | operationresults / profileresults | 否 | 否 |
> | operationresults / profileresults / endpointresults | 否 | 否 |
> | operationresults / profileresults / endpointresults / customdomainresults | 否 | 否 |
> | operationresults / profileresults / endpointresults / origingroupresults | 否 | 否 |
> | operationresults / profileresults / endpointresults / originresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | 設定檔 | Yes | Yes |
> | profiles / endpoints | 否 | 否 |
> | profiles / endpoints / customdomains | 否 | 否 |
> | profiles / endpoints / origingroups | 否 | 否 |
> | profiles / endpoints / origins | 否 | 否 |
> | validateprobe | 否 | 否 |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | certificateorders | 是 | 是 |
> | certificateorders / certificates | 否 | 否 |
> | 作業 | 否 | 否 |
> | validatecertificateregistrationinformation | 否 | 否 |

> [!IMPORTANT]
> 請參閱 [App Service 移動指引](./move-limitations/app-service-move-limitations.md)。

## <a name="microsoftchangeanalysis"></a>Microsoft.ChangeAnalysis

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 作業 | 否 | 否 |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | capabilities | 否 | 否 |
> | checkdomainnameavailability | 否 | 否 |
> | domainnames | 否 | 否 |
> | domainnames / capabilities | 否 | 否 |
> | domainnames / internalloadbalancers | 否 | 否 |
> | domainnames / servicecertificates | 否 | 否 |
> | domainnames / slots | 否 | 否 |
> | domainnames / slots / roles | 否 | 否 |
> | domainnames / slots / roles / metricdefinitions | 否 | 否 |
> | domainnames / slots / roles / metrics | 否 | 否 |
> | movesubscriptionresources | 否 | 否 |
> | operatingsystemfamilies | 否 | 否 |
> | operatingsystems | 否 | 否 |
> | 作業 | 否 | 否 |
> | operationstatuses | 否 | 否 |
> | quotas | 否 | 否 |
> | resourcetypes | 否 | 否 |
> | validatesubscriptionmoveavailability | 否 | 否 |
> | virtualmachines | 否 | 否 |
> | virtualmachines / diagnosticsettings | 否 | 否 |
> | virtualmachines / metricdefinitions | 否 | 否 |
> | virtualmachines / metrics | 否 | 否 |

> [!IMPORTANT]
> 請參閱[傳統部署移動指引](./move-limitations/classic-model-move-limitations.md)。 傳統部署資源可以透過該案例專屬的作業，跨訂用帳戶移動。

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | classicinfrastructureresources | 否 | 否 |

> [!IMPORTANT]
> 請參閱[傳統部署移動指引](./move-limitations/classic-model-move-limitations.md)。 傳統部署資源可以透過該案例專屬的作業，跨訂用帳戶移動。

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | capabilities | 否 | 否 |
> | expressroutecrossconnections | 否 | 否 |
> | expressroutecrossconnections / peerings | 否 | 否 |
> | gatewaysupporteddevices | 否 | 否 |
> | networksecuritygroups | 否 | 否 |
> | 作業 | 否 | 否 |
> | quotas | 否 | 否 |
> | reservedips | 否 | 否 |
> | virtualnetworks | 否 | 否 |
> | virtualnetworks / remotevirtualnetworkpeeringproxies | 否 | 否 |
> | virtualnetworks / virtualnetworkpeerings | 否 | 否 |

> [!IMPORTANT]
> 請參閱[傳統部署移動指引](./move-limitations/classic-model-move-limitations.md)。 傳統部署資源可以透過該案例專屬的作業，跨訂用帳戶移動。

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | capabilities | 否 | 否 |
> | checkstorageaccountavailability | 否 | 否 |
> | disks | 否 | 否 |
> | images | 否 | 否 |
> | 作業 | 否 | 否 |
> | osimages | 否 | 否 |
> | osplatformimages | 否 | 否 |
> | publicimages | 否 | 否 |
> | quotas | 否 | 否 |
> | storageaccounts | 否 | 否 |
> | storageaccounts / blobservices | 否 | 否 |
> | storageaccounts / fileservices | 否 | 否 |
> | storageaccounts / metricdefinitions | 否 | 否 |
> | storageaccounts / metrics | 否 | 否 |
> | storageaccounts / queueservices | 否 | 否 |
> | storageaccounts / services | 否 | 否 |
> | storageaccounts / services / diagnosticsettings | 否 | 否 |
> | storageaccounts / services / metricdefinitions | 否 | 否 |
> | storageaccounts / services / metrics | 否 | 否 |
> | storageaccounts / tableservices | 否 | 否 |
> | storageaccounts / vmimages | 否 | 否 |
> | vmimages | 否 | 否 |

> [!IMPORTANT]
> 請參閱[傳統部署移動指引](./move-limitations/classic-model-move-limitations.md)。 傳統部署資源可以透過該案例專屬的作業，跨訂用帳戶移動。

## <a name="microsoftclassicsubscription"></a>Microsoft.ClassicSubscription

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 作業 | 否 | 否 |

> [!IMPORTANT]
> 請參閱[傳統部署移動指引](./move-limitations/classic-model-move-limitations.md)。 傳統部署資源可以透過該案例專屬的作業，跨訂用帳戶移動。

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |
> | checkdomainavailability | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / checkskuavailability | 否 | 否 |
> | locations / deletevirtualnetworkorsubnets | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 作業 | 否 | 否 |
> | ratecard | 否 | 否 |
> | usageaggregates | 否 | 否 |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | availabilitysets | Yes | Yes |
> | cloudservices | 否 | 否 |
> | diskaccesses | 否 | 否 |
> | diskencryptionsets | 否 | 否 |
> | disks | Yes | 是 |
> | galleries | 否 | 否 |
> | galleries / images | 否 | 否 |
> | galleries / images / versions | 否 | 否 |
> | hostgroups | 否 | 否 |
> | hostgroups / hosts | 否 | 否 |
> | images | 是 | Yes |
> | 位置 | 否 | 否 |
> | locations / artifactpublishers | 否 | 否 |
> | locations / capsoperations | 否 | 否 |
> | locations / diskoperations | 否 | 否 |
> | locations / loganalytics | 否 | 否 |
> | locations / operations | 否 | 否 |
> | locations / publishers | 否 | 否 |
> | locations / runcommands | 否 | 否 |
> | locations / usages | 否 | 否 |
> | locations / virtualmachines | 否 | 否 |
> | locations / vmsizes | 否 | 否 |
> | locations / vsmoperations | 否 | 否 |
> | 作業 | 否 | 否 |
> | proximityplacementgroups | 是 | Yes |
> | restorepointcollections | 否 | 否 |
> | restorepointcollections / restorepoints | 否 | 否 |
> | sharedvmextensions | 否 | 否 |
> | sharedvmimages | 否 | 否 |
> | sharedvmimages / versions | 否 | 否 |
> | snapshots | Yes | Yes |
> | sshpublickeys | 否 | 否 |
> | swiftlets | 否 | 否 |
> | virtualmachines | Yes | Yes |
> | virtualmachines / extensions | Yes | Yes |
> | virtualmachines / metricdefinitions | 否 | 否 |
> | virtualmachines / runcommands | Yes | Yes |
> | virtualmachinescalesets | 是 | 是 |
> | virtualmachinescalesets / extensions | 否 | 否 |
> | virtualmachinescalesets / networkinterfaces | 否 | 否 |
> | virtualmachinescalesets / publicipaddresses | 否 | 否 |
> | virtualmachinescalesets / virtualmachines | 否 | 否 |
> | virtualmachinescalesets / virtualmachines / networkinterfaces | 否 | 否 |

> [!IMPORTANT]
> 請參閱[虛擬機器移動指引](./move-limitations/virtual-machines-move-limitations.md)。

## <a name="microsoftconnectedcache"></a>ConnectedCache

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | cachenodes | 否 | 否 |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | aggregatedcost | 否 | 否 |
> | balances | 否 | 否 |
> | budgets | 否 | 否 |
> | charges | 否 | 否 |
> | costtags | 否 | 否 |
> | credits | 否 | 否 |
> | 活動 | 否 | 否 |
> | forecasts | 否 | 否 |
> | lots | 否 | 否 |
> | marketplaces | 否 | 否 |
> | operationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | operationstatus | 否 | 否 |
> | pricesheets | 否 | 否 |
> | products | 否 | 否 |
> | reservationdetails | 否 | 否 |
> | reservationrecommendationdetails | 否 | 否 |
> | reservationrecommendations | 否 | 否 |
> | reservationsummaries | 否 | 否 |
> | reservationtransactions | 否 | 否 |
> | tags | 否 | 否 |
> | tenants | 否 | 否 |
> | terms | 否 | 否 |
> | usagedetails | 否 | 否 |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | containergroups | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / cachedimages | 否 | 否 |
> | locations / capabilities | 否 | 否 |
> | locations / deletevirtualnetworkorsubnets | 否 | 否 |
> | locations / operations | 否 | 否 |
> | locations / usages | 否 | 否 |
> | 作業 | 否 | 否 |
> | serviceassociationlinks | 否 | 否 |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / authorize | 否 | 否 |
> | locations / deletevirtualnetworkorsubnets | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / setupauth | 否 | 否 |
> | 作業 | 否 | 否 |
> | registries | 是 | 是 |
> | registries / agentpools | 否 | 否 |
> | registries / agentpools / listqueuestatus | 否 | 否 |
> | registries / builds | 否 | 否 |
> | registries / builds / cancel | 否 | 否 |
> | registries / builds / getloglink | 否 | 否 |
> | registries / buildtasks | 否 | 否 |
> | registries / buildtasks / listsourcerepositoryproperties | 否 | 否 |
> | registries / buildtasks / steps | 否 | 否 |
> | registries / buildtasks / steps / listbuildarguments | 否 | 否 |
> | registries / eventgridfilters | 否 | 否 |
> | registries / exportpipelines | 否 | 否 |
> | registries / generatecredentials | 否 | 否 |
> | registries / getbuildsourceuploadurl | 否 | 否 |
> | registries / getcredentials | 否 | 否 |
> | registries / importimage | 否 | 否 |
> | registries / importpipelines | 否 | 否 |
> | registries / listbuildsourceuploadurl | 否 | 否 |
> | registries / listcredentials | 否 | 否 |
> | registries / listpolicies | 否 | 否 |
> | registries / listusages | 否 | 否 |
> | registries / pipelineruns | 否 | 否 |
> | registries / privateendpointconnectionproxies | 否 | 否 |
> | registries / privateendpointconnectionproxies / validate | 否 | 否 |
> | registries / privateendpointconnections | 否 | 否 |
> | registries / privatelinkresources | 否 | 否 |
> | registries / queuebuild | 否 | 否 |
> | registries / regeneratecredential | 否 | 否 |
> | registries / regeneratecredentials | 否 | 否 |
> | registries / replications | 否 | 否 |
> | registries / runs | 否 | 否 |
> | registries / runs / cancel | 否 | 否 |
> | registries / runs / listlogsasurl | 否 | 否 |
> | registries / schedulerun | 否 | 否 |
> | registries / scopemaps | 否 | 否 |
> | registries / taskruns | 否 | 否 |
> | registries / taskruns / listdetails | 否 | 否 |
> | registries / tasks | 否 | 否 |
> | registries / tasks / listdetails | 否 | 否 |
> | registries / tokens | 否 | 否 |
> | registries / updatepolicies | 否 | 否 |
> | registries / webhooks | 否 | 否 |
> | registries / webhooks / getcallbackconfig | 否 | 否 |
> | registries / webhooks / listevents | 否 | 否 |
> | registries / webhooks / ping | 否 | 否 |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | containerservices | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / openshiftclusters | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operations | 否 | 否 |
> | locations / orchestrators | 否 | 否 |
> | managedclusters | 否 | 否 |
> | openshiftmanagedclusters | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftcontentmoderator"></a>MICROSOFT.CONTENTMODERATOR

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 應用程式所需 | 否 | 否 |

## <a name="microsoftcortanaanalytics"></a>MICROSOFT.CORTANAANALYTICS

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | alerts | 否 | 否 |
> | billingaccounts | 否 | 否 |
> | budgets | 否 | 否 |
> | cloudconnectors | 否 | 否 |
> | 連接器 | 否 | 否 |
> | costallocationrules | 否 | 否 |
> | departments | 否 | 否 |
> | dimensions | 否 | 否 |
> | enrollmentaccounts | 否 | 否 |
> | exports | 否 | 否 |
> | externalbillingaccounts | 否 | 否 |
> | externalbillingaccounts / alerts | 否 | 否 |
> | externalbillingaccounts / dimensions | 否 | 否 |
> | externalbillingaccounts / forecast | 否 | 否 |
> | externalbillingaccounts / query | 否 | 否 |
> | externalsubscriptions | 否 | 否 |
> | externalsubscriptions / alerts | 否 | 否 |
> | externalsubscriptions / dimensions | 否 | 否 |
> | externalsubscriptions / forecast | 否 | 否 |
> | externalsubscriptions / query | 否 | 否 |
> | 預測 | 否 | 否 |
> | 作業 | 否 | 否 |
> | 查詢 | 否 | 否 |
> | 註冊 | 否 | 否 |
> | reportconfigs | 否 | 否 |
> | reports | 否 | 否 |
> | settings | 否 | 否 |
> | showbackrules | 否 | 否 |
> | 檢視 | 否 | 否 |

## <a name="microsoftcostmanagementexports"></a>Microsoft.CostManagementExports

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 作業 | 否 | 否 |

## <a name="microsoftcustomerinsights"></a>MICROSOFT.CUSTOMERINSIGHTS

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | hubs | 否 | 否 |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 作業 | 否 | 否 |
> | requests | 否 | 否 |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | associations | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |
> | resourceproviders | 否 | 否 |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | jobs | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / availableskus | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / regionconfiguration | 否 | 否 |
> | locations / validateaddress | 否 | 否 |
> | locations / validateinputs | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | availableskus | 否 | 否 |
> | databoxedgedevices | Yes | 是 |
> | databoxedgedevices / checknameavailability | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / getnetworkpolicies | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |
> | workspaces | 否 | 否 |
> | workspaces / dbworkspaces | 否 | 否 |
> | workspaces / virtualnetworkpeerings | 否 | 否 |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | catalogs | 否 | 否 |
> | checknameavailability | 否 | 否 |
> | datacatalogs | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / jobs | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftdataconnect"></a>MICROSOFT.DATACONNECT

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | connectionmanagers | 否 | 否 |

## <a name="microsoftdataexchange"></a>MICROSOFT.DATAEXCHANGE

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | packages | 否 | 否 |
> | plans | 否 | 否 |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checkazuredatafactorynameavailability | 否 | 否 |
> | checkdatafactorynameavailability | 否 | 否 |
> | datafactories | 否 | 否 |
> | datafactories / diagnosticsettings | 否 | 否 |
> | datafactories / metricdefinitions | 否 | 否 |
> | datafactoryschema | 否 | 否 |
> | factories | 是 | 是 |
> | factories / integrationruntimes | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / configurefactoryrepo | 否 | 否 |
> | locations / getfeaturevalue | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftdatalake"></a>MICROSOFT.DATALAKE

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | datalakeaccounts | 否 | 否 |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |
> | accounts / datalakestoreaccounts | 否 | 否 |
> | accounts / storageaccounts | 否 | 否 |
> | accounts / storageaccounts / containers | 否 | 否 |
> | accounts / storageaccounts / containers / listsastokens | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / capability | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / usages | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |
> | accounts / eventgridfilters | 否 | 否 |
> | accounts / firewallrules | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / capability | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / deletevirtualnetworkorsubnets | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / usages | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |
> | 服務 | 否 | 否 |
> | services / projects | 否 | 否 |
> | slots | 否 | 否 |

## <a name="microsoftdataprotection"></a>Microsoft.DataProtection

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | backupvaults | 否 | 否 |
> | 位置 | 否 | 否 |
> | 作業 | 否 | 否 |
> | resourceoperationgatekeepers | 否 | 否 |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 是 | 是 |
> | accounts / shares | 否 | 否 |
> | accounts / shares / datasets | 否 | 否 |
> | accounts / shares / invitations | 否 | 否 |
> | accounts / shares / providersharesubscriptions | 否 | 否 |
> | accounts / shares / synchronizationsettings | 否 | 否 |
> | accounts / sharesubscriptions | 否 | 否 |
> | accounts / sharesubscriptions / consumersourcedatasets | 否 | 否 |
> | accounts / sharesubscriptions / datasetmappings | 否 | 否 |
> | accounts / sharesubscriptions / triggers | 否 | 否 |
> | listinvitations | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / consumerinvitations | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / rejectinvitation | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / azureasyncoperation | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / performancetiers | 否 | 否 |
> | locations / privateendpointconnectionazureasyncoperation | 否 | 否 |
> | locations / privateendpointconnectionoperationresults | 否 | 否 |
> | locations / privateendpointconnectionproxyazureasyncoperation | 否 | 否 |
> | locations / privateendpointconnectionproxyoperationresults | 否 | 否 |
> | locations / recommendedactionsessionsazureasyncoperation | 否 | 否 |
> | locations / recommendedactionsessionsoperationresults | 否 | 否 |
> | locations / securityalertpoliciesazureasyncoperation | 否 | 否 |
> | locations / securityalertpoliciesoperationresults | 否 | 否 |
> | locations / serverkeyazureasyncoperation | 否 | 否 |
> | locations / serverkeyoperationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | servers | 是 | 是 |
> | servers / advisors | 否 | 否 |
> | servers / privateendpointconnectionproxies | 否 | 否 |
> | servers / privateendpointconnections | 否 | 否 |
> | servers / privatelinkresources | 否 | 否 |
> | servers / querytexts | 否 | 否 |
> | servers / recoverableservers | 否 | 否 |
> | servers / topquerystatistics | 否 | 否 |
> | servers / virtualnetworkrules | 否 | 否 |
> | servers / waitstatistics | 否 | 否 |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / administratorazureasyncoperation | 否 | 否 |
> | locations / administratoroperationresults | 否 | 否 |
> | locations / azureasyncoperation | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / performancetiers | 否 | 否 |
> | locations / privateendpointconnectionazureasyncoperation | 否 | 否 |
> | locations / privateendpointconnectionoperationresults | 否 | 否 |
> | locations / privateendpointconnectionproxyazureasyncoperation | 否 | 否 |
> | locations / privateendpointconnectionproxyoperationresults | 否 | 否 |
> | locations / recommendedactionsessionsazureasyncoperation | 否 | 否 |
> | locations / recommendedactionsessionsoperationresults | 否 | 否 |
> | locations / securityalertpoliciesazureasyncoperation | 否 | 否 |
> | locations / securityalertpoliciesoperationresults | 否 | 否 |
> | locations / serverkeyazureasyncoperation | 否 | 否 |
> | locations / serverkeyoperationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | servers | 是 | 是 |
> | servers / advisors | 否 | 否 |
> | servers / keys | 否 | 否 |
> | servers / privateendpointconnectionproxies | 否 | 否 |
> | servers / privateendpointconnections | 否 | 否 |
> | servers / privatelinkresources | 否 | 否 |
> | servers / querytexts | 否 | 否 |
> | servers / recoverableservers | 否 | 否 |
> | servers / topquerystatistics | 否 | 否 |
> | servers / virtualnetworkrules | 否 | 否 |
> | servers / waitstatistics | 否 | 否 |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / administratorazureasyncoperation | 否 | 否 |
> | locations / administratoroperationresults | 否 | 否 |
> | locations / azureasyncoperation | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / performancetiers | 否 | 否 |
> | locations / privateendpointconnectionazureasyncoperation | 否 | 否 |
> | locations / privateendpointconnectionoperationresults | 否 | 否 |
> | locations / privateendpointconnectionproxyazureasyncoperation | 否 | 否 |
> | locations / privateendpointconnectionproxyoperationresults | 否 | 否 |
> | locations / recommendedactionsessionsazureasyncoperation | 否 | 否 |
> | locations / recommendedactionsessionsoperationresults | 否 | 否 |
> | locations / securityalertpoliciesazureasyncoperation | 否 | 否 |
> | locations / securityalertpoliciesoperationresults | 否 | 否 |
> | locations / serverkeyazureasyncoperation | 否 | 否 |
> | locations / serverkeyoperationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | servergroups | 否 | 否 |
> | servers | Yes | Yes |
> | servers / advisors | 否 | 否 |
> | servers / keys | 否 | 否 |
> | servers / privateendpointconnectionproxies | 否 | 否 |
> | servers / privateendpointconnections | 否 | 否 |
> | servers / privatelinkresources | 否 | 否 |
> | servers / querytexts | 否 | 否 |
> | servers / recoverableservers | 否 | 否 |
> | servers / topquerystatistics | 否 | 否 |
> | servers / virtualnetworkrules | 否 | 否 |
> | servers / waitstatistics | 否 | 否 |
> | serversv2 | Yes | Yes |
> | singleservers | 是 | 是 |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | artifactsources | 否 | 否 |
> | operationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | rollouts | 否 | 否 |
> | servicetopologies | 否 | 否 |
> | servicetopologies / services | 否 | 否 |
> | servicetopologies / services / serviceunits | 否 | 否 |
> | steps | 否 | 否 |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | applicationgroups | 否 | 否 |
> | applicationgroups / applications | 否 | 否 |
> | applicationgroups / desktops | 否 | 否 |
> | applicationgroups / startmenuitems | 否 | 否 |
> | hostpools | 否 | 否 |
> | hostpools / sessionhosts | 否 | 否 |
> | hostpools / sessionhosts / usersessions | 否 | 否 |
> | hostpools / usersessions | 否 | 否 |
> | 作業 | 否 | 否 |
> | workspaces | 否 | 否 |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | checkprovisioningservicenameavailability | 否 | 否 |
> | elasticpools | Yes | Yes |
> | elasticpools / iothubtenants | Yes | Yes |
> | iothubs | Yes | Yes |
> | iothubs / eventgridfilters | 否 | 否 |
> | iothubs/securitysettings | 否 | 否 |
> | operationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | provisioningservices | 是 | 是 |
> | usages | 否 | 否 |

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | pipelines | 否 | 否 |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | controllers | 否 | 否 |
> | controllers / listconnectiondetails | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / checkcontainerhostmapping | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | labcenters | 否 | 否 |
> | labs | 否 | 否 |
> | labs / environments | 否 | 否 |
> | labs / servicerunners | 否 | 否 |
> | labs / virtualmachines | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operations | 否 | 否 |
> | 作業 | 否 | 否 |
> | schedules | 否 | 否 |

## <a name="microsoftdigitaltwins"></a>Microsoft.DigitalTwins

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | digitaltwinsinstances | 否 | 否 |
> | digitaltwinsinstances/端點 | 否 | 否 |
> | digitaltwinsinstances/operationresults | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | databaseaccountnames | 否 | 否 |
> | databaseaccounts | Yes | Yes |
> | 位置 | 否 | 否 |
> | locations / deletevirtualnetworkorsubnets | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operationsstatus | 否 | 否 |
> | operationresults | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checkdomainavailability | 否 | 否 |
> | domains | 是 | 是 |
> | domains / domainownershipidentifiers | 否 | 否 |
> | generatessorequest | 否 | 否 |
> | listdomainrecommendations | 否 | 否 |
> | 作業 | 否 | 否 |
> | topleveldomains | 否 | 否 |
> | validatedomainregistrationinformation | 否 | 否 |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | 服務 | 否 | 否 |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | domains | 否 | 否 |
> | domains / topics | 否 | 否 |
> | eventsubscriptions | 否 - 無法獨立移動，但會自動與已訂閱的資源一起移動。 | 否 - 無法獨立移動，但會自動與已訂閱的資源一起移動。 |
> | extensionTopics | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / eventsubscriptions | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operationsstatus | 否 | 否 |
> | locations / topictypes | 否 | 否 |
> | operationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | operationsstatus | 否 | 否 |
> | partnernamespaces | 否 | 否 |
> | partnernamespaces / eventchannels | 否 | 否 |
> | partnerregistrations | 否 | 否 |
> | partnertopics | 否 | 否 |
> | partnertopics / eventsubscriptions | 否 | 否 |
> | systemtopics | 否 | 否 |
> | systemtopics / eventsubscriptions | 否 | 否 |
> | topics | 否 | 否 |
> | topictypes | 否 | 否 |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | availableclusterregions | 否 | 否 |
> | checknameavailability | 否 | 否 |
> | checknamespaceavailability | 否 | 否 |
> | clusters | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / deletevirtualnetworkorsubnets | 否 | 否 |
> | 命名空間 | 否 | 否 |
> | namespaces / authorizationrules | 否 | 否 |
> | namespaces / disasterrecoveryconfigs | 否 | 否 |
> | namespaces / disasterrecoveryconfigs / checknameavailability | 否 | 否 |
> | namespaces / eventhubs | 否 | 否 |
> | namespaces / eventhubs / authorizationrules | 否 | 否 |
> | namespaces / eventhubs / consumergroups | 否 | 否 |
> | namespaces / networkrulesets | 否 | 否 |
> | 作業 | 否 | 否 |
> | sku | 否 | 否 |

## <a name="microsoftexperimentation"></a>Microsoft.Experimentation

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | experimentworkspaces | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operations | 否 | 否 |

## <a name="microsoftfalcon"></a>Microsoft.Falcon

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 命名空間 | 否 | 否 |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | featureproviders | 否 | 否 |
> | 特性 | 否 | 否 |
> | 作業 | 否 | 否 |
> | 提供者 | 否 | 否 |
> | subscriptionfeatureregistrations | 否 | 否 |

## <a name="microsoftgenomics"></a>MICROSOFT.GENOMICS

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | automanagedaccounts | 否 | 否 |
> | automanagedvmconfigurationprofiles | 否 | 否 |
> | guestconfigurationassignments | 否 | 否 |
> | 作業 | 否 | 否 |
> | software | 否 | 否 |
> | softwareupdateprofile | 否 | 否 |
> | softwareupdates | 否 | 否 |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | hanainstances | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operations | 否 | 否 |
> | locations / operationsstatus | 否 | 否 |
> | 作業 | 否 | 否 |
> | sapmonitors | 否 | 否 |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | dedicatedhsms | 否 | 否 |
> | 位置 | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | clusters | 是 | 是 |
> | clusters / applications | 否 | 否 |
> | clusters / operationresults | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / azureasyncoperations | 否 | 否 |
> | locations / billingspecs | 否 | 否 |
> | locations / capabilities | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / usages | 否 | 否 |
> | locations / validatecreaterequest | 否 | 否 |
> | 作業 | 否 | 否 |

> [!IMPORTANT]
> 您可以將 HDInsight 叢集移至新的訂用帳戶或資源群組。 不過，您無法跨訂用帳戶來移動連結至 HDInsight 叢集的網路資源 (例如虛擬網路、NIC 或負載平衡器)。 此外，您無法將已連結至叢集虛擬機器的 NIC 移至新的資源群組。
>
> 將 HDInsight 叢集移至新的訂用帳戶時，請先移動其他資源 (例如儲存體帳戶)。 然後，移動 HDInsight 叢集本身。

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | 服務 | 否 | 否 |
> | services / privateendpointconnections | 否 | 否 |
> | services / privatelinkresources | 否 | 否 |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operationstatus | 否 | 否 |
> | machines | 否 | 否 |
> | machines / extensions | 是 | 是 |
> | 作業 | 否 | 否 |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | datamanagers | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsofthybridnetwork"></a>HybridNetwork

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | devices | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |
> | vnfs | 否 | 否 |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | components | 否 | 否 |
> | 位置 | 否 | 否 |
> | networkscopes | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | jobs | Yes | Yes |
> | 位置 | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | actiongroups | 否 | 否 |
> | activitylogalerts | 否 | 否 |
> | alertrules | Yes | Yes |
> | autoscalesettings | Yes | Yes |
> | baseline | 否 | 否 |
> | calculatebaseline | 否 | 否 |
> | components | 是 | 是 |
> | components / events | 否 | 否 |
> | components / linkedstorageaccounts | 否 | 否 |
> | 元件/中繼資料 | 否 | 否 |
> | components / metrics | 否 | 否 |
> | components / pricingplans | 否 | 否 |
> | components / query | 否 | 否 |
> | datacollectionrules | 否 | 否 |
> | diagnosticsettings | 否 | 否 |
> | diagnosticsettingscategories | 否 | 否 |
> | eventcategories | 否 | 否 |
> | eventtypes | 否 | 否 |
> | extendeddiagnosticsettings | 否 | 否 |
> | guestdiagnosticsettings | 否 | 否 |
> | listmigrationdate | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | logdefinitions | 否 | 否 |
> | logprofiles | 否 | 否 |
> | 記錄 | 否 | 否 |
> | metricalerts | 否 | 否 |
> | metricbaselines | 否 | 否 |
> | metricbatch | 否 | 否 |
> | metricdefinitions | 否 | 否 |
> | metricnamespaces | 否 | 否 |
> | metrics | 否 | 否 |
> | migratealertrules | 否 | 否 |
> | migratetonewpricingmodel | 否 | 否 |
> | myworkbooks | 否 | 否 |
> | notificationgroups | 否 | 否 |
> | 作業 | 否 | 否 |
> | privatelinkscopeoperationstatuses | 否 | 否 |
> | privatelinkscopes | 否 | 否 |
> | privatelinkscopes / privateendpointconnectionproxies | 否 | 否 |
> | privatelinkscopes / privateendpointconnections | 否 | 否 |
> | privatelinkscopes / scopedresources | 否 | 否 |
> | rollbacktolegacypricingmodel | 否 | 否 |
> | scheduledqueryrules | Yes | 是 |
> | 拓撲 | 否 | 否 |
> | 交易 | 否 | 否 |
> | vminsightsonboardingstatuses | 否 | 否 |
> | webtests | 是 | Yes |
> | webtest/gettestresultfile | 否 | 否 |
> | workbooks | 否 | 否 |
> | workbooktemplates | 否 | 否 |

> [!IMPORTANT]
> 確定移至新的訂用帳戶不會超過[訂用帳戶配額](azure-subscription-service-limits.md#azure-monitor-limits)。

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | apptemplates | 否 | 否 |
> | checknameavailability | 否 | 否 |
> | checksubdomainavailability | 否 | 否 |
> | iotapps | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | graph | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | deletedvaults | 否 | 否 |
> | hsmpools | Yes | Yes |
> | 位置 | 否 | 否 |
> | locations / deletedvaults | 否 | 否 |
> | locations / deletevirtualnetworkorsubnets | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | managedhsms | 否 | 否 |
> | 作業 | 否 | 否 |
> | vaults | Yes | Yes |
> | vaults / accesspolicies | 否 | 否 |
> | vaults / eventgridfilters | 否 | 否 |
> | vaults / secrets | 否 | 否 |

> [!IMPORTANT]
> 用於磁碟加密的 Key Vault 無法移至相同訂用帳戶中或跨訂用帳戶的資源群組。

## <a name="microsoftkubernetes"></a>Microsoft.Kubernetes

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | connectedclusters | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |
> | registeredsubscriptions | 否 | 否 |

## <a name="microsoftkubernetesconfiguration"></a>Microsoft.KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | sourcecontrolconfigurations | 否 | 否 |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | clusters | 否 | 否 |
> | clusters / attacheddatabaseconfigurations | 否 | 否 |
> | clusters / databases | 否 | 否 |
> | clusters / databases / dataconnections | 否 | 否 |
> | clusters / databases / eventhubconnections | 否 | 否 |
> | clusters / databases / principalassignments | 否 | 否 |
> | clusters / principalassignments | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | labaccounts | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operations | 否 | 否 |
> | 作業 | 否 | 否 |
> | users | 否 | 否 |

## <a name="microsoftlocationbasedservices"></a>MICROSOFT.LOCATIONBASEDSERVICES

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |

## <a name="microsoftlocationservices"></a>MICROSOFT.LOCATIONSERVICES.LOG

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | hostingenvironments | 否 | 否 |
> | integrationaccounts | 是 | 是 |
> | integrationserviceenvironments | 否 | 否 |
> | integrationserviceenvironments / managedapis | 否 | 否 |
> | isolatedenvironments | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / workflows | 否 | 否 |
> | 作業 | 否 | 否 |
> | workflows | 是 | 是 |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | commitmentplans | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operations | 否 | 否 |
> | locations / operationsstatus | 否 | 否 |
> | 作業 | 否 | 否 |
> | webservices | 否 | 否 |
> | workspaces | 否 | 否 |

## <a name="microsoftmachinelearningcompute"></a>MICROSOFT.MACHINELEARNINGCOMPUTE

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | operationalizationclusters | 否 | 否 |

## <a name="microsoftmachinelearningexperimentation"></a>MICROSOFT.MACHINELEARNINGEXPERIMENTATION

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |
> | accounts / workspaces | 否 | 否 |
> | accounts / workspaces / projects | 否 | 否 |
> | teamaccounts | 否 | 否 |
> | teamaccounts / workspaces | 否 | 否 |
> | teamaccounts / workspaces / projects | 否 | 否 |

## <a name="microsoftmachinelearningmodelmanagement"></a>MICROSOFT.MACHINELEARNINGMODELMANAGEMENT

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / computeoperationsstatus | 否 | 否 |
> | locations / quotas | 否 | 否 |
> | locations / updatequotas | 否 | 否 |
> | locations / usages | 否 | 否 |
> | locations / vmsizes | 否 | 否 |
> | locations / workspaceoperationsstatus | 否 | 否 |
> | 作業 | 否 | 否 |
> | workspaces | 否 | 否 |
> | workspaces / computes | 否 | 否 |
> | workspaces / eventgridfilters | 否 | 否 |

## <a name="microsoftmaintenance"></a>Microsoft.Maintenance

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | applyupdates | 否 | 否 |
> | configurationassignments | 否 | 否 |
> | maintenanceconfigurations | 否 | 否 |
> | updates | 否 | 否 |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 身分識別 | 否 | 否 |
> | 作業 | 否 | 否 |
> | userassignedidentities | 否 | 否 |

## <a name="microsoftmanagednetwork"></a>MICROSOFT.MANAGEDNETWORK

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | managednetworks | 否 | 否 |
> | managednetworks / managednetworkgroups | 否 | 否 |
> | managednetworks / managednetworkpeeringpolicies | 否 | 否 |
> | 通知 | 否 | 否 |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | marketplaceregistrationdefinitions | 否 | 否 |
> | 作業 | 否 | 否 |
> | operationstatuses | 否 | 否 |
> | registrationassignments | 否 | 否 |
> | registrationdefinitions | 否 | 否 |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | getentities | 否 | 否 |
> | managementgroups | 否 | 否 |
> | managementgroups / settings | 否 | 否 |
> | operationresults | 否 | 否 |
> | operationresults / asyncoperation | 否 | 否 |
> | 作業 | 否 | 否 |
> | resources | 否 | 否 |
> | starttenantbackfill | 否 | 否 |
> | tenantbackfillstatus | 否 | 否 |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |
> | accounts / eventgridfilters | 否 | 否 |
> | accounts / privateatlases | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | listavailableoffers | 否 | 否 |
> | offers | 否 | 否 |
> | offertypes | 否 | 否 |
> | offertypes / publishers | 否 | 否 |
> | offertypes / publishers / offers | 否 | 否 |
> | offertypes / publishers / offers / plans | 否 | 否 |
> | offertypes / publishers / offers / plans / agreements | 否 | 否 |
> | offertypes / publishers / offers / plans / configs | 否 | 否 |
> | offertypes / publishers / offers / plans / configs / importimage | 否 | 否 |
> | 作業 | 否 | 否 |
> | privategalleryitems | 否 | 否 |
> | privatestoreclient | 否 | 否 |
> | privatestores | 否 | 否 |
> | privatestores / offers | 否 | 否 |
> | products | 否 | 否 |
> | publishers | 否 | 否 |
> | publishers / offers | 否 | 否 |
> | publishers / offers / amendments | 否 | 否 |
> | 註冊 | 否 | 否 |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | classicdevservices | 否 | 否 |
> | listcommunicationpreference | 否 | 否 |
> | 作業 | 否 | 否 |
> | updatecommunicationpreference | 否 | 否 |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | agreements | 否 | 否 |
> | offertypes | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | mediaservices | 否 | 否 |
> | mediaservices / accountfilters | 否 | 否 |
> | mediaservices / assets | 否 | 否 |
> | mediaservices / assets / assetfilters | 否 | 否 |
> | mediaservices / contentkeypolicies | 否 | 否 |
> | mediaservices / eventgridfilters | 否 | 否 |
> | mediaservices / liveeventoperations | 否 | 否 |
> | mediaservices / liveevents | 否 | 否 |
> | mediaservices / liveevents / liveoutputs | 否 | 否 |
> | mediaservices / liveoutputoperations | 否 | 否 |
> | mediaservices / streamingendpointoperations | 否 | 否 |
> | mediaservices / streamingendpoints | 否 | 否 |
> | mediaservices / streaminglocators | 否 | 否 |
> | mediaservices / streamingpolicies | 否 | 否 |
> | mediaservices / transforms | 否 | 否 |
> | mediaservices / transforms / jobs | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftmicroservices4spring"></a>MICROSOFT.MICROSERVICES4SPRING

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | appclusters | 否 | 否 |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | assessmentprojects | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / assessmentoptions | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | migrateprojects | 否 | 否 |
> | movecollections | 否 | 否 |
> | 作業 | 否 | 否 |
> | projects | 否 | 否 |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | holographicsbroadcastaccounts | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | objectunderstandingaccounts | 否 | 否 |
> | 作業 | 否 | 否 |
> | remoterenderingaccounts | 否 | 否 |
> | spatialanchorsaccounts | 否 | 否 |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | netappaccounts | 否 | 否 |
> | netappaccounts / backuppolicies | 否 | 否 |
> | netappaccounts / capacitypools | 否 | 否 |
> | netappaccounts / capacitypools / volumes | 否 | 否 |
> | netappaccounts / capacitypools / volumes / mounttargets | 否 | 否 |
> | netappaccounts / capacitypools / volumes / snapshots | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | applicationgatewayavailablerequestheaders | 否 | 否 |
> | applicationgatewayavailableresponseheaders | 否 | 否 |
> | applicationgatewayavailableservervariables | 否 | 否 |
> | applicationgatewayavailablessloptions | 否 | 否 |
> | applicationgatewayavailablewafrulesets | 否 | 否 |
> | applicationgateways | 否 | 否 |
> | applicationgatewaywebapplicationfirewallpolicies | 否 | 否 |
> | applicationsecuritygroups | 否 | 否 |
> | azurefirewallfqdntags | 否 | 否 |
> | azurefirewalls | 否 | 否 |
> | bastionhosts | 否 | 否 |
> | bgpservicecommunities | 否 | 否 |
> | checkfrontdoornameavailability | 否 | 否 |
> | checktrafficmanagernameavailability | 否 | 否 |
> | connections | 否 | 否 |
> | ddoscustompolicies | 否 | 否 |
> | ddosprotectionplans | 否 | 否 |
> | dnsoperationresults | 否 | 否 |
> | dnsoperationstatuses | 否 | 否 |
> | dnszones | 否 | 否 |
> | dnszones / a | 否 | 否 |
> | dnszones / aaaa | 否 | 否 |
> | dnszones / all | 否 | 否 |
> | dnszones / caa | 否 | 否 |
> | dnszones / cname | 否 | 否 |
> | dnszones / mx | 否 | 否 |
> | dnszones / ns | 否 | 否 |
> | dnszones / ptr | 否 | 否 |
> | dnszones / recordsets | 否 | 否 |
> | dnszones / soa | 否 | 否 |
> | dnszones / srv | 否 | 否 |
> | dnszones / txt | 否 | 否 |
> | expressroutecircuits | 否 | 否 |
> | expressroutegateways | 否 | 否 |
> | expressrouteserviceproviders | 否 | 否 |
> | firewallpolicies | 否 | 否 |
> | frontdooroperationresults | 否 | 否 |
> | frontdoors | 否 | 否 |
> | frontdoors/frontendendpoints | 否 | 否 |
> | frontdoorwebapplicationfirewallmanagedrulesets | 否 | 否 |
> | frontdoorwebapplicationfirewallpolicies | 否 | 否 |
> | getdnsresourcereference | 否 | 否 |
> | internalnotify | 否 | 否 |
> | ipallocations | 否 | 否 |
> | ipgroups | 否 | 否 |
> | loadbalancers | 是 - 基本 SKU<br>否 - 標準 SKU | 是 - 基本 SKU<br>否 - 標準 SKU |
> | localnetworkgateways | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / autoapprovedprivatelinkservices | 否 | 否 |
> | locations / availabledelegations | 否 | 否 |
> | locations / availableprivateendpointtypes | 否 | 否 |
> | locations / availableservicealiases | 否 | 否 |
> | locations / baremetaltenants | 否 | 否 |
> | locations / batchnotifyprivateendpointsforresourcemove | 否 | 否 |
> | locations / batchvalidateprivateendpointsforresourcemove | 否 | 否 |
> | locations / checkacceleratednetworkingsupport | 否 | 否 |
> | locations / checkdnsnameavailability | 否 | 否 |
> | locations / checkprivatelinkservicevisibility | 否 | 否 |
> | locations / commitinternalazurenetworkmanagerconfiguration | 否 | 否 |
> | locations / effectiveresourceownership | 否 | 否 |
> | locations / nfvoperationresults | 否 | 否 |
> | locations / nfvoperations | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operations | 否 | 否 |
> | locations / servicetags | 否 | 否 |
> | locations / setresourceownership | 否 | 否 |
> | locations / supportedvirtualmachinesizes | 否 | 否 |
> | locations / usages | 否 | 否 |
> | locations / validateresourceownership | 否 | 否 |
> | locations / virtualnetworkavailableendpointservices | 否 | 否 |
> | natgateways | 否 | 否 |
> | networkexperimentprofiles | 否 | 否 |
> | networkintentpolicies | 否 | 否 |
> | networkinterfaces | 否 | 否 |
> | networkprofiles | 否 | No |
> | networksecuritygroups | 否 | No |
> | networkwatchers | 是 | 否 |
> | networkwatchers / connectionmonitors | 是 | 否 |
> | networkwatchers / flowlogs | 是 | 否 |
> | networkwatchers / pingmeshes | 是 | 否 |
> | 作業 | 否 | 否 |
> | p2svpngateways | 否 | 否 |
> | privatednsoperationresults | 否 | No |
> | privatednsoperationstatuses | 否 | No |
> | privatednszones | 否 | 否 |
> | privatednszones / a | 否 | No |
> | privatednszones / aaaa | 否 | 否 |
> | privatednszones / all | 否 | 否 |
> | privatednszones / cname | 否 | 否 |
> | privatednszones / mx | 否 | 否 |
> | privatednszones / ptr | 否 | No |
> | privatednszones / soa | 否 | No |
> | privatednszones / srv | 否 | 否 |
> | privatednszones / txt | 否 | 否 |
> | privatednszones / virtualnetworklinks | 否 | No |
> | privatednszonesinternal | 否 | No |
> | privateendpointredirectmaps | 否 | 否 |
> | privateendpoints | 否 | 否 |
> | privatelinkservices | 否 | No |
> | publicipaddresses | 是 - 基本 SKU<br>否 - 標準 SKU | 是 - 基本 SKU<br>否 - 標準 SKU |
> | publicipprefixes | 否 | No |
> | routefilters | 否 | No |
> | routetables | 否 | No |
> | securitypartnerproviders | 否 | 否 |
> | serviceendpointpolicies | 否 | No |
> | trafficmanagergeographichierarchies | 否 | No |
> | trafficmanagerprofiles | 否 | No |
> | trafficmanagerprofiles / heatmaps | 否 | 否 |
> | trafficmanagerusermetricskeys | 否 | 否 |
> | virtualhubs | 否 | 否 |
> | virtualnetworkgateways | 否 | 否 |
> | virtualnetworks | 否 | 否 |
> | virtualnetworktaps | 否 | 否 |
> | virtualrouters | 否 | 否 |
> | virtualwans | 否 | 否 |
> | vpngateways (虛擬 WAN) | 否 | 否 |
> | vpnserverconfigurations | 否 | 否 |
> | vpnsites (虛擬 WAN) | 否 | 否 |

> [!IMPORTANT]
> 請參閱[網路移動指引](./move-limitations/networking-move-limitations.md)。

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | checknamespaceavailability | 否 | 否 |
> | 命名空間 | 否 | 否 |
> | namespaces / notificationhubs | 否 | 否 |
> | operationresults | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | osnamespaces | 否 | 否 |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | hypervsites | 否 | 否 |
> | importsites | 否 | 否 |
> | 作業 | 否 | 否 |
> | serversites | 否 | 否 |
> | vmwaresites | 否 | 否 |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | clusters | 是 | Yes |
> | deletedworkspaces | 否 | 否 |
> | linktargets | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |
> | storageinsightconfigs | 否 | 否 |
> | workspaces | Yes | 是 |
> | workspaces / datasources | 否 | 否 |
> | workspaces / linkedservices | 否 | 否 |
> | workspaces / linkedstorageaccounts | 否 | 否 |
> | workspaces / metadata | 否 | 否 |
> | workspaces / query | 否 | 否 |
> | workspaces / scopedprivatelinkproxies | 否 | 否 |

> [!IMPORTANT]
> 確定移至新的訂用帳戶不會超過[訂用帳戶配額](azure-subscription-service-limits.md#azure-monitor-limits)。
>
> 無法移動具有連結自動化帳戶的工作區。 開始移動作業之前，請務必將任何自動化帳戶取消連結。

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | managementassociations | 否 | 否 |
> | managementconfigurations | 否 | 否 |
> | 作業 | 否 | 否 |
> | solutions | 否 | 否 |
> | 檢視 | 否 | 否 |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checkserviceprovideravailability | 否 | 否 |
> | legacypeerings | 否 | 否 |
> | 作業 | 否 | 否 |
> | peerasns | 否 | 否 |
> | peeringlocations | 否 | 否 |
> | peerings | 否 | 否 |
> | peeringservicecountries | 否 | 否 |
> | peeringservicelocations | 否 | 否 |
> | peeringserviceproviders | 否 | 否 |
> | peeringservices | 否 | 否 |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | asyncoperationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | policyevents | 否 | 否 |
> | policystates | 否 | 否 |
> | policytrackedresources | 否 | 否 |
> | remediations | 否 | 否 |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | consoles | 否 | 否 |
> | dashboards | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / consoles | 否 | 否 |
> | locations / usersettings | 否 | 否 |
> | 作業 | 否 | 否 |
> | usersettings | 否 | 否 |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | 作業 | 否 | 否 |
> | privatelinkservicesforpowerbi | 否 | 否 |
> | privatelinkservicesforpowerbi/operationresults | 否 | 否 |
> | tenants | 否 | 否 |
> | workspacecollections | 否 | 否 |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | capacities | 是 | 是 |
> | 位置 | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftpowerplatform"></a>PowerPlatform

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 作業 | 否 | 否 |

## <a name="microsoftprojectbabylon"></a>Microsoft.ProjectBabylon

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |
> | checknameavailability | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftproviderhub"></a>Microsoft.ProviderHub

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | availableaccounts | 否 | 否 |
> | providerregistrations | 否 | 否 |
> | providerregistrations / defaultrollouts | 否 | 否 |
> | providerregistrations / resourcetyperegistrations | 否 | 否 |
> | rollouts | 否 | 否 |

## <a name="microsoftquantum"></a>Microsoft.Quantum

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |
> | workspaces | 否 | 否 |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | backupprotecteditems | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / allocatedstamp | 否 | 否 |
> | locations / allocatestamp | 否 | 否 |
> | locations / backupaadproperties | 否 | 否 |
> | locations / backupcrossregionrestore | 否 | 否 |
> | locations / backupcrrjob | 否 | 否 |
> | locations / backupcrrjobs | 否 | 否 |
> | locations / backupcrroperationresults | 否 | 否 |
> | locations / backupcrroperationsstatus | 否 | 否 |
> | locations / backupprevalidateprotection | 否 | 否 |
> | locations / backupstatus | 否 | 否 |
> | locations / backupvalidatefeatures | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | 作業 | 否 | 否 |
> | replicationeligibilityresults | 否 | 否 |
> | vaults | 是 | 是 |

> [!IMPORTANT]
> 請參閱[復原服務移動指引](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)。

## <a name="microsoftredhatopenshift"></a>Microsoft.RedHatOpenShift

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operationsstatus | 否 | 否 |
> | openshiftclusters | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | 命名空間 | 否 | 否 |
> | namespaces / authorizationrules | 否 | 否 |
> | namespaces / hybridconnections | 否 | 否 |
> | namespaces / hybridconnections / authorizationrules | 否 | 否 |
> | namespaces / privateendpointconnections | 否 | 否 |
> | namespaces / wcfrelays | 否 | 否 |
> | namespaces / wcfrelays / authorizationrules | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 作業 | 否 | 否 |
> | 查詢 | 是 | 是 |
> | resourcechangedetails | 否 | 否 |
> | resourcechanges | 否 | 否 |
> | resources | 否 | 否 |
> | resourceshistory | 否 | 否 |
> | subscriptionsstatus | 否 | 否 |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | availabilitystatuses | 否 | 否 |
> | childavailabilitystatuses | 否 | 否 |
> | childresources | 否 | 否 |
> | emergingissues | 否 | 否 |
> | 活動 | 否 | 否 |
> | 中繼資料 | 否 | 否 |
> | 通知 | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | calculatetemplatehash | 否 | 否 |
> | checkpolicycompliance | 否 | 否 |
> | checkresourcename | 否 | 否 |
> | deployments | 否 | 否 |
> | deployments / operations | 否 | 否 |
> | deploymentscripts | 否 | 否 |
> | deploymentscripts / logs | 否 | 否 |
> | 連結 | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / deploymentscriptoperationresults | 否 | 否 |
> | notifyresourcejobs | 否 | 否 |
> | operationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | 提供者 | 否 | 否 |
> | resourcegroups | 否 | 否 |
> | resources | 否 | 否 |
> | subscriptions | 否 | 否 |
> | subscriptions / locations | 否 | 否 |
> | subscriptions / operationresults | 否 | 否 |
> | subscriptions / providers | 否 | 否 |
> | subscriptions / resourcegroups | 否 | 否 |
> | subscriptions / resourcegroups / resources | 否 | 否 |
> | subscriptions / resources | 否 | 否 |
> | subscriptions / tagnames | 否 | 否 |
> | subscriptions / tagnames / tagvalues | 否 | 否 |
> | tags | 否 | 否 |
> | templatespecs | 否 | 否 |
> | templatespecs / versions | 否 | 否 |
> | tenants | 否 | 否 |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 應用程式所需 | 否 | 否 |
> | checkmoderneligibility | 否 | 否 |
> | checknameavailability | 否 | 否 |
> | operationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | saasresources | 否 | 否 |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | checkservicenameavailability | 否 | 否 |
> | 作業 | 否 | 否 |
> | resourcehealthmetadata | 否 | 否 |
> | searchservices | 是 | 是 |

> [!IMPORTANT]
> 您無法在一個作業中移動不同區域中的數個搜尋資源， 而是要在不同作業中移動它們。

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | adaptivenetworkhardenings | 否 | 否 |
> | advancedthreatprotectionsettings | 否 | 否 |
> | alerts | 否 | 否 |
> | alertssuppressionrules | 否 | 否 |
> | allowedconnections | 否 | 否 |
> | applicationwhitelistings | 否 | 否 |
> | assessmentmetadata | 否 | 否 |
> | assessments | 否 | 否 |
> | autodismissalertsrules | 否 | 否 |
> | automations | 否 | 否 |
> | autoprovisioningsettings | 否 | 否 |
> | complianceresults | 否 | 否 |
> | compliances | 否 | 否 |
> | datacollectionagents | 否 | 否 |
> | devicesecuritygroups | 否 | 否 |
> | discoveredsecuritysolutions | 否 | 否 |
> | externalsecuritysolutions | 否 | 否 |
> | informationprotectionpolicies | 否 | 否 |
> | iotsecuritysolutions | 否 | 否 |
> | iotsecuritysolutions / analyticsmodels | 否 | 否 |
> | iotsecuritysolutions / analyticsmodels / aggregatedalerts | 否 | 否 |
> | iotsecuritysolutions / analyticsmodels / aggregatedrecommendations | 否 | 否 |
> | iotsecuritysolutions / iotalerts | 否 | 否 |
> | iotsecuritysolutions / iotalerttypes | 否 | 否 |
> | jitnetworkaccesspolicies | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / alerts | 否 | 否 |
> | locations / allowedconnections | 否 | 否 |
> | locations / applicationwhitelistings | 否 | 否 |
> | locations / discoveredsecuritysolutions | 否 | 否 |
> | locations / externalsecuritysolutions | 否 | 否 |
> | locations / jitnetworkaccesspolicies | 否 | 否 |
> | locations / securitysolutions | 否 | 否 |
> | locations / securitysolutionsreferencedata | 否 | 否 |
> | locations / tasks | 否 | 否 |
> | locations / topologies | 否 | 否 |
> | 作業 | 否 | 否 |
> | 原則 | 否 | 否 |
> | pricings | 否 | 否 |
> | regulatorycompliancestandards | 否 | 否 |
> | regulatorycompliancestandards / regulatorycompliancecontrols | 否 | 否 |
> | regulatorycompliancestandards / regulatorycompliancecontrols / regulatorycomplianceassessments | 否 | 否 |
> | securescorecontroldefinitions | 否 | 否 |
> | securescorecontrols | 否 | 否 |
> | securescores | 否 | 否 |
> | securescores / securescorecontrols | 否 | 否 |
> | securitycontacts | 否 | 否 |
> | securitysolutions | 否 | 否 |
> | securitysolutionsreferencedata | 否 | 否 |
> | securitystatuses | 否 | 否 |
> | securitystatusessummaries | 否 | 否 |
> | servervulnerabilityassessments | 否 | 否 |
> | settings | 否 | 否 |
> | subassessments | 否 | 否 |
> | 工作 | 否 | 否 |
> | topologies | 否 | 否 |
> | workspacesettings | 否 | 否 |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | aggregations | 否 | 否 |
> | alertrules | 否 | 否 |
> | alertruletemplates | 否 | 否 |
> | automationrules | 否 | 否 |
> | bookmarks | 否 | 否 |
> | 案例 | 否 | 否 |
> | dataconnectors | 否 | 否 |
> | dataconnectorscheckrequirements | 否 | 否 |
> | 實體 | 否 | 否 |
> | entityqueries | 否 | 否 |
> | incidents | 否 | 否 |
> | officeconsents | 否 | 否 |
> | 作業 | 否 | 否 |
> | settings | 否 | 否 |
> | threatintelligence | 否 | 否 |

## <a name="microsoftserialconsole"></a>Microsoft.SerialConsole

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | consoleservices | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / consoleservices | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftservermanagement"></a>MICROSOFT.SERVERMANAGEMENT

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | gateways | 否 | 否 |
> | nodes | 否 | 否 |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | checknamespaceavailability | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / deletevirtualnetworkorsubnets | 否 | 否 |
> | 命名空間 | 否 | 否 |
> | namespaces / authorizationrules | 否 | 否 |
> | namespaces / disasterrecoveryconfigs | 否 | 否 |
> | namespaces / disasterrecoveryconfigs / checknameavailability | 否 | 否 |
> | namespaces / eventgridfilters | 否 | 否 |
> | namespaces / networkrulesets | 否 | 否 |
> | namespaces / queues | 否 | 否 |
> | namespaces / queues / authorizationrules | 否 | 否 |
> | namespaces / topics | 否 | 否 |
> | namespaces / topics / authorizationrules | 否 | 否 |
> | namespaces / topics / subscriptions | 否 | 否 |
> | namespaces / topics / subscriptions / rules | 否 | 否 |
> | 作業 | 否 | 否 |
> | premiummessagingregions | 否 | 否 |
> | sku | 否 | 否 |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 應用程式所需 | 否 | 否 |
> | clusters | 否 | 否 |
> | clusters / applications | 否 | 否 |
> | containergroups | 否 | 否 |
> | containergroupsets | 否 | 否 |
> | edgeclusters | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / clusterversions | 否 | 否 |
> | locations / environments | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operations | 否 | 否 |
> | managedclusters | 否 | 否 |
> | networks | 否 | 否 |
> | 作業 | 否 | 否 |
> | secretstores | 否 | 否 |
> | 磁碟區 | 否 | 否 |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 應用程式所需 | 否 | 否 |
> | containergroups | 否 | 否 |
> | gateways | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / applicationoperations | 否 | 否 |
> | locations / gatewayoperations | 否 | 否 |
> | locations / networkoperations | 否 | 否 |
> | locations / secretoperations | 否 | 否 |
> | locations / volumeoperations | 否 | 否 |
> | networks | 否 | 否 |
> | 作業 | 否 | 否 |
> | 密碼 | 否 | 否 |
> | 磁碟區 | 否 | 否 |

## <a name="microsoftservices"></a>MICROSOFT.伺服器

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | rollouts | 否 | 否 |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | locations / usages | 否 | 否 |
> | 作業 | 否 | 否 |
> | signalr | Yes | 是 |
> | signalr / eventgridfilters | 否 | 否 |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | hybridusebenefits | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | applicationdefinitions | 否 | 否 |
> | 應用程式所需 | 否 | 否 |
> | jitrequests | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | instancepools | 否 | 否 |
> | 位置 | 是 | Yes |
> | locations / administratorazureasyncoperation | 否 | 否 |
> | locations / administratoroperationresults | 否 | 否 |
> | locations / auditingsettingsazureasyncoperation | 否 | 否 |
> | locations / auditingsettingsoperationresults | 否 | 否 |
> | locations / capabilities | 否 | 否 |
> | locations / databaseazureasyncoperation | 否 | 否 |
> | locations / databaseoperationresults | 否 | 否 |
> | locations / databaserestoreazureasyncoperation | 否 | 否 |
> | locations / deletevirtualnetworkorsubnets | 否 | 否 |
> | locations / deletevirtualnetworkorsubnetsazureasyncoperation | 否 | 否 |
> | locations / deletevirtualnetworkorsubnetsoperationresults | 否 | 否 |
> | locations / dnsaliasasyncoperation | 否 | 否 |
> | locations / dnsaliasoperationresults | 否 | 否 |
> | locations / elasticpoolazureasyncoperation | 否 | 否 |
> | locations / elasticpooloperationresults | 否 | 否 |
> | locations / encryptionprotectorazureasyncoperation | 否 | 否 |
> | locations / encryptionprotectoroperationresults | 否 | 否 |
> | locations / extendedauditingsettingsazureasyncoperation | 否 | 否 |
> | locations / extendedauditingsettingsoperationresults | 否 | 否 |
> | locations / failovergroupazureasyncoperation | 否 | 否 |
> | locations / failovergroupoperationresults | 否 | 否 |
> | locations / firewallrulesazureasyncoperation | 否 | 否 |
> | locations / firewallrulesoperationresults | 否 | 否 |
> | locations / instancefailovergroupazureasyncoperation | 否 | 否 |
> | locations / instancefailovergroupoperationresults | 否 | 否 |
> | locations / instancefailovergroups | 否 | 否 |
> | locations / instancepoolazureasyncoperation | 否 | 否 |
> | locations / instancepooloperationresults | 否 | 否 |
> | locations / jobagentazureasyncoperation | 否 | 否 |
> | locations / jobagentoperationresults | 否 | 否 |
> | locations / longtermretentionbackupazureasyncoperation | 否 | 否 |
> | locations / longtermretentionbackupoperationresults | 否 | 否 |
> | locations / longtermretentionbackups | 否 | 否 |
> | locations / longtermretentionmanagedinstancebackupazureasyncoperation | 否 | 否 |
> | locations / longtermretentionmanagedinstancebackupoperationresults | 否 | 否 |
> | locations / longtermretentionmanagedinstancebackups | 否 | 否 |
> | locations / longtermretentionmanagedinstances | 否 | 否 |
> | locations / longtermretentionpolicyazureasyncoperation | 否 | 否 |
> | locations / longtermretentionpolicyoperationresults | 否 | 否 |
> | locations / longtermretentionservers | 否 | 否 |
> | locations / manageddatabaseazureasyncoperation | 否 | 否 |
> | locations / manageddatabasecompleterestoreazureasyncoperation | 否 | 否 |
> | locations / manageddatabasecompleterestoreoperationresults | 否 | 否 |
> | locations / manageddatabaseoperationresults | 否 | 否 |
> | locations / manageddatabaserestoreazureasyncoperation | 否 | 否 |
> | locations / manageddatabaserestoreoperationresults | 否 | 否 |
> | locations / managedinstanceazureasyncoperation | 否 | 否 |
> | locations / managedinstanceencryptionprotectorazureasyncoperation | 否 | 否 |
> | locations / managedinstanceencryptionprotectoroperationresults | 否 | 否 |
> | locations / managedinstancekeyazureasyncoperation | 否 | 否 |
> | locations / managedinstancekeyoperationresults | 否 | 否 |
> | locations / managedinstancelongtermretentionpolicyazureasyncoperation | 否 | 否 |
> | locations / managedinstancelongtermretentionpolicyoperationresults | 否 | 否 |
> | locations / managedinstanceoperationresults | 否 | 否 |
> | locations / managedinstancetdecertazureasyncoperation | 否 | 否 |
> | locations / managedinstancetdecertoperationresults | 否 | 否 |
> | locations / managedserversecurityalertpoliciesazureasyncoperation | 否 | 否 |
> | locations / managedserversecurityalertpoliciesoperationresults | 否 | 否 |
> | locations / managedshorttermretentionpolicyazureasyncoperation | 否 | 否 |
> | locations / managedshorttermretentionpolicyoperationresults | 否 | 否 |
> | locations / notifyazureasyncoperation | 否 | 否 |
> | locations / privateendpointconnectionazureasyncoperation | 否 | 否 |
> | locations / privateendpointconnectionoperationresults | 否 | 否 |
> | locations / privateendpointconnectionproxyazureasyncoperation | 否 | 否 |
> | locations / privateendpointconnectionproxyoperationresults | 否 | 否 |
> | locations / securityalertpoliciesazureasyncoperation | 否 | 否 |
> | locations / securityalertpoliciesoperationresults | 否 | 否 |
> | locations / serveradministratorazureasyncoperation | 否 | 否 |
> | locations / serveradministratoroperationresults | 否 | 否 |
> | locations / serverazureasyncoperation | 否 | 否 |
> | locations / serverkeyazureasyncoperation | 否 | 否 |
> | locations / serverkeyoperationresults | 否 | 否 |
> | locations / serveroperationresults | 否 | 否 |
> | locations / shorttermretentionpolicyazureasyncoperation | 否 | 否 |
> | locations / shorttermretentionpolicyoperationresults | 否 | 否 |
> | locations / syncagentoperationresults | 否 | 否 |
> | locations / syncdatabaseids | 否 | 否 |
> | locations / syncgroupoperationresults | 否 | 否 |
> | locations / syncmemberoperationresults | 否 | 否 |
> | locations / tdecertazureasyncoperation | 否 | 否 |
> | locations / tdecertoperationresults | 否 | 否 |
> | locations / usages | 否 | 否 |
> | locations / virtualclusterazureasyncoperation | 否 | 否 |
> | locations / virtualclusteroperationresults | 否 | 否 |
> | locations / virtualnetworkrulesazureasyncoperation | 否 | 否 |
> | locations / virtualnetworkrulesoperationresults | 否 | 否 |
> | locations / vulnerabilityassessmentscanazureasyncoperation | 否 | 否 |
> | locations / vulnerabilityassessmentscanoperationresults | 否 | 否 |
> | managedinstances | 否 | 否 |
> | managedinstances / administrators | 否 | 否 |
> | managedinstances / databases | 否 | 否 |
> | managedinstances / databases / backuplongtermretentionpolicies | 否 | 否 |
> | managedinstances / databases / vulnerabilityassessments | 否 | 否 |
> | managedinstances / metricdefinitions | 否 | 否 |
> | managedinstances / metrics | 否 | 否 |
> | managedinstances / recoverabledatabases | 否 | 否 |
> | managedinstances / tdecertificates | 否 | 否 |
> | managedinstances / vulnerabilityassessments | 否 | 否 |
> | 作業 | 否 | 否 |
> | servers | 否 | 否 |
> | servers / administratoroperationresults | 否 | 否 |
> | servers / administrators | 否 | 否 |
> | servers / advisors | 否 | 否 |
> | servers / aggregateddatabasemetrics | 否 | 否 |
> | servers / auditingsettings | 否 | 否 |
> | servers / automatictuning | 否 | 否 |
> | servers / communicationlinks | 否 | 否 |
> | servers / databases | 否 | 否 |
> | servers / databases / advisors | 否 | 否 |
> | servers / databases / auditingsettings | 否 | 否 |
> | servers / databases / auditrecords | 否 | 否 |
> | servers / databases / automatictuning | 否 | 否 |
> | servers / databases / backuplongtermretentionpolicies | 否 | 否 |
> | servers / databases / backupshorttermretentionpolicies | 否 | 否 |
> | servers / databases / datamaskingpolicies | 否 | 否 |
> | servers / databases / datamaskingpolicies / rules | 否 | 否 |
> | servers / databases / extensions | 否 | 否 |
> | servers / databases / geobackuppolicies | 否 | 否 |
> | servers / databases / metricdefinitions | 否 | 否 |
> | servers / databases / metrics | 否 | 否 |
> | servers / databases / recommendedsensitivitylabels | 否 | 否 |
> | servers / databases / securityalertpolicies | 否 | 否 |
> | servers / databases / syncgroups | 否 | 否 |
> | servers / databases / syncgroups / syncmembers | 否 | 否 |
> | servers / databases / topqueries | 否 | 否 |
> | servers / databases / topqueries / querytext | 否 | 否 |
> | servers / databases / transparentdataencryption | 否 | 否 |
> | servers / databases / vulnerabilityassessment | 否 | 否 |
> | servers / databases / vulnerabilityassessments | 否 | 否 |
> | servers / databases / vulnerabilityassessmentscans | 否 | 否 |
> | servers / databases / vulnerabilityassessmentsettings | 否 | 否 |
> | servers / databases / workloadgroups | 否 | 否 |
> | servers / databasesecuritypolicies | 否 | 否 |
> | servers / disasterrecoveryconfiguration | 否 | 否 |
> | servers / dnsaliases | 否 | 否 |
> | servers / elasticpoolestimates | 否 | 否 |
> | servers / elasticpools | 否 | 否 |
> | servers / elasticpools / advisors | 否 | 否 |
> | servers / elasticpools / metricdefinitions | 否 | 否 |
> | servers / elasticpools / metrics | 否 | 否 |
> | servers / encryptionprotector | 否 | 否 |
> | servers / extendedauditingsettings | 否 | 否 |
> | servers / failovergroups | 否 | 否 |
> | servers / import | 否 | 否 |
> | servers / importexportoperationresults | 否 | 否 |
> | servers / jobaccounts | 否 | 否 |
> | servers / jobagents | 否 | 否 |
> | servers / jobagents / jobs | 否 | 否 |
> | servers / jobagents / jobs / executions | 否 | 否 |
> | servers / jobagents / jobs / steps | 否 | 否 |
> | servers / keys | 否 | 否 |
> | servers / operationresults | 否 | 否 |
> | servers / recommendedelasticpools | 否 | 否 |
> | servers / recoverabledatabases | 否 | 否 |
> | servers / restorabledroppeddatabases | 否 | 否 |
> | servers / securityalertpolicies | 否 | 否 |
> | servers / serviceobjectives | 否 | 否 |
> | servers / syncagents | 否 | 否 |
> | servers / tdecertificates | 否 | 否 |
> | servers / usages | 否 | 否 |
> | servers / virtualnetworkrules | 否 | 否 |
> | servers / vulnerabilityassessments | 否 | 否 |
> | virtualclusters | 否 | 否 |

> [!IMPORTANT]
> 資料庫和伺服器必須位於相同的資源群組。 當您移動 SQL 伺服器時，其所有資料庫也會跟著移動。 此行為會套用至 Azure SQL Database 和 Azure SQL Data Warehouse 資料庫。

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / availabilitygrouplisteneroperationresults | 否 | 否 |
> | locations / operationtypes | 否 | 否 |
> | locations / sqlvirtualmachinegroupoperationresults | 否 | 否 |
> | locations / sqlvirtualmachineoperationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | sqlvirtualmachinegroups | Yes | 是 |
> | sqlvirtualmachinegroups / availabilitygrouplisteners | 否 | 否 |
> | sqlvirtualmachines | 是 | Yes |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / asyncoperations | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / deletevirtualnetworkorsubnets | 否 | 否 |
> | locations / usages | 否 | 否 |
> | 作業 | 否 | 否 |
> | storageaccounts | 是 | 是 |
> | storageaccounts / blobservices | 否 | 否 |
> | storageaccounts / fileservices | 否 | 否 |
> | storageaccounts / listaccountsas | 否 | 否 |
> | storageaccounts / listservicesas | 否 | 否 |
> | storageaccounts / queueservices | 否 | 否 |
> | storageaccounts / services | 否 | 否 |
> | storageaccounts / services / metricdefinitions | 否 | 否 |
> | storageaccounts / tableservices | 否 | 否 |
> | usages | 否 | 否 |

## <a name="microsoftstoragecache"></a>MICROSOFT.STORAGECACHE

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | caches | 否 | 否 |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / checknameavailability | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operations | 否 | 否 |
> | locations / workflows | 否 | 否 |
> | 作業 | 否 | 否 |
> | storagesyncservices | 否 | 否 |
> | storagesyncservices / registeredservers | 否 | 否 |
> | storagesyncservices / syncgroups | 否 | 否 |
> | storagesyncservices / syncgroups / cloudendpoints | 否 | 否 |
> | storagesyncservices / syncgroups / serverendpoints | 否 | 否 |
> | storagesyncservices / workflows | 否 | 否 |

## <a name="microsoftstoragesyncdev"></a>MICROSOFT.STORAGESYNCDEV

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | 否 | 否 |

## <a name="microsoftstoragesyncint"></a>MICROSOFT.STORAGESYNCINT

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | 否 | 否 |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | managers | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | clusters | Yes | Yes |
> | 位置 | 否 | 否 |
> | locations / quotas | 否 | 否 |
> | 作業 | 否 | 否 |
> | streamingjobs | 是 | 是 |

> [!IMPORTANT]
> 無法移動執行中狀態的串流分析作業。

## <a name="microsoftstreamanalyticsexplorer"></a>MICROSOFT.STREAMANALYTICSEXPLORER

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | environments | 否 | 否 |
> | environments / eventsources | 否 | 否 |
> | 執行個體 | 否 | 否 |
> | instances / environments | 否 | 否 |
> | instances / environments / eventsources | 否 | 否 |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | cancel | 否 | 否 |
> | createsubscription | 否 | 否 |
> | enable | 否 | 否 |
> | operationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | 重新命名 | 否 | 否 |
> | subscriptiondefinitions | 否 | 否 |
> | subscriptionoperations | 否 | 否 |
> | subscriptions | 否 | 否 |

## <a name="microsoftsupport"></a>microsoft.support

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | operationresults | 否 | 否 |
> | 作業 | 否 | 否 |
> | operationsstatus | 否 | 否 |
> | 服務 | 否 | 否 |
> | services / problemclassifications | 否 | 否 |
> | supporttickets | 否 | 否 |

## <a name="microsoftsynapse"></a>Microsoft.Synapse

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 否 | 否 |
> | 作業 | 否 | 否 |
> | privatelinkhubs | 否 | 否 |
> | workspaces | 否 | 否 |
> | workspaces / bigdatapools | 否 | 否 |
> | workspaces / operationresults | 否 | 否 |
> | workspaces / operationstatuses | 否 | 否 |
> | workspaces / sqlpools | 否 | 否 |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | environments | 否 | 否 |
> | environments / accesspolicies | 否 | 否 |
> | environments / eventsources | 否 | 否 |
> | environments / referencedatasets | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsofttoken"></a>Microsoft.Token

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | stores | 否 | 否 |
> | stores / accesspolicies | 否 | 否 |
> | stores / services | 否 | 否 |
> | stores / services / tokens | 否 | 否 |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | imagetemplates | 否 | 否 |
> | imagetemplates / runoutputs | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operations | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | account | 否 | 否 |
> | account / extension | 否 | 否 |
> | account / project | 否 | 否 |
> | checknameavailability | 否 | 否 |
> | 作業 | 否 | 否 |

> [!IMPORTANT]
> 若要變更 Azure DevOps 的訂用帳戶，請參閱[變更用於計費的 Azure 訂用帳戶](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)。

## <a name="microsoftvmware"></a>Microsoft.VMware

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | arczones | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |
> | resourcepools | 否 | 否 |
> | vcenters | 否 | 否 |
> | virtualmachines | 否 | 否 |
> | virtualmachinetemplates | 否 | 否 |
> | virtualnetworks | 否 | 否 |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | dedicatedcloudnodes | 否 | 否 |
> | dedicatedcloudservices | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / availabilities | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / privateclouds | 否 | 否 |
> | locations / privateclouds / resourcepools | 否 | 否 |
> | locations / privateclouds / virtualmachinetemplates | 否 | 否 |
> | locations / privateclouds / virtualnetworks | 否 | 否 |
> | locations / usages | 否 | 否 |
> | 作業 | 否 | 否 |
> | virtualmachines | 否 | 否 |

## <a name="microsoftvnfmanager"></a>Microsoft.VnfManager

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | devices | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |
> | vnfs | 否 | 否 |

## <a name="microsoftvsonline"></a>Microsoft.VSOnline

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |
> | 作業 | 否 | 否 |
> | plans | 否 | 否 |
> | registeredsubscriptions | 否 | 否 |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | availablestacks | 否 | 否 |
> | billingmeters | 否 | 否 |
> | certificates | 否 | 否 |
> | checknameavailability | 否 | 否 |
> | connectiongateways | 是 | Yes |
> | connections | Yes | Yes |
> | customapis | 是 | 是 |
> | deletedsites | 否 | 否 |
> | deploymentlocations | 否 | 否 |
> | georegions | 否 | 否 |
> | hostingenvironments | 否 | 否 |
> | hostingenvironments / eventgridfilters | 否 | 否 |
> | hostingenvironments / multirolepools | 否 | 否 |
> | hostingenvironments / workerpools | 否 | 否 |
> | ishostingenvironmentnameavailable | 否 | 否 |
> | ishostnameavailable | 否 | 否 |
> | isusernameavailable | 否 | 否 |
> | kubeenvironments | 否 | 否 |
> | listsitesassignedtohostname | 否 | 否 |
> | 位置 | 否 | 否 |
> | locations / apioperations | 否 | 否 |
> | locations / connectiongatewayinstallations | 否 | 否 |
> | locations / deletedsites | 否 | 否 |
> | locations / deletevirtualnetworkorsubnets | 否 | 否 |
> | locations / extractapidefinitionfromwsdl | 否 | 否 |
> | locations / getnetworkpolicies | 否 | 否 |
> | locations / listwsdlinterfaces | 否 | 否 |
> | locations / managedapis | 否 | 否 |
> | locations / operationresults | 否 | 否 |
> | locations / operations | 否 | 否 |
> | locations / runtimes | 否 | 否 |
> | 作業 | 否 | 否 |
> | publishingusers | 否 | 否 |
> | Mahout | 否 | 否 |
> | resourcehealthmetadata | 否 | 否 |
> | runtimes | 否 | 否 |
> | serverfarms | Yes | Yes |
> | serverfarms / eventgridfilters | 否 | 否 |
> | sites | 是 | 是 |
> | sites / eventgridfilters | 否 | 否 |
> | sites / hostnamebindings | 否 | 否 |
> | sites / networkconfig | 否 | 否 |
> | sites / premieraddons | 否 | 否 |
> | sites / slots | 否 | 否 |
> | sites / slots / eventgridfilters | 否 | 否 |
> | sites / slots / hostnamebindings | 否 | 否 |
> | sites / slots / networkconfig | 否 | 否 |
> | sourcecontrols | 否 | 否 |
> | staticsites | 否 | 否 |
> | validate | 否 | 否 |
> | verifyhostingenvironmentvnet | 否 | 否 |

> [!IMPORTANT]
> 請參閱 [App Service 移動指引](./move-limitations/app-service-move-limitations.md)。

## <a name="microsoftwindowsesu"></a>Microsoft.WindowsESU

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | multipleactivationkeys | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | deviceservices | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="microsoftworkloadbuilder"></a>WorkloadBuilder

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | 位置 | 否 | 否 |
> | locations / operationstatuses | 否 | 否 |
> | 作業 | 否 | 否 |
> | 工作負載 | 否 | 否 |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | 資源類型 | 資源群組 | 訂用帳戶 |
> | ------------- | ----------- | ---------- |
> | components | 否 | 否 |
> | componentssummary | 否 | 否 |
> | monitorinstances | 否 | 否 |
> | monitorinstancessummary | 否 | 否 |
> | monitors | 否 | 否 |
> | notificationsettings | 否 | 否 |
> | 作業 | 否 | 否 |

## <a name="third-party-services"></a>協力廠商服務

協力廠商服務目前不支援移動作業。

## <a name="next-steps"></a>後續步驟
如需用來移動資源的命令，請參閱[將資源移動到新的資源群組或訂用帳戶](move-resource-group-and-subscription.md)。

若要以逗號區隔值檔案的形式取得相同資料，請下載 [move-support-resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv)。
