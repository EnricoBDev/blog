+++
title = "My First Post"
date = "2025-09-14T19:27:32+0000"
author = "EnricoBDev"
authorTwitter = "" #do not include @
cover = ""
coverCaption = ""
description = "Un esempio di descrizione"
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++

# Assembly MIPS

## Struttura di un file assembly

Prima di entrare nel dettaglio dell'assembly MIPS dobbiamo parlare di come è organizzato un file sorgente assembly, ci sono varie porzioni:

- commenti, iniziano con il carattere `#` e sono solo in linea
- labels, sono delle etichette utilizzate per organizzare il codice e per effettuare salti a quella porzione di codice
- sezioni, dividono la parte del programma per i dati inizializzati, sezione `.data`; la parte del programma per i dati non inizializzati, sezione `.bss` e la parte del programma per le istruzioni, sezione `.text`
- direttive

## Dati globali

Come detto in precedenza i dati globali vengono dichiarati nella sezione `.data`, il linguaggio ci offre varie direttive in base al dato che vogliamo memorizzare.

| Direttiva | Grandezza                     | Dettagli                     | Esempio                |
|-----------|-------------------------------|------------------------------|------------------------|
| `.byte`   | 1 byte                        | Equivalente di char          | char: .byte 60         |
| `.half`   | 2 byte                        | Equivalente di short         | short: .half 10        |
| `.word`   | 4 byte                        |                              | int: .word 420         |
| `.float`  | 4 byte                        |                              | single: .float 0       |
| `.double` | 8 byte                        |                              | long: .double 0.3        |
| `.asciiz` |                               | Stringa con terminatore `\0` | ciao: .asciiz "Ciao\0" |
| `.ascii`  |                               | Stringa senza terminatore    | ciao: .ascii "Ciao"    |
| `.space`  | Specificata dal programmatore | Spazio vuoto                 | array: .space 100      |

Come vediamo dagli esempi la sintassi per dichiarare variabili (tranne per `.space`) è la seguente: `nome_variabile: .direttiva valore`, per `.space` invece al posto del valore inseriamo la quantità di byte da riservare, lo spazio riservato è contiguo in memoria

## Registri

Una parte fondamentale di una CPU sono i registri, essi sono una memoria super veloce implementata direttamente nel die, in un programma assembly possono essere considerati come una variabile, alcuni sono GPR (general purpose registers) mentre altri sono riservati o usati per operazioni specifiche, in totale nell'architettura MIPS sono presenti 32 registri.

In un programma assembly MIPS un registro viene indicato con `$registro`, i registri sono:

- `$at` registro riservato all'assembler
- `$t0-$t9` registri temporanei
- `$s0-$s7` registri preservati tra chiamate di funzioni
- `$zero` registro contenente la costante 0
- `$v0-$v1` registri contenenti i valori di ritono delle procedure
- `$k0-$k1` registri riservati al kernel
- `$gp` global pointer
- `$sp` stack pointer
- `$fp` frame pointer
- `$ra` indirizzo di ritorno alla procedura chiamante

## Istruzioni e operandi

Le istruzioni sono i mattoni che compongono il nostro programma, un istruzione è formata da due parti:

- codice mnemonico, un codice che indica l'istruzione da effettuare
- operandi, gli input e gli output dell'istruzione

ad esempio nell'istruzione `add $t0, $t1, $t2`, `add` è il codice mnemonico mentre `$t0`, `$t1`, `$t2` sono gli operandi.

Possiamo classificare le istruzioni in varie categorie:

- Istruzioni logico-aritmetiche
- Istruzioni di shift logico
- Istruzioni di trasferimento dati
- Istruzioni di logica di controllo

### Istruzioni logico-aritmetiche

Sono le operazioni più elementari e più utilizzate, come ci indica il nome ci permettono di effettuare operazioni aritmetiche (somma, sottrazione, moltiplicazione) e operazioni logiche (AND, OR), esempi includono:

- `add $t0, $t1, $t2`, effettua la somma tra `$t1` e `$t2` e la mette in `$t0`
- `and $t0, $t1, $t2`, effettua l'and bit a bit tra `$t1` e `$t2` e mette il risultato in `$t0`

### Istruzioni di shift logico

Le istruzioni di shift logico possono essere considerate un sottoinsieme di quelle logico-aritmetiche, ci permettono di effettuare shift logico a destra `srl` e shift a sinistra `sll`, esse sono utili per fare moltiplicazioni e divisioni per 2 in un sistema binario, ad esempio:

- `sll $t0, $t1, 2`, effettua lo shift logico a sinistra di 2 posizioni di `$t1` e inserisce il risultato in `$t0`

### Istruzioni di trasferimento dati

Le istruzioni di trasferimento dati sono istruzioni potenti che ci permettono di trasferire valori dalla/alla memoria RAM, sono importantissime poichè abbiamo solo 32 registri ed essi non bastano per memorizzare strutture dati come array o liste, le più comuni e utilizzate sono `$lw` (load word) e `$sw` (store word).

Apriamo una parentesi sull'indirizzamento, l'architettura MIPS utilizza word di 32 bit ed effettua l'indirizzo al singolo byte, quindi ogni word sta a distanza di 4 indirizzi dall'altra.

Alcuni esempi sono:

- `lw $t0, 4($t1)`, carica una word contenuta nell'indirizzo base `$t1` offsettato di 4 byte nel registro $t0 (in parole povere l'indirizzo in memoria è dato da `$t1 + 4`)
- `sw $t0, 4($t1)`, salva una word contenuta nel registro `$t0` nell'indirizzo base `$t1` offsettato di 4 byte (in parole povere l'indirizzo in memoria è dato da `$t1 + 4`)

### Istruzioni di logica di controllo

Le istruzioni di logica di controllo sono le istruzioni che differenziano un calcolatore elettronico moderno da una semplice calcolatrice, esse permettono di cambiare il flusso di esecuzione in base ad alcune condizioni, ad esempio:

- `beq $t0, $t1, Else` passa all'esecuzione del codice sotto l'etichetta `Else` se i valori di `$t0` e di `$t1` sono uguali

### Operandi

Nell'assembly MIPS sono presenti 2 tipi di operandi:

- registri
- valori immediati, usati ad esempio nell'istruzione `addi $t0 $t1 1` che fa la somma tra il contentuto di `$t1` ed 1 e salva il valore in `$t0`

A questo [link](https://it.wikiversity.org/wiki/ISA_e_Linguaggio_Assemby_MIPS#Istruzioni_MIPS) è possibile trovare una tabella con tutte le istruzioni dell'assembly MIPS.

## Logica condizionale e cicli

Per costruire le condizioni il linguaggio ci permette l'utilizzo delle istruzioni di logica di controllo e alcune istruzioni di trasferimento dati condizionale, vediamo quali sono e come possiamo costruire i classici costrutti condizionali della programmazione combinandoli in vari modi.

Le istruzioni che useremo sono:

| Codice mnemonico                   | Sintassi            | Significato                                                                                  |
|------------------------------------|---------------------|----------------------------------------------------------------------------------------------|
| `beq` (branch equal)               | `beq $t0 $t1 Label` | Passa l'esecuzione all'etichetta Label se `$t0` e `$t1` contengono lo stesso valore          |
| `bne` (branch not equal)*          | `bne $t0 $t1 Label` | Passa l'esecuzione all'etichetta Label se `$t0` e `$t1` contengono valori diversi            |
| `slt` (set on less than)*          | `slt $s0 $t0 $t1`   | Imposta il registro `$s0` a 1 se `$t0 < $t1` (comparazione con numeri in complemento a 2)    |
| `sltu` (set on less than unsigned) | `sltu $s0 $t0 $t1`  | Imposta il registro `$s0` a 1 se `$t0 < $t1` (comparazione con numeri unsigned)              |
| `j` (jump)                         | `jump Label`        | Salta all'etichetta `Label`                                                                  |
| `jr` (jump register)               | `jump $t0`          | Salta all'indirizzo specificato nel registro `$t0`                                           |
| `jal` (jump and link)              | `jal Label`         | Salta all'etichetta `Label` e salva in `$ra` l'indirizzo di ritorno alla procedura chiamante |
|                                    |

\* esistono anche le versioni di queste istruzioni con comparazioni tra un registro e un valore immediato, rispettivamente `slti` e `sltui`

### Costrutto if-else

Uno dei costrutti if-else più semplice che possiamo costruire è:

```c
if (f == g){
    f += 1;
} else {
    f -= 1;
}
```

Traduciamolo ora in assembly supponendo che `f` sia `$t0` e `g` sia `$t1`

``` assembly
bne $t0, $t1, Else
addi $t0, $t0, 1
j Esci

Else:
    subi $t0, $t0, 1

Esci:
```

Vediamo passo passo cosa fa ogni linea:

- `bne $t0, $t1, Else` salta ad Else se i due registri sono diversi, abbiamo usato l'istruzione opposta per saltare al codice dell'else, ora abbiamo due possibilità
  - Se `$t0 == $t1`, ignoriamo l'istruzione di branch e andiamo alla riga successiva:
    - `subi $t0, $t0, 1` che va a fare la sottrazione tra `$t0` e l'immediato 1 e la salva in `$t0`
    - infine con `j Esci` saltiamo al label che indica la fine del costrutto

  - Se `$t0 != $t1` allora saltiamo all'etichetta `Else`
    - nell'etichetta `Else` abbiamo: `addi $t0 $t0 1` che va a fare la somma tra `$t0` e il valore immediato 1 e la salva in `$t0`
    - non è necessario un jump al label `Esci` poichè i label sono solo dei riferimenti a blocchi di codice che quindi vengono eseguiti linearmente

La condizione `f == g` è la più semplice da rappresentare in assembly MIPS e la utilizzeremo per tutti gli altri esempi successivi ma il linguaggio ci permette di scrivere tutti i tipi di condizione.

### Costrutto while

Il costrutto while è molto simile all'if - else, ecco un esempio:

```c
while (f == g){
    f += 1;
}
```

Traduciamolo ora in assembly supponendo che `f` sia `$t0` e `g` sia `$t1`

``` assembly
While:
    bne $t0, $t1, Esci
    addi $t0, $t0, 1
    j While

Esci:
```

Analizziamo passo passo cosa fa ogni linea:

- Inizia l'esecuzione dal Label `While`, `bne $t0, $t1, Esci` salta al label `Esci` se la condizione è falsa altrimenti va avanti
- `addi $t0, $t0, 1` incrementa il registro `$t0`
- `j While` effettua un salto incondizionale per ritornare all'inizio del loop

### Costrutto for

Il costrutto for è un ciclo while che ci ha creduto di più, vediamo:

```c
for (int i = 0; i == g; i++){
    g += f;
}
```

\* la condizione `i == g` è abbastanza inutile ma questo è giusto un esempio basilare, vedremo successivamente come creare altre condizioni più utili

Ora ecco la versione in assembly, supponendo che `f` sia `$t0` ed `g` sia `$t1` e che `i` sia `$s0`

``` assembly
add $s0 $zero $zero

For:
    bne $s0, $t1, Esci
    add $t1, $t1, $t0
    addi $s0, $s0, 1
    j For

Esci:
```

Analizziamolo:

- `add $s0, $zero, $zero` è un trucco dell'assembly MIPS (dato che non esiste un istruzione di move) per inizializzare il registro al valore 0, in pratica stiamo effettuando l'operazione `$s0 = 0 + 0`
- entriamo nel label `For` e abbiamo la solita istruzione di branching `bne $s0, $t1, Esci` che termina il ciclo nel caso di condizione falsa
- `add $t1, $t1, $t0` è l'equivalente di `g+=f`
- `addi $s0, $s0, 1` incrementa il contatore
- con il salto `j For` torniamo all'inizio

Fino ad ora l'unica condizione che abbiamo visto è `==`, vediamo come si creano tutte le altre.

Il blocco di codice che andremo a tradurre ogni volta è un semplice if, come al solito supponiamo che `f` sia `$t0` e che `g` sia `$t1`.

```c
if (f condizione g){
    f +=1;
} else {
    f -= 1;
}
```

### Condizione `!=`

Per ottenere questa condizione basta sostituire `bne` con `beq` nel codice che abbiamo già visto in precedenza:

```assembly
beq $t0, $t1, Else
addi $t0, $t0, 1
j Esci

Else:
    subi $t0, $t0, 1

Esci:
```

### Condizione `>=`

Per ottenere questa condizione sfrutteremo l'istruzione `slt` (set on less than), che imposta il registro specificato a 0 se la condizione non è verificata o ad 1 se è verificata, vediamo il codice:

```assembly
slt $s0, $t0, $t1
bne $s0, $zero, Else
addi $t0, $t0, 1
j Esci

Else:
    subi $t0, $t0, 1

Esci:
```

Ecco cosa accade ad ogni riga:

- `slt $s0, $t0, $t1` è la riga cruciale e sfrutta il fatto che la condizione complementare a `<` è proprio `>=`, quindi se la condizione che vogliamo ottenere è verificata l'output di `slt` sarà 0
- `bne $s0, $zero, Else` sfrutta il "trucco" che abbiamo usato fino ad ora ovvero invertire la condizione e branchare ad `Else`, quindi controlla che `$s0` sia diverso a 0, questo equivale a controllare che sia uguale ad 1 che nel nostro caso indica condizione falsa
- il resto del codice rimane invariato

### Condizione `<`

Per ottenere questa condizione basta cambiare `bne` in `beq`, dato che adesso se la condizione è vera l'output di `slt` sarà 1 mentre se è falsa l'output sarà 0, d'altronde l'istruzione `slt` verifica proprio il minore, ecco il codice:

```assembly
slt $s0, $t0, $t1
beq $s0, $zero, Else
addi $t0, $t0, 1
j Esci

Else:
    subi $t0, $t0, 1

Esci:
```

### Condizione `>`

Per ottenere questa condizione usiamo il fatto che invertendo gli operandi di una condizione `<`, otteniamo proprio la condizione `>`, vediamo l'implementazione:

```assembly
slt $s0, $t1, $t0
beq $s0, $zero, Else
addi $t0, $t0, 1
j Esci

Else:
    subi $t0, $t0, 1

Esci:
```

Vediamo nel dettaglio come funziona:

- ricordiamo che l'espressione che noi vogliamo codificare è `f > g` ovvero `$t0 > $t1`, ma dato che il linguaggio di default ci offre solo l'istruzione per fare il `<`, riscriviamola in questo modo `$t1 < $t0`, ora possiamo codificarla con l'istruzione `slt $s0 $t1 $t0`
- in questo caso l'espressione è falsa se l'output di `slt` è 0 quindi usiamo l'istruzione `beq $s0 $zero Else`
- il codice rimanente è uguale agli esempi precedenti

### Condizione `<=`

L'ultima condizione è `<=`, per farla sfruttiamo il fatto che `<=` è la condizione complementare di `>`:

``` assembly
slt $s0, $t1, $t0
bne $s0, $zero, Else
addi $t0, $t0, 1
j Esci

Else:
    subi $t0, $t0, 1

Esci:
```

Vediamo riga per riga:

- `slt $s0, $t1, $t0` è lo stesso controllo che abbiamo usato per la condizione `>`
- il cambiamento è nella riga `bne $s0 $zero Else`, infatti la condizione `<=` è vera se la condizione `>` (data dalla riga sopra) è falsa, qui come al solito controlliamo se la condizione che vogliamo ottenere è falsa per branchare ad `Else`
- il resto del codice rimane invariato

Adesso abbiamo tutte le condizioni necessarie per scrivere i nostri programmi ma manca una cosa, i connettivi logici (&&, ||, !), vediamo come fare:

### Connettivo OR `||`

Il connettivo OR restituisce vero se almeno uno dei due input è vero, vediamo un codice in C e traduciamolo supponendo `f = $t0` e `g = $t1` :

```c
if (f == 1 || g == 2){
    f+=1;
} else {
    f-=1;
}
```

Ecco la versione in assembly:

```assembly
addi $t2, $zero, 1
addi $t3, $zero, 2

beq $t0, $t2, Vero
beq $t1, $t3, Vero
subi $t0, $t0, 1
j Fine

Vero:
    addi $t0, $t0, 1

Fine:
```

Come è possibile notare non abbiamo seguito l'approccio visto fin'ora (invertire la condizione e fare branch al ramo else) ma abbiamo effettuato proprio il contrario, vediamo bene il codice:

- le prime due istruzioni `addi $t2, $zero, 1` e `addi $t3, $zero, 2` caricano in `$t2` e `$t3` i valori necessari per la condizione
- dato che OR restituisce vero se almeno un input è vero abbiamo messo in successione le due istruzioni `beq $t0, $t2, Vero` e `beq $t1, $t3, Vero`, in modo tale che se la prima condizione è vera viene effettuato il branch direttamente senza valutare la seconda, se invece entrambe sono false viene subito dopo viene eseguito il ramo else

Il meccanismo che abbiamo implementato dove se la prima condizione è vera la seconda viene ignorata è detto short-circuit ed è un trucco usato per rendere il codice più efficiente, esso viene impiegato anche in linguaggi ad alto livello come il C.

### Connettivo AND `&&`

Il connettivo AND restituisce vero se entrambi gli input sono veri, vediamo un esempio in C:

```c
if (f == 1 && g == 2){
    f+=1;
} else {
    f-=1;
}
```

Traduciamolo in assembly supponendo `f = $t0` e `g = $t1`:

```assembly
addi $t2, $zero, 1
addi $t3, $zero, 2

bne $t0, $t2, Falso
bne $t1, $t3, Falso
addi $t0, $t0, 1
j Fine

Falso:
    subi $t0, $t0, 1

Fine:
```

Analizziamo il codice:

- come nell'esempio precedente le prime due istruzioni caricano nei registri i valori costanti necessari per verificare le condizioni
- anche con l'AND implementiamo lo short-circuit, infatti abbiamo le due istruzioni di seguito `bne $t0, $t2, Falso` e `bne $t1, $t3, Falso`, qui sfruttiamo il fatto che basta un solo input con valore falso per far uscire in output un valore falso

### Connettivo NOT `!`

Per la negazione basta scegliere la condizione complementare rispetto a quella da negare, ad esempio se vogliamo valutare `!(f >= g)`, basta riscriverla come `f <  g`

### Pseudoistruzioni

Codificare le condizioni in assembly non è stata proprio una passeggiata, beh è stato del tutto inutile, penso di averti fatto perdere almeno mezz'ora del tuo tempo hehe, questo perchè esistono le pseudoistruzioni (che tral'altro abbiamo già usato).

Le pseudoistruzioni sono istruzioni che non vengono effettivamente eseguite dalla CPU ma vengono tradotte dall'assembler in istruzioni equivalenti, vediamo quali sono:

| Codice mnemonico                     | Sintassi             | Significato                                             |
|--------------------------------------|----------------------|---------------------------------------------------------|
| `subi` (subtract immediate)          | `subi $t0 $t1 const` | `$t0 = $t1 - const`                                     |
| `la` (load address)                  | `la $t0 Label`       | Carica nel registro `$t0` l'indirizzo del label `Label` |
| `li` (load immediate)                | `li $t0 const`       | Carica nel registro `$t0` il valore `const`             |
| `move`                               | `move $t0 $t1`       | Copia il contenuto di `$t1` nel registro `$t0`          |
| `bgt` (branch on greater than)       | `bgt $t0 $t1 Label`  | Salta al label `Label` se `$t0 > $t1`                   |
| `bge` (branch on greater equal than) | `bge $t0 $t1 Label`  | Salta al label `Label` se `$t0 >= $t1`                  |
| `blt` (branch on less than)          | `blt $t0 $t1 Label`  | Salta al label `Label` se `$t0 < $t1`                   |
| `ble` (branch on less equal than)    | `ble $t0 $t1 Label`  | Salta al label `Label` se `$t0 <= $t1`                  |

### Costrutto switch-case (jump table)

Un modo molto efficiente di effettuare tanti controlli con valori costanti insieme è il costrutto switch-case

Ad esempio dato questo if:

```c
if (f ==0){
    // fai qualcosa
} else if (f == 1){
    // fai qualcosa
} else if (f == 2){
    // fai qualcosa
} else {
    // fai qualcosa se non si verifica nessuna condizione
}
```

Possiamo scrivere uno switch-case equivalente:

```c
switch (f){
    case 0:
        // fai qualcosa
        break;
    case 1:
        // fai qualcosa
        break;
    case 2:
        // fai qualcosa
        break;
    default:
        // fai qualcosa se non si verifica nessuna condizione
}
```

Per implementarlo in assembly andremo a creare un array di indirizzi contigui, uno per ogni ramo dello switch e per raggiungere ogni indirizzo andremo ad offsettarlo con i valori presenti nel case, vediamo come è fatto (supponiamo `f = $t0`):

```assembly
.data
Jump_table:
    .word Case0
    .word Case1
    .word Case2

.text
la $t1, Jump_table

sll $t2, $t0, 2
add $t3, $t1, $t2
lw $t4, 0($t3)

jr $t4

Case0:
    # fai qualcosa
    j Esci

Case1:
    # fai qualcosa
    j Esci

Case2:
    # fai qualcosa
    j Esci

default:
    # fai qualcosa se non si verifica nessuna condizione

Esci:
```

In questo esempio manca una parte fondamentale ovvero il check sul valore di `$t0`, poichè potrebbe sforare il limite dato dai valori presenti nel case.

Vediamo come funziona:

- nella sezione `.data` dichiariamo un label `Jump_table` con dentro tre word che contengono ogniuna l'indirizzo di un label associato ad un case
- passando alla sezione `.text`, la prima istruzione `la $t1, Jump_table` carica l'indirizzo base dell'array di indirizzi nel registro `$t1`
- l'istruzione `sll $t2, $t0, 2` moltiplica per 4 (shift di 2 bit a sinistra) il valore da controllare ovvero la variabile `f` e lo salva in `$t2`, questo sarà l'offset necessario per ottenere la word giusta.

  Notiamo che la moltiplicazione per 4 è necessaria perchè l'architettura MIPS indirizza il singolo byte e una word è fatta di 4 byte
- poi sommiamo `$t1` e `$t2` per ottenere l'indirizzo effettivo della word contentente l'indirizzo a cui saltare
- infine con `$lw $t4, 0($t3)` carichiamo in `$t4` l'indirizzo a cui saltare
- `jr $t4` effettua il salto non condizionale alla label relativa al case giusto, notiamo che se nessun case è verificato si passa al label `default`
- ogni label `case` finisce con l'istruzione `j Esci`, questa è la codifica del `break`

## Procedure

Le funzioni e le procedure sono dei blocchi di codice riutilizzabili, ed esse sono alla base della programmazione, quando avete iniziato ad imparare il C ad esempio utilizzavate una procedura senza nemmeno saperlo, ovvero la procedura `main`, anche l'assembly MIPS ci offre varie istruzioni e registri per gestire le chiamate alle procedure, prima di addentrarci nella scrittura di procedure è necessario parlare di due concetti:

### Lo stack delle chiamate (call stack)

Alla base delle procedure è presente una struttura dati alo stesso semplice e importantissima, lo stack.

Lo stack è una lista leggermente più semplificata che segue lo schema LIFO (last in first out, l'ultimo elemento ad entrare è il primo ad uscire), una sua analogia nel mondo reale è un mazzo di carte, vediamo le due operazioni fondamentali:

- pop, quando effettuiamo un operazione di pop andiamo a rimuovere l'ultimo elemento inserito, seguendo la nostra analogia andiamo a rimuovere la prima carta dalla cima del mazzo
- push, quando effettuiamo un operazione di push andiamo ad aggiungere un elemento alla fine della lista, seguendo la nostra analogia andiamo ad aggiungere una carta alla cima del mazzo

Lo stack è fondamentale poichè ci permette di passare più di 4 argomenti alle funzioni (abbiamo solo 4 registri per gli argomenti) ma sopratutto di salvare lo stato dei registri al momento prima della chiamata, per poi ripristinarli al momento del ritorno alla procedura chiamante

### Registri callee-saved vs caller-saved

A proposito del salvataggio e ripristino dei registri, la convenzione per le chiamate (calling-convention), specifica quali registri devono essere salvati dalla procedura chiamante e quali dalla procedura chiamata, vediamoli:

|           |              |
|:---------:|--------------|
| `$v0-$v1` | Caller-saved |
| `$a0-$a3` | Caller-saved |
| `$t0-$t9` | Caller-saved |
| `$s0-$s7` | Callee-saved |
|   `$gp`   | Callee-saved |
|   `$sp`   | Callee-saved |
|   `$fp`   | Callee-saved |
|   `$ra`   | Callee-saved |

Seguendo questa convenzione, per quanto riguarda i registri callee-saved è responsabilità della procedura chiamata fare in modo che lo stato di questi registri sia invariato per la procedura chiamante, mentre per i registri caller-saved la responsabilità di mantenere invariato lo stato cade sulla procedura chiamante.

In un caso molto particolare, ovvero nelle funzioni ricorsive (funzioni che chiamano se stesse), non c'è distinzione tra funzione chiamata e chiamante quindi queste regole non ci importano (vedremo successivamente un esempio)

Infine prima di vedere qualche esempio di codice dobbiamo distinguere due tipi di procedura:

- procedura foglia, ovvero una procedura che non effettua chiamate ad altre procedure al suo interno
- procedura ramo, ovvero una procedura che effettua chiamate ad altre procedure al suo interno

<!--
Per gestire chiamate a procedure o funzioni l'architettura MIPS ci offre vari registri

- `$a0-$a3`, 4 registri per i parametri
- `$v0-$v1`, 2 registri per i valori di ritorno
- `$ra`, il registro contenente l'indirizzo di ritorno alla procedura chiamante

E con l'istruzione `jal` possiamo chiamare una procedura, vediamo un esempio in C:

```c
int increment(int n){
    return n+1
}
```

Con quello che sappiamo, proviamo a tradurla in assembly, passando come parametro `n=3`:

```assembly
.text
.global _start

_start:
    li $a0, 3
    jal increment

increment:
    addi $v0, $a0, 1
    jr $ra
```

Rispetto agli esempi che abbiamo visto fino ad ora, questo è un programma completo quindi è leggermente più complesso, vediamolo passo passo:

- con `.text` definiamo la sezione delle istruzioni del programma
- la direttiva `.global _start` è fondamentale poichè dice al linker che l'entrypoint del programma sarà il label `_start`
- come prima cosa con `li $a0, 3` impostiamo il nostro parametro con il valore 3
- tramite l'istruzione `jal increment` effettuiamo la chiamata
- una volta entrati nella procedura, con `addi $v0, $a0, 1` incrementiamo il valore del parametro e lo inseriamo nel registro `$v0` che è il registro che conterrà il valore di ritorno
- con `jr $ra` torniamo al programma chiamante

Questo è uno degli esempi più semplici e banali, ma ovviamente funzioni e procedure nella vita reale sono molto più complesse quindi sono state stabilite delle convenzioni da usare per le chiamate, le suddette calling conventions.

### Calling conventions

Le basi di queste convenzioni sono state già trattate nel capitolo precedente e nella loro totalità potrebbero sembrare inutilmente complicate ma sono essenziali per gestire ad esempio le funzioni ricorsive.

Uno dei pricipi base delle calling conventions è quello che ogni procedura deve "coprire le sue tracce" ovvero deve ripristinare il valore originale di alcuni registri dopo l'esecuzione di una procedura, per fare questo viene utilizzata la struttura dati stack.

Lo stack è una lista LIFO (Last In First Out), la possiamo pensare come un mazzo di carte con il quale possiamo fare due operazioni:

- togliere la prima carta dal mazzo (POP)
- inserire una nuova carta sulla cima del mazzo (PUSH)

Per gestire lo stack è presente il registro `$sp` che punta alla cima dello stack, la sua organizzazione in memoria è particolare, ovvero quando andiamo ad allocare nuovo spazio sullo stack, esso cresce verso indirizzi di memoria più bassi.

Tornando all'analogia del mazzo di carte, ogni volta che viene chiamata una procedura viene aggiunta una carta alla cima del mazzo, quando la procedura termina la carta viene rimossa, la carta è chiamata `stack frame` ed è organizzata in questo modo:

||
|:---------------------------------:|
| Indirizzi bassi (`$sp` punta qui) |
|               `$gp`               |
|          Variabili locali         |
|   Registri da salvare `$s0-$s7`   |
|               `$fp`               |
|               `$ra` (opzionale per funzioni non ricorsive)              |
|     Parametri attuali (se > 4)    |
|           Indirizzi alti          |

(in realtà lo stack frame organizzato così è necessario solo durante chiamate a funzioni ricorsive, altrimenti è possibile semplificarlo di molto)

Possiamo dire che il compito principale dello stack è quello di preservare il valore di registri e memoria allo stato precedente alla chiamata, in modo che quando si ritorna al programma chiamante non ci siano conflitti, dato che questi registri potrebbero essere modificati durante l'esecuzione della procedura.

Durante la scrittura di una procedura è importante seguire una serie di regole o linee guida:

- Il chiamante passa i primi 4 parametri in $a0–$a3 e, se ce ne sono di più, li mette sullo stack. È anche il chiamante che dealloca questi parametri dopo la chiamata.
- All’entrata della procedura si alloca lo stack frame per registri salvati e variabili locali.
- Se abbiamo caricato parametri sullo stack ora è il momento giusto per assegnarli a dei registri temporanei
- Si salvano `$ra`, `fp` e `$s0–$s7` sullo stack (solo se saranno usati).
- Adesso copiamo `$sp` in `$fp`, `$fp` rimarrà fisso e lo useremo come registro base al posto di `$sp` che invece potrà cambiare per aumentare la grandezza dello stack
- Alla fine si ripristinano i registri salvati e si dealloca lo stack frame, poi si ritorna con jr $ra.
- Dopo il ritorno, il chiamante dealloca lo spazio sullo stack che ospitava i parametri extra

In gergo la sezione di istruzioni che viene eseguita all'inizio procedura (allocazione stack, salvataggio parametri) è detta prologo, mentre la sezione finale (deallocazione stack, ritorno) è detta epilogo

## Syscall

## Gestione della memoria (heap e dati statici) ->
<!-- TODO -->
