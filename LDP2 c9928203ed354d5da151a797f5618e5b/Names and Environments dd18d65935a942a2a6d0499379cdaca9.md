# Names and Environments

# Oggetto denotabile

Un oggetto denotabile è una struttura software che può essere denotata e quindi indicata semanticamente con un *nome*. 

Può essere anche inteso come un oggetto capabile di associazione semantica con un nome.
Questa operazione associativa è anche detta *associazione con un nome* oppure **Binding**.

# Nome

Una sequenza di caratteri usata per denotare qualcos’altro.

- `const p = 3.14` **`p`** è il nome.
    - `3.14` è l’oggetto denotato.
- `var x` **`x`** è il nome
    - Una *variabile* è l’oggetto denotato.
- `void f(){...}` **`f`** è il nome.
    - La *definizione di f* è l’oggetto denotato.

In programmazione i **nomi** vengono detti *identificiatori* o ***identifiers***.

L’uso che si fa di un nome ne determina l’oggetto denotato, ad esempio:

- Oggetto simbolico
- Astrazioni (Come Dati tipo array o Controllo)

**I nomi possono essere definiti dall’utente:**

Variabili, parametri formali, procedure, tipi definiti dall’utente, etichette (labels), moduli, costanti definiti dall’utente, exceptions.

**Oppure definiti dal linguaggio:**

Tipi primitivi, operazioni primitive, costanti predefinite.

Il nome e l’oggetto denotato non sono la stessa cosa:

- Un nome è una stringa di caratteri
- L’oggetto denotato può essere un *oggetto complesso* (variabili, funzioni, tipi, etc…)
- Un singolo oggetto può avere più di un nome (operazione detta *aliasing*)
- Un singolo nome può denotare oggetti diversi in *"“occasioni diverse”"*

# Bindings

**Il** **binding** è il processo attraverso il quale viene determinata l’associazione di un nome al suo corrispettivo oggetto.

In che occasione incontriamo o dove avvengono i *bindings*?

- **Nel design del linguaggio:**`+` è l’addizione, `int` denota il tipo di dato, etc…
- **Durante la scrittura del programma:**
In questo caso il binding tra una variabile e il suo identifier è **definito** nel programma ma viene **creato** solo quando viene allocato lo spazio in memoria per la variabile.
- **A Compile Time:**
Il compilatore alloca spazio in memoria per delle strutture dati che possono essere staticamente valtutate (come le variabili globali). La connessione tra un identificatore e la locazione di memoria corrispondente è formata in questo momento.
- **A Run Time:**
Tutti i binding che non sono formati a Compile Time devono essere formati a Run Time.

# Static e Dynamic

**Static** è tutto ciò che accade prima dell’esecuzione. **Dynamic** è tutto ciò che accade durante l’esecuzione.
Ad esempio il management della memoria statica è gestita dal compilatore, quella della memoria dinamica è gestita dalle operazioni di Run Time.

# Environment

È una collezione di associazioni tra nomi e oggetti denotabili che esiste a Run Time in un punto e momento specifico del programma.

## Dichiarazione

La dichiarazione crea un associazione (binding) in un *environment.*

```c
int x;

int f() {
	return 0;
};

type T = int;
```

## Aliasing

Sappiamo già che un nome può denotare un oggetto diverso in diversi punti del programma. Ma in caso di aliasing un oggetto può essere denotato da oggetti diversi.

```c
int *X, *Y;
X = (int *) malloc(sizeof (int));  // allocate memory
*X = 5;                            // dereference
Y = X ; // Y points to the same object as X
*Y = 10;
write(*X);
```

```
graph LR
A --> 150
B --> 150
```

## Blocks

Nei moderni linguaggi di programmazione l’environment è *strutturato in blocchi*.
I blocchi sono sezioni di programma delimitati da *segni*, che contenono dichiarazioni *locali* per quella *regione*.
E possono essere:

- Anonimi (o in linea)
- Associati a una procedura

Seguono alcuni esempi di segni delimitatori:

- Algol, Pascal: `begin, end`
- C, Java: `{...}`
- LISP: `(...)`
- ML: `let .. in ...end`

L’uso dei blocchi ha diversi vantaggi tra cui: chiarezza, meno collisione di nomi, consente ricorsioni e in qualche caso ottimizzano l’uso della memoria.

### Nesting Blocks

Blocchi sovrapposti **devono essere annidati uno dentro l’altro**.

```
Se questi sono due blocchi:
{} e []
allora:
{
    [...]
} è possibile

{   ...
    [
    ...
}
    ...
    ] non è possibile
```

**Regole di visibilità:**
Una dichiarazione in un blocco è visibile nel blocco stesso e in tutti i blocchi annidati in esso, purchè non esistano dichiarazioni che mascherano (*mask* or *hide*) la dichiarazione precedente.

## Suddivisione dell’Environment

L’Environment in un blocco può essere suddiviso nelle seguenti parti:

- **Environment Locale:** Associato con l’entrata in un blocco
    - Variabili Locali
    - Parametri Formali
- **Environment non locale:** Associazioni ereditate da altri blocchi
- **Environment Globale:** Quella parte di Environment non locale che contiene associazioni comuni a tutti i blocchi
    - Dichiarazioni esplicite di variabili globali
    - Dichiarazioni nel blocco esterno
    - Associazioni ereditate da moduli etc…

## Operazioni in un Environment

- *Creazione di un associazione tra un nome e un oggetto denotabile*
    - Detto anche: Dichiarazione locale in un blocco
- *Referencing a un oggetto attraverso il suo nome*
    - Detto anche: Uso di un nome
- *Disattivazione/Sospensione di un associazione tra nome e oggetto denotabile*
    - Accade in entrata di un blocco che contiene un’associazione che maschera (mask) la precedente
- *Riattivazione di un’associazione tra nome e oggetto denotabile*
    - Accade in uscita dal blocco che mascherava l’associazione in questione
- *Distruzione di un’associazione tra nome e oggetto denotabile*
    - Accade in uscita da un blocco contenente una dichiarazione locale (Local Declaration)

## Operazioni su un oggetto denotabile

- Creazione
- Accesso
- Modifica (se l’oggetto è modificabile)
- Distruzione

**NOTA:** La creazione e la distruzione di un oggetto differiscono dalla creazione e la distruzione dei collegamenti a esso.

## Eventi fondamentali che accadono a oggetti e nomi

1. *Creazione di un object*
2. Creazione di un’associazione a un object
3. Reference a un’object, attraverso un’association
4. Disattivazione di un’association
5. Riattivazione di un’association
6. Distruzione di un’association
7. *Distruzione di un’object*

**NOTE:**
Da 1 a 7 si ha la *lifetime* di un **object**.
Da 2 a 6 si ha la *lifetime* di un’**association**

## Lifetime

### Di un object

La lifetime di un oggetto non coincide con quella di un’associazione per quell’oggetto. *La lifetime di un oggetto può essere **più lunga** di quella di una sua associazione.*

**Esempio:**

```fortran
procedure P (var X:integer);
begin
  ...
end;
var A:integer;
P(A);
```

Durante l’esecuzione di `P` avviene un’associazione di `X` a un *object* che esisteva prima ed esisterà anche dopo la sua associazione con `X`.

### Di una associazione

Come un oggetto può “morire” prima della sua associazione anche un’associazione può “morire” prima dell’oggetto coinvolto.

**Esempio:**

```c
int *X, *Y;
X = (int *) malloc (sizeof(int));
Y = X;  // Y is never unbinded/unlinked
free(X);
X = null; // X is nullified
```

Dopo il comando `free` l’oggetto bindato a `X` non esiste più ma c’è ancora una *dangling reference Y* all’oggetto.

# Scoping

Lo scope è la sezione (o sezioni, anche Run Time) di un programma dove un *binding* è valido.
Lo scope differisce dall’environment poiché lo scope è una *proprietà* di un *binding*, mentre l’environment è una porzione di *source code* o di *run time*.

Sono due i principali modi per risolvere lo scope:

- In modo *statico*: **Static scoping**
- In modo *dinamico*: **Dynamic scoping**

La risoluzione di un nome è detta *name resolution*.

Segue una rappresentazione grafica dell’environment del programma in ogni punto del Source Code:

```
																	 ┌──────────────────────────┐
                               ┌───┤Environment 3:            │
                               │   │                          │
                               │   │ x = 10;                  │
       Programma               │   │                          │
      ┌────────────────────┐   │   │ y non esiste questo nome │
      │ 1 { /* Blocco A*/  │   │   │                          │
      │ 2                  │   │   └──────────────────────────┘
     ─► 3   x = 10;        ├───┘
      │ 4                  │       ┌───────────────┐
      │ 5  { /* Blocco B*/ │   ┌───┤Environment:7: │
      │ 6                  │   │   │               │
     ─► 7    y = 20;       ├───┘   │ x = 10;       │
      │ 8                  │       │               │
     ─► 9    x = 22;       ├───┐   │ y = 20;       │
      │10                  │   │   │               │
     ─►11    z = 4;        ├─┐ │   └───────────────┘
      │12                  │ │ │
      │13  }               │ │ │   ┌────────────────────────────────┐
      │14                  │ │ └───┤Environment 9:                  │
    ┌─►15                  │ │     │                                │
    │ │16                  │ │     │ x = 22; è avvenuto un masking  │
    │ │17 }                │ │     │                                │
    │ └────────────────────┘ │     │ y = 20;                        │
    │                        │     │                                │
    │                        │     └────────────────────────────────┘
┌───┴──────────────────────┐ │
│Environment 15:           │ │     ┌───────────────┐
│                          │ └─────┤Environment 11:│
│ x = 10; /*riattivazione*/│       │               │
│                          │       │ x = 22;       │
│ y non esiste questo nome │       │               │
│                          │       │ y = 20;       │
│ z non esiste questo nome │       │               │
│                          │       │ z = 4;        │
└──────────────────────────┘       │               │
                                   └───────────────┘
```

Segue una rappresentazione grafica dei vari *bindings* in ogni area del Source Code:

```
                     Programma
                   ┌────────────────────┐
                   │ 1 { // Blocco A    │
                  ┌┤ 2                  │
                ┌─┼│ 3   x = 10;        │
                │ └┤ 4                  │
                │  │ 5  { // Blocco B   │
                │  │ 6                  │
                │  │ 7    y = 20;       ├┐
                │  │ 8                  ││
Scope del     ◄─┤  │ 9    x = 22;       │┼─┬─► Scope del
Binding x = 10; │  │10                  ││ │   Binding x = 22;
                │  │11    z = 4;        ├┘ │
                │  │12                  │  ├─► Scope del
                │  │13  }               │  │   Binding y = 20;
                │ ┌┤14                  │  │
                └─┼│15                  │  └─► Scope del
                  └┤16                  │      Binding z = 4;
                   │17 }                │
                   └────────────────────┘
```

## Static scoping

Nello **Static scoping**, detto anche **Lexical scoping**, il *Name non-locale* viene risolto nel blocco che lo include sintatticamente.

```c
int x = 0;  
void pippo (int n) {    
	x = n+1;  
}  
pippo (3);  
write(x);    // Output: 4  
{
	int x = 0;   
	pippo(3);    
	write(x);  // Output: 0  
}  
write(x);    // Output: 4
```

### Indipendenza della posizione di procedure

```c
int x = 10;   // [*]  
void foo() {   
	x++;  
}  
void fie() {    
	int x = 0;    
	foo();  
} 
fie();
```

`foo()` può essere posto ovunque e ha il medesimo effetto sulla variabile del associata alla `x` (quella del commento `[x]`).

### Indipendenza dei nomi locali

```c
int x = 10;   // [*]  
void foo() {    
	x++; 
}  
void fie() {    
	int y = 0;  // [!!!]    
	foo();
}  
fie();
```

Notiamo il binding `int y = 0;` nel caso di *Static scoping* non cambia nulla, mentre in caso di *Dynamic scoping* il programma sarebbe semanticamente differente.

## Dynamic scoping

Nel **Dynamic scoping** il *Name non-locale* viene risolto nel blocco più recentemente attivato che non è stato ancora disattivato.

```c
int x = 0;  
void pippo (int n) {
	x = n+1;  
}
pippo(3);  
write(x);    // Output: 4  
{
	int x = 0;    
	pippo(3);    
	write(x);  // Output: 4 [!!!]  
} 
write(x);    // Output: 4
```

## Dynamic scoping: Specializzazione di funzioni

```c
var colour = red;  
visualize(text);
```

`visualize(var text)` è una funzione che mostra un testo in un certo `colour` definito nel blocco dove viene chiamata la funzione.

## Static vs Dynamic scoping

- **Static scoping** (o Lexical scoping)
    - Tutte le informazioni sono incluse nel program text
    - Le associazioni possono essere calcolate a Compile-Time
    - Possiede il *Principio di indipendenza*
    - Più semplice da implementare e più efficiente
    - Utilizzato in Algol, Pascal, C, Java
- **Dynamic scoping**
    - Le informazioni sono ottenute durante l’esecuzioni
    - Spesso risulta difficile leggere i programmi
    - Più complesso da implementare e meno efficiente
    - Utilizzato in alcune versioni di Lisp, Perl

## C: Un caso speciale

Mentre in Algol, Pascal, Ada, e Java è consentito annidare blocchi di sottoprogrammi il **C** è speciale perché consente la definizione di *funzioni* solo nel blocco più esterno. Questo significa che risolvere reference non-locali è più semplice.
In **C** possiamo quindi dividere l’environment delle funzioni in due parti:

- Parte Locale
- Parte Globale

# Determinare l’Environment

**L’Environment è determinato da:**

- Regole di scope (Static o Dynamic)
- **Regole specifiche (Che approfondiremo successivamente)**
- Regole di passaggio dei parametri
- **Regole di binding (shallow or deep):**
Queste vengono applicate al passaggio di una funzione a un’altra funzione come parametro.

## Regole specifiche

**DOMANDA:** Quando una dichiarazione è *visibile* nel blocco in cui appare?

- Dalla dichiarazione fino al termine del blocco:
    - Esempio: In Java
    QUESTO È ILLEGALE.

        ```java
        a = 1;
        int a;

        // a viene dichiarato dopo l'utilizzo
        ```

- Sempre:
    - Esempio: Per la dichiarazione di metodi Java consente i metodi mutualmente ricorsivi

        ```java
        void f() {
        	g();  
        }
         
        void g() {    
        	f();  
        }
        ```

## Ricorsioni Mutuali

Come è possibile scrivere ricorsioni mutuali in linguaggi dove un nome deve essere sempre dichiarato prima dell’uso?

**Opzione uno:**
Fare come Java e fare un’eccezione alla regola per le funzioni e i tipi.

**Opzione due:**
Dichiarare prima e definire dopo.

```c
struct elem;
struct elem {  
	int info;  
	elem *next
};
```