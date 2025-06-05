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

# Definirea tabelelor 

## Crearea tabelelor 

Sintaxa simplificată este 

```sql
CREATE TABLE nume_tabel (
coloana_1 tip_date [DEFAULT valoare]
[constrangere_nivel_coloana [constrangere_nivel_coloana]...],
. . . . . . . . .
coloana_n tip_date [DEFAULT valoare]
[constrangere_nivel_coloana [constrangere_nivel_coloana]...],
[constrangeri_nivel_tabel]
);
```

Sau, cu *subcereri*

```sql
CREATE TABLE nume_tabel [(coloana_1,..., coloana_n)]
AS subcerere;
```

### Tipuri de constrângeri (constraints) în SQL

Constrângerile definite asupra unui tabel pot fi de următoarele tipuri:

- **NOT NULL**  
    Coloana nu poate conține valoarea `NULL`.  
    Exemplu:
    
    ```sql
    col_name TYPE NOT NULL
    ```
    
- **UNIQUE**  
    Coloane (sau combinații de coloane) care trebuie să aibă valori unice în cadrul tabelului.  
    Exemplu:
    
    ```sql
    UNIQUE (col1, col2)
    ```
    
- **PRIMARY KEY**  
    Identifică în mod unic fiecare înregistrare din tabel. Este echivalent cu `NOT NULL` + `UNIQUE`.  
    Exemplu:
    
    ```sql
    PRIMARY KEY (col1, col2)
    ```
    
- **FOREIGN KEY**  
    Stabilește o relație între o coloană din tabelul curent („copil”) și o coloană dintr-un alt tabel („părinte”).  
    Sintaxă:
    
    ```sql
    FOREIGN KEY (col_nume)
    REFERENCES parinte_tabel(coloana)
    [ON DELETE CASCADE | ON DELETE SET NULL]
    ```
    
    Explicații:
    
    - `FOREIGN KEY` este definită la nivel de tabel pentru a marca o coloană drept cheie externă.
        
    - `REFERENCES` identifică tabelul părinte și coloana sa.
        
    - `ON DELETE CASCADE`: la ștergerea unei linii din tabelul părinte, se șterg automat și liniile dependente.
        
    - `ON DELETE SET NULL`: valorile cheii externe devin `NULL` când linia părinte este ștearsă.
        
- **CHECK**  
    O condiție logică ce trebuie să fie adevărată pentru fiecare linie/coloană.  
    Exemplu:
    
    ```sql
    CHECK (salariu > 0)
    ```

 ***Observații***

- Constrângerile pot fi definite fie:
    
    - în momentul creării tabelului (`CREATE TABLE`), fie
    - ulterior, cu `ALTER TABLE`.
        
- `CHECK` se poate defini la nivel de coloană **doar dacă nu face referire la altă coloană**.
    
- Dacă cheia primară (sau o cheie unică) este compusă (mai multe coloane), trebuie definită **la nivel de tabel**, nu individual pe coloană.
    
- Constrângerea `NOT NULL` se poate defini **doar la nivel de coloană**.

### Tipuri de date

| Tip de date               | Descriere                                                                              |
| ------------------------- | -------------------------------------------------------------------------------------- |
| `VARCHAR2(n) [BYTE/CHAR]` | Șir de caractere de dimensiune variabilă, până la 4000 de octeți sau caractere.        |
| `CHAR(n) [BYTE/CHAR]`     | Șir de caractere de lungime fixă, până la 2000 de octeți.                              |
| `NUMBER(p, s)`            | Număr cu `p` cifre, dintre care `s` reprezintă partea zecimală.                        |
| `LONG`                    | Șiruri de caractere foarte lungi, până la 2GB.                                         |
| `DATE`                    | Date calendaristice valide, în intervalul 4712 înainte de Hristos – 9999 după Hristos. |
### Exemplu 

```sql
CREATE TABLE ANGAJATI (
    ID            NUMBER(5) PRIMARY KEY,
    NUME          VARCHAR2(50) NOT NULL,
    PRENUME       VARCHAR2(50),
    EMAIL         VARCHAR2(100) UNIQUE,
    TELEFON       CHAR(10),
    SALARIU       NUMBER(8,2) CHECK (SALARIU > 0),
    DATA_ANGAJARE DATE NOT NULL,
    DEPARTAMENT_ID NUMBER(4),
    FOREIGN KEY (DEPARTAMENT_ID) REFERENCES DEPARTAMENTE(ID)
        ON DELETE SET NULL
);

CREATE TABLE DEPARTAMENTE (
    ID   NUMBER(4) PRIMARY KEY,
    NUME VARCHAR2(100) NOT NULL
);
```

