---
title: Azure Resource Logs supported services and schemas
description: Understand the supported services and event schema for Azure resource logs.
ms.topic: reference
ms.date: 05/10/2021
---

# Common and service-specific schema for Azure Resource Logs

> [!NOTE]
> Resource logs were previously known as diagnostic logs. The name was changed in October 2019 as the types of logs gathered by Azure Monitor shifted to include more than just the Azure resource. 
> Also, the list of resource log categories you can collect used to be listed in this article. They are now at [Resource log categories](resource-logs-categories.md). 

[Azure Monitor resource logs](../essentials/platform-logs-overview.md) are logs emitted by Azure services that describe the operation of those services or resources. All resource logs available through Azure Monitor share a common top-level schema, with flexibility for each service to emit unique properties for their own events.

A combination of the resource type (available in the `resourceId` property) and the `category` uniquely identify a schema. This article describes the top-level schema for resource logs and links to the schemata for each service.


## Top-level common schema

| Name | Required/Optional | Description |
|---|---|---|
| time | Required | The timestamp (UTC) of the event. |
| resourceId | Required | The resource ID of the resource that emitted the event. For tenant services, this is of the form /tenants/tenant-id/providers/provider-name. |
| tenantId | Required for tenant logs | The tenant ID of the Active Directory tenant that this event is tied to. This property is only used for tenant-level logs, it does not appear in resource-level logs. |
| operationName | Required | The name of the operation represented by this event. If the event represents an Azure RBAC operation, this is the Azure RBAC operation name (for example, Microsoft.Storage/storageAccounts/blobServices/blobs/Read). Typically modeled in the form of a Resource Manager operation, even if they are not actual documented Resource Manager operations (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | Optional | The api-version associated with the operation, if the operationName was performed using an API (for example, `http://myservice.windowsazure.net/object?api-version=2016-06-01`). If there is no API that corresponds to this operation, the version represents the version of that operation in case the properties associated with the operation change in the future. |
| category | Required | The log category of the event. Category is the granularity at which you can enable or disable logs on a particular resource. The properties that appear within the properties blob of an event are the same within a particular log category and resource type. Typical log categories are "Audit" "Operational" "Execution" and "Request." |
| resultType | Optional | The status of the event. Typical values include Started, In Progress, Succeeded, Failed, Active, and Resolved. |
| resultSignature | Optional | The sub status of the event. If this operation corresponds to a REST API call, this field is the HTTP status code of the corresponding REST call. |
| resultDescription | Optional | The static text description of this operation, for example "Get storage file." |
| durationMs | Optional | The duration of the operation in milliseconds. |
| callerIpAddress | Optional | The caller IP address, if the operation corresponds to an API call that would come from an entity with a publicly available IP address. |
| correlationId | Optional | A GUID used to group together a set of related events. Typically, if two events have the same operationName but two different statuses (for example "Started" and "Succeeded"), they share the same correlation ID. This may also represent other relationships between events. |
| identity | Optional | A JSON blob that describes the identity of the user or application that performed the operation. Typically this field includes the authorization and claims / JWT token from active directory. |
| Level | Optional | The severity level of the event. Must be one of Informational, Warning, Error, or Critical. |
| location | Optional | The region of the resource emitting the event, for example "East US" or "France South" |
| properties | Optional | Any extended properties related to this particular category of events. All custom/unique properties must be put inside this "Part B" of the schema. |

## Service-specific schemas

The schema for resource logs varies depending on the resource and log category. This list shows services that make available resource logs and links to the service and category-specific schema where available. This list is changing all the time as new services are added, so if you don't see what you need below, use a search engine to discover additional documentation. Feel free to open a GitHub issue on this article so we can update it.

| Service | Schema & Docs |
| --- | --- |
| Azure Active Directory | [Overview](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), [Audit log schema](../../active-directory/reports-monitoring/overview-reports.md) and [Sign-ins schema](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | [Azure Analysis Services - Setup diagnostic logging](../../analysis-services/analysis-services-logging.md) |
| API Management | [API Management Resource Logs](../../api-management/api-management-howto-use-azure-monitor.md#resource-logs) |
| App Service | [App Service Logs](../../app-service/troubleshoot-diagnostic-logs.md)
| Application Gateways |[Logging for Application Gateway](../../application-gateway/application-gateway-diagnostics.md) |
| Azure Automation |[Log analytics for Azure Automation](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Azure Batch logging](../../batch/batch-diagnostics.md) |
| Cognitive Services | [Logging for Azure Cognitive Services](../../cognitive-services/diagnostic-logging.md) |
| Container Instances | [Logging for Azure Container Instances](../../container-instances/container-instances-log-analytics.md#log-schema) |
| Container Registry | [Logging for Azure Container Registry](../../container-registry/container-registry-diagnostics-audit-logs.md) |
| Content Delivery Network | [Azure Logs for CDN](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Azure Cosmos DB Logging](../../cosmos-db/monitor-cosmos-db.md) |
| Data Factory | [Monitor Data Factories using Azure Monitor](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Accessing logs for Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Accessing logs for Azure Data Lake Store](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Azure Data Explorer | [Azure Data Explorer logs](/azure/data-explorer/using-diagnostic-logs) |
| Azure Database for MySQL | [Azure Database for MySQL diagnostic logs](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| Azure Database for PostgreSQL | [Azure Database for PostgreSQL logs](../../postgresql/concepts-server-logs.md#resource-logs) |
| Azure Databricks | [Diagnostic logging in Azure Databricks](/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs) |
| DDoS Protection | [Logging for Azure DDoS Protection Standard](../../ddos-protection/diagnostic-logging.md#log-schemas) |
| Azure Digital Twins | [Set up Azure Digital Twins Diagnostics](../../digital-twins/troubleshoot-diagnostics.md#log-schemas)
| Event Hubs |[Azure Event Hubs logs](../../event-hubs/event-hubs-diagnostic-logs.md) |
| Express Route | Schema not available. |
| Azure Firewall | Schema not available. |
| Front Door | [Logging for Front Door](../../frontdoor/front-door-diagnostics.md) |
| IoT Hub | [IoT Hub Operations](../../iot-hub/monitor-iot-hub-reference.md#resource-logs) |
| Key Vault |[Azure Key Vault Logging](../../key-vault/general/logging.md) |
| Kubernetes Service |[Azure Kubernetes Logging](../../aks/view-control-plane-logs.md#log-event-schema) |
| Load Balancer |[Log analytics for Azure Load Balancer](../../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Logic Apps B2B custom tracking schema](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Media Services | [Media services monitoring schemas](../../media-services/latest/monitoring/monitor-media-services-data-reference.md#schemas) |
| Network Security Groups |[Log analytics for network security groups (NSGs)](../../virtual-network/virtual-network-nsg-manage-log.md) |
| Power BI Dedicated | [Logging for Power BI Embedded in Azure](/power-bi/developer/azure-pbie-diag-logs) |
| Recovery Services | [Data Model for Azure Backup](../../backup/backup-azure-reports-data-model.md)|
| Search |[Enabling and using Search Traffic Analytics](../../search/search-traffic-analytics.md) |
| Service Bus |[Azure Service Bus logs](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL Database | [Azure SQL Database logging](../../azure-sql/database/metrics-diagnostic-telemetry-logging-streaming-export-configure.md) |
| Stream Analytics |[Job logs](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Storage | [Blobs](../../storage/blobs/monitor-blob-storage-reference.md#resource-logs-preview), [Files](../../storage/files/storage-files-monitoring-reference.md#resource-logs-preview), [Queues](../../storage/queues/monitor-queue-storage-reference.md#resource-logs-preview),  [Tables](../../storage/tables/monitor-table-storage-reference.md#resource-logs-preview) |
| Traffic Manager | [Traffic Manager Log schema](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| Virtual Networks | Schema not available. |
| Virtual Network Gateways | Schema not available. |



## Next Steps

* [See the resource log categories you can collect](resource-logs-categories.md)
* [Learn more about resource logs](../essentials/platform-logs-overview.md)
* [Stream resource resource logs to **Event Hubs**](./resource-logs.md#send-to-azure-event-hubs)
* [Change resource log diagnostic settings using the Azure Monitor REST API](/rest/api/monitor/diagnosticsettings)
* [Analyze logs from Azure storage with Log Analytics](./resource-logs.md#send-to-log-analytics-workspace)