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

## Come Usare Elasticsearch
- L'esercizio segue la guida [Getting Started with Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.7/getting-started-index.html)
- Endpoint e comandi più comuni: https://gist.github.com/stephen-puiszis/212b8a8b37f67c670422
- Le API di Elasticsearch: https://www.elastic.co/guide/en/elasticsearch/reference/7.7/rest-apis.html
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
Nella sezione "hits" vedrai un'array con i primi dieci documenti, ma il campo "hits.total.value" indica il numero totale di documenti selezionati. Per vedere un numero di risultati diverso (es. 30), devi specificarlo nel body della query:
```
GET /bank/_search
{
  "from": 0,
  "size": 30
}
```

2. Tramite i campi del body puoi filtrare e organizzare i dati che ti interessano:
- "sort": specifica come ordinare i documenti (simile a `ORDER BY` in SQL)
  - es. per ordinarli in modo crescente in base al numero dell'account:
  ```
  {
    "sort": [{ "account_number": "asc" }]
  }
  ```
- "query": specifica come selezionare i documenti, prendendo solo quelli che rispondono ad un certo criterio o clausola (simile a `WHERE` in SQL)
  - es. per selezionare i documenti in cui il campo "address" contiene la parola "lane":
  ```
  {
    "query": { "match": { "address": "lane" } }
  }
  ```
- "query.bool": specifica una combinazione di **clausole** (simile a `WHERE ... AND ... AND ...` in SQL)
  - es. per selezionare gli account di *donne* che hanno *tra i 30 e i 40 anni* e che *non vivono in California*:
  ```
  {
    "query": {
      "bool": {
        "must": [
          { "match": { "gender": "F" } }
        ],
        "must_not": [
          { "match": { "state": "CA" } }
        ],
        "filter": {
          "range": {
            "age": {
              "gte": 30,
              "lte": 40
            }
          }
        }
      }
    }
  }
  ```
- "aggs": specifica come aggregare i documenti (simile a `GROUP BY` e alle funzioni di aggregazione in SQL)
  - es. per contare quanti account ci sono per ogni Stato ("group_by_state" è il nome che stai dando al risultato, "terms" indica il tipo di aggregazione, "state.keyword" indica di raggruppare sul campo "state"; "city.keyword" raggrupperebbe sul campo "city"):
  ```
  {
    "size": 0,
    "aggs": {
      "group_by_state": {
        "terms": {
          "field": "state.keyword"
        }
      }
    }
  }
  ```
  - es. per calcolare l'età media delle persone associate agli account:
  ```
  {
    "size": 0,
    "aggs": {
      "average_age": {
        "avg": {
          "field": "age"
        }
      }
    }
  }
  ```

### Documentazione completa
- Query: https://www.elastic.co/guide/en/elasticsearch/reference/7.3/query-dsl.html
- Aggregazioni: https://www.elastic.co/guide/en/elasticsearch/reference/7.3/search-aggregations.html

## Esplorazione di Dati su Kibana

### Esercizio 1
- L'esercizio segue il [tutorial ufficiale di Kibana](https://www.elastic.co/guide/en/kibana/current/tutorial-sample-data.html)

1. Dalla homepage di Kibana, clicca il link sotto **Add Sample Data** e aggiungi il dataset **Sample flight data**, che contiene dati fittizi sulle rotte aeree globali.
   - In questo modo verrà creato su Elasticsearch un nuovo indice *kibana_sample_data_flights* contenente 10000 voli

2. Esplora il nuovo indice nella console di Kibana usando `GET /kibana_sample_data_flights/_search` per vedere da quali campi sono composti i documenti dei voli (es. Paese d'origine, meteo all'arrivo, coordinate della destinazione, ...).

3. Clicca su **View Data** per aprire una dashboard già pronta sui nuovi dati

4. Il filtro temporale permette di selezionare i dati che rientrano in un certo intervallo di tempo (tra due momenti assoluti oppure in un periodo relativo ad adesso). Per avere più dati nella dashboard, impostalo sulle ultime 24 ore.

5. Per applicare dei filtri sui dati nella dashboard ci sono vari modi:
   - Con KQL ([Kibana Query Language](https://www.elastic.co/guide/en/kibana/current/kuery-query.html)):
   ```
   OriginCityName:Rome                               //seleziona i documenti in cui il campo OriginCityName = Rome
   not OriginCityName:Rome                           //seleziona i documenti in cui il campo OriginCityName != Rome
   AvgTicketPrice < 100                              //seleziona i documenti in cui il campo AvgTicketPrice < Rome
   OriginCityName:Rome and not DestCountry:US        //seleziona i documenti in cui OriginCityName = Rome e DestCountry != US
   OriginCityName:R*                                 //seleziona i documenti in cui il campo OriginCityName inizia con "R"
   ```
   - Cliccando su **Add filter** e usando i menu a tendina per impostare il filtro
   - Usando **Controls**, un [tipo speciale](https://www.elastic.co/guide/en/kibana/current/controls.html) di visualizzazione che permette di filtrare i dati tramite menu a tendina oppure range slider (bisogna cliccare su **Apply Changes** per applicare il filtro)

   Per esempio, usa uno dei metodi per selezionare Roma come città di partenza (OriginCityName=Rome) e solo i voli in ritardo (FlightDelay=true).

6. Cliccando su **Edit** puoi riorganizzare i riquadri all'interno della dashboard e aggiungere oppure modificare visualizzazioni. Per esempio, cliccando sull'ingranaggio e poi su **Edit visualization** nel riquadro intitolato "\[Flights] Total Flights", anziché il numero totale di voli puoi mostrare il numero di voli raggruppato per compagnia aerea (Carrier) oppure per la città di destinazione (DestCityName).
   - Documentazione sui tipi di visualizzazioni e su come configurarli: https://www.elastic.co/guide/en/kibana/current/visualize.html

7. Puoi ispezionare i documenti su cui si basa una visualizzazione cliccando **Inspect** sul suo menu (nell'angolo a destra). Passando da **View: Data** a **View: Requests** puoi vedere la query ad Elasticsearch e il risultato in formato JSON.

### Esercizio 2
1. Dalla homepage di Kibana, clicca su **Management** -> **Index Patterns** -> **Create index pattern** e inserisci il nome "bank" per leggere l'indice con i dati sugli account che hai caricato su Elasticsearch. Clicca su **Next step** e poi su **Create index pattern**.

2. Vai nella sezione **Discover**, seleziona "bank*" come indice e dai un'occhiata ai suoi dati. Per esempio, inserisci nella barra per le ricerche la query `account_number<100 AND balance>47500` per selezionare i primi 100 account con un saldo superiore a 47500. Puoi scegliere dalla lista **Available fields** quali campi dei documenti vuoi visualizzare (es. "account_number" o "balance").

3. Vai nella sezione **Visualize** e crea un pie chart seguendo le istruzioni che trovi [qui](https://www.elastic.co/guide/en/kibana/current/tutorial-visualizing.html#tutorial-visualize-pie).

4. Crea altre visualizzazioni sugli account aiutandoti con la documentazione sui tipi di grafici disponibili: https://www.elastic.co/guide/en/kibana/current/visualize.html

5. Vai nella sezione **Dashboard** e crea una nuova dashboard con le visualizzazioni che hai configurato.

### Esercizio 3
Carica su Elasticsearch gli indici sulle opere di Shakespeare e sui log che trovi su https://www.elastic.co/guide/en/kibana/current/tutorial-build-dashboard.html e crea delle nuove dashboard per esplorare questi dati.
