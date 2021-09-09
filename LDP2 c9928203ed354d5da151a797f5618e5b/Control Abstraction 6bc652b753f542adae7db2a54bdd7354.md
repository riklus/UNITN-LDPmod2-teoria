# Control Abstraction

Il Control Abstraction in riferimento a un linguaggio di programmazione permette al programmatore di definire i propri Control Flow Constructs.

Poter definire i propri Costrutti di Control Flow da al programmatore l'opportunità di creare interfacce proprie, incapsulando codice che sarebbe altrimenti ripetitivo da scrivere ogni volta.

Oltre ad evitare la ripetizione il programmatore fornisce all'utente, attraverso la creazione di nuovi costrutti, interfacce pulite e concise al fine di permettere la scrittura di programmi senza essere necessariamente a conoscenza dei dettagli implementativi delle interfacce impiegate.

Questo risulta in chiarezza semantica e riduzione del cosiddetto "boilerplate code" nel programma finale.

**Argomenti:**

- Abstraction of control
- Metodi di passaggio di parametri
- Higher-order functions
    - Funzioni come parametri
    - Funzioni come risultati
- Exception handling: 
Quando vi è un errore il controllo viene trasferito all'error handler

---

**Indice:**

# Astrazione

Con astrazione, in computer science, ci riferiamo principalmente a due punti:

- Identificazione delle proprietà più importanti della cosa che vogliamo descrivere
- Concentrazione sulle domande più rilevanti ignorando quelle marginali

Cosa noi riterremo più importante e più rilevante dipenderà dalla natura della cosa che vogliamo descrivere e dall'ambito del progetto.

Al fine di implementare un'astrazione abbiamo la necessità di definire e utilizzare i vari costrutti. Generalizzando, un costrutto $P$ viene sempre gestito in questo modo:

- Dichiarazione di $P$
- Definizione di $P$
- Utilizzo di $P$

# Parametri di funzione

Una funzione è dichiarata e definita nel seguente modo:

```c
int f(int n) {
	return n+1;
}
```

Viene utilizzata in questo modo:

```c
int x = f(y+3);
```

In questi due esempi:

- `n` è il **Parametro Formale (Formal parameter)**.
- `y+3` è il **Parametro Effettivo (Actual parameter),** cioè quello che effettivamente gli viene passato.

# Comunicazione tra funzione e il chiamante

Una funzione comunica con il chiamante attraverso:

- Il valore **ritornato (**Ex: `return n+1`)
- Passaggio di valori tramite parametri da/a `main` e `proc`.

# Metodi passaggio di parametri

Un chiamante può passare parametri a una funzione principalmente in due modi:

**Per valore (value):**

Il *valore passato* è quello assegnato al **Parametro formale** ed è trattato come una **variabile locale.**

Il dato è trasmesso da `main` a `proc` e modifiche al **Parametro formale** non hanno nessun effetto sul **Actual parameter**.

Ha una semantica semplice

L'implementazione è abbastanza semplice

È necessario un altro meccanismo per comunicare con la procedura chiamata

**Per reference (o variable):**

Il *valore passato* è una reference (o address che punta) all'**Actual parameter.** Referenze al **Parametro formale** sono in realtà referenze al **Actual parameter.** 

Il dato è trasmesso da `main` a `proc` e modifiche al **Parametro formale** hanno effetto sul **Actual parameter**.

Ha una semantica complessa: *Aliasing*

L'implementazione è abbastanza semplice

La chiamata è efficiente

Ve ne sono anche altri che tratteremo di seguito.

## Chiamata per valore (Call by value)

Prendiamo un esempio pratico:

```c
void foo(int x) {
	x = x+1;
}
y = 1;
foo(y+1);
```

In questo caso vediamo che:

- `x` è un **Formal parameter** ed è anche una **variabile locale** situata sullo **Stack** (posizionata nell'ARI della funzione).

**Inoltre:**

Durante la chiamata `y+1` viene evaluated, e il suo valore è assegnato al **Formal parameter** `x`.

In uscita (**exit**) da `foo`, `x` viene distrutta (Rimossa dallo Stack)

Questo tipo di chiamata è molto costosa in termine di risorse per grandi quantitativi di dati, poiché devono essere copiati ogni qualvolta la funzione viene chiamata.

Non vi è alcun collegamento tra `x` nel corpo di `foo` e `y`, che è l'**Actual parameter.** Dunque non è possibile trasferire alcun dato da `foo` a `main` tramite il parametro.

- Linguaggi che impiegano questo tipo di chiamata

    Java, Scheme, Pascal (default) e C.

**TLDR:**

Non Sussiste alcuna **two-way** communication tra `foo` e il chiamante.

## Chiamata per nome (Call by name o reference)

Prendiamo un esempio simile a quello sopra:

```c
void foo(reference int x) {
	x = x+1;
}
y = 1;
foo(y);
```

In questo caso vediamo che:

- Una **reference** (che sia address, pointer) viene passata

Questo significa che `x` è un'**alias** di `y`, mentre l'**Actual parameter** è una $l$-value (una **locator value**) ovvero un valore che si riferisce a una locazione di memoria.

In uscita (**exit**) da `foo`, il link che sussiste tra `x` e l'indirizzo di `y` viene distrutto.

Questo tipo di chiamata è efficiente ma la funzione presenta una indirection nel body.

- Linguaggi che impiegano questo tipo di chiamata

    Pascal (var), C pointers.

**TL/DR:**

Sussiste una **two-way** communication tra `foo` e il chiamante.

**Un piccolo esempio:**

```c
int y;
void fie (name int x) {
	int y;
	x = x + 1;
	y = 0;
}
y = 1;
fie(y); // y = 2
```

---

Storicamente per implementare questo tipo di chiamata, la procedura veniva trattata come una sorta di macro. Successivamente veniva espansa in modo semanticamente corretto. Questo perché:

Una chiamata alla procedura $P$ è come eseguire il corpo di $P$ dopo aver sostituito i Formal parameters con gli Actual parameters.

### Implementazione tecnica

Durante una chiamata una coppia di `<exp, env>` viene passata dove:

- `exp` è l'Actual parameter, non evaluated
- `env` è l'evaluation dell'environment (static scoping)

A ogni chiamata `exp` viene valutato in `env`.

Questo è però più costoso a livello di risorse poiché l'intero environment deve essere passato.

Per passare la coppia `exp, env>` dobbiamo avere:

- Un puntatore al text di `exp` (con text si intende il codice della funzione)
- Un puntatore alla Static Chain (che si trova nella Call Stack) che appartiene alla ARI (Activation Record Instance) del blocco chiamante.

Questo ci permette anche di passare funzioni come argomenti ad altre procedure.

## Chiamata Read-Only

La **Call by value** è troppo costosa in termini di prestazioni: 
Anche se il valore passato come parametro non viene modificato all'interno di una funzione, esso verrà comunque copiato.

Nelle chiamate **Read-only** questo non avviene:

- Le procedure non hanno il permesso di cambiare il valore del **Formal parameter** (Es: Attraverso un controllo statico del compilatore)
- E a discrezione del compilatore i valori sono passati per valore o per reference read-only (Es: "grandi" valori vengono passati per reference mentre "piccoli" valori per value)
- Linguaggi che impiegano questo tipo di chiamata

    Java attraverso il modificatore `final`

    ```java
    void foo(final int x) { /* code */ }
    ```

    Significa che `x` non può essere modificato.

**TL/DR:**

Non Sussiste alcuna **two-way** communication tra `foo` e il chiamante.

Sebbene l'**actual parameter** sia in realtà una reference read-only.

## Chiamata per valore-risultato (Call by value-result)

Esempio pratico:

```c
void foo(result int x) {
	x = 8;
}
y = 1;
foo(y);
```

**A fine esecuzione:** `y` è 8.

`x` è una variabile locale sullo Stack, come nelle Call by value.

Ma quando `foo` termina, il valore di `x` viene assegnato a `y`.

Vi è una sottile differenza tra la Call by result e la Call by name: 

**La Call by result** copia il valore del parametro effettivo nel parametro attuale ma `y`, ovvero il parametro effettivo, prende il valore di `x` solo dopo il termine dell'esecuzione della funzione.

**La Call by name** invece passa una reference come parametro attuale, ovvero una  $l$-value.

- Linguaggi che impiegano questo tipo di chiamata

    Ada: `out`

**TL/DR:**

Non Sussiste alcuna **two-way** communication tra `foo` e il chiamante.

Sebbene l'**actual parameter** prenda il valore del **formal parameter** a fine esecuzione della funzione.

# Higher-order functions

Alcuni linguaggi permettono alle procedure di:

- Ricevere **funzioni** **come argomenti.**
- Ritornare **funzioni** **come** **risultati**.

**Implementazione tecnica:**

Per gestire l'environment delle funzioni passate come **argomenti** possiamo utilizzare un puntatore (che punta) alla ARI posizionata nello Stack.

Mentre per gestire l'environment di funzioni passate come risultati dobbiamo salvare, non nello Stack, la ARI della funzione risultante.

## Funzione come parametro di una procedura

Un piccolo esempio pratico:

```c
int x = 4; int z = 0;  // one

int f(int y) {
	return x*y;
}

void g(int h(int n)) {
	int x;  // two
	x = 7;
	z = h(3) + x;
}

void main() {
	int x = 5;  // three
	g(f);
}
```

Abbiamo 3 dichiarazioni di `x`.

**La vera domanda è:** Quando `f` viene chiamata nella funzione `g`, con il nome di `h`, quale `x` viene usata?

- Nel caso di **Static Scoping** sarebbe stata utilizzata la `//one`.
- Ma nel caso di **Dynamic Scoping**?

    Nel caso di Dynamic Scoping non siamo sicuri di quale delle due `x` (`//two` or `//three`)  verranno utilizzate.

    Potrebbe utilizzare la `//three`, durante il passaggio di parametro la funzione `f` riceve il binding dal contesto di chiamata.

    Oppure potrebbe utilizzare la `//two`, dove la funzione `f` effettua il binding di `x` all'interno del body della funzione `g`.

    Dipendono dalla tipologia di Binding (di `f`) impiegata: **Deep Binding** o **Shallow Binding**.

# Regole di Binding

Quando una procedura `f` viene passata come parametro, viene creato un link tra un nome (formal `h`) e la procedura stessa (actual `f`).

Ma quale Environment non-locale viene applicato quando `f` viene eseguita ovvero chiamata via `h`?

- L'Environment al momento della creazione del link [passaggio come argomento] (**Deep Binding**):
    - Sempre applicato quando il linguaggio impiega lo **Static Scoping. (???)**
    - Talvolta applicato quando il linguaggio impiega il **Dynamic Scoping.**
- L'Environment al momento della chiamata (**Shallow Binding**):
    - Talvolta applicato quando il linguaggio impiega il **Dynamic Scoping.**

## Esempio di Deep vs Shallow binding

```c
int x = 1;    // external

int f(int y) {
	return x+y;
}

void g(int h(int b)) { //internal block
	int x = 2;
	return h(3) + x;     // A
}

{ // calling block
	int x = 4;
	int z = g(f);
}
```

Nel punto `//A` `h(3)` chiama `f`: Quale `x` viene acceduto?

**Static Scoping:**

- Deep Binding: `x = 1` `//external`
- Shallow Binding: `x = 1` `//external`

**Dynamic Scoping:**

- Deep Binding: `x = 4` `//calling block`
- Shallow Binding: `x = 2` `//internal block`

# Closure

Una **closure** è un'astrazione che combina la **funzione** e le **referenze a contesti non-locali** (Es. Il contesto di dichiarazione della closure).

Il contesto delle funzioni si determina in due modi:

Nel caso di **Static Scoping** il contesto della funzione coincide con quello di **definizione**. Si conosce a **priori**.

Nel caso di **Dynamic Scoping** il contesto della funzione è il contesto del **chiamante.** Si stabilisce a **posteriori**.

le condizioni per creare una closure derivano dalla possibilità delle procedure di passare o restituire funzioni.

Questo poiché se passo una funzione in un altro contesto e provo a chiamarla, essa deve poter determinare il proprio contesto e lo può fare in diversi modi:

- Lo determina a **priori**, durante la definizione. (No difference)
- Lo determina a **posteriori** in due occasioni:
    - Durante il **passaggio** ad argomento, o ritorno di funzione. *(Deep Binding)*
    - Durante la **chiamata**. *(Shallow Binding)*

La determinazione a **priori** è tipica dello Static Scoping.

La determinazione a **posteriori** invece è tipica del Dynamic Binding.

La determinazione a **posteriori durante il passaggio** è tipica del Deep Binding.

La determinazione a **posteriori durante la chiamata** è tipica dello Shallow Binding.

Dunque la determinazioni a **priori** e quella a **posteriori durante passaggio**, consentono l'incapsulamento (en-closure) di referenze a contesti esterni a quelli di chiamata. 

Nasce dunque la **closure** che permette di "congelare" un contesto e rendendolo accessibile attraverso la funzione di closure stessa.

**NB:** L'implementazione Static Scoping è sempre Deep Binding.

**NOTA:** 

In realtà ho mentito. Lo Static scoping non è sempre non ambiguo esistono delle eccezioni:

Ad esempio nelle funzioni ricorsive applicare le regole di Deep o Shallow binding cambia l'output del programma.

## Esempi di closure del tipo a priori

```c
int x = 1;

int f(int y) {
	return x + y;
}

int g(int h(int b)) {
	int x = 2;
	return h(3)+x;
}

{
	int x = 4;
	int z = g(f)
}
```

![Schermata 2021-08-27 alle 6.12.42 PM.png](Control%20Abstraction%206bc652b753f542adae7db2a54bdd7354/Schermata_2021-08-27_alle_6.12.42_PM.png)

**Notiamo:**

Sia il link al codice di `f` che il contesto non-locale sono passati alla funzione `g`.

La procedura quando viene passata come parametro alloca una ARI che prende il puntatore che punta alla Static Chain della closure.

## Riassumendo

Una closure mantiene i puntatori nello static environment del corpo di una funzione, quando chiamata il puntatore alla Static chain viene determinata attraverso la closure stessa.

Tutti i puntatori della Static chain puntano sempre alla Call stack, in questo modo è possibile jump over le ARI e accedere a variabili non locali.

**Deep vs. shallow binding**

*Dynamic scope*

- Possible with deep binding
    - Implementation with closure
- Or shallow binding
    - No special implementation needed

*Static scope*

- Always uses deep binding
    - Implemented with closure
- At first glance no difference between deep and shallow binding
    - The static scoping rules determine which nonlocal value to
    use
- That is not the case: There may be dynamically several instances of a block that declare a non-local name (for example with recursion)

**Implementazione in caso di Dynamic Scoping**

- Shallow Binding
    - Non c'è bisogno di fare nulla di speciale
        - Per accedere a una certa variabile `x` si una lo Stack
        - Si possono sfruttare le strutture standard come $A$-List o CRT
- Deep Binding
    - Bisogna sfruttare una forma di incapsulamento o closure per "congelare" lo scope così da poter essere attivato in futuro

## Ritornare una funzione

### Caso semplice

```c
int x = 1;

void->int F() {
	int g() {
		return x+1;
	}
	return g;
}

void->int gg = F();
int z = gg();
```

Nel caso semplice `F` ritorna una closure.

Modifica della macchina astratta per gestire la "chiamata da closure" come in `gg()`.

### Caso complesso

```c
void->int F() {
	int x = 1;
	int g() {
		return x+1;
	}
	return g;
}

void->int gg = F();
int z = gg();
```

![Schermata 2021-08-29 alle 1.15.53 PM.png](Control%20Abstraction%206bc652b753f542adae7db2a54bdd7354/Schermata_2021-08-29_alle_1.15.53_PM.png)

In questo caso `F` ritorna una closure e l'associazione per `x`, secondo quello che sappiamo, dopo la terminazione di `F` viene distrutta.

Come gestire la cosa?

Se volessimo mantenere la ARI di `F` perderemmo la proprietà FIFO della Call Stack.

Dunque come implementiamo uno stack in questo caso, senza che la ARI delle funzioni ritornate restino in memoria per sempre?

Implementiamo uno stack particolare

- Senza deallocazione automatica
- Con l'activation record salvati sulla heap
- Con Chain statiche e dinamiche che connettono al record
- Si chiama il garbage collector quando necessario

# Exception handling

In caso di **exception,** ovvero quando a Run time il codice presenta degli errori di esecuzione, bisogna "handle" gestire questo errore.

Per gestire l'errore, solitamente, il controllo viene passato a un'altra sezione di codice che a seconda dell'errore incontrato si comporta nel modo programmato.

Esistono due tecniche implementative utilizzate per gestire gli errori e sono:

- Structured exit
- Exception propagation

## Structured exit

Conclude una parte di computazione.

1. Esce da una costrutto
2. Passa i dati per mezzo di un salto
3. Ritorna il controllo al punto di attivazione più recente
    1. Le ARI che non sono più necessarie vengono deallocate assieme ad altre risorse nella heap

**Solitamente consiste di due costrutti:**

Dichiarazione di un Exception manager: Posizione e codice

Comandi to raise exception

**Esempio:**

```java
class EmptyExcp extends Throwable {
	int x = 0;
}

int average(int[] v) throws EmptyExcp() {
	if (length(v) == 0) throw new EmptyExcp();
	else {
		int s = 0;
		for (int i = 0, i<length(v), i++) {
			s = s+v[i];
		}
		return s / length(v);
	}
}

int[] w = {};
try {
	average(w);
} catch (EmptyExcp e) {
	write("Empty array");
}
```

In questo esempio:

- `average` calcola la media di un vettore `v`, se il vettore è vuoto raises un'Exception chiamata `EmptyExcp`
- L'exception handler è in un blocco statico di codice protetto

## Exception propagation

Se l'eccezione non viene gestita dalla procedura corrente:

La procedura termina e l'eccezione viene raised di nuovo al punto di chiamata (Calling Point).

Se il chiamante non gestisce l'eccezione, il chiamante del chiamante raises l'exception di nuovo etc..

Se nessuno gestirà l'eccezione, ovvero non viene incontrato alcun exception handler, una volta raggiunto il top level questo provvederà a fornire un Default Handler

Una volta incontrata un'eccezione i frame corrispondenti all'eccezione vengono rimossi dallo stack, ripristinando i valori dei registri.

Ogni routine possiede un Exception Handler "nascosto", che ripristina lo stato e propaga l'eccezione attraverso lo stack.

### Le eccezioni vengono propagate attraverso la Dynamic Chain

![Schermata 2021-08-29 alle 2.24.41 PM.png](Control%20Abstraction%206bc652b753f542adae7db2a54bdd7354/Schermata_2021-08-29_alle_2.24.41_PM.png)

![Schermata 2021-08-29 alle 2.23.32 PM.png](Control%20Abstraction%206bc652b753f542adae7db2a54bdd7354/Schermata_2021-08-29_alle_2.23.32_PM.png)

Quando l'argomento di `g` è il risultato dell'esecuzione, l'handler non è determinato staticamente.

## Implemetazione teorica delle eccezioni

**Modo semplice:**

All'inizio di un blocco protetto, si inserisce sullo stack del chiamante

Quando un'eccezione viene sollevata:

- Rimuovi il chiamante dallo stack e controlla se è quello giusto
- Se no, raise a new exception and repeat

Il modo semplice è inefficiente quando non si trova l'handler. Ogni tentativo deve inserire ed eliminare oggetti dallo stack

**Una soluzione migliore:**

Table of addresses