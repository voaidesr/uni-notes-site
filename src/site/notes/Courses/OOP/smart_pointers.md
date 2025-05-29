---
{"dg-publish":true,"permalink":"/courses/oop/smart-pointers/"}
---

# Introduction 

Sunt clase template care se comportă precum pointerii normali, dar care gestionează ***automat*** lifetime-ul obiectelor alocate dinamic. 

Urmează principiile [[Courses/OOP/poo.c7#Resource acquisition is initialization (RAII)\|RAII]]

Header-ul `<memory>`.

## Main types of smart pointers


| Type              | Descriere                                               | Ownership        | Use case                                 |
| ----------------- | ------------------------------------------------------- | ---------------- | ---------------------------------------- |
| `std::unique_ptr` | Deține obiectul în mod exclusiv, nu permite copii       | Unique           | Atunci când e nevoie de unique ownership |
| `std::shared_ptr` | Shared ownership with reference counting                | Shared           | Multiple owners share the resource       |
| `std::weak_ptr`   | Non-owning reference to an object owned by `shared_ptr` | Weak -> observer |                                          |
# `unique_ptr`

- doar un `unique_ptr` poate deține obiectul 
- nu poate fi copiat, poate fi **mutat**
- șterge obiectul când iese din scope 

Exemplu: 

```cpp
#include <memory>

std::unique_ptr<int> ptr1 = std::make_unique<int>(10); // pointer unic pt int(10)

std::unique_ptr<int> ptr2 = std::move(ptr1);  // transfer ownership

if(!ptr1) {
    cout << "empty pointer" << endl;
}
```

Utilizare:

```cpp
void foo(){
	std::unique_pointer<MyClass> p = std::make_unique<MyClass>(/* args */);
	// p se poate folosi ca un pointer normal 
	// p->method();
	// *p
	p.get() // -> returns the raw pointer owned by p
	p.release() // -> releases ownership and returns the raw pointer
	// MUST delete the pointer manually 
	p.reset() // -> deletes the object and takes ownership of the pointer
} // se apelează destructorul pt obiectul de tipul MyClass
```

# `shared_ptr`

- mai mulți `shared_ptr` pot deține același obiect 
- are reference counting intern (numără câte referințe există) pentru a ști când să șteargă obiectul 
- obiectul este șters atunci când ultimul `shared_ptr` care îl deține este șters 

Exemplu:

```cpp
#include <memory>

std::shared_ptr<int> p1 = std::make_shared<int>(20);

std::shared_ptr<int> p2 = p1;  // shared ownership

std::cout << p1.use_count();   // 2

```

Utilizare:

```cpp
    std::shared_ptr<MyClass> sp1 = std::make_shared<MyClass>(/* args */);
    std::shared_ptr<MyClass> sp2 = sp1;  // both share ownership
}  // object deleted only after both go out of scope
```

# `weak_ptr`

- nu deține, nu afectează reference count 
- observa obiectul deținut de `shared_ptr`, nu extinde lifetime-ul
- must be converted to `shared_ptr` to access object safely
`
Exemplu: 
``
```cpp
#include <memory>

std::shared_ptr<int> sp = std::make_shared<int>(30);
std::weak_ptr<int> wp = sp;

wp.expired(); // boolean -> returns true if object has expired (no owners)
wp.lock(); // returns shared pointer

if (auto spt = wp.lock()) { // try to get shared_ptr
    // use *spt safely
} else {
    // object no longer exists
}
```