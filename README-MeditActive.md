# MeditActive — Sistema di Gestione degli Elementi

Progetto Python di **Fabio Mencio**.

MeditActive è una piattaforma per obiettivi di benessere: meditare, muoversi,
ascoltarsi, dormire meglio. Ogni sessione completata vale monete virtuali
proporzionali ai minuti dedicati, e ogni 50 monete l'applicazione simula la
piantumazione di un albero.

Il programma è scritto con le sole strutture di base del linguaggio — liste,
dizionari, funzioni, cicli e condizioni — senza classi, `lambda`, comprensioni
di liste, eccezioni o librerie esterne. L'unica importazione è `datetime`.

**File del progetto:** [`MeditActive.ipynb`](./MeditActive.ipynb)

## Come si esegue

Aprire il notebook in Google Colab o in Jupyter con Python 3 ed eseguire tutte
le celle dall'alto verso il basso. Non servono file esterni, connessione di rete
o pacchetti da installare. Per ripartire dai dati iniziali basta riavviare il
kernel ed eseguire di nuovo tutto.

Il notebook esegue 19 celle di codice e si chiude con sette controlli automatici
sui risultati attesi.

## Tre scelte di progetto

**Le funzioni ricevono nei parametri tutto ciò che usano.** Nessuna funzione
legge o modifica una variabile della dimostrazione: non c'è `global`. La
collezione viene passata come parametro e il saldo viene restituito aggiornato,
così il flusso dei dati è leggibile dalla sola firma della funzione.

```python
saldo_monete = completa_obiettivo(collezione_obiettivi, 4, saldo_monete, adesso)
```

**L'istante di riferimento è un parametro, non l'orologio di sistema.** Le
funzioni che devono sapere che giorno è ricevono `adesso`. Il comportamento
diventa così verificabile: per mostrare cosa succede domani o fra dieci giorni
basta passare un'altra data, senza aspettare.

**Un dato che si può calcolare non viene memorizzato.** Le monete dipendono
dalla durata, lo stato di completamento dipende dall'ultima sessione e dalla
frequenza: entrambi si ricavano quando servono. Un campo salvato sarebbe una
seconda verità da tenere allineata, e alla prima modifica della durata
divergerebbe da quella che la genera.

## I dati

La collezione è una lista di dizionari. Ogni obiettivo ha le stesse chiavi:

| Chiave | Significato |
|---|---|
| `ID` | identificativo, testo di tre cifre |
| `nome` | nome dell'attività |
| `tipo` | categoria fra quelle ammesse |
| `frequenza` | periodicità prevista |
| `durata_minuti` | durata di una sessione |
| `volte_completato` | numero di sessioni registrate |
| `ultima_sessione` | data dell'ultima sessione, `None` se mai completato |
| `data_inserimento` | data di aggiunta alla collezione |

Le date sono oggetti `datetime`, non testo: si confrontano e si sommano
direttamente, e vengono formattate solo al momento della stampa.

Le regole del dominio sono costanti condivise, definite una volta sola fuori
dalle funzioni: `TIPI_AMMESSI`, `FREQUENZE_AMMESSE`, `GIORNI_PERIODO`,
`MINUTI_PER_MONETA` e `MONETE_PER_ALBERO`. Registrazione e ricerca validano i
dati contro le stesse liste, quindi non possono divergere.

## Le funzioni

| Funzione | Cosa fa |
|---|---|
| `monete_per_sessione(durata_minuti)` | converte i minuti in monete, una ogni cinque |
| `completato_nel_periodo(elemento, adesso)` | dice se il periodo in corso è già stato usato |
| `testo_data(momento)` | formatta una data per la stampa, `mai` se assente |
| `crea_collezione_iniziale()` | restituisce i dieci obiettivi di partenza |
| `registrazione_nuovo_elemento(collezione, nome, tipo, frequenza, durata_minuti, adesso)` | valida e inserisce, restituisce `True` o `False` |
| `visualizza_collezione(collezione, adesso)` | stampa gli obiettivi in forma leggibile |
| `completa_obiettivo(collezione, identificativo, saldo_monete, adesso)` | registra una sessione e restituisce il saldo |
| `filtro_avanzato(collezione, adesso, ...)` | cerca per categoria, durata, stato e nome |
| `statistiche_elementi(collezione, saldo_monete, adesso)` | dashboard e dizionario di riepilogo |

Tutte le funzioni hanno una docstring nella forma della
[PEP 257](https://peps.python.org/pep-0257/): una frase iniziale, una riga
vuota, poi i parametri e il valore restituito.

## La frequenza è una regola, non un'etichetta

Un obiettivo giornaliero vale una sessione al giorno; uno mensile, una ogni
trenta giorni. Prima di registrare una sessione il programma verifica se il
periodo in corso è già stato usato: in caso affermativo rifiuta il
completamento, non assegna monete e indica da quando la prossima sessione sarà
valida.

```text
Già completato: [001] Meditazione guidata mattutina
Frequenza giornaliera: una sessione al giorno.
Ultima sessione: 20/09/2026 07:30
Prossima sessione valida dal 21/09/2026 07:30.
```

Quando il periodo scade, l'obiettivo torna da solo fra quelli da completare.
Non c'è nessuna riga di codice che lo riazzera, perché non c'è nessun campo da
riazzerare: lo stato è il risultato di un confronto fra due date.

| Frequenza | Una sessione ogni |
|---|---|
| Giornaliera | 1 giorno |
| Settimanale | 7 giorni |
| Mensile | 30 giorni |
| Annuale | 365 giorni |

## Validazione

La registrazione rifiuta, con un messaggio esplicito e senza toccare la
collezione: nomi, categorie o frequenze che non siano testi non vuoti;
categorie e frequenze fuori dalle costanti ammesse; durate che non siano interi
positivi — `"trenta"`, `"30"`, `30.0`, `True`, zero e i negativi; nomi già
presenti, anche scritti con maiuscole diverse.

Sui testi il controllo usa `isinstance(nome, str)`. Sulla durata serve invece
`type(durata_minuti) != int`, perché in Python `True` è anche un intero e
`isinstance(True, int)` risponde `True`: solo il confronto diretto del tipo
riesce a rifiutare un booleano.

Anche la ricerca valida la categoria contro `TIPI_AMMESSI`. Senza quel
controllo una ricerca scritta male — `tipo_cercato="meditaz"` — restituirebbe
una lista vuota indistinguibile da «non ho trovato niente», e chi cerca
penserebbe che quegli obiettivi non esistono.

Gli identificativi sono calcolati come massimo presente più uno, non come
`len() + 1`: dopo una rimozione un ID già usato non viene riassegnato.

## Cosa dimostra il notebook

Oltre al flusso principale — dataset, inserimenti, completamenti, ricerche e
dashboard — il notebook mette alla prova i punti in cui un programma di solito
si rompe: collezione vuota, input del tipo sbagliato, nome duplicato, ricerca
senza risultati, categoria inesistente, identificativo inesistente, sessioni
ripetute fuori periodo, identificativi dopo una rimozione e riscatto di più
alberi con un solo saldo.

## Limiti

I dati vivono in memoria per la durata della sessione: non c'è archiviazione su
disco, quindi riavviando il kernel si riparte dal dataset iniziale. Le sessioni,
le monete e gli alberi sono simulati: non viene effettuato alcun acquisto e non
viene piantato alcun albero reale.

I periodi sono contati in giorni interi — un mese vale trenta giorni, un anno
trecentosessantacinque — senza tener conto della lunghezza reale dei mesi né
degli anni bisestili. È una semplificazione accettabile per obiettivi di
benessere, dove conta la cadenza e non la data esatta.
