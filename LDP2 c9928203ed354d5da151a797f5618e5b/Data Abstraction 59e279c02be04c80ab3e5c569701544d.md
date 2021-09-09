# Data Abstraction

Le slide di kuper sono leggermente confuse e non riesco a creare un discorso coeso per questo capitolo. Mi riduco solo a tradurlo.

---

**Index:**

# Abstraction and modularity

Il programma è diviso in costrutti chiamati **componenti**, che possono essere: Funzioni, data structures, modules.

Ogni componente presenta diverse caratteristiche:

1. **Interfaccia**
    1. Tipi e operazioni definite in un componente che sono visibili *outside* il component.
2. **Specifiche dell'interfaccia**
    1. Funzionalità intesa per il componente. Espressa dalle proprietà presentate dall'interfaccia.
3. **Implementazione del componente**
    1. Strutture dati e funzioni all'interno di un componente, non necessariamente visibili dall'esterno.

## Esempio: Data types

Componente**:** 

Nome: **Priority queue**

Data structure che ritorna elementi in ordine decrescente di priorità

Interfaccia

1. Type: `PrioQueue`
2. Operations
    1. `empty`: `PrioQueue`
    2. `insert`: `ElemType` * `PrioQueue`→ `PrioQueue`
    3. `deletemax`: `PrioQueue`→`ElemType` * `Prioqueue`
3. Specification
    1. `insert`: Aggiunge un elemento a un set di elementi.
    2. `deletemax`: Ritorna l'elemento della priorità più alta e ritorna la lista degli elementi rimanenti.

# Data abstraction

1. Data type: Valori e operazioni
    1. Tipo `int` che consiste di un valore nel range `[-maxint..maxint]` e operazioni come `+, -, *, div, mod`.
    2. Le operazioni sono il solo modo di manipolare gli interi
2. Data abstraction:
    1. È la rappresentazione e implementazione di dati e operazioni
    2. È inaccessibile dall'utente, nascosta dall'*encapsulation*

# Linguistic support for abstraction

Con Abstraction of control si intende anche nascondere l'implementazione dei body delle procedure.

Con Data Abstraction si intende anche nascondere le decisioni riguardanti la rappresentazione delle strutture dati e l'implementazione delle operazioni.

Ad esempio: La priority queue può essere implementata attraverso

Binanry search tree o partially ordered vector.

Ma quale supporto linguistico viene fornito dal linguaggio per implementare l'*information hiding*?

## Abstract data types

Uno delle maggiori contribuzioni degli anni '70.

L'idea di base delle ADT è quella di separare l'interfaccia dall'implementazione.

Si utilizza il type checking per garantire la separazione in questo modo:

L'utente ha accesso solo alle operazioni nell'interfaccia

L'implementazione è *incapsulata* in un costrutto appropriato chiamato ADT

## Encapsulation

L'incapsulazione rende il costrutto representation-indipendent ovvero:

- Due corrette implementazioni di un ADT non possono essere distinte dall'utente.
- L'implementazione può essere modificata senza interferire con il loro uso. Questo perchè l'utente non può accedere all'implementazione.

## Modules

Un modulo è un costrutto generale per implementare l'*information hiding,* è disponibile in molti linguaggi di programmazione.

È composto da due parti

- Interface
    - Set of names and types
- Implementation
    - Dichiarazione di tipi e funzioni per ogni nome dell'interfaccia
    - Dichiarazioni aggiuntive nascoste