# Cicli

D fornisce quattro costrutti per i cicli.

### 1) `while`

I cicli `while` eseguono il blocco di codice dato
finché una certa condizione è soddisfatta:

    while (condizione)
    {
        foo();
    }

### 2) `do ... while`

I cicli `do .. while` eseguono il blocco di codice dato
finché una certa condizione è soddisfatta, ma a differenza del `while`
il _blocco del ciclo_ viene eseguito prima che la condizione del ciclo
venga valutata per la prima volta.

    do
    {
        foo();
    } while (condizione);

### 3) Ciclo `for` classico

Il ciclo `for` classico
con _inizializzatore_, _condizione del ciclo_ e _istruzione del ciclo_:

    for (int i = 0; i < arr.length; i++)
    {
        ...
    }

### 4) `foreach`

Il ciclo [`foreach`](basics/foreach) che verrà introdotto più in dettaglio
nella prossima sezione:

    foreach (el; arr)
    {
        ...
    }

#### Parole chiave speciali ed etichette

La parola chiave speciale `break` interrompe immediatamente il ciclo corrente.
In un ciclo annidato, un'_etichetta_ può essere usata per uscire da qualsiasi ciclo esterno:

    outer: for (int i = 0; i < 10; ++i)
    {
        for (int j = 0; j < 5; ++j)
        {
            ...
            break outer;

La parola chiave `continue` passa alla prossima iterazione del ciclo.

### Approfondimenti

- Ciclo `for` in [_Programming in D_](http://ddili.org/ders/d.en/for.html), [specifiche](https://dlang.org/spec/statement.html#ForStatement)
- Ciclo `while` in [_Programming in D_](http://ddili.org/ders/d.en/while.html), [specifiche](https://dlang.org/spec/statement.html#WhileStatement)
- Ciclo `do-while` in [_Programming in D_](http://ddili.org/ders/d.en/do_while.html), [specifiche](https://dlang.org/spec/statement.html#do-statement)

## {SourceCode}

```d
import std.stdio : writeln;

/*
Calcola la media degli
elementi di un array.
*/
double average(int[] array)
{
    immutable initialLength = array.length;
    double accumulator = 0.0;
    while (array.length)
    {
        // questo potrebbe essere fatto anche
        // con .front
        // con import std.array : front;
        accumulator += array[0];
        array = array[1 .. $];
    }

    return accumulator / initialLength;
}

void main()
{
    auto testers = [ [5, 15], // 20
          [2, 3, 2, 3], // 10
          [3, 6, 2, 9] ]; // 20

    for (auto i = 0; i < testers.length; ++i)
    {
      writeln("La media di ", testers[i],
        " = ", average(testers[i]));
    }
}
```
