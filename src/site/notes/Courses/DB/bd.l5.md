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

## Modificarea structurii tabelelor 

Modificarea se face cu ALTER TABLE. 

Forma comenzii depinde de tipul modificării aduse:

- Adăugarea unei coloane (nu se poate specifica poziția, automat se adaugă ultima)

```sql
ALTER TABLE nume_tabel
ADD (coloana tip_de_date [DEFAULT expr][, ...]);
```

- Modificarea unei coloane (schimbare tip de date, dimensiunii, valorii implicite (-> val. implicită se aplică doar pentru valorile noi ce urmează a fi introduse))

```sql
ALTER TABLE nume_tabel
MODIFY (coloana tip_de_date [DEFAULT expr][, ...]);
```

- eliminarea unei coloane din structura tabelului 

```sql
ALTER TABLE nume_tabel
DROP COLUMN coloana;
```

**Observații:**

- dimensiunea unei coloane numerice sau de tip caracter poate fi mărită, dar nu poate fi micșorată decât dacă acea coloană conține numai valori `NULL` sau dacă tabelul nu conține nicio linie.
- tipul de date al unei coloane poate fi modificat doar dacă valorile coloanei respective sunt `NULL`.
- o coloană `CHAR` poate fi convertită la tipul de date `VARCHAR2` sau invers, numai dacă valorile coloanei sunt `NULL` sau dacă nu se modifică dimensiunea coloanei.
    
Comanda `ALTER` permite adăugarea unei constrângeri într-un tabel existent, eliminarea, activarea sau dezactivarea constrângerilor.

Pentru adăugare de constrângeri, comanda are forma:

```sql
ALTER TABLE nume_tabel
ADD [CONSTRAINT nume_constr] tip_constr (coloana);
```

Pentru eliminare de constrângeri:

```sql
ALTER TABLE nume_tabel
DROP PRIMARY KEY | UNIQUE (col1, col2, ...) | CONSTRAINT nume_constr;
```

Pentru activare/dezactivare de constrângeri:

```sql
ALTER TABLE nume_tabel
MODIFY CONSTRAINT nume_constr ENABLE | DISABLE;
```

sau:

```sql
ALTER TABLE nume_tabel
ENABLE | DISABLE CONSTRAINT nume_constr;
```

## Suprimarea tabelelor 

**Ștergerea fizică** a unui tabel, inclusiv a înregistrărilor acestuia, se realizează prin comanda:

```sql
DROP TABLE nume_tabel;
```

- Odată executată, instrucțiunea `DROP TABLE` este ireversibilă;
    
- Ca și în cazul celorlalte instrucțiuni ale limbajului de definire a datelor, această comandă nu poate fi anulată (`ROLLBACK`);
    
- Oracle 10g a introdus o nouă manieră pentru suprimarea unui tabel:
    - Când se șterge un tabel, baza de date nu eliberează imediat spațiul asociat tabelului;
    - Ea redenumește tabelul și acesta este plasat într-un "recycle bin" de unde poate fi eventual recuperat ulterior prin comanda `FLASHBACK TABLE`:

```sql
FLASHBACK TABLE exemplu TO BEFORE DROP;
```

- Ștergerea unui tabel se poate face simultan cu eliberarea spațiului asociat tabelului, dacă este utilizată clauza `PURGE` în comanda `DROP TABLE`;
    
- Nu este posibil un rollback pe o comandă `DROP TABLE` cu clauza `PURGE` — deci se pierde definitiv tabelul:

```sql
DROP TABLE exemplu PURGE;
```

Pentru **ștergerea conținutului** unui tabel și păstrarea structurii acestuia se poate utiliza comanda:

```sql
TRUNCATE TABLE nume_tabel;
```

Obs: Fiind operație LDD, comanda `TRUNCATE` are efect definitiv. De asemenea, se resetează și numărătoarea pentru coloane de autoincrement.

- `TRUNCATE` eliberează spațiul de memorie. `DELETE` nu face acest lucru;
- `TRUNCATE` nu utilizează clauza `WHERE`, asa cum o face `DELETE`;
- `TRUNCATE` este mai rapidă deoarece nu generează informații pentru `ROLLBACK` și nu activează declanșatori asociați operației de ștergere;
- Dacă tabelul este „părintele” unei constrângeri de integritate referențială, el **nu poate fi trunchiat**;
- Pentru a putea fi aplicată instrucțiunea `TRUNCATE`, constrângerea trebuie să fie mai întâi dezactivată;

## Redenumirea tabelor

Comanda `RENAME` permite redenumirea unui tabel, vizualizare sau secvență:

```sql
RENAME nume1_obiect TO nume2_obiect;
```

**Obs:**
- În urma redenumirii sunt transferate automat constrângerile de integritate, indecșii și privilegiile asupra vechilor obiecte.
- Sunt invalidate toate obiectele ce depind de obiectul redenumit, cum ar fi vizualizări, sinonime sau proceduri și funcții stocate.
## Consultarea dicționarului datelor

Informații despre tabelele create se găsesc în vizualizările:
- `USER_TABLES` – informații complete despre tabelele utilizatorului.
- `TAB` – informații de bază despre tabelele existente în schema utilizatorului.

Informații despre constrângeri găsim în `USER_CONSTRAINTS`, iar despre coloanele implicate în constrângeri în `USER_CONS_COLUMNS`.

## Exerciții 

### Exercițiul 1 

Să se creeze tabelul `ANGAJATI_pnu` (pnu se alcătuiește din prima literă din prenume și primele două litere din numele studentului) corespunzător schemei relaționale:
    
```sql
ANGAJATI_pnu (
    cod_ang#     NUMBER(4),
    nume         VARCHAR2(20),
    prenume      VARCHAR2(20),
    email        CHAR(15),
    data_ang     DATE,
    job          VARCHAR2(10),
    cod_sef      NUMBER(4),
    salariu      NUMBER(8,2),
    cod_dep      NUMBER(2)
)
```

În următoarele moduri:

**a)** cu precizarea cheilor primare **la nivel de coloană** și a constrângerilor `NOT NULL` pentru coloanele `nume` și `salariu`. De asemenea, se presupune că:
- valoarea implicită a coloanei `data_ang` este `SYSDATE`,
- iar adresa de e-mail trebuie să aibă o valoare **unică**.
**b)** cu precizarea cheii primare **la nivel de tabel** și a constrângerilor `NOT NULL` pentru coloanele `nume` și `salariu`.

**Obs:** Nu pot exista două tabele cu același nume în cadrul unei scheme, deci recrearea unui tabel va fi precedată de suprimarea sa prin comanda:

```sql
DROP TABLE ANGAJATI_pnu;
```

*Soluție*

```sql
CREATE TABLE ANGAJATI
(
COD_ANG NUMBER(4) PRIMARY KEY,
NUME VARCHAR2(20) NOT NULL,
PRENUME VARCHAR2(20),
EMAIL VARCHAR2(15) UNIQUE,
DATA_ANG DATE DEFAULT SYSDATE,
JOB VARCHAR2(10),
COD_SEF NUMBER(4),
SALARIU NUMBER(8,2) NOT NULL,
COD_DEPARTAMENT NUMBER(2)
);

DROP TABLE ANGAJATI;

CREATE TABLE ANGAJATI
(
    COD_ANG NUMBER(4),
    NUME VARCHAR2(20) NOT NULL,
    PRENUME VARCHAR2(20),
    EMAIL VARCHAR2(15) UNIQUE,
    DATA_ANG DATE DEFAULT SYSDATE,
    JOB VARCHAR2(10),
    COD_SEF NUMBER(4),
    SALARIU NUMBER(8,2) NOT NULL,
    COD_DEPARTAMENT NUMBER(2),
    CONSTRAINT pk_angajat PRIMARY KEY (COD_ANG)
);
```

### Exercițiul 2 

Inserează angajați. 

```sql
INSERT ALL
    INTO ANGAJATI VALUES(100, 'Nume1', 'Prenume1',
                         null, null, 'Director',
                         null, 20000, 10)
    INTO ANGAJATI VALUES(101, 'Nume1', 'Prenume2',
                         'Nume2', TO_DATE('02-02-2024', 'DD-MM-YYYY'), 
                         'Inginer', 100, 10000, 10)
SELECT * FROM DUAL;
```

### Exercițiul 3

Introduceți coloana comision in tabelul ANGAJATI_pnu. Coloana va avea tipul de date
NUMBER(4,2).

```sql
ALTER TABLE ANGAJATI
ADD (COMISION  NUMBER(4,2));
```

### Exercițiul 5

```sql
ALTER TABLE ANGAJATI
MODIFY (SALARIU DEFAULT 100);
```

### Exercițiul 6 

```sql
ALTER TABLE ANGAJATI
MODIFY(
    COMISION NUMBER(2,2),
    SALARIU NUMBER(10,2)
    );
```

# Implementarea unei secvențe și a unei chei compuse 

## Secvențe 

Au următoarea sintaxă

```sql
CREATE SEQUENCE nume_secv
START WITH 1         -- prima valoare generată
INCREMENT BY 1       -- pasul de incrementare
[MINVALUE n]         -- valoarea minimă permisă
[MAXVALUE n]         -- valoarea maximă permisă
[CYCLE | NOCYCLE]    -- revine la MINVALUE după MAXVALUE (sau nu)
[CACHE n | NOCACHE]  -- câte valori prealocate în memorie
[ORDER | NOORDER];   -- dacă păstrează ordinea cererilor (important la paralelism)
```

Exemplu, pentru generare de id

```sql
CREATE SEQUENCE seq_angajati
START WITH 1
INCREMENT BY 1
NOCACHE
NOCYCLE;
```

Utilizare 

```sql
INSERT INTO ANGAJATI (ID, NUME) 
VALUES (seq_angajati.NEXTVAL, 'Popescu');
```

## Cheie compusă 

Exemplu

```sql
CREATE TABLE INSCRIERI (
    ID_STUDENT NUMBER(4),
    ID_CURS    NUMBER(4),
    DATA_INSCR DATE DEFAULT SYSDATE,
    CONSTRAINT pk_inscrieri PRIMARY KEY (ID_STUDENT, ID_CURS)
);
```

- ambele coloane sunt not null (implicit) 

Inserări 

```
(1,2)
(1,3) -> valid
(2,2) -> valid
(1,2) -> invalid
```