# Memory Managment

In questo capitolo ci occuperemo della Gestione della Memoria.

Ogni qualvolta un oggetto viene creato è necessario che il calcolatore allochi dello spazio nella memoria, riservato a quell'oggetto.

Principalmente esistono due modi di allocare la memoria:

- **Static:** Memoria allocata a Compile Time
- **Dynamic:** Memoria allocata a Run Time
    - *Stack:* Oggetti allocati LIFO
    - *Heap:* Oggetti allocati e deallocati arbitrariamente

---

**Indice:**

# Allocazione Statica

Nell'allocazione statica un oggetto ha un indirizzo assoluto ed è mantenuto per tutta l’esecuzione del programma.

![Schermata 2021-08-22 alle 11.06.34 PM.png](Memory%20Managment%20d066870ddc664537ad64c09f8dc22ded/Schermata_2021-08-22_alle_11.06.34_PM.png)

Static allocation for subprograms

Le seguenti sono tipicamente allocate staticamente:

- Variabili Globali
- Variabili Locali di subprogrammi, in caso di assenza di ricorsione
- Costanti determinate a Run Time
- Tabelle usate durante il Run Time (Per il Type Checking, Garbage Collection etc…)

In caso di Allocazione Statica il compilatore sa già quanta memoria il programma andrà ad occupare.

**NOTA:**

**L’Allocazione Statica non permette ricorsione**

In alcune implementazioni dello Stack, non era concesso l'utilizzo di uno Stack che cresce dinamicamente. Questo impediva l'implementazione di determinate tecniche di programmazione.

Ad esempio vecchie versioni di Fortran non permettono Subroutines.
Infatti Fortran permetteva al massimo un unica Subroutine attiva in un dato momento.

# Allocazione Dinamica On Stack

**La** **Call Stack** si occupa di immagazzinare informazioni riguardo le varie subroutines attive in un programma. 

Per fare ciò la Call Stack memorizza il punto di ritorno delle subroutines, affinché qualvolta la subroutine ritornerà il programma sarà a conoscenza del punto di ritorno.

**L’implementazione di uno Stack utilizza:**

1. Sequenza di chiamata
2. Prologo
3. Epilogo
4. Sequenza di ritorno

# Activation Record

**L’Activation Record**, detto anche *Stack Frame,* è una struttura dati immagazzinata nella Call Stack che si occupa di tenere traccia delle informazioni necessarie a una subroutine come variabili locali, indirizzi di ritorno etc...

**NOTA:** L’indirizzo dell’Activation Record non è saputo a Compile Time.

Le informazioni contenute nell’Activation Record sono accessibili per via di un *offset*:

- I dati salvati hanno un indirizzo risultato dalla somma del Record Address (Stack Pointer) e il suo offset
- L’offset è determinato *staticamente*
- La somma è computata con una singola istruzione macchina (load o store)

**L’Activation Record Pointer**, detto anche *Stack Pointer,* punta all’Activation Record (Stack Frame) del blocco attivo.

## Negli In-line Block

Il **Dynamic Link** o Control Link, è un puntatore che punta all’Activation Record passato/precedente.

*Cosa succede quando viene aperto un blocco anonimo?*

In entrata del blocco avviene una *Push*

- Un nuovo Dynamic Link viene creato nel nuovo Record
- L’Activation Record Pointer viene settato a puntare al nuovo Record

In uscita dal blocco avviene una *Pop*

- Imposta l’Activation Record Pointer (Stack Poiner) a puntare il record precedente

**Esempio:**

```c
int x = 0;    
int y = x + 1;    
{       
	int z = (x+y)*(x-y);
}

// Record A    
[Link_Dinamico]    
x = 0;    
y = 1;

// Record B    
[Link_Dinamico] -> Punta al link dinamico precedente    
z = -1;    
x + y = 1;    
x - y = -1;

// Durante l'esecuzione l'Activation Record Pointer punta al Record Attivo
[Activation Record/SP] -> Punta al Record B
```

![Schermata 2021-08-22 alle 4.55.13 PM.png](Memory%20Managment%20d066870ddc664537ad64c09f8dc22ded/Schermata_2021-08-22_alle_4.55.13_PM.png)

### In pratica

In molti linguaggi i blocchi anonimi possono essere implementati senza uno stack. Il compilatore analizza tutti i blocchi annidati, alloca spazio per tutte le variabili. 

Sebbene questo consumi più memoria ne aumenta l’efficienza poiché non è necessario gestire lo stack.

## Nelle procedure

![Schermata 2021-08-22 alle 5.54.39 PM.png](Memory%20Managment%20d066870ddc664537ad64c09f8dc22ded/Schermata_2021-08-22_alle_5.54.39_PM.png)

*Come è costituito un Activation Record di Procedura*

**Esempio:**

![Schermata 2021-08-22 alle 5.58.09 PM.png](Memory%20Managment%20d066870ddc664537ad64c09f8dc22ded/Schermata_2021-08-22_alle_5.58.09_PM.png)

```c
int fact(int n) {        
	if (n <= 1) return 1;        
	else return n * fact(n-1);    
}
```

- **Variabili Locali:** Non presenti in questo esempio
- **Risultati Intermedi:** Spazio per tenere i valori di `fact(n-1)`
- **Indirizzo di Ritorno:** Punta dove deve ritornare nel codice
- **Indirizzo per il Risultato:** L’indirizzo della locazione di memoria dove il valore finale di `fact(n)` deve essere piazzato (Nell’Activation Record della chiamata)
- **Parametri:** Corrispondono al valore di `n` nelle sequenze di chiamata.

![Schermata 2021-08-22 alle 6.01.35 PM.png](Memory%20Managment%20d066870ddc664537ad64c09f8dc22ded/Schermata_2021-08-22_alle_6.01.35_PM.png)

# Gestione dello Stack

![Schermata 2021-08-22 alle 6.04.23 PM.png](Memory%20Managment%20d066870ddc664537ad64c09f8dc22ded/Schermata_2021-08-22_alle_6.04.23_PM.png)

# Allocazione Dinamica On Heap

L’Heap è la regione di memoria dove i blocchi (e sotto blocchi) possono essere allocati e deallocati arbitrariamente.

Questo è necessario quando il linguaggio permette:

- Allocazione esplicita di memoria a runtime
- Oggetti di dimensioni variabili
- Oggetti la cui Lifetime non è compatibile con una struttura LIFO

La gestione dell’heap è importante, serve allocare efficientemente lo spazio per evitare la *frammentazione*.

# Algoritmi per la Gestione della Heap

Dal momento che l’indirizzo preciso delle allocazioni non è saputo a priori, la memoria è accessibile indirettamente tramite *Pointer Reference*.

L’algoritmo specifico utilizzato per gestire l’area di memoria e per allocare/deallocare i vari chunk è legato al Kernel e può sfruttare i seguenti metodi:

## Heap a blocchi di Dimensione Fissa

L’Heap è diviso in blocchi di grandezza fissa (stessa dimensione). All’inizio questi blocchi sono tutti linkati a formare una *free list*.

L’Allocazione permette la memorizzazione in uno o più blocchi contigui. La Deallocazioni permette il ripristino dei blocchi per riformare la *free list*.

![Schermata 2021-08-22 alle 6.20.15 PM.png](Memory%20Managment%20d066870ddc664537ad64c09f8dc22ded/Schermata_2021-08-22_alle_6.20.15_PM.png)

![Schermata 2021-08-22 alle 6.20.28 PM.png](Memory%20Managment%20d066870ddc664537ad64c09f8dc22ded/Schermata_2021-08-22_alle_6.20.28_PM.png)

**Problemi:**

Le operazioni devono essere efficienti ed è necessario evitare spreco di memoria. Due problemi che spesso risultano con questa specifica gestione della Heap sono: 

- Internal Fragmentation
- External Fragmentation

## Heap a blocchi di Dimensione Variabile

L’Heap contiene inizialmente un singolo blocco, l’Allocazione trova un blocco della grandezza appropriata e la Deallocazione permette al blocco di re-linkarsi alla *free list*.

## Problemi nella Gestione

Le operazioni devono essere efficienti e bisogna limitare lo spreco di memoria. Esistono due problemi di *frammentazione* della memoria:

- Internal Fragmentation
- External Fragmentation

### Internal Fragmentation

Immaginiamo che dobbiamo allocare spazio per un blocco di dati grande $X$

- Un blocco $Y > X$ viene allocato

Ne risulta uno spreco di spazio di dimensione $Y − X$.

### External Fragmentation

Lo spazio è disponibile ma non utilizzabile poichè “spezzettato” in blocchi troppo piccoli.

Immaginiamo di dover allocare $X$ ma tutti i blocchi liberi sono solo ${Y : ∀Y, X > Y}$

![Schermata 2021-08-22 alle 6.25.09 PM.png](Memory%20Managment%20d066870ddc664537ad64c09f8dc22ded/Schermata_2021-08-22_alle_6.25.09_PM.png)

# Altri algoritmi

## Single Free List

La Free List è singola:

All’inizio la Heap è formata da un solo blocco, la cui grandezza coincide con quella della Heap.

Ad ogni richiesta di allocazione viene presa una decisione:

- *First Fit:* Viene scelto un blocco libero abbastanza grande per soddisfare la richiesta.
- *Best Fit:* Viene scelto il **più piccolo** blocco libero abbastanza grande per soddisfare la richiesta.

Se viene scelto un blocco troppo grande questo viene *diviso* in due e quello libero viene aggiunto alla *Free List*.

**Meglio Best o First Fit?**
La First Fit è più rapida ma utilizza male la memoria, mentre la Best Fit è più lenta ma consente un utilizzo più saggio della memoria.

## Multiple Free List

Con una singola lista l’allocazione è *linerare* al numero di blocchi liberi, mentre con *Multiple Lists* si può migliorare l’effecienza.
Nell’implementazione a *Multiple Lists* la divisione dei blocchi può essere di due tipi:

- **Statica:** Cioè che la divisione dei blocchi non varia nel tempo
- **Dinamica:** Che invece varia, vediamo due esempi
    - *Buddy System*
    - *Fibbonacci:* Usa i numeri di Fibbonacci.

### Buddy System

L’implementazione *Buddy System* è composta da $k$ liste, ogni lista ha blocchi di grandezza $2k$.
Nel caso viene richiesto uno spazio $2k$ che non è disponibile allora viene allocato un blocco di grandezza $2k + 1$ che viene diviso in due. Se un blocco di grandezza $2k$ viene deallocato allora l’algoritmo cerca di riunirlo con la sua metà (buddy) se questa è libera.

**If memory is to be allocated**

1. Look for a memory slot of a suitable size (the minimal 2 block that is larger or equal to that of the requested memory)
    1. If it is found, it is allocated to the program
    2. If not, it tries to make a suitable memory slot. The system does so by trying the following:
        - Split a free memory slot larger than the requested memory size into half
        - If the lower limit is reached, then allocate that amount of memory
        - Go back to step 1 (look for a memory slot of a suitable size)
        - Repeat this process until a suitable memory slot is found

**If memory is to be freed**

1. Free the block of memory
2. Look at the neighboring block – is it free too?
3. If it is, combine the two, and go back to step 2 and repeat this process until either the upper limit is reached (all memory is freed), or until a non-free neighbour block is encountered

(Source Wikipedia)