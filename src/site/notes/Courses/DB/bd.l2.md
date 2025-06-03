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
- [[Courses/DB/bd.l2#Funcții single-row\|single-row]]
- [[Courses/DB/bd.l2#Funcțiile multiple-row\|multiple-row (funcții agregat)]]

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


| Funcție                                           | Descriere                                                                                                          | Exemplu                                                                                    |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| `LENGTH(STR)`                                     | lungimea unui string                                                                                               | `LENGTH('Informatica') = 11`                                                               |
| `SUBSTR(str, start [,n])`                         | subșirul unui string care începe la poziția start și are lungime n (opțional). `n` neprecizat -> până la final     | `SUBSTR('Informatica', 1, 4) = 'Info'`                                                     |
| `LTRIM(str [,chars])`                             | șterge din stânga unui string orice caracter care apare în `chars`. Implicit, se șterg spațiile libere.            | `LTRIM('    ab') = 'ab'`<br><br>`LTRIM('aabea', 'ab') = 'ea'`                              |
| `RTRIM(str [,chars])`                             | la fel, dar pe dreapta                                                                                             | -                                                                                          |
| `TRIM(TRAILING\LEADING\BOTH chars FROM expresie)` | elimină caracterele `chars` de la început/final/ambele dintr-un șir.                                               | `TRIM(BOTH 'a' FROM 'abXa') = 'bX')`                                                       |
| `LPAD(str, length [, chars]`                      | adaugă `chars` la stânga șirului până când lungimea ***noului șir*** devine `lenght`                               | `LPAD('info', 6) = '  info'`                                                               |
| `RPAD(str, length [, chars]`                      | la fel, pe dreapta                                                                                                 | -                                                                                          |
| `REPLACE(str1, str2 [, str3])`                    | întoarce `str1` cu toate aparițiile lui `str2` înlocuite cu `str3`. (Implicit se șterg)                            | `REPLACE('b$bb$$', '$', 'a') = 'babbaa')`                                                  |
| `UPPER(str)`                                      | transformă toate literele în majuscule                                                                             | `UPPER('aBna') = 'ABNA'`                                                                   |
| `LOWER(str)`                                      | transformă toate literele în minuscule                                                                             | -                                                                                          |
| `INSTR(str, substr [, start [, n]])`              | caută într-un string, de la `start` a n-a apariție a unui substring.                                               | `INSTR('abcab', 'ab', 1, 2) = '4'`                                                         |
| `ASCII(char)`                                     | codul ASCII al primului caracter al unui șir.                                                                      | `ASCII('abc') = ASCII('a') = 97`                                                           |
| `CHR(n)`                                          | caracterul corespunzător codului ASCII specificat.                                                                 | `CHR(97) = 'a'`                                                                            |
| `CONCAT(str1, str2)`                              | concatenează două șiruri de caractere                                                                              | `CONCAT('ab', 'cd') = 'abcd'`                                                              |
| `TRANSLATE(str, src, dest)`                       | fiecare caracter care apare în string-urile `str` și `src` este transformat în caracterul corespunzător din `dest` | `TRANSLATE('$a$aa', '$', 'b') = 'babaa'`<br><br>`TRANSLATE('$a$aa', '$a', 'bc') = 'bcbcc'` |

### Funcții aritmetice

| Funcția                 | Descriere                            |
| ----------------------- | ------------------------------------ |
| `ABS`                   | modulul unui numar                   |
| `CEIL`                  | partea întreagă + 1                  |
| `FLOOR`                 | partea întreagă                      |
| `ROUND(n [, decimals])` | rotunjire cu n zecimale              |
| `TRUNC(n [, decimals])` | trunchiere cu n zecimale             |
| `EXP`                   | ridicarea la putere a lui $e$        |
| `LN`                    | logaritm natural                     |
| `LOG(base, n)`          | logaritm în baza `base`,             |
| `MOD(n1, n2)`           | restul împărțirii lui $n_1$ la $n_2$ |
| `POWER(n, pow)`         | $n^{pow}$                            |
| `SIGN`                  | semnul unui număr                    |
| `COS`                   | cosinus                              |
| `COSH`                  | cosinus hiperbolic                   |
| `SIN`                   | sinus                                |
| `SINH`                  | sinus hiperbolic                     |
| `SQRT`                  | $\sqrt{n}$                           |
| `TAN`                   | tangentă                             |
| `TANH`                  | tangentă hiperbolică                 |
| `LEAST(n1, ... , nk)`   | cel mai mic număr dintr-o listă      |
| `GREATEST(n1, ..., nk)` | cel mai mare număr dintr-o listă     |
### Funcții pentru prelucrarea datelor calendaristice 

| Funcție                        | Descriere                                                                                                                             | Exemplu                                                    |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| `SYSDATE`                      | întoarce data și timpul curent                                                                                                        | [[Courses/DB/bd.l1#Exercițiul 13\| ex. 13]]                           |
| `ADD_MONTHS(date, nr_months)`  | întoarce data care este după `nr_months` de la data `date`.                                                                           | `ADD_MONTHS('06-MAR-2025', 3) = '06-JUN-2025'`             |
| `NEXT_DAY(date, day)`          | întoarce următoarea dată după `date` în care ziua săptămânii este cea modificată prin șirul `day`                                     | `NEXT_DAY('03-JUN-2025', 'Monday') = '09-JUN-2025'`        |
| `LAST_DAY(date)`               | data corespunzătoare ultimei zile a lunii din care `date` face parte                                                                  | `LAST_DAY('03-JUN-2025') = 2025-06-30`                     |
| `MONTHS_BETWEEN(date1, date2)` | numărul de luni dintre două date                                                                                                      | `MONTHS_BETWEEN('10-FEB-2020', '31-MAR-2005') = -36.90322` |
| `TRUNC(date)`                  | data, dar cu timpul setat la 12:00 AM.                                                                                                | -                                                          |
| `ROUND(expr_date)`             | dacă este o dată înainte de miezul zilei, se întoarce ziua cu timpul setat la 12 AM, dacă nu, următoarea zi cu timpul setat la 12 AM. | -                                                          |
| `LEAST(date1, ... , datek)`    | prima dată în ordine cronologică                                                                                                      | -                                                          |
| `GREATEST(date1, ... , datek)` | ultima dată în ordine cronologică                                                                                                     | -                                                          |
#### Operațiile pe date calendaristice 

- `date +/- number` -> `date` scade/adună un număr de zile la o dată (nu trebuie să fie număr întreg)
- `date1 - date` -> `number` numărul de zile dintre două date 

### Funcții diverse


| Funcție                                                       | Descriere                                                                                                                         | Exemplu                                                                                           |
| ------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| `DECODE(value, if1, then1, if2, then2, ... ifN, thenN, else)` | returnează `elseI` dacă valoarea este `idI`. Dacă nu este egală cu niciun `if`, returnează `else`                                 | `DECODE('a', 'a', 'b', 'c') = 'b'`<br><br>`DECODE('b', 'a', 'b', 'c') = 'c'`                      |
| `NVL(e1, e2)`                                                 | dacă `e1` e `NULL`, întoarce `e2`. Dacă nu, întoarce `e1`<br><br>!! Cele două tipuri trebuie să fie valide/să poată fi convertite | `NVL(NULL, 1) = 1`<br><br>`NVL(1, 2) = 2`<br><br>`NVL('a', 1) = a`<br><br>`NVL(1, 'a') nu merge!` |
| `NVL2(e1, e2, e2)`                                            | dacă `e1` este `NULL`, întoarce `e2`, altfel întoarce `e3`                                                                        | `NVL(1, 2, 3) = 2`<br><br>`NVL(NULL, 2, 3) = 3`                                                   |
| `NULLIF(e1, e2)`                                              | dacă `e1 = e2` întoarce `NULL`, altfel întoarce `e1`                                                                              | `NULLIF(1,1) = NULL`                                                                              |
| `COALESCE(e1, ... , en)`                                      | returnează prima expresie `NOT NULL` din listă                                                                                    | `COALESCE(NULL, NULL, 1, 2) = 1`                                                                  |
| `UID`, `USER`                                                 | întorc id-ul, respectiv username-ul utilizatorului Oracle curent                                                                  | `SELECT USER FROM DUAL;`                                                                          |
| `VSIZE(expr)`                                                 | întoarce numărul de bytes ai unei expresii de tipul `NUMBER`, `VARCHAR2`, `DATE`.                                                 | `SELECT VSIZE(SALARY) FROM EMPLOYEES WHERE EMPLOYEE_ID = 20;`                                     |
>[!tip] Case
>Utilizarea `DECODE` este echivalentă cu utilizarea clauzei `CASE`

Case:

```sql
CASE expr
WHEN e1 THEN val1
[WHEN e2 THEN val2]
...
[ELSE val]
END
```

- se întoarce prima valoare pentru care `expr = ei`, dacă nu, `else`
- toate valorile trebuie să aibă același tip
- nu se poate specifica `NULL` pentru toate valorile de returnat

## Funcțiile multiple-row

Sunt funcțiile care calculează o valoare pe baza mai multor rânduri dintr-un tabel.

- apar în clauzele `SELECT`, `ORDER_BY`, `HAVING`, dar doar în combinație cu `GROUP BY`

Exemple:
- `AVG`
- `SUM`
- `MAX`
- `MIN`
- `COUNT`
- `STDDEV`
- `VARIANCE`

Tipuri de date acceptate:
- `CHAR`, `VARCHAR2`
- `NUMBER` (numere)
- `DATE` (date calendaristice)

**Notă:**
- `AVG`, `SUM`, `STDDEV`, `VARIANCE` -> doar pe numere.
- `MAX`, `MIN` -> pot fi aplicate și pe text sau date.

**Toate funcțiile grup, cu excepția lui COUNT(*), ignoră valorile null.**

- `COUNT(*)` numără **toate rândurile**, indiferent dacă au valori nule.
- `COUNT(coloana)` numără **doar rândurile unde coloana NU e `NULL`**.

Când este utilizată clauza GROUP BY, server-ul sortează implicit mulţimea rezultată în
ordinea crescătoare a valorilor coloanelor după care se realizează gruparea.









