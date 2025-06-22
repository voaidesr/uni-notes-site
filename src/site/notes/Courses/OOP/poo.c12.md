---
{"dg-publish":true,"permalink":"/courses/oop/poo-c12/"}
---

# Principiile SOLID 

Sunt 5 principii de proiectare care urmăresc să facă sistemele software mai ușor de înțeles, flexibile și mentenabile. 
## Single-responsibility principle 

>[!tip] S.
O clasă ar trebui să aibă o singură responsabilitate. (Un singur motiv de a fi modificată).

## Open-closed principle 

>[!tip] O.
>Entitățile software ar trebui să fie deschise pentru extindere, dar închise pentru modficare. 

## Liskov substitution principle 

>[!tip] L.
>Subtipurile trebuie să poată înlocui tipurile de bază fără a modifica corectitudinea programului. 

## Interface segregation principle 

>[!tip] I. 
>Niciun client nu trebuie forțat să depindă de interfețe pe care nu le folosește. 

## Dependency inversion principle 

>[!tip] D.
>Modulele de nivel înalt nu trebuie să depindă de nivel scăzut. Ambele trebuie să depindă de abstracțiuni. 

# Șabloane de proiectare 

- o problemă care se întâlnește în mod frecvent în proiectarea programelor 
- soluția generală pentru problema respectivă 

## Clasificare 

1. **După scop:**
	- creaționale - privesc modul de creare al obiectelor 
	- structurale - se referă la compoziția claselor sau a obiectelor
	- comportamentale - caracterizează modul în care obiectele și clasele interacționează și își distribuie responsabilitățile 
2. **După domeniul de aplicare:**
	- șabloanele claselor (relații dintre clase, statice)
	- șabloanele obiectelor (relații dintre obiecte, dinamice)

## Șabloane comportamentale 

### Singleton 

Vrem să asigurăm o singură instanță a unei clase, împărțit de-a lungul programului. 

```cpp 
template <typename T> class Singleton {
	Singleton();
	static T* instance_;
public:
	static T* instance();
};

template <typename T>
T* Singleton::instance_ = 0;

template <typename T>
T* Singleton::instance() {
	if (Singleton<T>::instance_ == 0) {
		Singleton<T>::instance_ = new Singleton<T>;
	}
	return Singleton<T>::instance_;
}
```

### Builder 

Builder-ul permite crearea pas cu pas a unui obiect complex, ascunzând detaliile de inițializare și oferind o interfață pentru configurare.

### Factory 

Factory-ul oferă o metodă unică pentru crearea de obiecte, ascunzând instanțierea claselor concrete și permițând extinderea ușoară cu noi tipuri fără a modifica codul client. 

```cpp 
struct Product {
    virtual ~Product() = default;
    virtual void use() const = 0;
};

// derivate
struct ConcreteProductA : Product {
    void use() const override {
        std::cout << "Folosește produs A\n";
    }
};

struct ConcreteProductB : Product {
    void use() const override {
        std::cout << "Folosește produs B\n";
    }
};

class Factory {
public:
    static std::unique_ptr<Product> create(const std::string& type) {
        if (type == "A") {
            return std::make_unique<ConcreteProductA>();
        } else if (type == "B") {
            return std::make_unique<ConcreteProductB>();
        } else {
            return nullptr;
        }
    }
};
```

## Șabloane comportamentale 

### Observer 

Definește dependența **one-to-many** între obiecte (Subject și Observers) astfel încât, la schimbarea stării Subject-ului, toți observatorii săi să fie notificați automat prin metoda `update()`. Ideal pentru evenimente, interfețe reactive sau orice scenariu unde componentele trebuie să rămână sincronizate.

### Strategy 

Presupune încapsularea separată a fiecărui algoritm dintr-o familie, făcând astfel ca algoritmii respectivi să fie interschimbabili.

