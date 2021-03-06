---
title: Azure Resource Manageru úplný režim odstranění podle typu prostředku
description: Ukazuje, jak postupovat při odstraňování úplný režim v šablonách Azure Resource Manageru typy prostředků.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 02/13/2019
ms.author: tomfitz
ms.openlocfilehash: b38b1af8c72b814813804a49642668c28f3898d6
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/15/2019
ms.locfileid: "56302683"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Odstranění prostředků Azure pro úplný režim nasazení
Tento článek popisuje, jak postupovat při odstranění, když není v šabloně, která je nasazena v režimu úplné typy prostředků.

Typy prostředků označené `Yes` se odstraní, když typ není v šabloně nasazený s režimem dokončení. 

Typy prostředků označené `No` nejsou automaticky odstraněny při není v šabloně však, jste odstranit, pokud se nadřazený prostředek odstraní. Úplný popis chování najdete v tématu [režimy nasazení Azure Resource Manageru](deployment-modes.md).

Chcete-li získat stejná data jako soubor hodnot oddělených čárkami, stáhněte si [dokončení deletion.csv režimu](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv).

## <a name="microsoftaad"></a>Microsoft.AAD
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| DomainServices | Ano | 
| DomainServices/oucontainer | Ne | 

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| diagnosticSettings | Ne | 
| diagnosticSettingsCategories | Ne | 

## <a name="microsoftaddons"></a>Microsoft.Addons
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| supportProviders | Ne | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| aadsupportcases | Ne | 
| addsservices | Ne | 
| agenti | Ne | 
| anonymousapiusers | Ne | 
| konfigurace | Ne | 
| Protokoly | Ne | 
| sestavy | Ne | 
| services | Ne | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Konfigurace | Ne | 
| generateRecommendations | Ne | 
| doporučení | Ne | 
| potlačení | Ne | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| actionRules | Ne | 
| výstrahy | Ne | 
| alertsList | Ne | 
| alertsSummary | Ne | 
| alertsSummaryList | Ne | 
| smartDetectorAlertRules | Ne | 
| smartDetectorRuntimeEnvironments | Ne | 
| smartGroups | Ne | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| servers | Ano | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| reportFeedback | Ne | 
| service | Ano | 
| validateServiceName | Ne | 

## <a name="microsoftattestation"></a>Microsoft.Attestation
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| attestationProviders | Ne | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| classicAdministrators | Ne | 
| denyAssignments | Ne | 
| elevateAccess | Ne | 
| Zámky | Ne | 
| oprávnění | Ne | 
| policyAssignments | Ne | 
| policyDefinitions | Ne | 
| policySetDefinitions | Ne | 
| providerOperations | Ne | 
| roleAssignments | Ne | 
| roleDefinitions | Ne | 

## <a name="microsoftautomation"></a>Microsoft.Automation
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| automationAccounts | Ano | 
| automationAccounts/configurations | Ano | 
| automationAccounts/jobs | Ne | 
| automationAccounts/runbooks | Ano | 
| automationAccounts/softwareUpdateConfigurations | Ne | 
| automationAccounts/webhooks | Ne | 

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| environments | Ne | 
| prostředí/účty | Ne | 
| prostředí/účty nebo obory názvů | Ne | 
| prostředí/účty nebo obory názvů/konfigurace | Ne | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| b2cDirectories | Ano | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| registrations | Ano | 
| registrations/customerSubscriptions | Ne | 
| registrace a produkty | Ne | 

## <a name="microsoftbatch"></a>Microsoft.Batch
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| batchAccounts | Ano | 

## <a name="microsoftbilling"></a>Microsoft.Billing
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| billingAccounts | Ne | 
| billingAccounts/billingProfiles | Ne | 
| billingAccounts/billingProfiles/billingSubscriptions | Ne | 
| billingAccounts/billingProfiles/invoices | Ne | 
| billingAccounts/billingProfiles/invoices/pricesheet | Ne | 
| billingAccounts/billingProfiles/operationStatus | Ne | 
| billingAccounts/billingProfiles/paymentMethods | Ne | 
| billingAccounts/billingProfiles/policies | Ne | 
| billingAccounts/billingProfiles/pricesheet | Ne | 
| billingAccounts/billingProfiles/products | Ne | 
| billingAccounts/billingProfiles/transactions | Ne | 
| billingAccounts/billingSubscriptions | Ne | 
| billingAccounts/oddělení | Ne | 
| billingAccounts/eligibleOffers | Ne | 
| billingAccounts/enrollmentAccounts | Ne | 
| billingAccounts/faktury | Ne | 
| billingAccounts/invoiceSections | Ne | 
| billingAccounts/invoiceSections/billingSubscriptions | Ne | 
| billingAccounts/invoiceSections/billingSubscriptions/transfer | Ne | 
| billingAccounts/invoiceSections/importRequests | Ne | 
| billingAccounts/invoiceSections/initiateImportRequest | Ne | 
| billingAccounts/invoiceSections/initiateTransfer | Ne | 
| billingAccounts/invoiceSections/operationStatus | Ne | 
| billingAccounts/invoiceSections/produkty | Ne | 
| billingAccounts/invoiceSections/transfers | Ne | 
| billingAccounts/produkty | Ne | 
| billingAccounts/projekty | Ne | 
| billingAccounts/projects/billingSubscriptions | Ne | 
| billingAccounts/projects/importRequests | Ne | 
| billingAccounts/projects/initiateImportRequest | Ne | 
| billingAccounts/projects/operationStatus | Ne | 
| billingAccounts/projektů/produkty | Ne | 
| billingAccounts/transakce | Ne | 
| billingPeriods | Ne | 
| BillingPermissions | Ne | 
| billingProperty | Ne | 
| BillingRoleAssignments | Ne | 
| BillingRoleDefinitions | Ne | 
| CreateBillingRoleAssignment | Ne | 
| Oddělení | Ne | 
| enrollmentAccounts | Ne | 
| importRequests | Ne | 
| importRequests/acceptImportRequest | Ne | 
| importRequests/declineImportRequest | Ne | 
| Faktury | Ne | 
| Převody | Ne | 
| přenosy/acceptTransfer | Ne | 
| přenosy/declineTransfer | Ne | 
| transfers/operationStatus | Ne | 
| usagePlans | Ne | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| mapApis | Ano | 
| updateCommunicationPreference | Ne | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| BizTalk | Ano | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| blueprintAssignments | Ne | 
| blueprintAssignments/assignmentOperations | Ne | 
| blueprintAssignments/operations | Ne | 
| podrobné plány. | Ne | 
| plány a artefakty | Ne | 
| plány a verze | Ne | 
| plány/verze/artefaktů | Ne | 

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| botServices | Ano | 
| botServices/channels | Ne | 
| botServices/připojení | Ne | 

## <a name="microsoftcache"></a>Microsoft.Cache
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Redis | Ano | 
| RedisConfigDefinition | Ne | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| appliedReservations | Ne | 
| calculatePrice | Ne | 
| catalogs | Ne | 
| commercialReservationOrders | Ne | 
| reservationOrders | Ne | 
| reservationOrders/calculateRefund | Ne | 
| reservationOrders/merge | Ne | 
| reservationOrders/rezervace | Ne | 
| reservationOrders/rezervace/revize | Ne | 
| vrácení hodnoty reservationOrders | Ne | 
| reservationOrders/rozdělení | Ne | 
| reservationOrders/swap | Ne | 
| rezervace | Ne | 
| zdroje | Ne | 
| validateReservationOrder | Ne | 

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| edgenodes | Ne | 
| profiles | Ano | 
| profiles/endpoints | Ano | 
| profiles/endpoints/customdomains | Ne | 
| profily/koncové body/zdroje | Ne | 
| validateProbe | Ne | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| objednávky certifikátu | Ano | 
| certifikáty objednávky certifikátu | Ne | 
| validateCertificateRegistrationInformation | Ne | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Možnosti | Ne | 
| domainNames | Ne | 
| domainNames/možnosti | Ne | 
| domainNames/internalLoadBalancers | Ne | 
| domainNames/serviceCertificates | Ne | 
| domainNames/slots | Ne | 
| domainNames/slots/roles | Ne | 
| moveSubscriptionResources | Ne | 
| operatingSystemFamilies | Ne | 
| operatingSystems | Ne | 
| quotas | Ne | 
| resourceTypes | Ne | 
| validateSubscriptionMoveAvailability | Ne | 
| virtualMachines | Ne | 
| virtualMachines/diagnosticSettings | Ne | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| classicInfrastructureResources | Ne | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Možnosti | Ne | 
| expressRouteCrossConnections | Ne | 
| expressRouteCrossConnections/peerings | Ne | 
| gatewaySupportedDevices | Ne | 
| networkSecurityGroups | Ne | 
| quotas | Ne | 
| reservedIps | Ne | 
| virtualNetworks | Ne | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Ne | 
| virtualNetworks/virtualNetworkPeerings | Ne | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Možnosti | Ne | 
| disks | Ne | 
| images | Ne | 
| OSImage | Ne | 
| osPlatformImages | Ne | 
| publicImages | Ne | 
| quotas | Ne | 
| storageAccounts | Ne | 
| storageAccounts/services | Ne | 
| storageAccounts/services/diagnosticSettings | Ne | 
| storageAccounts/vmImages | Ne | 
| vmImages | Ne | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| accounts | Ano | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| RateCard | Ne | 
| UsageAggregates | Ne | 

## <a name="microsoftcompute"></a>Microsoft.Compute
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| availabilitySets | Ano | 
| disks | Ano | 
| images | Ano | 
| restorePointCollections | Ano | 
| restorePointCollections/restorePoints | Ne | 
| sharedVMImages | Ano | 
| sharedVMImages/versions | Ano | 
| snapshots | Ano | 
| virtualMachines | Ano | 
| virtualMachines/diagnosticSettings | Ne | 
| virtualMachines/extensions | Ano | 
| virtualMachineScaleSets | Ano | 
| virtualMachineScaleSets/extensions | Ne | 
| virtualMachineScaleSets/networkInterfaces | Ne | 
| virtualMachineScaleSets/publicIPAddresses | Ne | 
| virtualMachineScaleSets/virtualMachines | Ne | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Ne | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| AggregatedCost | Ne | 
| Zůstatky | Ne | 
| Rozpočty | Ne | 
| Poplatky | Ne | 
| CostTags | Ne | 
| Kredity | Ne | 
| stránka events | Ne | 
| Prognózy | Ne | 
| Velké | Ne | 
| Marketplaces | Ne | 
| Pricesheets | Ne | 
| produkty | Ne | 
| ReservationDetails | Ne | 
| ReservationRecommendations | Ne | 
| ReservationSummaries | Ne | 
| ReservationTransactions | Ne | 
| Značky | Ne | 
| Výrazy | Ne | 
| UsageDetails | Ne | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| containerGroups | Ano | 
| serviceAssociationLinks | Ne | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| registries | Ano | 
| Registry/sestavení | Ne | 
| Registry/sestavení/zrušit | Ne | 
| Registry/sestavení/getLogLink | Ne | 
| Registry/buildTasks | Ano | 
| Registry/buildTasks/kroky | Ne | 
| registries/eventGridFilters | Ne | 
| registries/getBuildSourceUploadUrl | Ne | 
| Registry/GetCredentials | Ne | 
| registries/importImage | Ne | 
| registries/queueBuild | Ne | 
| Registry/regenerateCredential | Ne | 
| Registry/regenerateCredentials | Ne | 
| registries/replications | Ano | 
| Registry/spuštění | Ne | 
| Registry/spuštění a zrušit | Ne | 
| registries/scheduleRun | Ne | 
| registries/tasks | Ano | 
| registries/updatePolicies | Ne | 
| registries/webhooks | Ano | 
| registries/webhooks/getCallbackConfig | Ne | 
| Registry a webhooky/příkaz ping | Ne | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| containerServices | Ano | 
| managedClusters | Ano | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| applications | Ano | 
| updateCommunicationPreference | Ne | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| accounts | Ano | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Výstrahy | Ne | 
| BillingAccounts | Ne | 
| Konektory | Ano | 
| Oddělení | Ne | 
| Dimenze | Ne | 
| enrollmentAccounts | Ne | 
| Dotaz | Ne | 
| Registrace | Ne | 
| Reportconfigs | Ne | 
| Reports | Ne | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| hubs | Ano | 
| rozbočovače/authorizationPolicies | Ne | 
| rozbočovače a konektorů | Ne | 
| rozbočovače a konektorů/mapování | Ne | 
| rozbočovače a interakce | Ne | 
| hubs/kpi | Ne | 
| rozbočovače/odkazy | Ne | 
| rozbočovače a profily | Ne | 
| rozbočovače/roleAssignments | Ne | 
| rozbočovače/role | Ne | 
| hubs/suggestTypeSchema | Ne | 
| rozbočovače a zobrazení | Ne | 
| rozbočovače/widgetTypes | Ne | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| jobs | Ano | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| DataBoxEdgeDevices | Ano | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| workspaces | Ano | 
| workspaces/virtualNetworkPeerings | Ne | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| catalogs | Ano | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| connectionManagers | Ano | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| dataFactories | Ano | 
| dataFactories/diagnosticSettings | Ne | 
| dataFactorySchema | Ne | 
| factories | Ano | 
| objekty pro vytváření/integrationRuntimes | Ne | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| accounts | Ano | 
| accounts/dataLakeStoreAccounts | Ne | 
| účty/storageAccounts | Ne | 
| accounts/storageAccounts/containers | Ne | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| accounts | Ano | 
| accounts/eventGridFilters | Ne | 
| účty/firewallRules | Ne | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| services | Ano | 
| services/projects | Ano | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| servers | Ano | 
| servery pro/recoverableServers | Ne | 
| servery pro/virtualNetworkRules | Ne | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| servers | Ano | 
| servery pro/recoverableServers | Ne | 
| servery pro/virtualNetworkRules | Ne | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| servers | Ano | 
| servery pro/poradci | Ne | 
| servers/queryTexts | Ne | 
| servery pro/recoverableServers | Ne | 
| servers/topQueryStatistics | Ne | 
| servery pro/virtualNetworkRules | Ne | 
| servers/waitStatistics | Ne | 

## <a name="microsoftdevices"></a>Microsoft.Devices
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| IotHubs | Ano | 
| IotHubs/eventGridFilters | Ne | 
| ProvisioningServices | Ano | 
| Použití | Ne | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Kontrolery | Ano | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| labs | Ano | 
| praktická cvičení/serviceRunners | Ano | 
| labs/virtualMachines | Ano | 
| schedules | Ano | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| databaseAccountNames | Ne | 
| databaseAccounts | Ano | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| domains | Ano | 
| domény/domainOwnershipIdentifiers | Ne | 
| generateSsoRequest | Ne | 
| topLevelDomains | Ne | 
| validateDomainRegistrationInformation | Ne | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| lcsprojects | Ne | 
| lcsprojects/clouddeployments | Ne | 
| lcsprojects nebo konektory | Ne | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| domains | Ano | 
| domény a témat | Ne | 
| eventSubscriptions | Ne | 
| extensionTopics | Ne | 
| topics | Ano | 
| topicTypes | Ne | 

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| clusters | Ano | 
| namespaces | Ano | 
| obory názvů a autorizačních pravidel | Ne | 
| obory názvů/disasterrecoveryconfigs | Ne | 
| obory názvů/eventhubs | Ne | 
| obory názvů/eventhubs/autorizačních pravidel | Ne | 
| namespaces/eventhubs/consumergroups | Ne | 

## <a name="microsoftfeatures"></a>Microsoft.Features
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Database | Ne | 
| Zprostředkovatelé | Ne | 

## <a name="microsoftgallery"></a>Microsoft.Gallery
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Zaregistrovat | Ne | 
| galleryitems | Ne | 
| generateartifactaccessuri | Ne | 
| myareas | Ne | 
| myareas/oblastí | Ne | 
| myareas/oblasti a oblasti | Ne | 
| myareas/oblasti a oblasti/galleryitems | Ne | 
| myareas/oblasti/galleryitems | Ne | 
| myareas/galleryitems | Ne | 
| Registrace | Ne | 
| zdroje | Ne | 
| retrieveresourcesbyid | Ne | 

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| guestConfigurationAssignments | Ne | 
| software | Ne | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| hanaInstances | Ano | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| clusters | Ano | 
| clustery/aplikace | Ne | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| jobs | Ano | 

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| labelGroups | Ne | 
| labelGroups/popisky | Ne | 
| labelGroups/labels/conditions | Ne | 
| labelGroups/labels/subLabels | Ne | 
| labelGroups/labels/subLabels/conditions | Ne | 

## <a name="microsoftinsights"></a>microsoft.insights
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| actiongroups | Ano | 
| activityLogAlerts | Ano | 
| alertrules | Ano | 
| automatedExportSettings | Ne | 
| autoscalesettings | Ano | 
| základní hodnota | Ne | 
| calculatebaseline | Ne | 
| components | Ano | 
| součásti/události | Ne | 
| components/pricingPlans | Ne | 
| součásti či dotazu | Ne | 
| diagnosticSettings | Ne | 
| diagnosticSettingsCategories | Ne | 
| eventCategories | Ne | 
| eventtypes | Ne | 
| extendedDiagnosticSettings | Ne | 
| logDefinitions | Ne | 
| logprofiles | Ne | 
| Protokoly | Ne | 
| metricAlerts | Ano |
| migrateToNewPricingModel | Ne | 
| myWorkbooks | Ne | 
| dotazy | Ne | 
| rollbackToLegacyPricingModel | Ne | 
| scheduledqueryrules | Ano | 
| vmInsightsOnboardingStatuses | Ne | 
| webtests | Ano | 
| workbooks | Ano | 

## <a name="microsoftintune"></a>Microsoft.Intune
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| diagnosticSettings | Ne | 
| diagnosticSettingsCategories | Ne | 

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| IoTApps | Ano | 

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Graph | Ano | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| deletedVaults | Ne | 
| vaults | Ano | 
| trezory/accessPolicies | Ne | 
| Trezory/tajných klíčů | Ne | 

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| clusters | Ano | 
| clustery a databází | Ne | 
| clustery/databází/dataconnections | Ne | 
| clustery/databází/eventhubconnections | Ne | 

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| labaccounts | Ano | 
| uživatelé | Ne | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| accounts | Ano | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| accounts | Ano | 

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Protokoly | Ne | 

## <a name="microsoftlogic"></a>Microsoft.Logic
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| integrationAccounts | Ano | 
| workflows | Ano | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| commitmentPlans | Ano | 
| Webové služby | Ano | 
| Pracovní prostory | Ano | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| accounts | Ano | 
| accounts/workspaces | Ano | 
| accounts/workspaces/projects | Ano | 
| teamAccounts | Ano | 
| teamAccounts/pracovní prostory | Ano | 
| teamAccounts/pracovních prostorů a projektů | Ano | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| accounts | Ano | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| workspaces | Ano | 
| pracovní prostory a výpočetní prostředí | Ne | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Identity | Ne | 
| userAssignedIdentities | Ano | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| getEntities | Ne | 
| managementGroups | Ne | 
| zdroje | Ne | 
| startTenantBackfill | Ne | 
| tenantBackfillStatus | Ne | 

## <a name="microsoftmaps"></a>Microsoft.Maps
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| accounts | Ano | 
| accounts/eventGridFilters | Ne | 

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Nabízí | Ne | 
| offerTypes | Ne | 
| offerTypes/zdroje | Ne | 
| offerTypes/vydavatele a nabídky | Ne | 
| offerTypes/publishers/offers/plans | Ne | 
| offerTypes/publishers/offers/plans/agreements | Ne | 
| offerTypes/publishers/offers/plans/configs | Ne | 
| offerTypes/publishers/offers/plans/configs/importImage | Ne | 
| privategalleryitems | Ne | 
| produkty | Ne | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| classicDevServices | Ano | 
| updateCommunicationPreference | Ne | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| Smlouvy | Ne | 
| offertypes | Ne | 

## <a name="microsoftmedia"></a>Microsoft.Media
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| mediaservices | Ano | 
| mediaservices/accountFilters | Ne | 
| mediaservices/assets | Ne | 
| mediaservices/assets/assetFilters | Ne | 
| mediaservices/contentKeyPolicies | Ne | 
| mediaservices/eventGridFilters | Ne | 
| mediaservices/liveEventOperations | Ne | 
| mediaservices/liveEvents | Ano | 
| mediaservices/liveEvents/liveOutputs | Ne | 
| mediaservices/liveOutputOperations | Ne | 
| mediaservices/streamingEndpointOperations | Ne | 
| mediaservices/streamingEndpoints | Ano | 
| mediaservices/streamingLocators | Ne | 
| mediaservices/streamingPolicies | Ne | 
| mediaservices/transforms | Ne | 
| mediaservices/transforms/jobs | Ne | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| projects | Ano | 

## <a name="microsoftnetwork"></a>Microsoft.Network
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| applicationGateways | Ano | 
| applicationSecurityGroups | Ano | 
| azureFirewallFqdnTags | Ne | 
| azureFirewalls | Ano | 
| bgpServiceCommunities | Ne | 
| connections | Ano | 
| ddosCustomPolicies | Ano | 
| ddosProtectionPlans | Ano | 
| dnsOperationStatuses | Ne | 
| dnszones | Ano | 
| dnszones/A | Ne | 
| dnszones/AAAA | Ne | 
| dnszones/all | Ne | 
| dnszones/CAA | Ne | 
| dnszones/CNAME | Ne | 
| dnszones/MX | Ne | 
| dnszones/NS | Ne | 
| dnszones/PTR | Ne | 
| dnszones/sady záznamů | Ne | 
| dnszones/SOA | Ne | 
| dnszones/SRV | Ne | 
| dnszones/TXT | Ne | 
| expressRouteCircuits | Ano | 
| expressRouteServiceProviders | Ne | 
| frontdoors | Ano | 
| frontdoorWebApplicationFirewallPolicies | Ano | 
| getDnsResourceReference | Ne | 
| interfaceEndpoints | Ano | 
| internalNotify | Ne | 
| Load Balancerech | Ano | 
| localNetworkGateways | Ano | 
| natGateways | Ano | 
| networkIntentPolicies | Ano | 
| networkInterfaces | Ano | 
| networkProfiles | Ano | 
| networkSecurityGroups | Ano | 
| networkWatchers | Ano | 
| networkWatchers/connectionMonitors | Ano | 
| networkWatchers/přehledů | Ano | 
| networkWatchers/pingMeshes | Ano | 
| privateLinkServices | Ano | 
| publicIPAddresses | Ano | 
| publicIPPrefixes | Ano | 
| routeFilters | Ano | 
| routeTables | Ano | 
| serviceEndpointPolicies | Ano | 
| trafficManagerGeographicHierarchies | Ne | 
| trafficmanagerprofiles | Ano | 
| trafficmanagerprofiles/heatMaps | Ne | 
| virtualHubs | Ano | 
| virtualNetworkGateways | Ano | 
| virtualNetworks | Ano | 
| virtualNetworkTaps | Ano | 
| virtualWans | Ano | 
| vpnGateways | Ano | 
| vpnSites | Ano | 
| webApplicationFirewallPolicies | Ano | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| namespaces | Ano | 
| namespaces/notificationHubs | Ano | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| zařízení | Ne | 
| linkTargets | Ne | 
| storageInsightConfigs | Ne | 
| workspaces | Ano | 
| pracovní prostory/zdroje dat | Ne | 
| pracovní prostory/linkedServices | Ne | 
| pracovní prostory/dotazu | Ne | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| managementassociations | Ne | 
| managementconfigurations | Ano | 
| solutions | Ano | 
| views | Ano | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| policyEvents | Ne | 
| policyStates | Ne | 
| policyTrackedResources | Ne | 
| nápravy | Ne | 

## <a name="microsoftportal"></a>Microsoft.Portal
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| konzoly nástroje | Ne | 
| dashboards | Ano | 
| userSettings | Ne | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| workspaceCollections | Ano | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| capacities | Ano | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| accounts | Ano | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| backupProtectedItems | Ne | 
| vaults | Ano | 

## <a name="microsoftrelay"></a>Microsoft.Relay
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| namespaces | Ano | 
| obory názvů a autorizačních pravidel | Ne | 
| obory názvů/hybridconnections | Ne | 
| obory názvů/hybridconnections/autorizačních pravidel | Ne | 
| namespaces/wcfrelays | Ne | 
| obory názvů/wcfrelays/autorizačních pravidel | Ne | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| zdroje | Ne | 
| subscriptionsStatus | Ne | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| availabilityStatuses | Ne | 
| childAvailabilityStatuses | Ne | 
| childResources | Ne | 
| stránka events | Ne | 
| impactedResources | Ne | 
| oznámení | Ne | 

## <a name="microsoftresources"></a>Microsoft.Resources
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| nasazení | Ne | 
| Nasazení/operace | Ne | 
| Odkazy | Ne | 
| notifyResourceJobs | Ne | 
| Zprostředkovatelé | Ne | 
| resourceGroups | Ne | 
| zdroje | Ne | 
| předplatná | Ne | 
| Předplatná a poskytovatelé | Ne | 
| subscriptions/resourceGroups | Ne | 
| předplatné/resourcegroups/prostředky | Ne | 
| Předplatná a prostředky | Ne | 
| předplatné/tagnames | Ne | 
| subscriptions/tagNames/tagValues | Ne | 
| Tenanti | Ne | 

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| applications | Ano | 
| saasresources | Ne | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| flows | Ano | 
| jobcollections | Ano | 

## <a name="microsoftsearch"></a>Microsoft.Search
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| resourceHealthMetadata | Ne | 
| searchServices | Ano | 

## <a name="microsoftsecurity"></a>Microsoft.Security
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| advancedThreatProtectionSettings | Ne | 
| výstrahy | Ne | 
| allowedConnections | Ne | 
| appliances | Ne | 
| applicationWhitelistings | Ne | 
| AutoProvisioningSettings | Ne | 
| Dodržování předpisů | Ne | 
| dataCollectionAgents | Ne | 
| discoveredSecuritySolutions | Ne | 
| externalSecuritySolutions | Ne | 
| InformationProtectionPolicies | Ne | 
| jitNetworkAccessPolicies | Ne | 
| sledování | Ne | 
| Monitoring/antimalwaru | Ne | 
| monitorování a standardních hodnot | Ne | 
| Monitoring/patch | Ne | 
| zásady | Ne | 
| ceny | Ne | 
| securityContacts | Ne | 
| securitySolutions | Ne | 
| securitySolutionsReferenceData | Ne | 
| securityStatus | Ne | 
| securityStatus/endpoints | Ne | 
| securityStatus/subnets | Ne | 
| securityStatus/virtualMachines | Ne | 
| securityStatuses | Ne | 
| securityStatusesSummaries | Ne | 
| settings | Ne | 
| úlohy | Ne | 
| Topologie | Ne | 
| workspaceSettings | Ne | 

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| diagnosticSettings | Ne | 
| diagnosticSettingsCategories | Ne | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| namespaces | Ano | 
| obory názvů a autorizačních pravidel | Ne | 
| obory názvů/disasterrecoveryconfigs | Ne | 
| obory názvů/eventgridfilters | Ne | 
| obory názvů/fronty | Ne | 
| obory názvů/fronty/autorizačních pravidel | Ne | 
| obory názvů a témat | Ne | 
| obory názvů nebo témat/autorizačních pravidel | Ne | 
| obory názvů předplatná | Ne | 
| obory názvů nebo témata/předplatná/pravidla | Ne | 
| premiumMessagingRegions | Ne | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| clusters | Ano | 
| clustery/aplikace | Ne | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| applications | Ano | 
| gateways | Ano | 
| networks | Ano | 
| tajné kódy | Ano | 
| volumes | Ano | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| SignalR | Ano | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| applianceDefinitions | Ano | 
| appliances | Ano | 
| applicationDefinitions | Ano | 
| applications | Ano | 
| jitRequests | Ano | 

## <a name="microsoftsql"></a>Microsoft.SQL
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| managedInstances | Ano |
| managedInstances/databáze | Ano (viz poznámka níže) |
| managedInstances/databases/backupShortTermRetentionPolicies | Ne |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | Ne |
| managedInstances/databases/vulnerabilityAssessments | Ne |
| managedInstances/databases/vulnerabilityAssessments/rules/baselines | Ne |
| managedInstances/encryptionProtector | Ne |
| managedInstances/klíče | Ne |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Ne |
| managedInstances/vulnerabilityAssessments | Ne |
| servers | Ano | 
| servery pro/správci | Ne | 
| servery pro/communicationLinks | Ne | 
| servers/databases | Ano (viz poznámka níže) | 
| servers/encryptionProtector | Ne | 
| servery pro/firewallRules | Ne | 
| servery pro/klíče | Ne | 
| servers/restorableDroppedDatabases | Ne | 
| servery pro/serviceobjectives | Ne | 
| servers/tdeCertificates | Ne | 

> [!NOTE]
> Hlavní databáze nepodporuje značky, ale další databáze, včetně databází datového skladu, podporují značky.


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| SqlVirtualMachineGroups | Ano | 
| SqlVirtualMachineGroups/AvailabilityGroupListeners | Ne | 
| SqlVirtualMachines | Ano | 

## <a name="microsoftstorage"></a>Microsoft.Storage
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| storageAccounts | Ano | 
| storageAccounts/blobServices | Ne | 
| storageAccounts/fileServices | Ne | 
| storageAccounts/queueServices | Ne | 
| storageAccounts/services | Ne | 
| storageAccounts/tableServices | Ne | 
| Použití | Ne | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| storageSyncServices | Ano | 
| storageSyncServices/registeredServers | Ne | 
| storageSyncServices/syncGroups | Ne | 
| storageSyncServices/syncGroups/cloudEndpoints | Ne | 
| storageSyncServices/syncGroups/serverEndpoints | Ne | 
| storageSyncServices/workflows | Ne | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| managers | Ano | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| streamingjobs | Ano (viz poznámka níže) | 
| streamingjobs/diagnosticSettings | Ne | 

> [!NOTE]
> Při spuštění streamingjobs nelze přidat značku. Zastavte prostředek, který chcete přidat značku.

## <a name="microsoftsubscription"></a>Microsoft.Subscription
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| CreateSubscription | Ne | 
| SubscriptionDefinitions | Ne | 
| SubscriptionOperations | Ne | 

## <a name="microsoftsupport"></a>microsoft.support
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| supporttickets | Ne | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| providerRegistrations | Ano | 
| zdroje | Ano | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| environments | Ano | 
| prostředí/accessPolicies | Ne | 
| environments/eventsources | Ano | 
| prostředí/referenceDataSets | Ano | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| account | Ano | 
| account/extension | Ano | 
| account/project | Ano | 

## <a name="microsoftweb"></a>Microsoft.Web
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| apiManagementAccounts | Ne | 
| apiManagementAccounts/apiAcls | Ne | 
| apiManagementAccounts/rozhraní API | Ne | 
| apiManagementAccounts/apis/apiAcls | Ne | 
| apiManagementAccounts/apis/connectionAcls | Ne | 
| apiManagementAccounts/API/připojení | Ne | 
| apiManagementAccounts/apis/connections/connectionAcls | Ne | 
| apiManagementAccounts/API/localizedDefinitions | Ne | 
| apiManagementAccounts/connectionAcls | Ne | 
| apiManagementAccounts/připojení | Ne | 
| billingMeters | Ne | 
| certificates | Ano | 
| connectionGateways | Ano | 
| connections | Ano | 
| customApis | Ano | 
| deletedSites | Ne | 
| functions | Ne | 
| hostingEnvironments | Ano | 
| hostingEnvironments/multiRolePools | Ne | 
| hostingEnvironments/multiRolePools/instances | Ne | 
| hostingEnvironments/workerPools | Ne | 
| hostingEnvironments/workerPools/instances | Ne | 
| publishingUsers | Ne | 
| doporučení | Ne | 
| resourceHealthMetadata | Ne | 
| Moduly runtime | Ne | 
| serverFarms | Ano | 
| serverFarms/workers | Ne | 
| sites | Ano | 
| sites/domainOwnershipIdentifiers | Ne | 
| sites/hostNameBindings | Ne | 
| servery a instancí | Ne | 
| servery/instance/rozšíření | Ne | 
| sites/premieraddons | Ano | 
| servery a doporučení | Ne | 
| sites/resourceHealthMetadata | Ne | 
| sites/slots | Ano | 
| sites/slots/hostNameBindings | Ne | 
| servery/sloty/instance | Ne | 
| sites/slots/instances/extensions | Ne | 
| sourceControls | Ne | 
| Ověření | Ne | 
| verifyHostingEnvironmentVnet | Ne | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| diagnosticSettings | Ne | 
| diagnosticSettingsCategories | Ne | 

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| DeviceServices | Ano | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor
| Typ prostředku | Úplný režim odstranění |
| ------------- | ----------- |
| components | Ne | 
| componentsSummary | Ne | 
| monitorInstances | Ne | 
| monitorInstancesSummary | Ne | 
| Monitorování | Ne | 
| notificationSettings | Ne | 

## <a name="next-steps"></a>Další postup
Zjistěte, jak použít značky na prostředky, najdete v článku [používání značek k uspořádání prostředků Azure](resource-group-using-tags.md).
