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

7. Crea una nuova dashboard "Risultati 2018" nella collection E-Shop e aggiungi le domande create finora (sfrutta la griglia per ridimensionarle e posizionarle per renderle leggibili al meglio).

---
- sample database: creazione dashboard senza filtri e senza grafici multi-serie, aggiunta di filtri e multi-serie
	1. incassi totali nel tempo *
	2. tasse versate nel tempo *
	3. numero di utenti iscritti nel tempo
	4. numero di ordini nel tempo
	5. numero totale di utenti iscritti
	6. incassi totali
	7. classifica dei prodotti più acquistati
- database di Alberto: creazione dashboard con filtri e multiserie
