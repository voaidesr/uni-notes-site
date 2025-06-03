---
{"dg-publish":true,"permalink":"/courses/db/bd-l2/"}
---

# Funcții SQL

- Sunt **funcții predefinite** în sistemul Oracle și pot fi folosite în instrucțiuni SQL. A nu se confunda cu instrucțiunile definite de utilizator, scrise în PL/SQL.
- Dacă o funcție SQL este apelată cu un alt tip de date decât cel așteptat, sistemul convertește implicit argumentul înainte să evalueze funcția.
- *De obicei* dacă o funcție SQL e apelată cu argument `null`, atunci ea întoarce `null`.  

>[!tip] Excepție
>Funcțiile care nu urmează ultima regulă sunt `CONCAT`, `NVL`, `REPLACE`.

Pot fi clasificate în următoarele categorii:
- single-row
- multiple-row (funcții agregat)

## Funcții single-row

Returnează câte o singură *linie rezultat* pentru fiecare linie a tabelului sau vizualizării interogate. 

Ele pot apărea în:
- listele de expresii din clauza `SELECT`
- clauzele `WHERE`, `START WITH`, `CONNECT BY` și `HAVING`.

>[!tip]
>Pentru a testa funcțiile, cea mai ușoară metodă este:
>
>```sql
>SELECT functie(...) FROM DUAL;
>```
### Funcții de conversie

| Funcție     | Descriere                                                               | Exemplu                                                                   |
| ----------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `TO_CHAR`   | convertește/formatează un număr/dată calendaristică în șir de caractere | `TO_CHAR(7) = '7'`<br><br>`TO_CHAR(SYSDATE, 'DD-MM-YYYY') = '03-03-2025'` |
| `TO_DATE`   | convertește număr/șir de caractere la dată calendaristică               | `TO_DATE('22-04-2024','DD-MM-YYYY')`                                      |
| `TO_NUMBER` | convertește șir de caractere la număr                                   | `TO_NUMBER('-23.799', 99.999) = -23.799`                                  |
**Observație**

Există două tipuri conversie:
- implicită (realizată de sistem atunci când e necesar)
- explicită (realizată de utilizator prin intermediul funcțiilor de conversie)

*Conversii implicite*

- `VARCHAR2` sau `CHAR` la `NUMBER`
- `VARCHAR2` sau `CHAR` la `DATE`
- și reciprocele 

### Funcții pentru prelucrarea caracterelor 


| Funcție                   | Descriere                                                                                                      | Exemplu                                |
| ------------------------- | -------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| `LENGTH(STR)`             | lungimea unui string                                                                                           | `LENGTH('Informatica') = 11`           |
| `SUBSTR(str, start [,n])` | subșirul unui string care începe la poziția start și are lungime n (opțional). `n` neprecizat -> până la final | `SUBSTR('Informatica', 1, 4) = 'Info'` |
| `LTRIM(str [,chars])`     |                                                                                                                |                                        |



















