# MeditActive — Progetto Python

## Descrizione

**MeditActive** è un progetto realizzato nell'ambito dello studio della programmazione Python.

L'obiettivo è sviluppare un semplice **Sistema di Gestione degli Elementi (SGE)** per la gestione di attività legate al benessere personale, alla meditazione, al movimento e alla sostenibilità.

Il progetto permette di applicare in modo pratico alcuni dei principali concetti base della programmazione Python:

- variabili
- liste
- dizionari
- funzioni
- cicli `for` e `while`
- condizioni `if`
- parametri opzionali
- ricerca e filtraggio dei dati
- semplici analisi statistiche

---

## Obiettivo del progetto

MeditActive simula una piattaforma nella quale un utente può creare e completare obiettivi personali.

Ogni attività possiede alcune caratteristiche, tra cui:

- ID
- nome dell'attività
- categoria
- frequenza
- durata in minuti
- valore in monete virtuali
- stato di completamento

Quando un'attività viene completata, l'utente riceve delle **monete virtuali** proporzionali alla durata dell'attività.

Le monete accumulate possono successivamente essere utilizzate per simulare il finanziamento di iniziative ambientali, come la piantumazione di un albero.

---

## Funzionalità implementate

### 1. Dataset iniziale

Il programma contiene una collezione iniziale di attività appartenenti a diverse categorie:

- Meditazione
- Movimento
- Introspezione
- Sonno

I dati vengono memorizzati attraverso una **lista di dizionari Python**.

### 2. Registrazione di nuovi obiettivi

La funzione:

```python
registrazione_nuovo_elemento()
```

permette di aggiungere un nuovo obiettivo alla collezione.

Per ogni nuovo elemento viene generato automaticamente un ID progressivo.

Il valore delle monete viene calcolato in base alla durata:

```python
durata_minuti // 5
```

quindi viene assegnata una moneta ogni 5 minuti di attività.

### 3. Visualizzazione della collezione

La funzione:

```python
visualizza_collezione()
```

permette di visualizzare tutti gli obiettivi presenti nel sistema mostrando:

- ID
- nome
- categoria
- frequenza
- durata
- valore in monete
- stato dell'attività

### 4. Completamento degli obiettivi

La funzione:

```python
completa_obiettivo()
```

consente di registrare il completamento di un'attività.

L'obiettivo può essere ricercato attraverso:

- ID
- nome dell'attività

Ad ogni completamento vengono aggiunte le relative monete al saldo dell'utente.

Il programma tiene inoltre traccia del numero di volte in cui la stessa attività viene completata.

### 5. Sistema delle monete e sostenibilità

Le attività completate generano monete virtuali.

Quando il saldo raggiunge almeno:

```text
50 monete
```

il programma simula la possibilità di **piantare un albero**, sottraendo 50 monete dal saldo disponibile.

L'utilizzo di un ciclo `while` permette di gestire anche la presenza di monete sufficienti per più alberi.

### 6. Ricerca e filtraggio multicriterio

La funzione:

```python
filtro_avanzato()
```

permette di filtrare gli obiettivi utilizzando diversi criteri:

- categoria
- durata massima
- stato di completamento

I parametri sono opzionali grazie all'utilizzo del valore:

```python
None
```

In questo modo è possibile utilizzare uno o più criteri contemporaneamente.

Esempio:

```python
filtro_avanzato(
    tipo_cercato="Meditazione",
    durata_massima=30
)
```

ricerca tutte le attività di meditazione con durata non superiore a 30 minuti.

### 7. Dashboard statistica

La funzione:

```python
statistiche_elementi()
```

produce alcune semplici statistiche sulla collezione:

- numero totale degli obiettivi
- obiettivi completati almeno una volta
- numero totale delle sessioni
- saldo delle monete
- distribuzione delle attività per categoria
- percentuale delle diverse categorie

Per il conteggio delle categorie viene utilizzato il metodo:

```python
.get()
```

dei dizionari Python.

---

## Concetti Python utilizzati

Durante lo sviluppo del progetto sono stati utilizzati principalmente:

```python
list
dict
def
if / else
for
while
None
global
len()
.get()
.lower()
.capitalize()
.append()
.items()
round()
return
```

---

## Esecuzione

Il progetto è stato sviluppato tramite **Google Colab**.

Il notebook può essere eseguito anche localmente utilizzando Python 3 e Jupyter Notebook.

Per una corretta esecuzione è consigliato eseguire tutte le celle del notebook dall'inizio nell'ordine in cui sono presentate.

---

## File principali

```text
MeditActive.ipynb    Notebook principale del progetto
README.md            Documentazione del progetto
```

[Apri il notebook](./MeditActive.ipynb)

---

## Finalità didattica

Il progetto è stato sviluppato principalmente con finalità didattiche.

L'obiettivo non è realizzare un'applicazione completa destinata alla produzione, ma mettere in pratica i concetti fondamentali della programmazione Python attraverso un caso d'uso concreto.

Particolare attenzione è stata dedicata alla comprensione delle strutture dati, delle funzioni e della logica utilizzata nel programma.

---

## Possibili sviluppi futuri

Come evoluzioni successive del progetto potrebbero essere introdotti:

- salvataggio dei dati su file
- gestione di più utenti
- interfaccia grafica
- grafici statistici
- utilizzo di Pandas per l'analisi dei dati
- storico delle attività completate

Queste funzionalità non fanno parte della versione attuale del progetto.

---

## Autore

**Fabio Mencio**

Progetto realizzato nell'ambito del percorso di studio della programmazione Python.

GitHub: [fabiom-coder](https://github.com/fabiom-coder)
