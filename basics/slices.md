# Slice

Le slice sono oggetti di tipo `T[]` per qualsiasi tipo `T`.
Rappresentano una vista su un sottoinsieme di un array di valori `T`, oppure sull'intero array.
**In D, le slice e gli array dinamici sono la stessa cosa.**

Una slice è composta da due elementi:
- Un puntatore all'elemento iniziale
- La lunghezza della slice:

    T* ptr;
    size_t length; // unsigned 32 bit su sistemi a 32bit, unsigned 64 bit su sistemi a 64bit

### Creazione di una slice tramite allocazione

Quando si crea un nuovo array dinamico, viene restituita una slice che punta alla memoria appena allocata:

    auto arr = new int[5];
    assert(arr.length == 5); // la memoria è referenziata in arr.ptr

In questo caso, la memoria allocata è gestita automaticamente dal garbage collector. La slice funge da "vista" sugli elementi sottostanti.

### Creazione di una slice su memoria esistente

È possibile ottenere una slice che punta a memoria già esistente utilizzando l'operatore di slicing. Questo operatore può essere applicato a:
- Un'altra slice
- Array statici
- Struct/classi che implementano `opSlice`
- Altre entità compatibili

Nell'espressione `origin[start .. end]`, l'operatore di slicing crea una vista di tutti gli elementi di `origin` da `start` fino all'elemento _precedente_ a `end`:

    auto newArr = arr[1 .. 4]; // l'indice 4 NON è incluso
    assert(newArr.length == 3);
    newArr[0] = 10; // modifica newArr[0] che corrisponde ad arr[1]

Le slice creano una nuova vista sulla memoria esistente, *non* una copia dei dati. La memoria verrà liberata dal garbage collector solo quando nessuna slice mantiene più riferimenti ad essa o a una sua parte.

Grazie alle slice, è possibile scrivere codice molto efficiente per operazioni che lavorano su blocchi di memoria (come i parser), accedendo solo alle porzioni di interesse senza necessità di nuove allocazioni.

Come visto nella [sezione precedente](basics/arrays), `[$]` è una sintassi abbreviata per `arr.length`. Quindi `arr[$]` tenta di accedere all'elemento oltre la fine della slice, generando un `RangeError` (se i controlli dei limiti sono attivi).

### Approfondimenti

- [Introduzione alle Slice in D](http://dlang.org/d-array-article.html)
- [Slice in _Programming in D_](http://ddili.org/ders/d.en/slices.html)

## {SourceCode}

```d
import std.stdio : writeln;

void main()
{
    int[] test = [ 3, 9, 11, 7, 2, 76, 90, 6 ];
    test.writeln;
    writeln("Primo elemento: ", test[0]);
    writeln("Ultimo elemento: ", test[$ - 1]);
    writeln("Escludi i primi due elementi: ",
        test[2 .. $]);

    writeln("Le slice sono viste sulla "
        ~ "memoria:");
    auto test2 = test;
    auto subView = test[3 .. $];
    // incrementa ogni elemento di 1
    test[] += 1;
    test.writeln;
    test2.writeln;
    subView.writeln;

    // Crea una slice vuota
    assert(test[2 .. 2].length == 0);
}
```
