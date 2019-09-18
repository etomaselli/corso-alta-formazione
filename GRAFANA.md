Link utili:
- Documentazione: https://grafana.com/docs/
- Getting Started: https://grafana.com/docs/guides/getting_started/
- Grafana Play: https://play.grafana.org/d/000000012/grafana-play-home?orgId=1
- Configurazione dei pannelli: https://grafana.com/docs/features/panels/graph/

## Configurazione del Database
Crea un nuovo database "Misurazioni" su PostgreSQL e aggiungi la tabella "log_series":

```
CREATE TABLE log_series (measurement timestamp with time zone, active_users integer, inactive_users integer);
```

Inserisci una serie di dati temporali:

```
INSERT INTO log_series(measurement, active_users, inactive_users)
SELECT generate_series as measurement, round(random()*50) as active_users, round(random()*50) as inactive_users
FROM generate_series('2019-09-01 00:00'::timestamp with time zone,'2019-09-30 12:00', '15 seconds');
```

## Download e Configurazione di Grafana
- https://grafana.com/grafana/download?platform=windows -> **Download the zip file**
- Segui le istruzioni di configurazione: https://grafana.com/docs/installation/windows/ (la porta 10000 di solito è utilizzabile)

Da linea di comando, naviga alla cartella \path\di\grafana\bin ed esegui

```
grafana-server.exe
```

Raggiungi l'indirizzo http://localhost:10000 e autenticati come admin/admin.

## Configurazione di una Datasource
1. Crea una nuova [datasource PostgreSQL](https://grafana.com/docs/features/datasources/postgres/#adding-the-data-source):
   - name: Misurazioni
   - host: localhost:5432
   - database: Misurazioni
   - user: postgres
   - password: postgres
   - SSL mode: disable
