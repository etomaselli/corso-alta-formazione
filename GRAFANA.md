Link utili:
- Documentazione: https://grafana.com/docs/
- Getting Started: https://grafana.com/docs/guides/getting_started/
- Grafana Play: https://play.grafana.org/d/000000012/grafana-play-home?orgId=1
- Configurazione dei pannelli: https://grafana.com/docs/features/panels/graph/

## Configurazione del Database
Crea un nuovo database "Misurazioni" su PostgreSQL e aggiungi le tabelle:

```
CREATE TABLE log_series (measurement timestamp with time zone, active_users integer, inactive_users integer);
CREATE TABLE server_status (measurement timestamp with time zone, server_id character varying(50), cpu double precision, num_of_requests integer);
```

Inserisci i dati:

```
INSERT INTO log_series(measurement, active_users, inactive_users)
SELECT generate_series as measurement, round(random()*50) as active_users, round(random()*50) as inactive_users
FROM generate_series('2019-09-01 00:00'::timestamp with time zone,'2019-09-30 12:00', '15 seconds');

INSERT INTO server_status(measurement, server_id, cpu, num_of_requests)
SELECT generate_series as measurement, 'server1' as server_id, round(random()::numeric, 1) as cpu, round(random()*50) as num_of_requests
FROM generate_series('2019-09-01 00:00'::timestamp with time zone,'2019-09-30 12:00', '15 seconds')
UNION
SELECT generate_series as measurement, 'server2' as server_id, round(random()::numeric, 1) as cpu, round(random()*50) as num_of_requests
FROM generate_series('2019-09-01 00:00'::timestamp with time zone,'2019-09-30 12:00', '15 seconds')
UNION
SELECT generate_series as measurement, 'server3' as server_id, round(random()::numeric, 1) as cpu, round(random()*50) as num_of_requests
FROM generate_series('2019-09-01 00:00'::timestamp with time zone,'2019-09-30 12:00', '15 seconds');
```

## Download e Configurazione di Grafana
#Per Windows:
- https://grafana.com/grafana/download/6.3.5?platform=windows -> **Standalone Windows Binaries**
- Segui le istruzioni di configurazione: https://grafana.com/docs/grafana/v6.3/installation/windows/ (la porta 10000 di solito è utilizzabile)

Da linea di comando, naviga alla cartella \path\di\grafana\bin ed esegui

```
grafana-server.exe
```

Raggiungi l'indirizzo http://localhost:10000 e autenticati come admin/admin.

#Per Linux:
- https://grafana.com/grafana/download/6.3.5?platform=linux
- Istruzioni per l'avvio https://grafana.com/docs/grafana/v6.3/installation/debian/

Da linea di comando esegui `sudo service grafana-server start`.

Raggiungi l'indirizzo http://localhost:3000 e autenticati come admin/admin.

## Configurazione di una Datasource
Crea una nuova [datasource PostgreSQL](https://grafana.com/docs/features/datasources/postgres/#adding-the-data-source):
   - name: Misurazioni
   - host: localhost:5432
   - database: Misurazioni
   - user: postgres
   - password: postgres
   - SSL mode: disable

## Come Usare il Query Editor
- https://grafana.com/docs/features/datasources/postgres/#query-editor

## Esercizio 1
1. Crea una nuova dashboard "Misurazioni" e seleziona "Last 1 hour" dal filtro temporale come periodo da analizzare
2. Aggiungi un pannello intitolato **Utenti attivi e inattivi per minuto** con un grafico che mostri le due serie
   - usa "sum" come funzione di aggregazione sulle colonne "active_users" e "inactive_users" e raggruppa usando la funzione *time(1m, none)*
   - nella legenda mostra anche il valore massimo raggiunto nel periodo scelto da ciascuna serie
3. Aggiungi un pannello intitolato **Utenti attivi** con il numero totale (singlestat) di utenti attivi
4. Aggiungi un pannello intitolato **Utenti inattivi** con il numero totale di utenti inattivi
5. Aggiungi un pannello intitolato **Utenti totali** con il numero totale di utenti (*active_users + inactive_users*)
6. Aggiungi un pannello intitolato **Numero medio di utenti attivi e inattivi per minuto** con una tabella
   - usa "avg" come funzione di aggregazione sulle colonne "active_users" e "inactive_users" e raggruppa usando la funzione *time(1m, none)*

## Esercizio 2
Crea una nuova dashboard e crea almeno cinque pannelli a tua scelta che mostrino i dati presenti nelle tabelle *log_series* e *server_status*, utilizzando anche delle variabili.

Puoi prendere spunto dalle dashboard presenti su Grafana Play (link a inizio pagina) e dalla documentazione che riguarda le variabili (https://grafana.com/docs/reference/templating/).
