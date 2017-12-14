---
title: "Swift, La mia personale scoperta del linguaggio. (Vol. 1)"
teaser: "Un approccio pragmatico a Swift. Volume 1: Intro al linguaggio, come andrà a finire?"
category: programming
tags: [swift, programming, apple, objective-c]
---

![Swift Logo](/post-imgs/2015-05-24-swift-intro-vars/swift.png){:class="img-responsive"}

Swift è un linguaggio di programmazione presentato da Apple all'annuale conferenza per gli sviluppatori (WWDC) nel 2014. [Link](https://www.youtube.com/watch?v=MO7Ta0DvEWA)

Il nuovo linguaggio from Cupertino è stato presumibilmente l'unica cosa degna di nota presentata negli ultimi tempi da Apple (vedi l'ultima terrificante iterazione di [OS X](https://www.apple.com/it/osx/) o il nuovo [Macbook monoporta](https://www.apple.com/it/macbook/): il mondo della programmazione su piattaforme Apple era fondamentalmente dominato dall'amato/odiato Objective-C, linguaggio concepito nella prima metà degli anni '80 dalle menti di Brad Cox e Tom Love nella quasi-ignota [Stepstone](http://en.wikipedia.org/wiki/Stepstone).

<!--break-->

## (dis) Introduzione a Objective-C.

Insieme a C++ (praticamente contemporaneo), Objective-C è stato uno dei primi linguaggi ad implementare, modernamente, il paradigma [Object Oriented].[^1]

Per quanto ricco di funzionalità e caratteristiche notevoli, una su tutte l'essere uno strict-superset di C, Objective-C mi è stato sempre cordialmente antipatico per la sua sintassi non proprio amichevole a prima vista.

Immaginiamo di avere una classe Calcolatrice che ci mette a disposizione semplici metodi con le operazioni matematiche basilari, composta di un *Header (Calcolatrice.h)*:

```objective_c
#import <Foundation/Foundation.h>

@interface Calcolatrice : NSObject

-(int) sommaX: (int)x conY: (int)y;

//Altre dichiarazioni di metodi o variabili d'istanza...


@end
```

e di una *implementazione (Calcolatrice.m)*:
```objective_c
#import "Calcolatrice.h"

@implementation Calcolatrice

-(int) sommaX: (int)x conY: (int)y {
    return x+y;
}

//Implementazioni di altri metodi definiti nell'header...
@end
```

Al fine di testare la classe, sarà necessario implementarne un oggetto relativo e chiamarne il relativo metodo *(main.m)*:

```objective_c
#import <Foundation/Foundation.h>
#import "Calcolatrice.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Calcolatrice *calc = [[Calcolatrice alloc] init];
        int risultato = [calc sommaX:3 conY:4];
        printf("il risultato è: %d",risultato);
    }
    return 0;
}
```

*Odioso, no?*

## Il passaggio a Swift.

Già a partire dalla sua introduzione, Swift è stato definito come "Objective-C senza C". L'idea di Chris Lattner (e ovviamente del team Apple al lavoro sul progetto) era: facciamo un linguaggio di programmazione più moderno e dalla sintassi concisa, compatibile con tutto ciò che i nostri colleghi sviluppatori hanno scritto con Objective-C, più sicuro e soprattutto veloce.

## Objective-C senza C

Swift non fa uso di puntatori o di modalità di accesso dirette alla memoria, a differenza di Objective-C, che, nonostante la semplificazione nella gestione delle istanze dell'Automatic Reference Counting, utilizza i puntatori per fare riferimento diretto agli oggetti in memoria.

### Compatibile e veloce

Swift è costruito sulla base dell'infrastruttura [LLVM](http://llvm.org/), acronimo di "Low Level Virtual Machine" (che ormai ha poco a che fare con l'infrastruttura in sé), adottata dal qualche anno anche per Objective-C.
Il codice, quindi, è fortemente ottimizzato per la macchina per il quale lo si compila.
Classi Swift e Objective-C (che girano fondamentalmente sulla stessa runtime) possono essere usate senza troppi problemi nella stessa applicazione (Xcode aiuta creando automaticamente un header "ponte", per rendere accessibili le caratteristiche della classe Swift nel codice Obj-C), utilizzando la classica istruzione di `#import`.

## Ok, grazie, ora possiamo cominciare?

**Certamente!**
Prima di tutto ti serve un [Mac](https://www.youtube.com/watch?v=T8FnACj25xM) con almeno OS X 10.9 e con l'ultima versione di Xcode installata.

Un ottimo punto di partenza sono i cosiddetti **Playground**

Avvia Xcode e fai click su **Get started with a playground**

![Un Playground di XCode](/post-imgs/2015-05-24-swift-intro-vars/playground.png){:class="img-responsive"}

I playground sono degli ambienti REPL (Read-Eval-Print-Loop) che permettono la prototipazione veloce delle applicazioni, un po' come avviene per linguaggi come Python o Ruby con le rispettive console.

Sulla sinistra si scrive il codice, mentre, nella parte destra della finestra è possibile vedere in real-time il risultato delle istruzioni proprie istruzioni.

## Le istruzioni

Una delle prime cose che si nota a partire dall'apertura del vostro nuovo e fiammante Playground è che manca qualcosa a cui noi sviluppatori Java (o Objective-C) siamo più che abituati: i punti e virgola.
In Swift, **la conclusione (quindi la separazione) degli statement per mezzo di punti e virgole è opzionale**, è sufficiente andare a capo (a meno che non si vogliano scrivere più istruzioni sulla stessa riga!).

Il nuovo playground, di default si presenta più o meno così:

```swift
// Playground - noun: a place where people can play

import Cocoa

var str = "Hello, playground"
```

Già a partire da questo esempio, si notano alcuni degli elementi fondamentali più o meno di qualsiasi linguaggio di programmazione:
* Commenti
* Import
* Variabili

## I Commenti

I commenti, come ogni buon programmatore potrà confermare, sono la parte più importante del codice: sono probabilmente la ragione per cui non tutti i nostri colleghi passano il loro tempo libero dall'analista.

In Swift i commenti sono tipicamente C-like.
Ce ne sono di tre tipi:

Su singola riga:
```swift
//Questo è un commento su singola riga
```
Su più righe:
```swift
/* 	Questo è un commento su più
	righe
	ci
	scrivo
	un sacco
	di roba
	interessante e utile
*/
```
Documentale ([Headerdoc](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/HeaderDoc/intro/intro.html#//apple_ref/doc/uid/TP40001215-CH345-SW1), una sorta di JavaDoc):
```swift
/** 
	Descrivo il ruolo di una funzione,
	così, quando la richiamo, Xcode mi ricorda come diavolo funziona
*/
```


h3. Gli Import

Lo statement `import` permette di ottenere l'accesso ad elementi (ad esempio variabili e funzioni) presenti all'esterno del file sul quale si sta lavorando.

La regola ci viene particolamente utile nel momento in cui è necessario utilizzare classi scritte in Objective-C (come le API Cocoa e Cocoa Touch, con i rispettivi NSQualcosa(R)(TM)(C)), i cosiddetti "moduli".

```swift
import Cocoa
import AppKit
```

Una cosa interessante di Swift è che, ad esempio, per effettuare operazioni basilari di IO, oppure con le stringhe, non sono necessarie particolari istruzioni di import.

## Le Variabili

**Finalmente.**

Per chiunque, che come me, è accompagnato nel suo lavoro da un linguaggio tipizzato staticamente (*Java FTW*), l'approccio iniziale a Swift è traumatico.

Sì, perché Swift, come un po' tutti i *linguaggi radical chic* di oggi, è *tipizzato dinamicamente*.
E non mi dite che non avevate notato quella `var`, universalmente considerata dai programmatori Java come una parolaccia (qualcuno ha detto JavaScript?!?).

Per coloro che vengono, invece, da linguaggi come Ruby e Python, la reazione a questa notizia sconcertante sarà più o meno [così](https://www.youtube.com/watch?v=hF_ceNO5Rus).

Fondamentalmente, dichiaro una qualsivoglia variabile e gli assegno un valore.

```swift
var miaVar = 10
```

Quando una variabile ha un valore, automaticamente assume il tipo del valore, in questo caso, numerico.

```swift
println("Il tipo della variabile è: \(_stdlib_getDemangledTypeName(miaVar))")</code>
```

Con output:
`"Il tipo della variabile è: Swift.Int"`

Ciò significa che, una volta tipizzata, non posso assegnare a quella variabile un valore dal tipo differente da quello con il quale è stata inizializzata:

```swift
miaVar = "Ciao" //Errore! 
```

A differenza di altri linguaggi tipizzati dinamicamente, Swift permette di utilizzare un'annotazione per dichiarare staticamente il tipo di variabile che si desidera utilizzare (evviva!):

```swift
var str : String = "Sono una stringa!"
var num : Int = 1
```

E le costanti?
In Swift, c'è una dichiarazione apposita per le variabili che non cambiano nel tempo, è `let`

```swift
let pi : Double = 3.14
```


## E ora?

Nella prossima puntata:
*La scoperta continua... Classi, oggetti, metodi (che qua si chiamano funzioni), operazioni, l'algoritmo definitivo per il calcolo del senso della vita!*
Il nostro eroe riuscirà a non parlare male o sarcasticamente di qualsiasi cosa che non sia Java[^2]? 

Se vi è piaciuto (o avete odiato) questo articolo, fatemelo sapere con un [tweet](http://www.twitter.com/garambo).

---

[^1]:
	In realtà, il primo linguaggio di programmazione Object-Oriented è considerato Simula 67 (anno millenovecentosesssantasette!), seguito a ruota da Smalltalk, uno dei più influenti linguaggi di programmazione della storia, ad opera di "Alan Kay":http://en.wikipedia.org/wiki/Alan_Kay e soci.

[^2]:
	Sicuramente no.
