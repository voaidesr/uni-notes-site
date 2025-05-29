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

Deci `Orchestrnevirtualăa` permite copiere:

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

Exemplu de RAII: [[Courses/OOP/smart_pointers\|smart pointers]]

Dezavantaj smart pointers: nu putem folosi tipuri de date **covariante**.

### Tipuri de date covariante 

- Într-o clasă de bază, o funcție virtuală poate avea un anumit tip de returnare, de exemplu `Baza*`
- În clasa derivată, când suprascrii acea funcție, poți să returnezi un pointer la o clasă derivată din `Baza` (ex: `Derivata1*`), adică un tip **mai specific**
- Aceasta este o caracteristică a C++ numită **covarianță a tipurilor de returnare**

```cpp
class Baza {
public:
    virtual ~Baza() = default;
    virtual Baza* clone() const = 0; // funcție virtuală pură
};

class Derivata1 : public Baza {
public:
// tip covariant (Derivata1* în loc de Baza*)
    Derivata1* clone() const override {
        return new Derivata1(*this);
    }
    void f() { std::cout << "f der1\n"; }
};

class Derivata2 : public Baza {
public:
    Derivata2* clone() const override {  // tip covariant
        return new Derivata2(*this);
    }
    void g() { std::cout << "g der2\n"; }
};

int main() {
    Baza* b1 = new Derivata1;
    // Derivata1* d1 = b1->clone();  // eroare -> nu este cast explicit
    // chiar daca prin virtual ar apela functia buna, virtual nu poate schimba tipul static al b1->clone(), care, in baza, este Base*
    // b1->f();  // eroare
    delete b1;
    Baza* b2 = new Derivata2;
    Derivata2 d2;
    // Derivata2* d2_1 = b2->clone();  // eroare
    Derivata2* d2_2 = d2.clone();  // ok
    d2_2->g();  // ok
    delete b2;
    delete d2_2;
}
```

### Interfață non-virtuală (NVI) 

Toate clasele derivate au o implementare comună și trebuie să suprascrie doar anumite porțiuni. 
- clasa de bază oferă funcții publice non-virtuale care implementează *interfața*
- aceste funcții invocă alte funcții (usually private sau protected)
- aceste funcții sunt particularizate în clasele derivate (sunt suprascrise) 

```cpp
class Base {
public:
    // NVI
    void process() { // nevirtuala
	    preProcess()
        doProcess();
    }

protected:
    virtual void doProcess() = 0;  // customization point

private:
    void preProcess() {
        // setup code 
    }
};

class Derived1 : public Base {
protected:
    void doProcess() override {
        // customized behavior here 
    }
};

class Derived2 : public Base {
protected:
    void doProcess() override {
        // customized behavior here
    }
};
```

Prin această implementare este mult mai ușor să modificăm structura implementării la nivelul întregii ierarhii.

NVI asigură ca apelul se face doar prin funcția publică ne-virtuală -> clasele derivate nu pot evita implementarea comună dată de clasa de bază. 

- D - din SOLID

### Tratarea excepțiilor în C++ 

- Mecanisme de tratare a erorilor:
	- coduri de eroare 
	- aserțiuni 
	- excepții
	- tipul de date rezultat 

Excepțiile (în C++) pot fi cauzate de:
- în mod implicit de limbaj (*alocare dinamică*) și de funcțiile din `stdlib` (*argumente invalide, erori de conversie*) 
- în mod explicit de noi 

Sintaxă: 
- în bloc `try/catch` prindem excepții aruncate cu  `throw` 
- în fiecare clauză se tratează un anumit tip de eroare

```cpp
try {
// try block
} catch (type1 arg) {
// catch 1
} catch (type2 arg) {
// catch 2
} 
...
catch(typeN arg) {
// catch N 
}
```

>[!important] 
>Tipul argumentului din `catch` arată care bloc este executat.

Dacă nu este generată excepție $\implies$ nu se execută bloc `catch`. 

Instrucțiunile `catch` sunt verificate în ordine, fiind executat primul de tipul erorii.

*Observații*
- `throw` fără `try` $\implies$ eroare
- nu există `catch` care să fie asociat (prin tip) cu `throw`, programul se termină prin `terminate`
- `.terminate()` poate să fie redefinită să facă altceva 

```cpp
void customTerminate() {
    std::cerr << "Custom terminate handler calleds\n";
    std::abort(); 
}

int main() {
    std::set_terminate(customTerminate);

    throw 42;  // No catch, triggers terminate()
    return 0;
}
```

- nu se recomandă folosirea excepțiilor dacă locul unde are loc eroarea este foarte apropiat `catch`-ul asociat

#### Excepții standard 

Toate se moștenesc din `std::exception`.

Multe din ele sunt în `<exception>` sau `<stdexcept>`.

```cpp
try {
        // Code that may throw
    }
    catch (const std::exception& e) {  // Catch any standard exception
        std::cerr << "Caught exception: " << e.what() << std::endl;
    }
```

Unde `.what()` întoarce un `string` care detaliază eroarea. 

#### Aruncarea erorilor din clase de bază/derivate

>[!important]
>Un `catch` pentru tipul de bază va fi executat și pentru un obiect din tipul derivat. 

Deci, `catch`-ul din tipul derivat trebuie pus primul și apoi `catch`-ul de bază. 

```cpp
class B{};
class D : public B {};

int main()
{
	D derived;
	try {
		throw derived;
	} catch (const B& b) {
		cout << "caught base" << endl;
	} catch (const D& d) {
		cout << "caught derived" << endl;
	} // va afisa caught base
}
```

*Când să aruncăm excepții?*

- în constructori și funcții care creează obiecte în care rezultatul ar putea fi invalid 
	- previne construirea unui obiect invalid 
	- execuția sare la primul `catch` care se potrivește 
- atunci când alternativa cu coduri de eroare e mai complicată 
- atunci când codul e mai clar de înțeles cu excepții 
	- separare clară între happy path și bad path

*Ce punem în catch*?
- prindem excepțiile prin referință (chiar cu `const` dacă nu le modificăm)
- încercăm să găsim echilibru între erori generale și specifice 

>[!proof] C++ specific
>C++ permite definirea unei ierarhii de erori de la zero

- nu e recomandat
	- comun e să derivezi din `std::runtime_error` și `std::logic_error`
	- se poate și din `std::exception` dar mai puțin convenient (nu există constructor cu mesaj de eroare) 

```cpp
class MyCustomError : public std::runtime_error {
public:
    explicit MyCustomError(const std::string& message)
        : std::runtime_error(message) {}
};
```

Observații:
- `noexcept` specifier:
  - Se poate specifica dacă o funcție aruncă excepții sau nu.
  - Exemple:

    ```cpp
    void Xhandler1(int test) noexcept;
    void Xhandler2(int test) noexcept(false);
    ```
    
  - `noexcept` înseamnă că funcția **nu aruncă excepții**.
  - `noexcept(false)` înseamnă că funcția **poate arunca excepții**.

- Re-aruncarea unei excepții:
  - Se face cu `throw;` fără a specifica obiectul excepției.
  - Util pentru handleri care tratează erori comune, dar doresc să trimită excepția mai sus în lanț.

- Atenție la `throw err;`:
  - Creează o copie prin valoare a obiectului excepției.
  - Poate cauza **object slicing**, pierzând comportamentul polimorfic.
  - Pentru a păstra polimorfismul, folosește `throw;` fără argumente pentru re-aruncare.