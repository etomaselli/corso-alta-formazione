Link utili:
- Documentazione: https://grafana.com/docs/
- Getting Started: https://grafana.com/docs/guides/getting_started/
- Grafana Play: https://play.grafana.org/d/000000012/grafana-play-home?orgId=1

## Configurazione del Database
Scarica il file <FILE DB FERRANDINA> e importalo in un nuovo database su PostgreSQL.

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
   - name: Rilevamenti Aria
   - host: localhost:5432
   - database: ferrandina
   - user: postgres
   - password: postgres
   - SSL mode: disable
