---
{"dg-publish":true,"permalink":"/courses/db/bd-l5/"}
---

# Limbajul de definire a datelor 

Aceste instrucțiuni permit:
- crearea, modificarea și suprimarea (CREATE, ALTER, DROP)
- modificarea numelor (RENAME)
- ștergerea datelor fără suprimarea structurii obiectelor respective (TRUNCATE)

>[!proof] Commit
>Implicit, LDD fac commit.

**Reguli de numire a obiectelor bazei de date**
- Identificatorii obiectelor trebuie să înceapă cu o literă și să aibă maximum 30 de caractere, cu excepția numelui bazei de date care este limitat la 8 caractere și celui al legăturii unei baze de date, a cărui lungime poate atinge 128 de caractere.
- Numele poate conține caracterele A-Z, a-z, 0-9, _, $ și #.
- Două obiecte ale aceluiași utilizator al server-ului Oracle nu pot avea același nume.
- Identificatorii nu pot fi cuvinte rezervate ale server-ului Oracle.
- Identificatorii obiectelor nu sunt case-sensitive.


