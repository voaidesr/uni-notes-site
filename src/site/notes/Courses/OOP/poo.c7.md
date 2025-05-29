---
{"dg-publish":true,"permalink":"/courses/oop/poo-c7/"}
---

### Destructori și virtualizare

În general, două situații
- **Destructor virtual și public**
	- avem și alte funcții virtuale, folosim pointeri către bază 
- **Destructor protected și non-virtual**
	- dacă folosim doar obiecte derivate
#### Destructori protected și non-virtuali 

Utilizați dacă vrem să ținem într-o clasă de bază comună funcții și atribute, dar construim doar obiecte derivate și nu avem nevoie de pointeri/referință la bază. 

Nu este *public* pentru a preveni instanțierea bazei din exterior (nu avem object slicing). 

```cpp
class Base
{
protected:
    ~Base() = default;
};

int main()
{
    Base base; // eroare de compilare, nu se poate instanta
    Base* base1; // merge, doar se aloca pointer
}
```

#### Problemă

```cpp
class Instrument
{
public:
    virtual ~Instrument() = 0;
    virtual void play() const = 0;
};

Instrument::~Instrument() {}

class Violin : public Instrument
{
public:
    void play() const override;
};

class Drums: public Instrument
{
public:
    void play() const override;
};

class Orchestra
{
    std::vector<Instrument*> instruments;
public:
    void add(Instrument* instrument);
    void rehearse() const;
};

void Orchestra::add(Instrument* instrument)
{
    instruments.push_back(instrument);
}

void Orchestra::rehearse() const
{
    for (const auto& instrument : instruments)
    {
        instrument->play();
    }
}

int main()
{
    Orchestra o1;
    o1.add(new Drums);
    o1.add(new Violin);
    o1.rehearse();
    return 0;
}
```

Prima problemă -> memory leaks, nu se dezalocă niciodat pointerii alocați în `Orchestra`.

*idee* -> destructor pentru Orchestra, care dezalocă pointerii. Duce la altă problemă, la copiere implicită de obiecte , se face shallow copy (sunt copiate adresele pointerilor), lucru ce va duce la ștergerea acelorași zone din memorie de mai multe ori (*nu e eroare de compilare, dar risky*)

idee -> constructor de copiere, cu `static_cast` sau `dynamic_cast`

```cpp
if (dynamic_cast<Violin*>(ptr)) return new Violin(*ptr);
else if (dynamic_cast<Drums*>(ptr)) return new Drums(*ptr);
```

Very bad design 
- pentru fiecare care clasă derivată nouă, trebuie modificat codul clasei de bază -> recompilarea tuturor claselor 
- multe blocuri if/else 

***Soluție 1***

Nu se mai permit copieri, se folosesc doar mutări. Doar un obiect `Orchestra` deține obiectele. 

Disable copy: 

```cpp
Orchestra(const Orchestra&) = delete;
Orchestra& operator=(const Orchestra&) = delete;
```

Allow move operations

```cpp
Orchestra& operator=(const Orchestra& other) = delete;
Orchestra(Orchestra&& other) = default;
Orchestra& operator=(Orchestra&& other) = default;
```

***Soluție 2***

Funcție de `clone`, fiecare subclasă ar trebui să știe să se copieze pe sine.

```cpp
class Instrument
{
public:
    virtual ~Instrument();
    virtual void play() const = 0;
    virtual Instrument* clone() const = 0;
}

class Violin : public Instrument
{
public:
    void play() const override;
    Instrument* clone() const override;
};

Instrument* Violin::clone() const
{
    return new Violin(*this);
}
```

Deci `Orchestra` permite copiere:

```cpp
Orchestra::Orchestra(const Orchestra& other)
{
    for (const auto& instrument : other.instruments)
    {
        instruments.push_back(instrument->clone());
    }
}
```

Și destructor:

```cpp
Orchestra::~Orchestra()
{
    for (const auto& instrument : instruments)
    {
        delete instrument;
    }
}
```

### Regula celor trei 

Dacă într-o clasă trebuie să suprascriem **constructor de copiere**, **operatorul `=`** sau **destructor**, cel mai probabil trebuie suprascrise **toate** cele trei funcții. 

#### Copy and swap 

Pentru a evita starea invalidă, operatorul de atribuire trebuie să folosească **copy-and-swap** 
- mai întâi se face copie a obiectului `other`
- se schimbă conținutul cu copia `swap` 
- astfel, dacă copierea eșuează, obiectul rămâne neschimbat

```cpp
Orchestra& Orchestra::operator=(const Orchestra& other)
{
    if (this == &other)
        return *this;
    auto copie = other; // constructor copiere
    std::swap(instruments, copie.instruments);
    return *this;
} // aici destructor copie
```

Partea de `swap` poate fi refolosită, motiv pentru care se poate defini o funcție separată:
```cpp
friend void swap(Orchestra& o1, Orchestra& o2) {
    using std::swap;
    swap(o1.instruments, o2.instruments);
}
```

### Resource acquisition is initialization (RAII)

Ideea fundamentală de gestionare a resurselor în C++ e că resursele ar trebui alocate doar în constructori și dealocate doar în destructori.

*De ce?* -> pentru că destructorii și constructorii sunt apelați automat de limbaj. Dacă nu se fac alocări/dezalocări, memory leak-urile sunt imposibile. 

Nu există garbage collection, RAII este suficient. 

Exemplu de RAII: *smart pointers*.
