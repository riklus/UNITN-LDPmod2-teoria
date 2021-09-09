# Tipi

## Base

- Int:
  ```ml
  val num = 10; (*) num è: int
  val b = 0xab;
  ```
- Real:
  ```ml
  val num = 10.0; (* num è: real *)
  val b = 3.14e12; 
  ```
- Char:
  ```ml
  val c = #"a"; (*) c è: char
  ```
- String:
  ```ml
  val s = "house"; (*) s è: string
  ```
- Boolean:
  ```ml
  val b = true; (*) b è: boolean
  ```
- nil:
  ```ml
  [] = nil; (*) true
  ```
  nil è un valore nullo che equivale a una lista vuota.

__NB:__ Numeri negativi:  
```ml
val neg = ~10;
```

## Tuple and List

- Tuple:
  ```ml
  val tup = (1, 2, 3); (*) tup è tuple: int * int * int
  ```
  __NB:__ Le tuple possono avere valori di tipo diverso
- List:
  ```ml
  val l = [1, 2, 3, 4]; (*) l è: int list
  ```

# Operatori

Gli operatori accettano la notazione _pre-fissa_ `+(1,2)` e alcuni per convenienza accettano quella _infissa_ `1+2`.

__NB:__ Questi operatori accettano solo operandi dello stesso _tipo_.

## Aritmetici

- `+ ` Addizione
- `- ` Sottrazione
- `* ` Moltiplicazione 
- `mod ` Modulo: __NB:__ _Solo per gli interi_
- Divisione:
  - `/ ` __NB:__ Solo per i reali
  - `div ` __NB:__ Solo per gli interi

## Booleani

### Comparazione

- `= ` Equal
- `< ` Less than
- `<=` Less and equal
- `> ` Greater than
- `>= ` Greater and equal 
- `<> ` Different

### Logici

- `orelse ` OR
- `andalso ` AND
- `not `  NOT

## Conversione

__NB:__ Non esiste _coercion_ in ML, ovvero la conversione implicità non è presente nel linguaggio.

- `chr ` int to char
- `real ` int to real
- `ord ` char to int
- `str ` char to string


### real to int

- `floor ` arrotonda per difetto
- `ceil ` arrotonda per eccesso
- `round ` arrotonda al più vicino
- `trunc ` tronca

# Working with String

- `^ ` Concatenazione
  ```ml
  val s = "dog" ^ "house";
  (*) s = "doghouse";
  ```
- `explode ` String to List of Char
- `implode ` List of Char to String

# Working with Lists

- `hd ` Restituisce l'elemento in testa alla lista
- `tl ` Restituisce la lista stessa ma senza la sua testa

- Concatenazione:
  ```ml
  [1,2] @ [3,4] (*) int list = [1,2,3,4]
  ```
- Pre-pending:
  ```ml
  1 :: [2,3] (*) int list = [1,2,3]
  ```

# Working with Tuples

- Accessing the Elements:
  ```ml
  val second = #2 ("a", "b");
  (*) second = "b": string
  ```

# If then else

In ML __tutto__ è una _expression_ dunque l'else non è opzionale e il costrutto `if then else` deve risultare sempre un'espressione.

```ml
> if 1<2 then 3+4 else 5+6;
val it = 7: int
```
# Functions

Le funzioni possono essere definite con la keyword `fun`. 
Ogni funzione in ML è un'espressione che accetta come argomento un solo parametro, questo parametro può anche essere una tupla contenente più valori di tipo diverso.   

Questa funzione converte un carattere minuscolo in uno maiuscolo (non controllando se già maiuscolo).

```ml
fun upper(c) = chr(ord(c) - 32);
```

```ml
(* Type of upper *)
val upper = fn: char -> char
```

`upper` è una funzione che da _char_ manda in _char_.

## Parameter type

ML deduce automaticamente il _tipo_ dei parametri.

```ml
fun square (x) = x * x;
```

```ml
(* Type of square *)
val square = fn: int -> int
```

Se vogliamo indicare esplicitamente a ML il tipo di parametro possiamo scrivere:

```ml
fun square (x:real) = x * x;
```

```ml
(* Type of square *)
val square = fn: real -> real
```
---

Questa funzione ritorna il massimo di tre parametri:

```ml
fun max3(a:real,b,c) = (* maximum of reals *)
    if a>b then
        if a>c then a else c
    else
        if b>c then b else c;
```

```ml
(* Type of max3 *)
val max3 = fn: real * real * real -> real
```

__NB:__ Si nota che da una sola indicazione del tipo di parametro ML la propaga agli altri 2. Se non avessimo specificato il tipo ML l'avrebbe posto a _Int_.

## Referencing extrernal value

ML implementa uno scoping di tipo __statico__.  
In ML le variabili non esistono, esistono solo costanti. Quando una costante viene ridefinita non cessa di esistere ma il suo oggetto non è più accessibile tramite quel _name_. Viene dunque creata una nuova reference con lo stesso _name_ a un _oggetto diverso_.

```ml
val x=3;
fun addx(a) = a+x;
val x=10;
addx(2);
```

```ml
(* Result: *)
> addx(2);
val it = 5: int
```

# Esempi di Funzioni

Un piccolo elenco di funzioni _definite sequenzialmente_.

## Cubo di Real

```ml
fun cube(x: real) = x * x * x;

> val cube = fn: real -> real
```

## Minimo di tre

```ml
fun min3(a, b, c) = 
if a<b then
    if a<c then a else c
  else
    if c<b then c else b;

> val min3 = fn: int * int * int -> int
```

## Terzo elemento di una Lista

```ml
fun third(l) = hd( tl( tl(l) ) );

> val third = fn: ’a list -> ’a
```

## Reverse di una Tupla di tre elementi

```ml
fun reverse(a, b, c) = (c, b, a)

> val reverse = fn: ’a * ’b * ’c -> ’c * ’b * ’a
```

## Terzo Carattere di una String

```ml
fun thirdchar(s) = third( explode(s) );

> val thirdchar = fn: string -> char
```

## Buffer circolare di una Lista

[A, b, c, d] -> [b, c, d, A]

```ml
fun cycle(l) = tl(l) @ [hd(l)];

> val cycle = fn: ’a list -> ’a list
```

## (Min, Max) di tre Interi

```ml
fun minmax(a, b, c) = ( min3(a, b, c), max3(a, b, c) );

> val minmax = fn: int * int * int -> int * int
```

## Dati tre Interi produci una lista ordinata (crescente) di questi interi

```ml
fun sort(a, b, c) = [min3(a,b,c)] @ 
  [
    if a<b then
      if c<a then a else c
    else  (* b<a *)
      if c<b then b else c
  ] @ [max3(a,b,c)];

> val sort = fn: int * int * int -> int list
```

## Arrotonda un reale al 0.1

```ml
fun rnd(r: real) = (real( round(r*10.0) )) / 10.0;

> val rnd = fn: real -> real
```

## Data una Lista rimuovi l'elemento 2

```ml
fun rem(l) = hd(l) :: tl(tl(l));

> val rem = fn: 'a list -> 'a list
```

# Funzioni ricorsive

slide 123.

La ricorsione consiste in:

- Passo Base
- Passo Induttivo

## Lista al contrario

_Se:_ la lista è nulla allora ritorna `nil`.  
_altrimenti:_ fai il rovescio della coda `l` e appendine la testa sulla destra.

```ml
fun reverse(l) =
  if l = nil then nil
  else
    reverse(tl l) @ [hd l];

> val reverse = fn: ’’a list -> ’’a list
```

## Combinatoria

Il _binomio di newton_ è rappresentato nel modo seguente:

$$\binom{n}{k}$$

Dove $n$ sono gli elementi e $k$ i posti.

__Caso Base:__

_Condizione:_  

- Se $k =  0$
- Se $k = n$

$$\binom{n}{0} = \binom{k}{k} = 1$$

__Passo Induttivo:__

_Condizione:_  

- Se $0 < k < n$

$$\binom{n}{k} = \binom{n - 1}{k} + \binom{n - 1}{k- 1}$$

__In ML:__

```ml
fun comb(n,m) = (* assume 0 <= m <= n *)
  if m=0 orelse m=n then 1
  else comb(n-1,m) + comb(n-1,m-1);

> val comb = fn: int * int -> int
```

## Ricorsione mutuale (AND)

Una funzione `A` che chiama una funzione `B` che chiama la funzione `A` che chiama la funzione `B` e così via...  

In questo esempio definiamo due funzioni e le chiamiamo su una lista [1, 2, 3, 4, 5]:
- `take`: [1, 3, 5]
- `skip`: [2, 3, 4]

```ml
fun
take(L) =
  if L = nil then nil
  else hd(L) :: skip(tl(L))
and
skip(L) =
  if L = nil then nil
  else take(tl(L));
```

## NB

Esistono altre cose da sapere di ML, come il Local e il Let..in, controllate sulle slides o sugli appunti di Pater999. In alternativa potete modificare questo file e fare una pull request.