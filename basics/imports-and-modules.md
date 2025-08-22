# Imports e moduli

Per creare un programma "Hello World" in D, è necessario utilizzare gli `import`.
Le istruzioni `import` consentono di accedere alle funzioni e ai tipi pubblici definiti in un **modulo**.

La libreria standard di D, chiamata [Phobos](https://dlang.org/phobos/),
si trova nel **package** `std`.
Per utilizzare i suoi moduli, si usa la sintassi `import std.MODULE`.

È possibile importare selettivamente specifici simboli da un modulo:

    import std.stdio : writeln, writefln;

Questi import selettivi hanno due vantaggi:
- Rendono il codice più leggibile, evidenziando la provenienza dei simboli
- Prevengono conflitti tra simboli con lo stesso nome provenienti da moduli diversi

A differenza di altri linguaggi, in D le istruzioni `import` non devono necessariamente essere posizionate all'inizio del file. Possono essere utilizzate all'interno di funzioni o in qualsiasi altro punto del codice.

## {SourceCode}

```d
void main()
{
    import std.stdio;
    // o: import std.stdio : writeln;
    writeln("Ciao Mondo!");
}
```
