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
   - Raggruppa per mese sul campo "Created At"
   - Cliccando su Visualizzazione puoi cambiare o configurare il tipo di grafico ([Opzioni possibili](https://metabase.com/docs/v0.33.0/users-guide/05-visualizing-results.html))
   - Salva la domanda nella collection E-Shop con il titolo "Incasso totale per mese nell'anno 2018"

3. Segui la stessa procedura per creare la domanda **Tasse versate per mese nell'anno 2018**.

4. Crea le domande **Numero di utenti iscritti per mese nell'anno 2018** (sulla tabella Sample Dataset - People) e **Numero di ordini ricevuti per mese nell'anno 2018** (su Sample Dataset - Orders).
   - Non essendo più somme ma conteggi, la metrica da usare per queste domande è "Conteggio di righe".

5. Crea le domande **Numero totale di utenti iscritti nell'anno 2018** (conteggio su Sample Dataset - People) e **Incassi totali nell'anno 2018** (somma su Sample Dataset - Orders, "Total").
   - Su Visualizzazione si può modificare il modo in cui è rappresentato il numero (es. simbolo della valuta, arrotondamento dei decimali)
   - Se si aggiunge un raggruppamento sul campo "Created At", anziché il numero totale si può visualizzare la [tendenza](https://metabase.com/docs/v0.33.0/users-guide/05-visualizing-results.html#trends) (il numero totale nell'ultimo periodo e la differenza in percentuale rispetto al periodo precedente)

6. Crea con l'editor la [domanda personalizzata](https://metabase.com/docs/v0.33.0/users-guide/custom-questions.html) **Prodotti più acquistati nell'anno 2018** (cliccando sulle frecce a destra delle sezioni si può vedere l'anteprima dei dati ad ogni passaggio):
   - Nell'editor, scegli come tabella di partenza Sample Dataset - Orders e aggiungi un join con Sample Dataset - Products (automaticamente verrà selezionato il left outer join con la condizione Orders.ProductID = Products.ID)
   - Aggiungi un filtro su Orders.CreatedAt per selezionare solo gli ordini fatti nel 2018
   - Seleziona come metrica il "Conteggio di righe" raggruppando per Product.Title, per ottenere il numero di ordini per ciascun prodotto
   - Per creare una classifica, ordina per "Conteggio" e scegli un numero massimo di righe da includere (es. 10)
   - Clicca su Visualizza per vedere il risultato
   - Scegli il tipo di grafico che vuoi per rappresentare la classifica (es. a barre, a righe, tabella) e salva la domanda in E-Shop

7. Crea una nuova dashboard "Risultati 2018" nella collection E-Shop e aggiungi le domande create finora (sfrutta la griglia per ridimensionarle e posizionarle). Per rendere più leggibile la dashboard, puoi ritoccare l'aspetto dei grafici sulla dashboard (es. titoli, titoli degli assi) senza che la domanda originale venga modificata.

## Esercizio 2
1. Duplica la dashboard "Risultati 2018" e rinomina la nuova copia "Risultati 2018 - Compatta".

2. Modifica la nuova copia per [riunire in un unico grafico](https://metabase.com/docs/v0.33.0/users-guide/09-multi-series-charting.html) le domande **Incasso totale per mese nell'anno 2018** e **Tasse versate per mese nell'anno 2018**:
   - Clicca su "Aggiungi" nell'angolo del grafico sull'incasso e seleziona la seconda domanda per aggiungerla allo stesso grafico
   - Per rendere i dati confrontabili (entrambe le serie sono in dollari), modifica le impostazioni del grafico per mostrare anche la seconda serie sull'asse verticale di sinistra
   - Adesso che le due domande sono combinate, il grafico sulle tasse versate è superfluo e può essere rimosso dalla dashboard

3. Clicca su "Aggiungi un filtro" per inserire alcuni [filtri](https://metabase.com/docs/v0.33.0/users-guide/08-dashboard-filters.html) nella dashboard:
   - Scegli il tipo di filtro "Tempo" e seleziona "Mese e anno" per poter filtrare i dati relativi ad un mese specifico
   - Su ogni grafico, seleziona il campo temporale ("Created At") su cui dovrà agire il filtro
   - Salva la dashboard e testa il filtro (dato che tutti i grafici sono già filtrati sul 2018, scegliendo altri anni non ci saranno dati)
   - Torna su "Aggiungi un filtro" e inserisci un filtro di tipo "Altre categorie" per filtrare i dati relativi ad uno Stato specifico
   - Su ogni grafico seleziona il campo User.State
   - Salva di nuovo e testa il filtro

- Se aggiungi filtri che non sono validi per tutti i grafici della dashboard, quelli su cui il filtro non è applicabile non saranno influenzati dal filtro e continueranno a mostrare gli stessi dati (es. se filtri sul campo Product.Category, il filtro non sarà applicabile ai due grafici sugli utenti)

## Esercizio 3
- Connessione allo schema **twitter** del database **corso_af**

1. Dalla home page di Metabase vai su Impostazioni (in alto a destra) -> Admin -> Aggiungi Database e inserisci:
   - Database type: PostgreSQL
   - Nome: Twitter
   - Host: localhost
   - Porta: 5432
   - Database: corso_af (o il nome che hai usato)
   - Username: postgres
   - Password: postgres

2. Clicca su "Sfoglia i dati" e verifica che Metabase legga correttamente la tabella **Utenti**

3. Crea una dashboard con alcune domande per analizzare la tabella Utenti (es. da quali Paesi proviene il maggior numero di utenti, da quali il maggior numero di utenti maschi o femmine, qual è il dominio più comune a cui sono registrate le mail, ...)

- numero di utenti per ogni dominio email:

```
select distinct substring(email, position('@' in email)+ 1), count(id) from Twitter.utenti
group by substring(email, position('@' in email)+ 1)
```
