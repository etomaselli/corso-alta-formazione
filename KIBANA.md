Link utili:
- Guida di Elasticsearch: https://www.elastic.co/guide/en/elasticsearch/reference/7.3/index.html
- Guida di Kibana: https://www.elastic.co/guide/en/kibana/current/index.html
- Tutorial introduttivo: https://medium.com/@victorsmelopoa/an-introduction-to-elasticsearch-with-kibana-78071db3704

## Download di Elasticsearch
- https://www.elastic.co/start -> Scarica ed estrai l'archivio

Avvio (dalla cartella root di Elasticsearch):
- Windows: `.\bin\elasticsearch.bat`
- Linux: `./bin/elasticsearch`

Url: http://localhost:9200/ (mostra il nome del cluster e alcune informazioni)

## Download di Kibana
- https://www.elastic.co/guide/en/kibana/current/install.html -> Install on Windows (o Install with .tar.gz) -> Scarica ed estrai l'archivio con **licenza Apache 2.0**

Avvio (dalla cartella root di Kibana):
- Windows: `.\bin\kibana.bat`
- Linux: `./bin/kibana`

Url: http://localhost:5601 (per vedere lo status del server: http://localhost:5601/status)

## Primi Passi con Elasticsearch
- L'esercizio segue la guida [Getting Started with Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.3/getting-started-index.html)
- Endpoint e comandi più comuni: https://gist.github.com/stephen-puiszis/212b8a8b37f67c670422
- Le API di Elasticsearch: https://www.elastic.co/guide/en/elasticsearch/reference/7.3/rest-apis.html
- Se non hai cURL installato (verifica con `curl --version`), installalo da https://curl.haxx.se/dlwiz/?type=bin

### Carica dei dati in Elasticsearch
1. Copia e salva in un file il dataset [accounts.json](https://raw.githubusercontent.com/elastic/elasticsearch/master/docs/src/test/resources/accounts.json), che contiene una serie di account fittizi.

2. Dalla cartella in cui hai salvato il file, carica in Elasticsearch i dati usando l'API `_bulk`:
```
curl -H "Content-Type: application/json" -XPOST "localhost:9200/bank/_bulk?pretty&refresh" --data-binary "@accounts.json"
```

3. Per verificare che l'inserimento sia andato a buon fine, con l'API `_cat` controlla che l'indice "bank" sia stato creato e abbia 1000 documenti (docs.count):
```
curl "http://localhost:9200/_cat/indices/bank?v"
```

### Esplora i dati tramite il Query DSL dalla console di Kibana
1. Dalla console puoi vedere i dati inseriti chiamando:
```
GET /bank/_search
```
Nella sezione "hits" vedrai un'array con i primi dieci documenti. Per vedere un numero di risultati diverso, devi specificarlo nel body della query:
```
GET /bank/_search
{
  "from": 0,
  "size": 2
}
```

2. Tramite i campi del body puoi filtrare e organizzare i dati che ti interessano:
- "sort": specifica come ordinare i documenti (simile a `ORDER BY` in SQL)
  - es. `"sort": [{ "account_number": "asc" }]` li ordina in modo crescente in base al numero dell'account
- "query"
