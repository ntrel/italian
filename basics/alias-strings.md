# Alias & Stringhe

Ora che sappiamo cosa sono gli array, abbiamo preso confidenza con `immutable` e abbiamo dato una rapida occhiata ai tipi base, è il momento di introdurre due
nuovi costrutti in una riga:

    alias string = immutable(char)[];

Il termine `string` è definito tramite un'istruzione `alias` che lo dichiara
come una slice di `immutable(char)`. Questo significa che, una volta creata, una `string`
non potrà mai essere modificata. Ed è proprio questa la seconda novità importante:
benvenute stringhe UTF-8!

Grazie alla loro immutabilità, le `string` possono essere condivise in modo sicuro tra
thread diversi. Inoltre, essendo slice, è possibile estrarne porzioni senza
allocare nuova memoria. Ad esempio, la funzione standard
[`std.algorithm.splitter`](https://dlang.org/phobos/std_algorithm_iteration.html#.splitter)
divide una stringa in base ai caratteri di accapo senza effettuare alcuna allocazione.

Oltre alla `string` UTF-8, ci sono altri due tipi:

    alias wstring = immutable(wchar)[]; // UTF-16
    alias dstring = immutable(dchar)[]; // UTF-32

Le varianti possono essere convertite facilmente tra loro usando
il metodo `to` da `std.conv`:

    dstring myDstring = to!dstring(myString);
    string myString   = to!string(myDstring);

### Stringhe Unicode

Una `string` è definita come un array di [code unit](http://unicode.org/glossary/#code_unit) Unicode a 8 bit.
Tutte le operazioni disponibili per gli array possono essere applicate alle stringhe, ma agiranno a livello di code unit
e non di carattere. Gli algoritmi della libreria standard, invece, interpretano le `string` come sequenze
di [code point](http://unicode.org/glossary/#code_point). È anche possibile trattarle come sequenze di
[grafemi](http://unicode.org/glossary/#grapheme) utilizzando esplicitamente
[`std.uni.byGrapheme`](https://dlang.org/library/std/uni/by_grapheme.html).

Questo piccolo esempio illustra la differenza di interpretazione:

    string s = "\u0041\u0308"; // Ä

    writeln(s.length); // 3

    import std.range : walkLength;
    writeln(s.walkLength); // 2

    import std.uni : byGrapheme;
    writeln(s.byGrapheme.walkLength); // 1

Qui la lunghezza effettiva dell'array `s` è 3, perché contiene 3 code unit:
`0x41`, `0x03` e `0x08`. Gli ultimi due definiscono un singolo code point
(carattere diacritico combinante) e
[`walkLength`](https://dlang.org/library/std/range/primitives/walk_length.html)
(funzione della libreria standard per calcolare la lunghezza di un range arbitrario) conta due code
point in totale. Infine, `byGrapheme` esegue calcoli piuttosto costosi
per riconoscere che questi due code point si combinano in un singolo carattere
visualizzato.

L'elaborazione corretta dell'Unicode può essere molto complicata, ma la maggior parte delle volte gli
sviluppatori D possono semplicemente considerare le variabili `string` come array magici di byte e
affidarsi agli algoritmi della libreria standard per fare il lavoro giusto.
Se si desidera l'iterazione per elemento (code unit), si può usare
[`byCodeUnit`](http://dlang.org/phobos/std_utf.html#.byCodeUnit).

L'auto-decodifica in D è spiegata più in dettaglio
nel [capitolo sulle gemme Unicode](gems/unicode).

### Stringhe multi-linea

Le stringhe in D possono sempre estendersi su più righe:

    string multiline = "
    Questo
    potrebbe essere un
    lungo documento
    ";

Quando appaiono virgolette nel documento, si possono usare le stringhe Wysiwyg (vedi sotto) o
le [stringhe heredoc](http://dlang.org/spec/lex.html#delimited_strings).

### Stringhe Wysiwyg

È anche possibile utilizzare stringhe grezze per minimizzare il laborioso escaping
di simboli riservati. Le stringhe grezze possono essere dichiarate usando sia i backtick (`` `
... ` ``) sia il prefisso r(aw) (`r" ... "`).

    string raw  =  `raw "string"`; // raw "string"
    string raw2 = r"raw `string`"; // raw `string`

D fornisce ancora più modi per rappresentare le stringhe - non esitare
a [esplorarli](https://dlang.org/spec/lex.html#string_literals).

### Approfondimenti

- [Gemma Unicode](gems/unicode)
- [Caratteri in _Programming in D_](http://ddili.org/ders/d.en/characters.html)
- [Stringhe in _Programming in D_](http://ddili.org/ders/d.en/strings.html)
- [std.utf](http://dlang.org/phobos/std_utf.html) - Algoritmi di codifica/decodifica UTF
- [std.uni](http://dlang.org/phobos/std_uni.html) - Algoritmi Unicode
- [String Literals nelle specifiche D](http://dlang.org/spec/lex.html#string_literals)

## {SourceCode}

```d
import std.stdio : writeln, writefln;
import std.range : walkLength;
import std.uni : byGrapheme;
import std.string : format;

void main() {
    // format genera una stringa usando una
    // sintassi simile a printf. D permette la
    // gestione nativa delle stringhe UTF!
    string str = format("%s %s", "Hellö",
        "Wörld");
    writeln("My string: ", str);
    writeln("Array length (code unit count)"
        ~ " of string: ", str.length);
    writeln("Range length (code point count)"
        ~ " of string: ", str.walkLength);
    writeln("Character length (grapheme count)"
        ~ " of string: ",
        str.byGrapheme.walkLength);

    // Le stringhe sono semplicemente array
    // normali, quindi qualsiasi operazione che
    // funziona sugli array funziona anche qui!
    import std.array : replace;
    writeln(replace(str, "lö", "lo"));
    import std.algorithm : endsWith;
    writefln("Does %s end with 'rld'? %s",
        str, endsWith(str, "rld"));

    import std.conv : to;
    // Converti in UTF-32
    dstring dstr = to!dstring(str);
    // .. che ovviamente appare uguale!
    writeln("My dstring: ", dstr);
}
```
