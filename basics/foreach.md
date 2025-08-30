# Foreach

{{#img-right}}dman-teacher-foreach.jpg{{/img-right}}

D dispone di un ciclo `foreach` che permette un'iterazione
più leggibile e meno soggetta a errori.

### Iterazione sugli elementi

Dato un array `arr` di tipo `int[]`, è possibile
iterare sugli elementi usando un ciclo `foreach`:

    foreach (int e; arr) {
        writeln(e);
    }

Il primo parametro in un ciclo `foreach` è il nome della variabile
usata per ogni iterazione. Il suo tipo può essere dedotto automaticamente così:

    foreach (e; arr) {
        // typeof(e) è int
        writeln(e);
    }

Il secondo campo deve essere un array - o uno speciale oggetto iterabile
chiamato **range** che verrà introdotto nella [prossima sezione](basics/ranges).

### Accesso per riferimento

Gli elementi verranno copiati dall'array o dal range durante l'iterazione.
Questo è accettabile per i tipi base, ma potrebbe essere un problema per
tipi più grandi. Per prevenire la copia o per consentire la *modifica in-place*,
si può usare `ref`:

    foreach (ref e; arr) {
        e = 10; // sovrascrive il valore
    }

### Iterare `n` volte

D ci permette di scrivere iterazioni che devono essere eseguite
`n` volte in modo più conciso con la sintassi `..`:

    foreach (i; 0 .. 3) {
        writeln(i);
    }
    // 0 1 2

L'ultimo numero in `a .. b` è escluso dal range,
quindi il corpo del ciclo viene eseguito `3` volte.

### Iterazione con contatore indice

Per gli array, è anche possibile accedere a una variabile indice separata.

    foreach (i, e; [4, 5, 6]) {
        writeln(i, ":", e);
    }
    // 0:4 1:5 2:6

### Iterazione inversa con `foreach_reverse`

Una collezione può essere iterata in ordine inverso con
`foreach_reverse`:

    foreach_reverse (e; [1, 2, 3]) {
        writeln(e);
    }
    // 3 2 1

### Approfondimenti

- [`foreach` in _Programming in D_](http://ddili.org/ders/d.en/foreach.html)
- [`foreach` con Struct e Classi _Programming in D_](http://ddili.org/ders/d.en/foreach_opapply.html)
- [Specifiche di `foreach`](https://dlang.org/spec/statement.html#ForeachStatement)

## {SourceCode}

```d
import std.stdio : writefln;

void main() {
    auto arr = [
        [5, 15],      // 20
        [2, 3, 2, 3], // 10
        [3, 6, 2, 9], // 20
    ];

    foreach (i, row; arr)
    {
        double total = 0.0;
        foreach (e; row)
            total += e;

        auto avg = total / row.length;
        writefln("MEDIA [riga=%d]: %.2f", i,
            avg);
    }
}
```
