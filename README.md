# SNA4Slack gruppo Dijkstra

## Indice
[1. Introduzione](#1introduzione)

[2. Modello concettuale](2modello-concettuale)

[3. Requisiti specifici](3requisiti-sprecifici)

[4. Architettura](#4architettura)

[5. System Design](#5system-design)

[6. Riepilogo del test](#6riepilogo-test)

[7. Manuale utente](#7manuale-utente)

[8. Processo di sviluppo e organizzazione del lavoro](#8processo-di-sviluppo-e-organizzazione-del-lavoro)

[9. Analisi retrospettiva](#9analisi-retrospettiva)


## 1.Introduzione
SNA4Slack del **gruppo Dijkstra** è un Software analitico a *command-line interface*.
Come suggerisce il nome, SNA4Slack, presentiamo una *Social Network Analysis*
adattata al Software Californiano [Slack](https://slack.com/).

Lo **scopo principale** di una SNA è studiare intere strutture sociali (reti complete)
o reti locali (reti egocentrate) individuando e analizzando tra individui o gruppi,
i nodi della rete, le relazioni che sussistono tra loro,gli archi.

Nello specifico, il Software qui presentato permette di analizzare un workspace Slack, che
può essere esportato dall'amministratore in formato .zip, ed avere informazioni sui
canali pubblici. Il nostro **obiettivo** è stato quello di analizzare l'intero file .zip,
mostrare all'utente una lista di canali membri e mentions, e dare la possibilità
di potare il grafo con dei parametri. In particolare i filtri utilizzati
sono i nomi di un utente, e i nomi di un canale, utilizzabili in base ai comandi che
vengono descritti più nel particolare nel punto **7.Manuale Utente**

Riferimenti del progetto: [sna4slack](http://score-contest.org/2018/projects/sna4slack.php)

## 2.Modello Concettuale
Ci sono due Classi: **Member** e **Channel**, con realzione *Appartenere* di molteplicità uno o più di uno, e con relaizone *Crare* di molteplicità uno a molti. Member è in relazione ricorsiva con se stesso per mezzo di *Menzionare* di molteplicità zero o più di uno.

Ciò si traduce con: un membro appartiene ad uno o più canali, almeno un membro crea uno o più canali, ed un membro può menzionare più membri.

![ModelloConcettuale](https://i.imgur.com/1eedHXX.png)

## 3.Requisiti Specifici
- Help
  - L'help può essere richiesto digitando il nome del programma senza parametri aggiuntivi
  - L'help è suggerito se un comando digitato non è valido
  - L'help è mostrato su standard output
  - L'help mostra i comandi per tutte le funzionalità
- Lista Members
  - Si può fare richiesta standard da input
  - Si può specificare il workspace
  - Vi è una cartella esportata associata al workspace
  - I membri sono visualizzati uno per riga
  - I membri del workspace sono tutti presenti
  - Non vi sono membri estranei al workspace
- Lista Channels
  - Si può fare richiesta standard da input
  - Si può specificare il workspace
  - Vi è una cartella esportata associata al workspace
  - I canali sono visualizzati uno per riga
  - I canali del workspace sono tutti presenti
  - Non vi sono canali estranei al workspace
- Lista Members di un Channel
  - Si può fare richiesta standard da input
  - Si può specificare il workspace
  - Vi è una cartella esportata associata al workspace
  - I membri e i canali sono visualizzati uno per riga, con i membri visualizzati subito dopo il canale a cui appartengono
  - È possibile distinguere quale nome è un "Member" e quale è un "Channel"
  - I canali del workspace sono tutti presenti
  - I membri del canale sono tutti presenti
  - Non vi sono canali o membri estranei al workspace
- Lista Members per Channel
  - Si può fare richiesta standard da input
  - Si può specificare il workspace
  - Vi è una cartella esportata associata al workspace
  - I membri e i canali sono visualizzati uno per riga, con i membri visualizzati subito dopo il canale a cui appartengono
  - È possibile distinguere quale nome è un "Member" e quale è un "Channel"
  - I canali del workspace sono tutti presenti
  - I membri del canale sono tutti presenti
  - Non vi sono canali o membri estranei al workspace
- Lista Mentions
  - Per ogni mention è visualizzata una riga con la coppia (From, To) dove From è lo User che scrive il messaggio con il mention e To è lo User menzionato.
  - Le coppie (From, To) non sono ripetute
  - Le coppie (From, To) corrispondenti a un mention sono tutte presenti
  - Sono visualizzate solo coppie (From, To) corrispondenti a un mention
  - È possibile specificare il Channel e, nel caso sia specificato, la lista è ristretta ai soli mention del Channel
- Lista Mentions che partono da uno User
  - È possibile specificare lo User da cui partono i mention
  - Per ogni mention è visualizzata una riga con la coppia (From, To) dove From è lo User che scrive il messaggio con il mention e To è lo User menzionato.
  - Le coppie (From, To) non sono ripetute
  - Le coppie (From, To) corrispondenti a un mention sono tutte presenti
  - Sono visualizzate solo coppie (From, To) corrispondenti a un mention
  - È possibile specificare il Channel e, nel caso sia specificato, la lista è ristretta ai soli mention del Channel
- Lista Mentions che arrivano ad uno User
  - È possibile specificare lo User a cui arrivano i mention
  - Per ogni mention è visualizzata una riga con la coppia (From, To) dove From è lo User che scrive il messaggio con il mention e To è lo User menzionato e specificato nel comando
  - Le coppie (From, To) non sono ripetute
  - Le coppie (From, To) corrispondenti a un mention sono tutte presenti
  - Sono visualizzate solo coppie (From, To) corrispondenti a un mention
  - È possibile specificare il Channel e, nel caso sia specificato, la lista è ristretta ai soli mention del Channel
- Lista Mentions Pesate
  - Per ogni mention è visualizzata una riga con la tripla (From, To, Weight) dove From è lo User che scrive il messaggio con il mention e To è lo User menzionato, e Weight il numero di mention
  - Le triple (From, To, Weight) non sono ripetute
  - Le triple (From, To, Weight) corrispondenti a un mention sono tutte presenti
  - Sono visualizzate solo triple (From, To, Weight) corrispondenti a un mention
  - È possibile specificare il Channel e, nel caso sia specificato, la lista è ristretta ai soli mention del Channel  
- Lista Mentions Pesate che partono da uno User
  - È possibile specificare lo User da cui partono i mention
  - Per ogni mention è visualizzata una riga con la tripla (From, To, Weight) dove From è lo User che scrive il messaggio con il mention e To è lo User menzionato, e Weight il numero di mention
  - Le triple (From, To, Weight) non sono ripetute
  - Le triple (From, To, Weight) corrispondenti a un mention sono tutte presenti
  - Sono visualizzate solo triple (From, To, Weight) corrispondenti a un mention
  - È possibile specificare il Channel e, nel caso sia specificato, la lista è ristretta ai soli mention del Channel
- Lista Mentions Pesate che arrivano ad uno User
  - È possibile specificare lo User a cui arrivano i mention
  - Per ogni mention è visualizzata una riga con la tripla (From, To, Weight) dove From è lo User che scrive il messaggio con il mention e To è lo User menzionato, e Weight il numero di mention
  - Le triple (From, To, Weight) non sono ripetute
  - Le triple (From, To, Weight) corrispondenti a un mention sono tutte presenti
  - Sono visualizzate solo triple (From, To, Weight) corrispondenti a un mention
  - È possibile specificare il Channel e, nel caso sia specificato, la lista è ristretta ai soli mention del Channel

## 4.Architettura
Lo stile architetturale adottato è stato a strati (***Layered***), in quanto solo le interfacce delle classi principali vengono utilizzate. Il primo strato infatti è di presentazione, ed è posto nei package con dicitura ```interface```.
Le principali interfacce richiamano le classi individuate nel modello concettuale:

- **Users**: i membri del workspace sono identificati da un ID univoco e vari campi opzionali che corrispondono al nome. Noi abbiamo deciso di inserire durante il parsing del file .JSON il nome che l'utente mostra nello Slack UI, questo ha comportato una nostra decisione di controllo nel caso in cui non sia presente il parametro display_name. Tutti questi controlli sono stati nascosti attraverso l'uso dell'interfaccia, in modo da avere sia una demarcazione tra strato logico e strato esterno, sia una modificabilità estesa degli strati superiori che non influisca a cascata quelli inferiori.

- **Channels**: i canali del workspace sono identificati  da un ID univoco, un nome ed una lista di Users. Ancora una volta mostriamo solo le interfacce di classi predisposte alla gestione di singoli canali e liste di utenti.

- **Mentions**: le menzioni, che dal modello concettuale hanno luogo dalla relazione ricorsiva di un utente, sono costituite da interfacce per la gestione dei mittenti e il/i proprio/i destinatatio/i. Per l'aggiunta della pesatura delle mentions, il sistema a strati ci ha permesso una piccola modifica del codice, utilizzando un HashMap formato da una stringa che rappresenta il destinatario e la propria chiave che invece rappresenta il peso, ovvero il numero di menzioni scambiate da un utente ad un altro.

L'architettura gode di una statificazione lasca in modo tale da poter invocare operazioni di qualsiasi strato inferiore.
## 5.System Design

## 6.Riepilogo Dei Test

## 7.Manuale Utente
L'utente può scaricare l'ultima immagine disponibile del programma attraverso Docker con il comando
```
docker pull softenginfuniba/dijkstra
```
Per creare una directory virtuale all'interno del container, aggiungere "*-v /c:/ns -it*"
```
docker run -v /c:/ns -it softenginfuniba/dijkstra ns/
```
Un comando prevede il percorso assoluto o realtivo del workspace in formato .zip come primo parametro.
```
docker run -v /c:/ns -it softenginfuniba/dijkstra ns/path/file.zip <comandi>
```
 A questo si possono giustapporre i comandi mostrati in figura.

![Commands](https://i.imgur.com/6KqTw1A.png)

## 8.Processo Di Sviluppo e Organizzazione Del Lavoro
Dopo un'analisi iniziale assieme al nostro *product-owner* si è stabilito di proseguire con uno **sviluppo agile** il quale ha permesso una divisione naturale del carico di lavoro in 5 iterazioni fondamentali, e con un gradiente di difficoltà crescente che ha permesso di avere un ritmo di lavoro costante.
Le metodologie agili usate sono state Scrum e Kanban tramite una tabella virtuale offerta da [GitHub](https://help.github.com/articles/about-project-boards/).

Per quanto riguarda **Scrum** si è seguito il modello: vi è stato un *Product Backlog* mostratoci dal nostro *product-owner* ad inizio progetto, successivamente il lavoro è stato diviso in sprint di circa 10 giorni con uno *Sprint Backlog* ad inizio di ognuno, in fine il gruppo Dijkstra ha effettuato quando possibile dei *Daily Scrum Meeting* sia face-to-face che con  comunicazioni online.

Ad ogni sprint abbiamo provveduto all'assegnazione delle *User-Story* dividendole in base alla loro difficoltà e l'esperienza dei membri del team. Lavorando lontani l'uno dagli altri, siamo riusciti a controllare lo svolgimento effettivo del lavoro, anche prima del meeting giornaliero, attraverso il **Kanban** virtuale ed automatico offertoci da github.

Il gruppo Dijkstra è formato da quattro membri : Ammendolia Piera, Angelini Giuseppe, Chekalin Nazar, Lapenta Francesco Davide. Inizialmente i membri erano cinque, ma dopo una decisione di gruppo abbiamo deciso di espellerne uno, per il suo scarso contributo e poca partecipazione.
Quest'ultima decisione è stata presa la settimana precedente alla consegna finale, in modo da offrire un possibilità anche con pair-programming, che però non è stata colta.

## 9.Analisi Retrospettiva
- *Cosa da tenere*: La comunicazione tramite Slack con tutti i gruppi è stata un'esperienza positiva. Le soluzioni offerte da alcuni membri hanno permesso che non rallentassimo il lavoro, sia per problemi che inzialmente non ci eravamo posti sia per problemi a cui cercavamo risposta.

- *Problemi*: Scarsa comunicazione inter gruppo. Ci siamo resi conto che il daily scrum meeting è fondamentale, il nostro gruppo non è riuscito nella riunione giornaliera per via delle lezioni mattutine e pomeridiane. A questa mancanza abbiamo ovviato con riunioni meno sporadiche e più corpose.

- *Cosa da provare*: Aiutare e invogliare i gruppi al daily scrum meeting durante le pause delle lezioni.

