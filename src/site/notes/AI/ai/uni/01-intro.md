---
{"dg-publish":true,"permalink":"/ai/ai/uni/01-intro/"}
---

>[!note] Testul Turing 
>Un computer prezintă un nivel de inteligență uman dacă un om nu poate să distingă, în urma unei conversații purtată în limbaj natural, dacă vorbește cu un om sau cu un calculator. 

Învățarea automată respectă paradigma statistică - dacă nu știm regulile, putem rezolva probleme învățând din date. 

- strong/true AI/AGI = inteligență la nivel uman 
- weak/narrow AI = se concentrează pe o anumită problemă 

Se reduce la învățarea unei funcții $f$ care 

$$
f : X \to Y
$$
Numită funcție target, prin aproximarea ei cu 

$$
g : X \to Y
$$
Un model. 

## Paradigme ale învățării 

### Învățare Supervizată 

>[!note]
>clasificare = a găsi clase discrete pentru fiecare input 
>regresie = a găsi un rezultat continuu pentru fiecare input

Se folosește de obiecte etichetate pentru a învăța un mod de a eticheta. 

Un exemplu este detectarea unei fețe. Se poate plimba o fereastră în imagine și problema se reduce la a clasifica fiecare fereastră în dacă conține sau nu o față. 

#### Pași

1. modelarea problemei 
2. colectarea unor date corect clasificate 
3. reprezentarea datelor (cum le reprezentam)
4. modelarea: alegerea unui spațiu de ipoteze $H = \{g : X \to Y \}$
5. învățarea: găsirea celor mai buni parametri din spațiul de ipoteze 
6. încercarea mai multor modele și alegerea celui mai bun

### Învățare nesupervizată

Se folosește de obiecte care nu sunt etichetate. 

Formele canonice sunt 

- grupare/clustering 
- reducerea dimensiunii 

### Învățare semi-supervizată 

Avem exemple de obiecte, din care unele sunt etichetate iar altele nu. 

### Învățare prin întărire / reinforcement learning 

Sistemul învață pe baza unei recompense (reinforcement signal).

Se poate formaliza cu procese Markov.(a concept used do describe a system in which we can transition from a state to another, and the system is memoryless (the future depends only on the present.))

### Învățarea activă 

Set mare de exemple, din care alegem un subset mult mai mic pe care să îl etichetăm pentru a obține un clasificator cât mai bun.

### Învățarea prin transfer 

Se folosește un model preantrenat pe o anumită problemă pentru a test




