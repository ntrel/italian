# Array Associativi

D dispone nativamente di *array associativi*, conosciuti anche come hashmap.
Un array associativo con chiavi di tipo `string` e valori di tipo `int`
si dichiara in questo modo:

    int[string] arr;

I valori possono essere inseriti e acceduti utilizzando la loro chiave:

    arr["key1"] = 10;

Per verificare la presenza di una chiave nell'array associativo, è possibile
utilizzare l'operatore `in`:

    if ("key1" in arr)
        writeln("Yes");

L'operatore `in` restituisce un puntatore al valore se questo esiste,
altrimenti restituisce un puntatore `null`. Questo permette di combinare
in modo elegante il controllo dell'esistenza con l'assegnazione:

    if (auto val = "key1" in arr)
        *val = 20;

L'accesso a una chiave che non esiste genera un `RangeError`
che termina immediatamente l'applicazione. Per un accesso sicuro
con un valore predefinito, si può utilizzare `get(key, defaultValue)`.

Gli array associativi hanno la proprietà `.length` come gli array normali e forniscono
un membro `.remove(key)` per rimuovere le voci tramite la loro chiave.
È lasciato come esercizio al lettore esplorare
i range speciali `.byKey` e `.byValue`.

### Approfondimenti

- [Array associativi in _Programming in D_](http://ddili.org/ders/d.en/aa.html)
- [Specifiche degli array associativi](https://dlang.org/spec/hash-map.html)
- [std.array.byPair](http://dlang.org/phobos/std_array.html#.byPair)

## {SourceCode}

```d
import std.array : assocArray;
import std.algorithm.iteration: each, group,
    splitter, sum;
import std.string: toLower;
import std.stdio : writefln, writeln;

void main()
{
    string text = "Rock D with D";

    // Itera su tutte le parole e conta
    // ogni parola una volta
    int[string] words;
    text.toLower()
        .splitter(" ")
        .each!(w => words[w]++);

    foreach (key, value; words)
        writefln("key: %s, value: %d",
                       key, value);

    // `.keys` e `.values` restituiscono array
    writeln("Words: ", words.keys);

    // `.byKey`, `.byValue` e `.byKeyValue`
    // restituiscono range pigri e iterabili
    writeln("# Words: ", words.byValue.sum);

    // Un nuovo array associativo può essere
    // creato con `assocArray` passando un
    // range di tuple chiave/valore
    auto array = ['a', 'a', 'a', 'b', 'b',
                  'c', 'd', 'e', 'e'];

    // `.group` raggruppa elementi consecutivi
    // equivalenti in una singola tupla
    // contenente l'elemento e il numero delle
    // sue ripetizioni
    auto keyValue = array.group;
    writeln("Key/Value range: ", keyValue);
    writeln("Associative array: ",
             keyValue.assocArray);
}
```
