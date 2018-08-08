# Azure-metrics-exporter

**Work in progress**

Azure metrics exporter for [Prometheus.](https://prometheus.io)

Allows for the exporting of metrics from Azure applications using the [Azure monitor API.](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough)

# Rate limits

Note that Azure imposes a [15,000 API read limit](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-request-limits) so the number of metrics you're querying for should be proportional to your scrape interval.

# Retrieving Metric definitions

In order to get all the metric definitions for the resources specified in your configuration file, run the following:

`./Azure-metrics-exporter --list.definitions`

This will print your resource id's application/service name along with a list of each of the available metric definitions that you can query for for that resource.

# Example azure-metrics-exporter config

`azure_resource_id` and `subscription_id` can be found under properties in the Azure portal for your application/service.

`tenant_id` is found under `Azure Active Directory > Properties` and is listed as `Directory ID`.

`client_id` is the `application_id` of your application under `Azure Active Directory`, and the `client_secret` is generated by selecting your application/service under Azure Active Directory and selecting `keys`.

```
credentials:
  subscription_id: <secret>
  client_id: <secret>
  client_secret: <secret>
  tenant_id: <secret>

targets:
  - resource: "azure_resource_id"
    metrics:
    - name: "BytesReceived"
    - name: "BytesSent"
  - resource: "azure_resource_id"
    metrics:
    - name: "Http2xx"
    - name: "Http5xx"
```

# Example Prometheus config

```
global:
  scrape_interval:     60s # Set a high scrape_interval either globally or per-job to avoid hitting Azure Monitor API limits.

scrape_configs:
  - job_name: azure
    static_configs:
      - targets: ['localhost:9276']
```
