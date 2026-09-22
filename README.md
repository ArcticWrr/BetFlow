# BetFlow

Progetto per il Laboratorio di Web Scraping dedicato all'analisi dell'attività di **SatoshiDice** attraverso dati della blockchain Bitcoin. Il notebook combina analisi delle transazioni, visualizzazioni, ricostruzione di catene di puntate e scraping di WalletExplorer.

## Analisi svolte

1. **Preparazione dei dati:** caricamento dei dataset, controllo di duplicati e valori mancanti, conversione degli importi da satoshi a BTC.
2. **Analisi delle puntate:** andamento temporale, popolarità degli indirizzi SatoshiDice, relazione tra commissioni e importi e intervalli tra puntate consecutive.
3. **Catene di simple bet:** selezione di transazioni con un input e due output, uno di puntata e uno di resto. Le transazioni vengono collegate in un grafo diretto quando una puntata successiva spende il resto della precedente.
4. **Scraping di WalletExplorer:** analisi degli indirizzi delle catene più lunghe per confrontare i wallet a cui il servizio li associa.

## Contenuto del repository

| File | Contenuto |
| --- | --- |
| [progetto_BetFlow.ipynb](progetto_BetFlow.ipynb) | Notebook con codice, grafici e commenti all'analisi |
| [progetto_BetFlowV3.pdf](progetto_BetFlowV3.pdf) | Relazione del progetto |
| [satoshiDiceInfos.tsv](satoshiDiceInfos.tsv) | Indirizzi e parametri delle puntate SatoshiDice |
| [SatoshiDiceBetting.pdf](SatoshiDiceBetting.pdf) | Documento di riferimento |
| [.gitignore](.gitignore) | Esclusioni per dataset voluminosi, file temporanei e copie di consegna |

## Dataset necessari

Per rieseguire il notebook sono necessari quattro CSV, **non inclusi nel repository** per le loro dimensioni complessive (circa 1,8 GB). Devono essere procurati separatamente e collocati nella stessa cartella del notebook:

| File | Colonne attese, nell'ordine |
| --- | --- |
| `transactions.csv` | `timestamp`, `blockId`, `txId`, `isCoinbase`, `fee` |
| `inputs.csv` | `txId`, `prevTxId`, `prevTxPos` |
| `outputs.csv` | `txId`, `position`, `addressId`, `amount_satoshi`, `scriptType` |
| `mapAddr2Ids8708820.csv` | `address`, `addressId` |

Il notebook legge questi CSV senza intestazione e assegna i nomi delle colonne durante il caricamento. Il file `satoshiDiceInfos.tsv`, già incluso, viene letto separatamente saltando la riga iniziale contenente la fonte.

Il repository non include una procedura di download dei quattro CSV: il solo clone non è sufficiente per rieseguire l'analisi.

## Esecuzione

È necessario un ambiente Python con Jupyter e le librerie utilizzate dal notebook:

```bash
python3 -m pip install jupyter numpy pandas matplotlib seaborn networkx selenium
```

Avvia Jupyter dalla cartella principale del progetto:

```bash
python3 -m jupyter notebook
```

Apri `progetto_BetFlow.ipynb` ed esegui le celle in ordine. I percorsi dei dataset sono costruiti a partire dalla directory di lavoro (`Path.cwd()`).

La configurazione attuale usa `FULL_VALIDATION = True` e carica tutti i dati; le copie dei DataFrame e le operazioni di unione possono richiedere molta più memoria della dimensione dei file. Per una prima esplorazione puoi impostare `FULL_VALIDATION = False`: verranno lette le prime 100.000 righe di ciascun CSV. Questo campionamento può interrompere i collegamenti tra dataset e non riproduce i risultati dell'analisi completa.

### Scraping

La fase 4 usa Selenium con `webdriver.Safari()`: così com'è scritta richiede macOS, Safari configurato per l'automazione remota e una connessione Internet. Per utilizzare un altro browser occorre adattare l'inizializzazione del WebDriver.

Le prime tre fasi lavorano sui file locali. La quarta dipende dalla disponibilità e dalla struttura delle pagine di WalletExplorer; i risultati possono cambiare rispetto a quelli salvati nel notebook. Le associazioni tra indirizzi e wallet sono attribuzioni del servizio, non prove dell'identità dei titolari.

## Autore

Antonio Fatticcioni
