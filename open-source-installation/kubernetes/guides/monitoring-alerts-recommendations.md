# Monitoring/Alerts Recommendations

This guide outlines the key system metrics that require continuous monitoring and creating alerts based on them.

**Log Monitoring:** The system should trigger alerts for every occurrence of an ERR log in Memphis services logs. Filtering based on the ERR statement is essential. For instance:

```css
[ERR] Error trying to connect to route (attempt 1)
```

**CPU:** Under regular usage, Memphis should not excessively consume the allocated CPU. The ideal thresholds for CPU consumption, based on Memphis recommendations, are 50%, 75%, and 90%. Monitored metrics include `<memphis_varz_cpu>`.

**Memory:** Memory consumption is contingent on the chosen storage type. If memory is the selected storage, monitoring with appropriate thresholds is crucial. The assigned amount will not be released until the retention takes effect. Monitored metrics include `<memphis_varz_mem>`.

**Storage:** To prevent pod crashes, Memphis restricts disk usage to a maximum of 95%. Configuring thresholds is recommended to avoid unexpected disruptions during full capacity. The monitored Memphis metric can be calculated using the formula:

```bash
sum(memphis_varz_jetstream_stats_storage{pod=~"$server",namespace=~"$namespace"}) / sum(memphis_varz_jetstream_config_max_storage{pod=~"$server",namespace=~"$namespace"})
```

Alternatively, Kubernetes PVC default metrics can be utilized.

**Connections:** While numerous connections between clients and Memphis may not indicate issues, monitoring is necessary to identify anomalies. For instance, the first threshold can be set at 100 connections. The monitored metric is `<memphis_varz_connections>`.

**Station Data Size:** For a more granular analysis of storage consumption, thresholds can be set on the station consumption itself using:

```arduino
sum(memphis_stream_total_bytes{namespace=~"memphis",stream_name=~"station_a"}/3)
```

Adjustments are required based on the station replication ratio. For example, a station with 3 replicas.

**DLS (Dead Letter Queue Service):** Awareness of DLS messages is recommended. Metrics for both "unacked" and "schemavarese" messages can be calculated using the following examples:

```bash
memphis_stream_total_messages{stream_name="$memphis_dls_unacked"}
memphis_stream_total_messages{stream_name="$memphis_dls_schemaverse"}
```
