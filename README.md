### Link Utili

Documentazione di Prometheus: https://prometheus.io/docs/introduction/overview/

Documentazione di Grafana: http://docs.grafana.org/guides/getting_started/

Documentazione di Superset: https://superset.incubator.apache.org/tutorial.html

### Metriche per le Query a Prometheus

La lista completa delle metriche registrate è visibile su http://localhost:9090/metrics, http://localhost:8080/actuator/prometheus e http://localhost:8085/actuator/prometheus


* up: stato dell'endpoint (1=raggiungibile, 0=non raggiungibile)
* system_load_average_1m: numero medio di processi in coda o in esecuzione nell'ultimo minuto
* system_cpu_count: numero di CPU disponibili
* system_cpu_usage: utilizzo della CPU
* logback_events_total: numero di eventi loggati, divisi per livello (info, trace, warn, error)
* jvm_memory_used_bytes: numero di bytes di memoria utilizzati dalla Java Virtual Machine
* jvm_memory_committed_bytes numero di bytes di memoria dedicati alla Java Virtual Machine

##### Filtri
* sintassi per scrivere un filtro: {<proprietà>=""}

Es.: {job="eureka-discovery-catalog"}

* sintassi per un filtro con più valori: {<proprietà>=~"valore1|valore2"}

Es.: {level=~"error|warn"}
