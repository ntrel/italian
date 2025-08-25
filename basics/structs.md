# Struct

Un modo per definire tipi composti o personalizzati in D è utilizzare una `struct`:

    struct Persona {
        int eta;
        int altezza;
        float etaXAltezza;
    }

Per impostazione predefinita, le `struct` vengono costruite sullo stack (a meno che non vengano create
con `new`) e vengono copiate **per valore** nelle assegnazioni o
come parametri nelle chiamate di funzione.

    auto p = Persona(30, 180, 3.1415);
    auto t = p; // copia

Quando viene creato un nuovo oggetto di tipo `struct`, i suoi membri possono essere inizializzati
nell'ordine in cui sono definiti nella `struct`. Un costruttore personalizzato può essere definito attraverso
una funzione membro `this(...)`. Se necessario per evitare conflitti di nomi, l'istanza corrente
può essere esplicitamente acceduta con `this`:

    struct Persona {
        this(int eta, int altezza) {
            this.eta = eta;
            this.altezza = altezza;
            this.etaXAltezza = cast(float)eta * altezza;
        }
            ...

    Persona p = Persona(30, 180); // inizializzazione
    p = Persona(30, 180);  // assegnazione a nuova istanza

Una `struct` può contenere un numero qualsiasi di funzioni. Per impostazione predefinita
sono `public` e accessibili dall'esterno. Potrebbero anche essere
`private` e quindi chiamabili solo da altre funzioni della stessa
`struct` o da altro codice nello stesso modulo.

    struct Persona {
        void faiQualcosa() {
            ...
        private void faiQualcosaDiPrivato() {
            ...

    // In un altro modulo:
    p.faiQualcosa(); // chiama il metodo faiQualcosa
    p.faiQualcosaDiPrivato(); // proibito

### Funzioni membro const

Se una funzione è dichiarata con `const`, non potrà
modificare nessuno dei membri della struttura. Questo è garantito dal compilatore.
Rendere una funzione `const` la rende chiamabile su qualsiasi oggetto
`const` o `immutable`, ma garantisce anche ai chiamanti che
la funzione membro non cambierà mai lo stato dell'oggetto.

### Funzioni membro statiche

Se una funzione membro è dichiarata come `static`, sarà chiamabile
senza un oggetto istanziato (es. `Persona.miaFunzioneStatica()`) ma non
potrà accedere a membri non-`static`. Può essere usata se un
metodo non ha bisogno di accedere ai campi membro dell'oggetto ma logicamente
appartiene alla stessa classe. Può anche essere usata per fornire funzionalità
senza creare un'istanza esplicita, per esempio, alcune implementazioni del pattern
Singleton usano `static`.

### Ereditarietà

Nota che una `struct` non può ereditare da un'altra `struct`.
Le gerarchie di tipi possono essere costruite solo usando le classi,
che vedremo in una sezione successiva.
Tuttavia, con `alias this` o `mixin` si può facilmente ottenere
l'ereditarietà polimorfica.

### Approfondimenti

- [Struct in _Programming in D_](http://ddili.org/ders/d.en/struct.html)
- [Specifiche Struct](https://dlang.org/spec/struct.html)

### Esercizio

Data la `struct Vector3`, implementa le seguenti funzioni e fai
funzionare correttamente l'applicazione di esempio:

* `length()` - restituisce la lunghezza del vettore
* `dot(Vector3)` - restituisce il prodotto scalare di due vettori
* `toString()` - restituisce una rappresentazione in stringa di questo vettore.
  La funzione [`std.string.format`](https://dlang.org/phobos/std_format.html)
  restituisce una stringa usando una sintassi simile a `printf`:
  `format("MioInt = %d", mioInt)`. Le stringhe saranno spiegate in dettaglio in una
  sezione successiva.

## {SourceCode:incomplete}

```d
struct Vector3 {
    double x;
    double y;
    double z;

    double length() const {
        import std.math : sqrt;
        // DA FARE: implementare la lunghezza
        return 0.0;
    }

    // rhs verrà copiato
    double dot(Vector3 rhs) const {
        // DA FARE: implementare il prodotto
        // scalare
        return 0.0;
    }
}

void main() {
    auto vec1 = Vector3(10, 0, 0);
    Vector3 vec2;
    vec2.x = 0;
    vec2.y = 20;
    vec2.z = 0;

    // Se una funzione membro non ha parametri,
    // le parentesi () possono essere omesse
    assert(vec1.length == 10);
    assert(vec2.length == 20);

    // Testa la funzionalità del prodotto
    // scalare
    assert(vec1.dot(vec2) == 0);

    // 1 * 1 + 2 * 1 + 3 * 1
    auto vec3 = Vector3(1, 2, 3);
    assert(vec3.dot(Vector3(1, 1, 1)) == 6);

    // 1 * 3 + 2 * 2 + 3 * 1
    assert(vec3.dot(Vector3(3, 2, 1)) == 10);
}
```
