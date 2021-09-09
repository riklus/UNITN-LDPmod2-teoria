# Macchine Astratte

# The Computer

Un computer è composto dalle seguenti componenti fisiche:

1. **Un processore (CPU)**
    - Esegue le istruzioni macchina.
    - Muove i dati da e alla memoria.
2. **Memoria centrale (RAM)**
    - Memorizza dati e programmi (sequenze di istruzioni macchina).
    - Veloce ma volatile.
3. **Mass storage device**
    - Più lento della RAM ma persistente.
4. **Periferiche per I/O**

## Architettura

Le diverse componenti di un computer: CPUs, RAM, I/O devices, etc…Sono connessi da un *bus*

- Tipo il *System Bus*

Il bus viene usato per trasmettere istruzioni e dati tra CPU e RAM e per input/outpud dai e per i mass storage devices.

### Von Neumann Architecture

![Von_Neumann.png](Macchine%20Astratte%2058a99dc45481448c9a26c344c9994cc7/Von_Neumann.png)

Von Neumann e un Bus

La memoria RAM contiene sia i dati che le istruzioni. Il Bus connette la CPU alla memoria e ai I/O Devices.

## Il Processore

Ottiene le istruzioni dalla memoria e le esegue.
Possiede due parti:

1. **Control part:** Ottiene ed esegue istruzioni.
2. **Operative part:** una Arithmetic Logic Unit (ALU) esegue operazioni artimetico-logiche.

Le moderne CPUs hanno anche una Floating Aritmetic Unit (FPU) etc…

Contiene anche dei *registri* che possono essere visibili o nascosti al programmatore.

### I Registri

Possono essere di due tipi:

1. Invisibili (Non accessibili direttamente dalle istruzioni macchina)
E sono:
    - **Address Register (AR):** Indirizzo per accedere al bus.
    - **Data Register (DR):** Data da leggere e/o scrivere.
2. Visibili (Alle istruzioni macchina)
E sono:
    - **Program Counter (PC):** L’indirizzo della prossima istruzione d eseguire, chiamata anche _Instruction Pointer_ (IP).
    - **Status Register (SR):** Flags che descrivono il risultato di un’operazione dell’ALU e lo stato della macchina, chaiamata anche F (Flag Register).
    - Molti registri per dati e indirizzi.

### Esecuzione delle Istruzioni

Sono *tre* gli step che il processore impiega per eseguire un’operazione:

1. **Fetch**, legge la prossima istruzione da eseguire
    - Copia il Program Counter (PC) nell’Address Register (AR).
    - Trasferisce il dato indirizzato nell’AR dalla RAM al Data Register (DR).
    - Salva il DR in un registro invisibile.
    - Incrementa il Program Counter.
2. **Decode**, decodifica dell’istruzione.
3. **Execute**, esecuzione dell’istruzione decodificata.
    - Se necessario carica i dati dalla memoria, imposta l’AR, legge il DR etc…
    - Se necessatio salva i dati in memoria, setta l’AR etc…
    - **Se necessario modifica il PC (JUMP INSTRUCTIONS)**
4. In aggiunta alle precedenti tre fasi, ve ne sono altre due necessarie solo in certi casi in cui è necessario accedere alla memoria:
    - *fase di caricamento dati in memoria*, che effettua il caricamento di un dato dalla memoria;
    - *fase di salvataggio dati*, che permette di salvare il risultato di un'operazione all'interno di un registro.

![execflow.png](Macchine%20Astratte%2058a99dc45481448c9a26c344c9994cc7/execflow.png)

Esecuzione di un programma

La CPU cicla ed esegue le stesse istruzioni, e di solito le esegue in modo *sequenziale*.

Ogni macchina è progettata per eseguire un linguaggio specifico, **LM**: Machine Language.

## Main Memory

Per l’architettura di Von Neumann la stessa memoria può contenere sia i dati che le istruzioni. 
È composta da un set di celle o *locations*, ognuna lunga 8 bit (i computer moderni ne hanno di più). Si accede alla RAM tramite il *bus*.

L’accesso in memoria avviene nel seguente modo:

1. **Load**, caricamento dell’indirizzo d’interesse, che punta in memoria, nell’AR register.
2. **In caso di write operation**, si carica il dato da scrivere nel DR register.
3. **Signal**, si segnala via il bus che operazione eseguire *write/read*
4. **In caso di read operation** il dato letto si troverà nel DR register.

![Untitled](Macchine%20Astratte%2058a99dc45481448c9a26c344c9994cc7/Untitled.png)

Esempio di una CPU

Questo è un esempio semplificato di CPU. Le CPU moderne sono costituite anche da *pipelines* e *parallelism*.

# Physical Machine

Una macchina fisica o *Computer* è una macchina *progettata* per l’esecuzione di programmi.

Ogni macchina esegue programmi scritti nel proprio linguaggio.

Esiste una stretta relazione tra la macchina e un linguaggio:
Ogni macchina ha il suo linguaggio che può capire ed eseguire, ma lo stesso linguaggio può essere eseguito da diverse macchine. Inoltre, una macchina può eseguire **SOLO** il linguaggio che può comprendere, cioè il suo linguaggio macchina.

L’esecuzione di un programma può avvenire attraverso:

- La **CPU** la quale implementa il cilo di esecuzione in *hardware*.
- **Interpretazione** ovvero un algoritmo che capisce ed esegue il suo linguaggio macchina.

In linea teorica il **processore** è una *implementazione fisica di un algoritmo e strutture dati*.

# Abstract Machine

Una **macchina astratta** $M_L$ è una collezione di algoritmi e strutture dati che ci consente di eseguire programmi scritti in *L*. A differenza di un processore la macchina astratta è implementata a livello **software**. Similarmente a una macchina fisica anche la macchina astratta è associata a un **linguaggio**.

- $L$ è il *linguaggio macchina* per $M_L$.
- Un **programma** è una *sequenza di istruzioni scritte in* $L$.

## Operazione di una Macchina Astratta

Per eseguire un programma scritto in $L,$ $M_L$ deve:

1. Eseguire le operazioni elementari, sarebbe l’ALU nell’hardware.
2. Controllare la sequenza di esecuzione:
    - Le operazioni non sequenzial come *salti* e i *cicli*.
3. Trasferire i dati da e alla memoria, e implementare metodi per indirizzare alla memoria.
4. Organizzazione della memoria:
    - Allocazione dinamica, gestione dello stack, gestione della memoria per i dati e per i programmi etc…

![Untitled](Macchine%20Astratte%2058a99dc45481448c9a26c344c9994cc7/Untitled%201.png)

Esempio di una Macchina Astratta

Il ciclo di esecuzione è simile a quello di una CPU ma implementato via *software*.

## Implementazione di un Linguaggio

$M_L$ capisce il linguaggio macchina $L$, come nel caso della CPU esiste un *unico linguaggio* per ogni **macchina astratta**; ma lo stesso linguaggio puè essere capito ed eseguito da diverse macchine astratte che possono differire in implementazione, strutture dati etc…

Implementare un linguaggio $L$ significa realizzare una macchina astratta che può eseguire programmi scritti in $L$ stesso. L’implementazione può essere fatta a livello hardware, software o firmware.

## Implementazione di una Macchina Astratta attraverso il Software

L’implementazione di una $M_L$ tramite software esegue programmi scritti in $L$. Il software gira su una **Macchina Host** $MO_{LO}$ che esegue il linguaggio $LO$.

Ora, abbiamo due approcci per eseguire il linguaggio $L$:

1. **Interpretato**: Un programma scritto in $LO$ comprende ed esegue il linguaggio $L$.
    - Implementa il ciclo fetch/decode/load/exec/save
2. **Compilato**: Un programma traduce il programma in questione da $L$ a $LO$.
3. **Ibrido**: Il compilatore compila il programma in una *lingua intermedia* $LI$, poi viene eseguito da un programma che gira su $MO_{LO}$.

### Implementazione Interpretativa

![Untitled](Macchine%20Astratte%2058a99dc45481448c9a26c344c9994cc7/Untitled%202.png)

Interpretative Implementation

Interprete traduce da *L* a *LO* istruzione per istruzione.

### Implementazione Compilata

![Untitled](Macchine%20Astratte%2058a99dc45481448c9a26c344c9994cc7/Untitled%203.png)

Compiled Implementation

Il compilatore traduce l’intero programma da $L$ a $LO$ prima dell’esecuzione. Il compilatore può anche non essere scritto in $LO$ e può essere eseguito su una macchina astratta differente da $MO_{LO}$.

### Implementazione Ibrida

![Untitled](Macchine%20Astratte%2058a99dc45481448c9a26c344c9994cc7/Untitled%204.png)

Hybrid Implementation

Programma in $L$ → compilato in lingua intermedia $LI$ → interpretato da un programma per $MO_{LO}$.

Esempi di implementazione ibrida sono:

- **Java**: compila in bytecode ed esegue in una JVM.
- **C**: Il compilatore crea del codice che poi deve essere linkato.

In base alle differenze e somiglianze tra $LI$ e $LO$ si parla dell’implementazione come *mainly compilative* oppure *mainly interpreted*.

Ad esempio in **Java** $LI$ è il bytecode che è molto diverso dal linguaggio macchina, dunque l’implementazione del linguaggio è *mainly interpreted*, sebbene alcune JVM siano basate su compilazione.

In **C** invece $LI$ è più o meno già il linguaggio macchina, dunque l’implementazione è *mainly compilative*, sebbene non identico al linguaggio macchina poichè contiene system calls, funzioni libreria etc… 

**Un’eccezione:** Sono i “freestanding” compilers, usati per compilare il kernel.

### Compilers vs. interpreters

Which is better?

- **Purely interpretive**
    - Implementation of *ML* is less efficient
    - But it is more flexible and portable
    - Debugging is simpler, with interpretation at run-time
- **Purely compliative**
    - Implementation of *ML* is more efficient
    - But also more complex, due to the “distance” between *L* and *LI* or *LO*
    - Debugging is usually harder

### Practical examples

It is not always clear whether a language is usually compiled or interpreted.
Some examples:

- **Typically compiled:**
    - C, C++, FORTRAN, Pascal, ADA
- **Typically interpreted:**
    - ML, Perl, Postscript, Prolog, Smalltalk
- **Less clear:** Java, LISP

# Implementazione di una Macchina Astratta attraverso il Firmware

Abbiamo visto come si implementa una macchina astratta in software e come la CPU sia un’implementazione hardware.
Esistono anche delle vie intermedie di implementazione, come le**CPU Microprogrammabili:**

- La macchina astratta che esegue il linguaggio macchina non è implementata a livello hardware ma attraverso un **microinterprete**
- Il fetch/decode/load/execute/save cycle è implementato attraverso *microistruzioni* invisibili all’utente normale.
- Le strutture dati e gli algroitmi della macchina astratta sono realizzate come *microprogrammi*.

Questa implementazione è *più flessibilie* di una classica implementazione hardware.

# Linking and Loading

Il compilatore non genera un codice completamente eseguibile, le locazioni di memoria sono relative e non assolute. Il linker/loader rimpiazza queste locazioni con indirizzi assoluti. Questo permette la compilazione separata di parti del programma e l’uso di librerie precompilate (che potrebbero anche essere scritte in un altro linguaggio).

# Definizioni Formali

Si può intendere un programma come una funzione.

Un programma scritto il linguaggio $L$ può essere definito come:

$$ ⁍$$

$P^L(i) = o$ dove:

- $i ∈ D$ è l’input.
- $o ∈ D$ è l’output.

Interpreti e compilatori sono anch’essi programmi che prendono come input altri programmi.

## Definizione di un interprete

Definiamo, per esempio, un interprete di un linguaggio $L$ scritto in linguaggio $LO$ il quale implementa una *macchina astratta* $M_L$ su una *macchina astratta* $MO_{LO}$.

![Schermata 2021-08-22 alle 7.06.58 PM.png](Macchine%20Astratte%2058a99dc45481448c9a26c344c9994cc7/Schermata_2021-08-22_alle_7.06.58_PM.png)

Questo interprete è definito come:

$$𝐼^L_{LO}:(𝑃𝑅^𝐿×𝐷)→𝐷$$

$I^L_{LO}$ scritto in $LO$ per $L$ prende come input tutti i $(PR^L × D)$ dove $PR^L$ è **l’insieme** di tutti i programmi $P^L$ scritti in $L$, e $D$ sono i Dati.

**NOTA:** $I^L_{LO}$ Gira su una macchina ****$MO_{LO}$

Dunque:

$$\forall i ∈ D, I^L_{LO}(P^L, i) = P^L(i)$$

Per ogni input $i$ l’interprete applicato su $P^L$ e $i$, ritorna lo stesso risultato di $P^L$ applicato su $i$.

$P^L(i)$ è calcolato su una macchina astratta $M_L$, mentre $I^L_{LO}(P^L, i)$ è calcolato dalla macchina astratta $MO_{LO}$.

## Definizione di un Compilatore

Un compilatore da linguaggio $L$ a linguaggio $LO$, scritto in linguaggio $LA$, trasforma un programma da $P^L ∈ PR^L$ a $P^{LO} ∈ PR^{LO}$.

Il compilatore implementa $M_L$ su $MO_{LO}$.

Questo compilatore è definito come:

$$C^{LA}_{L,LO} : PR^L → PR^{LO}$$

Quindi:

$$\forall i \in D \quad P^{LO}_C = C_{L,LO}^{LA} (P^L) \implies P^{LO}_C (i) = P^L(i)$$

- Per ogni $i$ il programma compilato applicato a $i$ da lo stesso risultato di $P_L$ applicato a $i$
- $P^L(i)$ è calcolato dalla macchina astratta $M_L$
- $P^{LO}_C(i)$ è calcolato dalla macchina astratta $MO_{LO}$
- Il compilatore è calcolato da $MA_{LA}$
