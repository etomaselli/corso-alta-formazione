Link utili:
- Guida utenti: https://metabase.com/docs/v0.33.0/users-guide/start.html
- Visualizzazioni disponibili: https://metabase.com/docs/v0.33.0/users-guide/05-visualizing-results.html

## Download e Configurazione di Metabase
- https://metabase.com/start/other.html -> **Download Metabase**

Da linea di comando, naviga alla cartella in cui hai scaricato Metabase e avvia il server:

```
java -jar metabase.jar
```

Raggiungi l'indirizzo http://localhost:3000.
Segui le indicazioni in https://metabase.com/docs/v0.33.0/setting-up-metabase.html -> **Setting up an admin account** per creare un account di amministratore (salta il passaggio di connessione al database).
In fondo alla homepage troverai il database Sample Dataset.

## Esercizio 1
1. Crea una nuova collection "E-Shop" (Homepage -> La nostra analisi -> Sfoglia tutti gli elementi -> Nuova collezione).
2. Aiutandoti con le guide [Getting Started](https://metabase.com/docs/v0.33.0/getting-started.html) e [Creare una domanda semplice](https://metabase.com/docs/v0.33.0/users-guide/04-asking-questions.html), crea una nuova domanda sulla tabella **Sample Dataset - Orders** per visualizzare l'**Incasso totale per mese nell'anno 2018**:
   - Filtra sul campo "Created At" per selezionare solo gli ordini fatti dopo il 31 dicembre 2017 e prima del 1 gennaio 2019
   - Seleziona la metrica "Somma di..." sul campo "Total" per sommare l'incasso degli ordini
   - Raggruppa per mese sul campo "created At"
   - (opzionale) Cliccando su Visualizzazione puoi cambiare o configurare il tipo di grafico ([Opzioni possibili](https://metabase.com/docs/v0.33.0/users-guide/05-visualizing-results.html))
   - Salva la domanda nella collection E-Shop con il titolo "Incasso totale per mese nell'anno 2018"

---
- get started: https://metabase.com/docs/v0.33.0/getting-started.html
- sample database: creazione dashboard senza filtri e senza grafici multi-serie, aggiunta di filtri e multi-serie
	1. incassi totali nel tempo |
	2. tasse versate nel tempo  |
	3. numero di utenti iscritti nel tempo
	4. numero di ordini nel tempo
	5. classifica dei prodotti più acquistati e loro rating
	6. numero totale di utenti iscritti
	7. numero totale di incassi
- database di Alberto: creazione dashboard con filtri e multiserie
