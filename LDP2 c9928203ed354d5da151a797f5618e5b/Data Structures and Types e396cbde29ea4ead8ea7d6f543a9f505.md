# Data Structures and Types

Le **strutture dati** sono organizzazione, gestione, archiviazione e accesso di dati creati per adattarsi al meglio al progetto o programma in questione.

I **tipi** sono attributi di una dato o di una collezione di dati che indicano al compiler o all'interprete come il programmatore intende usare gli stessi.

**Indice:**

# Data types

I **tipi** sono attributi di una dato o di una collezione di dati che indicano al compiler o all'interprete come il programmatore intende usare gli stessi.

Realmente cosa sia un tipo di dato dipende dal linguaggio di programmazione

## Come vengono usati di tipi?

I tipi significano e vengono usati nei seguenti modi:

**A livello di progetto:** Organizzazione dell'informazione

- Diversi tipi sono impiegati per diversi concetti
- Spesso vengono usati come commenti per l'uso che si intende fare dei dati

**A livello di programma:** Identificare e evitare errori

- I tipi, al contrario dei commenti, possono essere automaticamente verificati
- Ad esempio `3 + "pippo"` da errore in certi linguaggi

**A livello implementativo:** Permette certe ottimizzazioni

- Un `boolean` necessita di meno bit di un `real`
- Utilizzati per precalcolare l'offset di un record/struct

## Type systems

Il **type system** di un linguaggio determina i tipi predefiniti, i meccanismi per definire nuovi tipi, meccanismi di controllo sui tipi e specifica se i tipi sono static o dynamic.

Con meccanismi di controllo su tipi si intende:

- Come gestire l'equivalenza
- Come gestire la compatibilità
- E l'inferenza

Un **type system** è definito *type safe* se a Run time non abbiamo errori non rilevabili che derivino da errori di tipo.

## Scalar types

I tipi scalari rappresentano un singolo valore preciso, ovvero non sono aggregati di altri valori.

Hanno una rappresentazione diretta nell'implementazione.

1. **Reals**
    1. **Val:** numeri razionali contenuti in un certo intervallo
    2. **Op:** +, -, *, div, etc...
    3. **Repr:** Several Bytes (4), mobile decimal point
    4. **NOTA:** Long reals (anche di 8 bytes), possono avere problemi di portabilità, quando non specificato dal linguaggio
2. **Complex Numbers**
    1. **Val:**
    2. **Op:**
    3. **Repr:** 2 reals
    4. **NOTE:** Usato in Scheme e Ada
3. **Fixed point**
    1. **Val:** numeri razionali contenuti in un certo intervallo
    2. **Op:** +, -, *, div, etc...
    3. **Repr:** Several Bytes (2 or 4)
    4. **NOTA:** Supportati da Ada

### Ordinal/discrete types

*Sottoinsieme degli scalari*

Ogni valore possiede un successore e un predecessore (tranne per il primo/ultimo valore).

Possiamo iterare su questi tipi di dati e usarli come indici di un array.

1. **Boolean**
    1. **Val:** True or False
    2. **Op:** or, and, not, condiitonals
    3. **Repr:** One byte
    4. **NOTA:** C non ha questo tipo di dato
2. **Characters**
    1. **Val:** a, A, b, B, c, C ... 1, 2, 3 ... !, =, * etc...
    2. **Op:** Equality, codifica/decodifica, language-dependant
    3. **Repr:** One byte (ASCII) or two bytes (UNICODE)
3. **Integer**
    1. **Val:** 1, 2,-1, -2, 8, 10 etc...
    2. **Op:** +, -, *, mod, div
    3. **Repr:** Several Bytes (2 or 4)
    4. **NOTA:** Long integers (anche di 8 bytes), possono avere problemi di portabilità, quando non specificato dal linguaggio
4. **Enumeration (vedi sotto)**
5. **Intervals (vedi sotto)**

### **The void type**

1. **Val:** Un singolo valore
2. **Op:** Nessuna
- Utilizzato per definire il tipo di operazioni che modificano lo stato senza ritornare un valore

```c
void f(...) {...}
```

Il tipo di `f` deve avere un valore.

Il valore di tipo `void`, ritornato da `f`, è sempre lo stesso.

### Enumeration type

Vengono introdotti nel linguaggio Pascal

```pascal
type Days = (Mon, Tue, Wed, Thu, Fri, Sat, Sun)
```

I programmi scritti in questo modo sono più comprensibili, ed essendo gli `enum` valori ordinati è possibile eseguire operazioni del tipo `Tue < Fri`.

È possibile iterare: `for i = Mon to Sat do`

Possono essere rappresentati da uno Short Int (1 byte)

in C

```c
enum days = (Mon,Tue,Wed,Thu,Fri,Sat,Sun)

// Equivalente a scrivere
typedef int days;
const days Mon=0, Tue=1 ...
```

### Intervals (subrange)

Introdotti anch'essi in Pascal.

Valori sono un intervallo di valori di un tipo ordinale.

Esempio:

```pascal
type LessThanTen = 0..9;
type WorkDays = Mon..Fri;
```

Rappresentati come il Base Type.

L'utilizzo degli intervalli invece che il base type consente di generare una documentazione "controllabile" e codice efficiente.

# Structured/non-scalar Types

1. **Record:**
    1. Collezione di "campi" ognuno di un tipo differente (a discrezione)
    2. Un campo è selezionato dal suo nome
2. **Record varianti:**
    1. Record dove solo uno dei vari campi è attivo per uno specifico datum
3. **Array:**
    1. Possono essere intese come una funzione di mappatura, che mappano un indice a un altro tipo (a discrezione obv).
    2. Gli array di caratteri vengono chiamati stringhe, con operatori speciali
4. **Sets:**
    1. Un subset di un base type
5. **Pointer:**
    1. Reference to an object di un altro tipo

## Records

Manipolano i dati di tipo differente in una maniera uniforme.

Sono implementati nei linguaggi: C, C++, CommonLisp, Algol67 con la keyword `struct`

Java non ha uno speciale tipo record, ma è presunta una classe

**Esempio in C:**

```c
struct student {
	char[] name(20);
	int number;
}

// Selection of a field
student s;
s.number = 123456;
```

I record possono essere annidati

**Storable, expressible and denotable**

- Pascal: No notion of constant record, except at the initialization
step.
- Equality usually not defined (exception Ada)

### **Memory layout**

I record sono sequenzialmente disposti in memoria, un campo dopo l'altro.

**Esistono due modi per disporre i campi:**

1. Allineandoli a word boundaries (cioè ogni campo vale una word): Spreca memoria
2. Packed records: Non sono allineati con le word boundaries ma è più costoso l'accesso al campo in memoria

## Variant record

In un variant record ci sono molteplici alternative (exclusive) per alcuni tipi di record.

```pascal
type student = record
	name : packed array [1..6] of char;
	number: integer;
	case finished : Boolean of
		true: (last_year: 2000..maxint);
		false:(year:(first,second,third));
	end;

var s: student;
s.finished := true;
s.last_year:= 2001;
```

Due campi varianti `last_year` e `finished` possono usare la stessa locazione di memoria

I variant record sono permessi in molti linguaggi di programmazione

```cpp
struct student {
	char name[6];
	int numberM
	bool finished;
	union {
		int last_year;
		int : variant records
	};
}
```

In Pascal, Modula e Ada: Unions e records sono uniti

## Array

Gli **array** sono collezioni di dati omogenei, ovvero dello stesso tipo.

Possono essere intesi come una funzioni che mappano un `index` a un element type.

L'**index type** degli array è solitamente discrete.

Gli elementi possono essere di qualsiasi tipo, difficilmente però sono function type.

**Dichiarazione**

```c
int vec[30]
```

```pascal
var vect:array[0..29] of integer;
```

**Array multidimensionali**

Funzioni che mappano da index type a array type

```pascal
var mat : array [0..29, a..z] of real;
var mat : array [0..29] of array [a..z] of real;
```

Non sono equivalenti in Ada, il secondo permette lo slicing.

### Operazioni sugli array

**Operazioni principali**

Selezione di un elemento `vect[3], mat[10, 'c']`

Attenzione: La modifica di un elemento non è un operazione di array, ma sulla locazione di memoria che conserva l'elemento dell'array.

**Alcuni linguaggi permettono lo *slicing***

Selezione di parti contigue di un array

Esempio:

```pascal
mat array(1..10) of array(1..19) of real;
```

`mat(3)` indica la terza riga della matrice `mat`.

In presenza di slicing alcune operazioni globali sono possibili ad esempio:

- Fortran90: `A+B` Somma di elementi di `A` e di `B` (dello stesso tipo)
- APL ha molte operazioni per manipolare gli array

![Schermata 2021-08-29 alle 5.10.55 PM.png](Data%20Structures%20and%20Types%20e396cbde29ea4ead8ea7d6f543a9f505/Schermata_2021-08-29_alle_5.10.55_PM.png)

```pascal
int W[21..30];
type Nano = {Grumpy, Cucciolo, Dotto, Eolo, Gongolo, Mammolo, Pisolo};
int V[1..10,1..10];
char C[Dotto..Mammolo,0..10,1..10]
```

### Storage of arrays

Gli elementi di un array sono conservati in locazioni contigue, abbiamo due possibilità:

- Ordinati una riga dopo l'altra (Più usata)
- Ordinati una colonna dopo l'altra

L'ordine è importante nei sistemi con caching

### Calcolo dell'address

**Esempio:** *Calcolo dell'indirizzo di un certo elemento in posizione `A[i, j, k]` (ordinati in riga).*

Si calcolano le dimensioni $S_1, S_2, S_3$ di un array `A[l1..u1, l2..u2, l3..u3]`

$S_3$ è la dimensione di un elemento

$S_2 = (u3-l3+1) * S_3$ è la dimensione di una riga

$S_1 = (u2-l2+1) * S_2$ è la dimensione di un piano

La posizione di `**A[i, j, k]`** è la somma di:

$\alpha$: Indirizzo di `A[0]`

$(k - l3) * S_3$: Inizio di `A[i, j, k]`

$(j - l2) * S_2$: Inizio della riga `A[i, j, k]`

$(i - l1) * S_1$: Inizio del piano `A[i, j, k]`

Se sappiamo la forma a Compile time allora possiamo riscrivere il tutto come:

$$i*S_1+j*S_2+k*S_3+\alpha$$

### Dope vector

Solitamente il Compiler è a conoscenza della forma dell'array, ma se la forma può variare durante il Run time allora l'informazione è mantenuta in un array chiamato Dope Vector dell'array.

Il Dope Vector contiene:

- Puntatore all'inizio dell'array
- Il numero delle dimensioni (1, 2, 3, etc)
- Lower bound (Upper bound se vogliamo controllarlo a Run time)
- Spazio necessario a ogni dimensione

Il Dope Vector è stored in una sezione fissa dell'Activation record.

![Schermata 2021-08-29 alle 6.56.05 PM.png](Data%20Structures%20and%20Types%20e396cbde29ea4ead8ea7d6f543a9f505/Schermata_2021-08-29_alle_6.56.05_PM.png)

## Pointers

Alcuni linguaggi permetto di riferirsi e modificare una $l$-value senza dereferenziarla.

**Valori**: References ($l$-values) e constant come `null` o `nil`

**Operazioni**:

- Creazione (Una funzione di libreria che ritorna un puntatore, Es. `malloc`)
- Dereference (Accesso all'oggetto puntato, Es. `*p`)
- Equality test, incluso il test con `null`

### Puntatori e Array in C

In C i puntatori e gli array sono intercambiabili

```c
int n;
int *a; // pointers to integers
int b[10]; // array of 10 integers
...
a = b; // a points to the initial element of b
n = a[3]; // n has the value of the third element of b
n = *(a+3); // idem
n = b[3]; // idem
n = *(b+3); // idem
```

# Type equivalence e compatibility

Due tipi $T$ e $S$ sono equivalenti se ogni oggetto di tipo $T$ è anche di tipo $S$ e viceversa.

$T$ è compatibile con $S$, se ogni oggetto di tipo $T$ può essere usato in un contesto dove un oggetto di tipo $S$ era previsto.

## Equivalenza per nome

Due tipi sono equivalenti se hanno lo stesso nome.

È usato in Pascal, Ada e Java.

**Loose equivalence by name:** Pascal, Modula-2

La dichiarazione di un alias di un tipo genera un nuovo nome, non un nuovo tipo.

```pascal
type A = record ... end;
type B = A;
```

`A` e `B` sono nomi dello stesso tipo.

## Structured equivalence

Due tipi sono equivalenti se hanno la stessa **struttura.** 

L'equivalenza strutturale tra due tipi è la minima relazione di equivalenza che soddisfa:

- Un nome di un tipo è equivalente a se stesso
- Se $T$ è definito come `T = expression` allora $T$ è equivalente a `espression`
- Due tipi costruiti usando lo stesso type constructor, basato su tipi equivalenti, sono equivalenti

L'equivalenza è controllata dalla riscrizione (???)

## Compatibility

$T$ è compatibile con $S$, se ogni oggetto di tipo $T$ può essere usato in un contesto dove un oggetto di tipo $S$ era previsto.

La definizione dipende dal linguaggio, in genere $S$ è compatibile con $T$ se:

- $T$ e $S$ sono equivalenti
- I valori di $T$ sono un subset dei valori di $S$ (intervallo)
- Tutte le operazioni sui valori di $S$ possono essere eseguite su valori di tipo $T$ (Estensione di record)
- C'è una corrispondenza naturale tra i valori di $T$ e i valori di $S$ (`int e float`
- I valori di $T$ possono in qualche modo corrispondere a qualche valore di $S$ (`float to int` troncando)

## Type conversion

Se $T$ è compatibile con $S$ allora esiste un meccanismo di conversione

I principali sono:

- Conversione implicita, chiamata anche **coercion.** La macchina astratta esegue la conversione senza alcuna menzione dell'operazione nel codice.
- Conversione esplicita, o **cast**, quando la conversione viene menzionata nel codice.

### Coercion

La coercion indica come la conversione deve essere eseguita in caso di compatibilità dei tipi.

Abbiamo tre opzioni:

1. Hanno lo stesso valore e stessa rappresentazione. 
Es: tipi strutturalmente equivalenti ma con nomi differenti
Conversione a compile time
2. Hanno valori differenti, ma i valori in comune hanno la stessa rappresentazione.
Es: intervalli e interi
È necessario codice per un controllo dinamico quando c'è un'intersezione.
3. Diversa rappresentazione per i valori
Es: reali e interi
È necessario codice per la conversione

### Cast

In alcuni casi il programmatore deve esplicitare il tipo di conversione da effettuare

```c
S s = (S) T;
```

I casi sono simili alla coercion

Non tutte le conversioni esplicite sono permesse, solo quando il linguaggio sa come effettuare la conversione. Può essere sempre fatto quando i tipi sono compatibili.

I linguaggi moderni preferiscono il cast alla coercion.

# Polymorfismo

Un singolo valore (value) ha molteplici tipi.

Esistono tre forme di polimorfismo:

1. Ad hoc polymorfism (overloading)
2. Universal polymorfism
    1. Parametric polymorfism (Explicit or implicit)
    2. Subtype polymorfism

## Ad hoc polymorphism

Chiamato anche **overloading**.

Lo stesso simbolo ha significati diversi

- `3 + 5`
- `4.3 + 5.3`

Il compilatore traduce `+` in modi diversi, viene risolto in Compile time dopo l'operazione di type inference.

## Parametric polymorphism

Un valore ha un polimorfismo universale parametrizzato quando il valore ha un numero infinito di tipi possibili, ottenuto attraverso l'istanziazione di un general type schema.

Una funzione polimorfica è una singola definizione di una funzione che è applicabile uniformemente a tutte le istanze di un General type.

```c
Ide(x) = x;
sort(v) = ... ;
```

`Ide` ha type `<T>-><T>`
`Ide` ha type `<T>[]->void`
`T` è un tipo variabile

### Polimorfismo parametrizzato (esplicito)

In C++ esiste la template function

```c
void swap (int &x, int &y){
	int tmp = x; 
	x = y; 
	y = tmp;
}
```

```c
template <typename T>
void swap (T &x, T &y){
	T tmp = x; 
	x = y; 
	y = tmp;
}
```

A link time `T` diventa del tipo impiegato in caso il template viene chiamato.

```c
int i,j; swap(i,j);
```

```c
float r,s; swap(r,s);
```

```c
String v,w; swap(v,w);
```

### Polimorfismo parametrizzato in ML (implicito)

La funzione di swap in ML è cosi implementata:

```
swap(x, y) = 
	let val tmp = !x in x = !y; 
	y = tmp 
end;
val swap = fn : ’a ref * ’a ref -> unit
```

ML Inserisce il template automaticamente quando possibile.

L'instantiation avviene a compile time

```
swap(x,y);
val it = (): unit
```

```
swap(v,w);
val it = (): unit
```

## Subtype polymorphism

È simile al polimorfismo esplicito, ma non tutti i tipi possono essere usati per instanziare quello generale (il template).

Supponiamo che $T$ sia un sottotipo di $S$, scritto come $T<S$

Un valore possiede un subtype (or limited) polymorphism se ha un infinito numero di tipi possibili, ottenuti tramite sostituzione a un parametro tutti i sottotipi possibili del tipo dato.

Penso intenda tipo una Classe in java e le sue sottoclassi???

Una funzione polimorfica è un singolo programma che può essere applicato in modo uniforme a tutte le legal instances del general type.

# Memory

È importante gestire al meglio i blocchi di memoria soprattutto tenere traccia quando questi non sono più utili o referenziati nel codice.

Spesso, se bad code viene scritto, si incontrano diversi errori tra i quali.

## Handling dangling reference

Una dangling reference è un puntatore a un oggetto che non è più valido

### Tombstones

![Schermata 2021-08-30 alle 9.48.10 PM.png](Data%20Structures%20and%20Types%20e396cbde29ea4ead8ea7d6f543a9f505/Schermata_2021-08-30_alle_9.48.10_PM.png)

### Locks and keys

![Schermata 2021-08-30 alle 9.48.49 PM.png](Data%20Structures%20and%20Types%20e396cbde29ea4ead8ea7d6f543a9f505/Schermata_2021-08-30_alle_9.48.49_PM.png)

## Garbage collection

Nei linguaggi che implementano il Garbage collection, un utente alloca memoria mentre la deallocazione esplicita non è permessa.

Il sistema è dunque responsabile di recuperare la memoria allocata che non è più utilizzata nel codice.

Con "non più in uso" si intende che non esiste un modo valido di accederci.

### Reference count

![Schermata 2021-08-30 alle 10.15.16 PM.png](Data%20Structures%20and%20Types%20e396cbde29ea4ead8ea7d6f543a9f505/Schermata_2021-08-30_alle_10.15.16_PM.png)

Un possibile problema di questo approccio è la circular structure

![Schermata 2021-08-30 alle 10.16.46 PM.png](Data%20Structures%20and%20Types%20e396cbde29ea4ead8ea7d6f543a9f505/Schermata_2021-08-30_alle_10.16.46_PM.png)

### Mark and sweep

Marca tutti gli oggetti come "inutilizzati". Partendo da un puntatore non in heap controlla tutte le strutture linkate, marcando tutti gli oggetti visitati come "usati".

Ripristinare i blocchi di memoria inutilizzati.

**NOTA:** La tecnica mark and sweep identifica anche i dangling pointers

**Informazioni:**

- é necessario spazio per il gc
- quando il gc gira, l'utente nota una riduzione significativa delle prestazioni

Java usa un GC incrementale

![Schermata 2021-08-30 alle 11.33.50 PM.png](Data%20Structures%20and%20Types%20e396cbde29ea4ead8ea7d6f543a9f505/Schermata_2021-08-30_alle_11.33.50_PM.png)

Stack for depth first visit

![Schermata 2021-08-30 alle 11.34.06 PM.png](Data%20Structures%20and%20Types%20e396cbde29ea4ead8ea7d6f543a9f505/Schermata_2021-08-30_alle_11.34.06_PM.png)

Pointer reversal