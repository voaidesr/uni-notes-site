---
{"dg-publish":true,"permalink":"/courses/oop/poo-c8/"}
---

Mare parte din teorie deja tratată în [[Courses/OOP/poo.c7\|cursul 7]].

# Completări 

>[!important] Excepții în funcții
>La definiția unei funcții se poate preciza lista tipurilor de excepții care pot fi generate în cadrul funcției. 
>```cpp
>void foo(int test) throw(int, char, ...)
>```


Pentru a fi siguri că orice excepție este prinsă, fără a ști însă tipul:

```cpp
try {
// ...
} catch (...) {
// prinde orice excepție care nu a fost prinsă până acum
}
```

#eroare 

>[!danger] Eroare posibilă
>`catch(...)` trebuie să fie ultimul handler, altfel nu compilează 
>- dacă sun mai multe handlere `catch(...)` nu compilează din exact același motiv

