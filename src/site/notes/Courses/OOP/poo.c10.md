---
{"dg-publish":true,"permalink":"/courses/oop/poo-c10/"}
---

# Standard Template Library (STL)

Bibliotecă de clase, parte din standard library.

Oferă structuri de date și algoritmi fundamentali. Oferă componente generice, parametrizabile (aproape toate clasele din STL sunt parametrizabile -> [[Courses/OOP/poo.c9#Șabloane\|templates]]).

STL conține clase pentru:
- containere 
- iteratori
- algoritmi
- functori (function objects) 
- allocators 

## Container 

Tipuri de container: 
- de tip secvență (listă liniară)
	- vector 
	- deque 
	- list 
- asociativi (regăsirea informației pe bază de chei) 
	- set 
	- multiset 
	- map 
	- multimap 
- adaptor de containere 
	- stack 
	- queue 
	- priority_queue 

### Vector 

```cpp 
template <class T, class Allocator = allocator<T>> class vector
```

`T` = tipul de date. `Allocator` = tipul de alocator utilizat (de obicei cel standard). 
#### Constructori 

Utilizarea constructorilor pentru `vector`. 

```cpp 
vector<int> iv; // vector de int cu 0 elemente 

vector<int> iv(10); // vector de int cu 10 elemente, se initializeaza cu 0

vector<int> iv(10, 7); // vector de int cu 10 elemente, fiecare egal cu 7 

vector<int> iv2(iv); // vector de int reprezentand copia lui iv 

////
list<int> L = {1, 2, 3, 4};
vector<int> v_din_list(L.begin(), L.end()); 
```

>[!warning] resize 
>Pentru accesarea/inserarea valorilor cu indice: 
>Pentru un vector declarat cu dimensiune fixă, trebuie schimbată dimensiunea cu `resize()`. Nu cauzează eroare, dar valorile nu sunt luate în considerare pentru un indice >= `v.size()`. 
>
>Dacă se lucrează cu `push_back()` `size`-ul e este schimbat automat.

```cpp
template <typename T>
void print(const vector<T>& v) {
    for(int i = 0; i < v.size(); i++)
        cout << v[i] << " ";
    cout << endl;
}

int main() {
    vector<int> v(3);
    print(v); // 0 0 0
    v[0] = 3;
    v[1] = 4;
    v[2] = 5;
    v[3] = 7;
    print(v); // 3 4 5 (7 nu e stocat in vector)
    v.resize(5);
    v[3] = 7; 
    print(v); // 3 4 5 7 0
    return 0;
}
```

Se pot insera/șterge elemente și cu iteratori. 

```cpp 
vector<int> v(10);
vector<int>::iterator it;
int i = 0;
for(it = v.begin(); it != v.end(); it++, i++)
	*it = i;
print(v); // 0 1 ... 9
```


Insert 

```cpp
it = v.begin() + 2;
v.insert(it, 3, 999); // insereaza 3 de 999 inainte de al treilea element din v
```

Erase 

Șterge elementul de la o poziție marcată printr-un iterator, sau între doi iteratori. 

### Listă 

Implementată ca listă dublu înlănțuită. 

Constructori 

```cpp 
list<int> iv;

list<int> iv(10);

list<int> iv(10, 7);

list<int> iv2(iv); // copie 

list<int> iv3(iv.begin(), iv.end());
```

>[!note] Specific list 
>Pot fi merge-uite `list<T>::merge(list<T>& other);`
>Nu pot fi accesate prin indice. 

#### Apelare constructori 

```cpp
class Test {
    int i;
public:
    Test(int x = 0) : i(x) { cout << "C" << x << " "; }
    Test(const Test& x) { i = x.i, cout << "CC" << i << " "; }
    ~Test() { cout << "D" << i << " ";}
};

int main() {
    list<Test> v(3);
    cout << endl;
    v.push_back(10);
    cout << endl;
    v.push_back(11);
    cout << endl;
    v.push_back(12);
    cout << endl;
    Test ob(13);
    v.push_back(ob);
    cout << endl;
}
```

Va afișa 

```
C0 C0 C0 // se construiesc atunci cand se initializeaza lista
C10 CC10 D10 // se construieste un obiect temporar, este copiat in lista, se sterge obiectul temporar
C11 CC11 D11 
C12 CC12 D12 
C13 CC13 
D13 D0 D0 D0 D10 D11 D12 D13 // de-abia la final sunt șterse elementele din lista
```

### Deque 

- Elementele sunt stocate în blocuri de memorie 
- Elementele pot fi adăugate/șterse eficient la ambele capete 

### Adaptor de container

În capsulează un container de tip secvență și folosesc acest obiect pentru a oferi funcționalități specifice container-ului. 

#### Stack 

```cpp 
template <class T, class Container = deque<T>> class stack;
```

```cpp
stack<int, vector<int>> s;  // primul parametru, tipul elementelor 
							// al doilea parametru, modul de stocare
```

Cu clasa `Test` definită anterior putem arăta importanța celui de-al doilea parametru:

```cpp
int main() {
	stack<Test> s;
	cout << endl;
	s.push(1);
	cout << endl;
	s.push(2);
	cout << endl;
	s.push(3);
	cout << endl;
}
```

Afișează 

```
C1 CC1 D1 // construcția unui obiect temporar, copiere și distrugere
C2 CC2 D2 
C3 CC3 D3 
D1 D2 D3 // distrugerea listei 
```

În acest caz, este folosit implicit `deque<Test>`, care, pentru că lucrează prin blocuri de memorie, nu trebuie decât să găsească memorie pentru a stoca obiectele noi (obiectele anterioare nu trebuie mutate). 

Dacă folosim `vector`

```cpp
int main() {
	stack<Test, vector<Test>> s;
	cout << endl;
	s.push(1);
	cout << endl;
	s.push(2);
	cout << endl;
	s.push(3);
	cout << endl;
}
```

Va afișa 

```
C1 CC1 D1 
C2 CC2 CC1 D1 D2 
C3 CC3 CC1 CC2 D1 D2 D3 
D1 D2 D3 
```

`vector` ține obiectele într-un bloc continuu de memorie. Atunci când este depășit buffer-ul alocat pentru vector, trebuie căutat o nouă zonă de memorie și toate elementele anterioare sunt copiate în această nouă zonă. Acest lucru se întâmplă în incremente de puteri ale lui 2.

Explicarea exemplului anterior
- `C1 CC1 D1` -> Se creează un obiect temporar, se copiază în vector, este șters obiectul temporar
- `C2 CC2 CC1 D1 D2` -> Se creează obiect temporar. Vectorul avea inițial dimensiune $2^0 = 1$, dublăm dimensiunea. Este copiat obiectul temporar 2 în noua locație, este copiat și obiectul din locația veche a vectorului, după care este șters acel obiect și este șters și obiectul temporar. 
- `C3 CC3 CC1 CC2 D1 D2 D3` -> avem $2^1 = 2$ spații în vector, dorim și un al treilea element, noul buffer va avea mărime $2^2 = 4$ (deci pentru al patrulea push nu vor mai fi făcute copieri). Se observă adăugarea noului element, copierea vechilor elemente, ștergerea vechiului vector și a obiectului temporar.
- `D1 D2 D3` -> vectorul este șters 

Dacă am continua, se poate observa doar copierea în pași de $2^n$.

```cpp
int main() {
    stack<Test, vector<Test>> a;
    cout << endl;
    for(int i = 0; i < 16; i ++) {
        a.push(i);
        cout << endl;
    }
}
```

Va afișa 

```
C0 CC0 D0 
C1 CC1 CC0 D0 D1 
C2 CC2 CC0 CC1 D0 D1 D2 
C3 CC3 D3 
C4 CC4 CC0 CC1 CC2 CC3 D0 D1 D2 D3 D4 
C5 CC5 D5 
C6 CC6 D6 
C7 CC7 D7 
C8 CC8 CC0 CC1 CC2 CC3 CC4 CC5 CC6 CC7 D0 D1 D2 D3 D4 D5 D6 D7 D8 
C9 CC9 D9 
C10 CC10 D10 
C11 CC11 D11 
C12 CC12 D12 
C13 CC13 D13 
C14 CC14 D14 
C15 CC15 D15 
D0 D1 D2 D3 D4 D5 D6 D7 D8 D9 D10 D11 D12 D13 D14 D15 
```

