Link utili:
- Documentazione: https://grafana.com/docs/
- Getting Started: https://grafana.com/docs/guides/getting_started/
- Grafana Play: https://play.grafana.org/d/000000012/grafana-play-home?orgId=1

## Configurazione del Database
Scarica il file <FILE DB FERRANDINA> e importalo in un nuovo database su PostgreSQL.

```
CREATE TABLE rilevamenti
(
	id serial NOT NULL,
	stazione character varying(50),
	parametro character varying(50),
	udm character varying(50),
	giorno date,
	ora time,
	media double precision,
	validita integer,
	CONSTRAINT rilevamenti_pkey PRIMARY KEY (id)
)

COPY rilevamenti(stazione,parametro,udm,giorno,ora,media,validita)
FROM 'path\to\ferrandina.csv' DELIMITER ',' CSV HEADER;
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
   - name: Rilevamenti Aria
   - host: localhost:5432
   - database: ferrandina
   - user: postgres
   - password: postgres
   - SSL mode: disable
