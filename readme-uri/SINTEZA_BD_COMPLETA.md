# SINTEZĂ COMPLETĂ — BAZE DE DATE (BD)
> Acoperă toate cursurile (introducere, algebră relațională, dependențe funcționale/multivaluate, normalizare, indexare, procesarea interogărilor, constrângeri/triggere/view-uri).
> Organizat după frecvența la examen (⭐). La final: test grilă.

---

## CUPRINS (după prioritatea la examen)

| # | Temă | Recurență 2019–2025 | Prioritate |
|---|------|---------------------|------------|
| 1 | [Algebra relațională: operații + echivalent SQL](#1-algebra-relațională-operații--sql-) | în fiecare an, de mai multe ori | ⭐⭐⭐⭐⭐ |
| 2 | [Forme normale (1NF–4NF, BCNF) și relațiile dintre ele](#2-forme-normale-) | în fiecare an | ⭐⭐⭐⭐⭐ |
| 3 | [Indexare (B+ arbori, hash, bitmap, dens/rar)](#3-indexare-) | în fiecare an | ⭐⭐⭐⭐⭐ |
| 4 | [Dependențe funcționale](#4-dependențe-funcționale-) | frecvent | ⭐⭐⭐⭐ |
| 5 | [Dependențe multivaluate](#5-dependențe-multivaluate-) | frecvent | ⭐⭐⭐⭐ |
| 6 | [Chei și constrângeri](#6-chei-și-constrângeri-) | frecvent | ⭐⭐⭐⭐ |
| 7 | [Subinterogări (corelate/necorelate) + plan de execuție](#7-subinterogări-) | frecvent | ⭐⭐⭐ |
| 8 | [Tranzacții și ACID](#8-tranzacții-și-acid-) | frecvent | ⭐⭐⭐ |
| 9 | [Tabele virtuale (view-uri)](#9-tabele-virtuale-views-) | frecvent | ⭐⭐⭐ |
| 10 | [Procesarea interogărilor + algoritmi de join](#10-procesarea-interogărilor-) | ⭐⭐ | ⭐⭐ |
| 11 | [Descompunerea join fără pierdere](#11-descompunerea-join-fără-pierdere-) | ⭐⭐ | ⭐⭐ |
| 12 | [Declanșatoare (triggere)](#12-declanșatoare-triggers-) | ⭐ | ⭐ |
| — | [Anexă: introducere, modele BD, SGBD](#anexă--introducere-modele-de-baze-de-date) | context | — |

---

## 1. Algebra relațională: operații + SQL ⭐⭐⭐⭐⭐
> Cea mai frecventă temă. *„Relații și operații cu relații în modelul relațional și corespondența în SQL (cel puțin 5 operații)."*

### Elemente ale modelului relațional
- **Atribut** = coloană; **domeniu** = mulțimea valorilor posibile ale unui atribut.
- **Tuplu (uplu)** = linie; **relație (r)** = tabel; **schema de relație R[U]** = structura tabelei (nume + atribute).
- **Bază de date** = mulțime de relații peste o schemă de BD.

### Două categorii de operatori

**A. Operatori din teoria mulțimilor** (relațiile trebuie peste **aceeași** mulțime de atribute, exceptând ×):

| Operator | Simbol | Definiție | SQL |
|----------|--------|-----------|-----|
| **Reuniune** | ∪ | `r₁ ∪ r₂ = {t | t∈r₁ sau t∈r₂}` | `UNION` |
| **Diferență** | − | `r₁ − r₂ = {t | t∈r₁ și t∉r₂}` | `MINUS` (Oracle) / `EXCEPT` |
| **Intersecție** | ∩ | `r₁ ∩ r₂ = {t | t∈r₁ și t∈r₂}` | `INTERSECT` |
| **Produs cartezian** | × | toate combinațiile; atributele se reunesc | `SELECT * FROM r1, r2` |

> Intersecția se poate obține din diferență: `r₁ ∩ r₂ = r₁ − (r₁ − r₂)`.

**B. Operatori specifici algebrei relaționale:**

| Operator | Simbol | Ce face | SQL |
|----------|--------|---------|-----|
| **Proiecție** | π_X(r) | păstrează doar atributele X (coloane) | `SELECT nume, prenume FROM ...` |
| **Selecție** | σ_θ(r) | păstrează tuplele care satisfac condiția θ (linii) | `SELECT * FROM ... WHERE ...` |
| **Redenumire** | ρ | schimbă numele unui atribut | `... AS "nume_nou"` |
| **Join natural** | ⋈ | îmbină după atributele comune (egale) | `NATURAL JOIN` / `JOIN ... ON` |
| **θ-join** | ⋈_θ | join cu o condiție oarecare θ | `JOIN ... ON condiție` |
| **Equijoin** | ⋈_= | θ-join cu operatorul de egalitate | `JOIN ... ON a = b` |
| **Semijoin** | ⋉, ⋊ | tuplele din stânga care au corespondent în dreapta | — |
| **Antijoin** | ▷ | tuplele din stânga FĂRĂ corespondent în dreapta | — |
| **Join stânga** | ⟕ | tot din stânga + potriviri (NULL unde lipsesc) | `LEFT JOIN` |
| **Join dreapta** | ⟖ | tot din dreapta + potriviri | `RIGHT JOIN` |
| **Join extern** | ⟗ | stânga ∪ dreapta | `FULL JOIN` |

### Proiecția (π)
Restrânge un tuplu/relație la o submulțime de atribute X ⊆ U.
```sql
π_{nume,prenume}(studenti)  ≡  SELECT nume, prenume FROM studenti;
```

### Selecția (σ)
Păstrează tuplele care satisfac o **expresie booleană** θ (formată din `AφB`, `Aφc`, cu ∧, ∨).
```sql
σ_{an=2 ∧ bursa IS NULL}(studenti)  ≡  SELECT * FROM studenti WHERE an=2 AND bursa IS NULL;
```

### Join natural (⋈)
`r₁ ⋈ r₂` = tuplele care coincid pe atributele comune (U₁ ∩ U₂). Echivalent: produs cartezian filtrat pe egalitatea atributelor comune, cu eliminarea coloanei duplicate.
```sql
SELECT nume, valoare FROM studenti NATURAL JOIN note;
SELECT nume, valoare FROM studenti JOIN note ON studenti.nr_matricol = note.nr_matricol;
```
Proprietate: `r₁ ⋈_θ r₂ = σ_θ(r₁ × r₂)` (join oarecare = selecție pe produs cartezian). Dacă U₁∩U₂=∅ → join natural = produs cartezian.

### Join-uri externe (LEFT / RIGHT / FULL)
Păstrează și tuplele **fără corespondent**, completate cu **NULL**:
```sql
SELECT * FROM studenti LEFT JOIN profesori ON studenti.prenume = profesori.prenume;
-- toți studenții + profesorii cu același prenume (NULL unde nu există)
```

### Agregarea
Funcții: **avg, min, max, sum, count** (+ var). Operatorul: `G₁..Gₙ 𝒢 F₁(A₁)..Fₙ(Aₙ)(E)` — grupare după G, aplică funcția F.
```sql
SELECT an, AVG(bursa) FROM studenti GROUP BY an;
```
> **Agregarea NU se poate exprima** cu ceilalți operatori de bază ai algebrei relaționale (spre deosebire de join/intersecție).

---

## 2. Forme normale ⭐⭐⭐⭐⭐
> *„Formele normale toate, pe scurt. Relația/legătura dintre ele. BCNF și 4NF. 2NF și 3NF."*

**Normalizarea** = proces de **eliminare a redundanțelor** și păstrare a **consistenței** datelor. Se face în faza de **proiectare** (asupra **schemei**, nu a relației concrete). Fiecare formă adaugă ceva față de precedenta.

### Noțiuni necesare
- **Atribut prim** = face parte dintr-o cheie candidat; **neprim** = nu.
- **Dependență plină:** `X → A` e plină dacă nicio submulțime proprie a lui X nu determină A.
- **Atribut tranzitiv dependent** de X: există Y cu `X → Y`, `Y → A`, dar `Y → X ∉ Σ⁺`.
- **Dependență trivială:** `X → Y` cu Y ⊆ X.

### Formele normale

**1NF (1971):** domeniile atributelor sunt **indivizibile** și fiecare valoare e **atomică** (fără grupuri repetitive). *Fix: elimini valorile multiple într-o relație separată legată prin cheie.*

**2NF:** e în 1NF **ȘI** orice atribut **neprim** e **dependent plin** de orice **cheie** (nu doar de o parte a cheii).
> Observație: dacă nu există chei multi-atribut, relația e automat în 2NF.

**3NF:** e în 2NF **ȘI** niciun atribut neprim NU e **tranzitiv dependent** de vreo cheie (atributele neprime depind de chei, nu de alte atribute neprime).
> Mnemonic (Bill Kent): *"non-key must provide a fact about the key, the whole key, and nothing but the key."*

**BCNF (Boyce-Codd, ≡ 3.5NF, 1975):** e în 1NF **ȘI** pentru orice dependență funcțională netrivială `X → A ∈ Σ⁺`, **X este supercheie**.
> **BCNF ⟹ 3NF** (o schemă în BCNF e și în 3NF).

**4NF (Fagin, 1977):** e în 1NF **ȘI** pentru orice dependență **multivaluată** netrivială `X ↠ A ∈ Δ⁺`, **X este supercheie**.
> **4NF ⟹ BCNF** (o schemă în 4NF e și în BCNF).

### Relația/ierarhia dintre forme (întrebare frecventă!)
```
1NF ⊃ 2NF ⊃ 3NF ⊃ BCNF ⊃ 4NF
(fiecare o rafinează pe precedenta; 4NF e cea mai strictă)

4NF ⟹ BCNF ⟹ 3NF ⟹ 2NF ⟹ 1NF
(dacă e în 4NF, e în toate cele anterioare)
```

### Exemple
**2NF (violare):** `OS[nume, versiune, an, companie]`, cheie `(nume,versiune)`, dar `nume → companie` → `companie` NU e dependent plin → nu e 2NF. Fix: scoți `companie` într-o relație `R'[nume, companie]`.

**3NF (violare):** `Concursuri[materie, an, castigator, IQ]`, cheie `(materie,an)`, dar `castigator → IQ` → `IQ` e tranzitiv dependent → nu e 3NF. Fix: `Concursuri[materie,an,castigator]` + `Inteligenta[castigator,IQ]`.

**4NF (violare):** `Student[cnp, nume, facultate, pasiune]` cu `cnp,nume ↠ facultate` → dependență multivaluată nebanală, `cnp,nume` nu e supercheie. Fix: `S1[cnp,nume]`, `S2[cnp,facultate]`, `S3[cnp,pasiune]`.

---

## 3. Indexare ⭐⭐⭐⭐⭐
> *„Indexare — rol, structuri de date, tipuri, eficiență. Când, cum, de ce se folosesc indecși. Hash și bitmap."*

### Motivație
SGBD-ul petrece majoritatea timpului **căutând** (rezolvând interogări). Datele se stochează secvențial pe disc. Dacă sunt sortate după cheia de căutare → **căutare binară O(log₂N)**. Dar dacă interoghezi după alt atribut, ai nevoie de un **fișier index**.

### Concepte
- **Cheie de căutare** = atribut(e) după care cauți; **cheie de sortare** = după care e ordonat fișierul de date.
- **Fișier index** = perechi `(valoare cheie, pointer)`, mai mic decât fișierul de date.
- **Index dens** = o intrare pentru fiecare valoare a cheii; **index rar** = doar unele valori (aplicabil doar când cheia de căutare = cheia de sortare).
- **Index primar** = cheia de căutare = cheia de sortare (max un index primar/tabel); **index secundar** = altă ordonare.

### Când/cum folosim indecși
```
CÂND: pe coloane cu (majoritar) valori DISTINCTE, cu selectivitate ridicată
      (filtrează puține tuple dintr-un tabel mare), folosite în JOIN.
CÂND NU: pe tabele mici, pe coloane modificate des (indexul trebuie actualizat).
CUM (alegerea structurii):
  • interogări de tip INTERVAL → B+ arbori
  • interogări PUNCTUALE frecvente → hash
  • coloane cu puține valori distincte + condiții compuse → bitmap
```

### Tipuri de indecși

**1. Indecși ordonați secvențiali** — intrări sortate după cheie. Se **degradează** după multe operații DML (necesită reorganizare).

**2. B+ arbori** (cei mai folosiți în SGBD-uri relaționale):
- arbore **echilibrat** (toate frunzele pe același nivel);
- nod cu constanta **m** (max m valori, m+1 pointeri), calibrat ca un nod = un bloc de disc;
- **doar frunzele** trimit spre fișierul de date (nivelurile superioare = index rar); frunzele sunt legate → **listă dublu înlănțuită** (bune pentru interogări de interval + `ORDER BY`);
- **complexitate:** căutare/inserare/ștergere `O(log n)` — mai exact, căutare ≤ `log⌈(m+1)/2⌉(K)` blocuri; inserare/ștergere ≤ `2·log(K)`.
- Reguli de ocupare: rădăcina ≥ 2 pointeri; nod intern ≥ ⌈(m+1)/2⌉ pointeri; frunză ≥ ⌈m/2⌉ valori. La inserare, dacă un nod e plin → **divizare** (split), propagată în sus (poate crește adâncimea).

**B-arbori** (vs B+): permit o **singură apariție** a unei valori (valorile pot fi și în noduri interne). Avantaj: găsești uneori mai repede. Dezavantaje: arbori mai adânci, implementare complicată, nu poți scana pe nivelul frunză → **B+ e preferat**.

**3. Indecși hash:**
- valorile cheii sunt distribuite în **bucket-uri** cu o **funcție hash** `h: K → B` (dispersie);
- înregistrări cu chei diferite pot ajunge în același bucket (**coliziune** → scanezi bucket-ul);
- **eficiență:** căutare punctuală `O(1)` (fără coliziuni, un singur bloc); **NU e eficient pentru interogări de interval** (valorile nu sunt ordonate).
- **Hash static** (nr. fix de bucket-uri) vs **hash dinamic/extensibil** (numărul de bucket-uri crește/scade după necesități, folosind un prefix de biți) → performanța nu scade la creșterea fișierului.

**4. Indecși bitmap:**
- pentru atribute cu **puține valori distincte** (categoriale);
- pentru fiecare valoare a cheii, un **șir binar** de lungime = nr. de înregistrări (1 = înregistrarea are acea valoare);
- interogări cu selecții multiple rezolvate cu **operatori pe biți**: AND (intersecție), OR (reuniune), NOT (complement) — foarte rapizi (o instrucțiune CPU pe word).
```sql
CREATE BITMAP INDEX idx ON tabel(topic);
-- WHERE judet IN ('Arad','Cluj') AND an <> 2010  →  (Arad OR Cluj) AND NOT(2010)
```

### În SQL
```sql
CREATE INDEX c_index ON student(judet);
DROP INDEX c_index;
CREATE BITMAP INDEX ... ;   -- Oracle
```
Oracle: implicit **B+ arbori**; suportă bitmap; NU suportă indecși hash (dar are organizare hash a fișierului). Indecșii se creează implicit la constrângeri `UNIQUE`.

---

## 4. Dependențe funcționale ⭐⭐⭐⭐
> *„Dependențe funcționale: definiție, exemple, rol în proiectare. Axiomele lui Armstrong."*

### Definiție
Fie X, Y ⊆ U. Relația `r` satisface dependența funcțională **X → Y** dacă:
```
∀ t₁, t₂ ∈ r:  t₁[X] = t₂[X]  ⟹  t₁[Y] = t₂[Y]
```
adică: dacă două tuple coincid pe X, atunci coincid și pe Y. Spunem că **X determină funcțional Y**.

**Exemplu:** în `U = {nume, l(nume), data_nastere, zodie, varsta}`:
- `nume → l(nume)` (numele determină lungimea);
- `data_nastere → varsta`, `data_nastere → zodie`.

### Rol în proiectare
Dependențele funcționale îmi arată **la ce COLOANE pot renunța** astfel încât să le pot reface ulterior (elimină redundanța). Se folosesc pentru: **găsirea cheilor candidat** și **normalizare** (2NF, 3NF, BCNF).

### Proprietăți (FD1–FD8)
- **FD1 Reflexivitate:** Y ⊆ X ⟹ X → Y.
- **FD2 Extensie/Augmentare:** X → Y, Z⊆W ⟹ XW → YZ.
- **FD3 Tranzitivitate:** X → Y, Y → Z ⟹ X → Z.
- **FD4 Pseudotranzitivitate**, **FD5 Uniune**, **FD6 Descompunere**, FD7/FD8 Proiectabilitate.

### Axiomele lui Armstrong (reguli de inferență)
```
A1 (reflexivitate):  A₁...Aₙ → Aᵢ
A2 (augmentare/descompunere-uniune):  A₁..Aₘ → B₁..Bᵣ  ⟺  A₁..Aₘ → Bⱼ (∀j)
A3 (tranzitivitate):  A₁..Aₘ → B₁..Bᵣ, B₁..Bᵣ → C₁..Cₚ  ⟹  A₁..Aₘ → C₁..Cₚ
```
{A1, A2, A3} sunt **complete și corecte** (echivalente cu regulile R1 = {FD1f, FD2f, FD3f}).

**Închiderea** `X⁺` = mulțimea atributelor determinate de X (`{A | Σ ⊢ X → A}`). `X → Y` e demonstrabilă ⟺ `Y ⊆ X⁺`.

---

## 5. Dependențe multivaluate ⭐⭐⭐⭐
> *„Ce sunt dependențele multivaluate, unde se folosesc, exemple. Unde intervin în proiectare."*

### Definiție
Fie X, Y ⊆ U și Z = U − XY (numit „rest"). Relația `r` satisface dependența multivaluată **X ↠ Y** dacă pentru orice t₁, t₂ ∈ r cu t₁[X]=t₂[X], există t₃, t₄ ∈ r astfel încât:
```
t₃[X]=t₁[X], t₃[Y]=t₁[Y], t₃[Z]=t₂[Z]
t₄[X]=t₂[X], t₄[Y]=t₂[Y], t₄[Z]=t₁[Z]
```
Intuitiv: pentru un X fixat, valorile lui **Y sunt independente** de valorile lui Z (apar toate combinațiile).

### Exemplu (clasic)
Persoana cu CNP=1 e admisă la 2 facultăți (Info, Mate) și are permis pentru 2 categorii (A, B). Facultatea și categoria de permis sunt **independente** → trebuie să apară toate 4 combinațiile:
```
CNP  Facultate   Permis
1    Info        A
1    Mate        B
1    Info        B   ← deductibile (redundante)
1    Mate        A   ←
```
Deci `CNP ↠ Facultate` (și `CNP ↠ Permis`).

### Unde intervin (rol în proiectare)
- **Dependențele funcționale** îmi spun la ce **coloane** pot renunța; **dependențele multivaluate** îmi spun la ce **LINII** pot renunța (redundante) astfel încât să le pot reface.
- Stau la baza formei normale **4NF** (o schemă e în 4NF dacă orice MVD netrivială are X supercheie).
- **Legătura cu FD:** dacă `r` satisface `X → Y`, atunci satisface și `X ↠ Y` (orice dependență funcțională e și multivaluată).

**Proprietăți MVD:** MVD0 (complementariere), MVD1 (reflexivitate), MVD2 (extensie), MVD3 (tranzitivitate), MVD4-6, plus reguli mixte FD-MVD.

---

## 6. Chei și constrângeri ⭐⭐⭐⭐
> *„Cheie candidat — cum și unde se folosește, cum se obține. Constrângeri (chei, tipuri de chei)."*

### Tipuri de chei
- **Supercheie** — un atribut/mulțime de atribute care **identifică unic** un tuplu.
- **Cheie candidat** — o supercheie **minimală** (nicio submulțime proprie nu e supercheie). Formal: `X⁺ = U` și `∀X'⊊X, X'⁺ ≠ U`.
- **Cheie primară** — cheia candidat **aleasă** pentru identificare.
- **Cheie alternativă** — chei candidat neselectate ca primară.
- **Cheie străină** — atribut(e) care **referă** o cheie candidat a **altei** relații.

### Cum obții cheile candidat (din dependențe funcționale)
Organizezi atributele după unde apar în dependențele din Σ:
```
STÂNGA (doar în stânga)  → atribute PRIME (obligatoriu în orice cheie)
DREAPTA (doar în dreapta) → atribute NEPRIME (niciodată singure în cheie)
MIJLOC (și stânga și dreapta) → pot fi în oricare
```
Calculezi `X⁺` pentru candidați; X e cheie candidat dacă `X⁺ = U` și e minimal.
*Ex: `Σ = {A→BD, B→C, DE→F}` → Stânga = {A,E}, deci `AE⁺ = U` → AE e cheie.*

### Constrângeri de integritate (statice) în SQL
Restricționează stările posibile ale BD → forțează consistența. Tipuri: **non-null, chei, integritate referențială, check, aserțiuni**.
```sql
CREATE TABLE tabel (
  a1 tip NOT NULL,                          -- valori nenule
  a2 tip UNIQUE,                            -- cheie candidat (1 atribut)
  a3 tip PRIMARY KEY,                       -- cheie primară (implicit NOT NULL + UNIQUE)
  a4 tip REFERENCES tabel2(b1),             -- cheie străină
  a5 tip CHECK (a5 BETWEEN 5 AND 10),       -- condiție booleană
  PRIMARY KEY (a1,a2),                      -- cheie primară multi-atribut
  FOREIGN KEY (a3,a4) REFERENCES tabel2(b1,b2)
);
```
**Integritate referențială:** fiecare valoare a cheii străine R.A trebuie să apară în cheia primară/unică S.B. Acțiuni la ștergere/actualizare: `ON DELETE/UPDATE RESTRICT | SET NULL | CASCADE`.

---

## 7. Subinterogări ⭐⭐⭐
> *„Subinterogări corelate și necorelate. Exemple. Planul de execuție în cele două cazuri."*

**Subinterogare** = o interogare imbricată în alta (în `WHERE`, `FROM`, `SELECT`).

**Subinterogare NECORELATĂ:** interogarea interioară **NU depinde** de cea exterioară → se poate executa **o singură dată**, independent, iar rezultatul e folosit de cea exterioară.
```sql
SELECT nume FROM studenti
WHERE an = (SELECT MAX(an) FROM studenti);   -- subinterogarea rulează o dată
```
*Plan de execuție:* interioara se evaluează **întâi și o singură dată**.

**Subinterogare CORELATĂ:** interogarea interioară **depinde** de un atribut din cea exterioară → se re-evaluează **pentru fiecare** tuplu al celei exterioare.
```sql
SELECT s.nume FROM studenti s
WHERE s.bursa > (SELECT AVG(b.bursa) FROM studenti b WHERE b.an = s.an);
-- subinterogarea rulează o dată PENTRU FIECARE s (depinde de s.an)
```
*Plan de execuție:* interioara se re-execută **per tuplu** al exterioarei → în general **mai costisitoare**. Adesea poate fi rescrisă ca join sau cu `EXISTS`.

---

## 8. Tranzacții și ACID ⭐⭐⭐
> *„Ce este o tranzacție? ACID. Comenzi SQL. Ce marchează începutul și încheierea unei tranzacții."*

**Tranzacție** = o secvență de operații executată ca o **unitate logică** (totul sau nimic).

**ACID** (proprietățile tranzacției în BD relaționale):
- **A — Atomicitate:** toate operațiile se execută **ca o singură entitate** sau, în caz de eșec, **niciuna** (BD revine la starea inițială).
- **C — Consistență:** la finalul tranzacției, datele sunt corecte conform logicii aplicației.
- **I — Izolare:** tranzacțiile nu se influențează reciproc, chiar executate simultan (fiecare „crede" că e singura).
- **D — Durabilitate:** după **commit**, efectele sunt permanente (nimic nu le mai poate anula).

**Comenzi SQL (Transaction Control Language):**
```sql
BEGIN TRANSACTION;   -- (sau BEGIN / START TRANSACTION) marchează ÎNCEPUTUL
   ... operații DML ...
SAVEPOINT s1;        -- punct de restaurare intermediar
COMMIT;              -- marchează ÎNCHEIEREA cu succes (efectele devin permanente)
-- sau
ROLLBACK;            -- anulează tranzacția (revine la starea inițială / la un savepoint)
```
- **Începutul:** `BEGIN TRANSACTION` (implicit, la prima comandă după commit/rollback).
- **Încheierea:** `COMMIT` (salvează) sau `ROLLBACK` (anulează).

> **Notă:** modelul nerelațional folosește **BASE** (Basic Availability, Soft-state, Eventual consistency) în loc de ACID. **Teorema CAP:** un sistem distribuit poate satisface doar 2 din {Consistency, Availability, Partition tolerance}.

---

## 9. Tabele virtuale (views) ⭐⭐⭐
> *„Tabele virtuale (view-uri): ce sunt, rol, tipuri, comportament la interogare și la actualizare. Când se poate actualiza?"*

**View (tabel virtual)** = o **interogare stocată** peste tabele sau alte view-uri. Schema view-ului = schema rezultatului interogării.
```sql
CREATE VIEW numeView [a1,a2,...] AS <frază_select>;
```

**Rol/motivație:** acces **modular**; **ascunderea** unor date față de anumiți utilizatori; **ușurarea** formulării interogărilor complexe.

**Comportament la interogare:** conceptual, un view se interoghează ca orice tabel; în realitate, interogarea e **rescrisă** inserând definiția view-ului, apoi optimizată.

**Comportament la actualizare (INSERT/UPDATE/DELETE):** modificările pe view trebuie **rescrise** în modificări pe tabelele de bază. Nu e mereu posibil (ex. view cu agregare — cum actualizezi un `AVG`?).

**View-uri actualizabile (updatable) — standardul SQL** permite actualizarea dacă:
- e creat cu `SELECT` **fără `DISTINCT`** pe **o singură tabelă** T;
- atributele din T care nu apar în view pot fi NULL / au valoare default;
- subinterogările nu referă T;
- **fără `GROUP BY`** sau altă agregare.

**Alternativă:** declanșatoare de tip **`INSTEAD OF`** (rescriu manual orice modificare).

**View-uri materializate** (`CREATE MATERIALIZED VIEW`): rezultatul e **stocat efectiv** ca tabel → interogări mai rapide, dar ocupă spațiu și trebuie **reîmprospătat** la modificarea tabelelor de bază.

---

## 10. Procesarea interogărilor ⭐⭐
> *„Pașii procesării interogărilor. Plan de execuție. Algoritmi de join."*

### Etapele procesării unei interogări
```
COMPILARE:
  1. Analiză sintactică (parsare) → arbore de parsare
  2. Preprocesare (analiză semantică): rescrierea view-urilor, verificarea
     existenței tabelelor/atributelor, verificarea tipurilor
  3. Rescriere în algebra relațională → PLAN LOGIC
OPTIMIZARE:
  4. Optimizarea planului logic (rescrieri algebrice echivalente:
     împingerea selecțiilor/proiecțiilor devreme, ordonarea join-urilor)
  5. Selecția algoritmilor și a ordinii → PLAN FIZIC (alege primitivele de execuție)
EXECUȚIE:
  6. Execuția planului fizic
```
**Optimizări cheie:** „împinge" selecțiile și proiecțiile **cât mai devreme** (reduci dimensiunea relațiilor înainte de join); ordonarea join-urilor (pentru n relații există `(2(n-1))!/(n-1)!` ordonări → programare dinamică). **Plan de execuție:** în Oracle `EXPLAIN PLAN FOR <sql>`.

### Algoritmi de join
- **Nested-loop join** (bucle imbricate): pentru fiecare tuplu din r, parcurgi tot s. Cost mare.
- **Indexed nested-loop join:** folosești un **index** pe atributul de join al relației interioare (echi/natural join).
- **Merge join** (cu fuziune): **sortezi** ambele relații după atributul de join, apoi le fuzionezi. Doar pentru echi-join. Cost `br + bs` + sortare.
- **Hash join:** o funcție hash partiționează ambele relații; tuplele din `rᵢ` se compară doar cu cele din `sᵢ`. Doar pentru echi-join.

**Materializare** (rezultate intermediare pe disc) vs **pipelining** (tuplele trec imediat la operatorul superior).

---

## 11. Descompunerea join fără pierdere ⭐⭐
> *„Descompunere join cu și fără pierdere."*

O descompunere ρ = {R₁, R₂, ...} a lui R e o **descompunere de tip join** dacă reuniunea atributelor lor = atributele lui R. Ea e **fără pierdere** (lossless) dacă orice relație r se reconstruiește exact prin join: `r = r[R₁] ⋈ r[R₂] ⋈ ...`.

**Teoremă (pentru 2 scheme):** ρ = {R₁, R₂} e **fără pierdere** ⟺
```
R₁ ∩ R₂ → R₁ − R₂  ∈ Σ⁺   SAU   R₁ ∩ R₂ → R₂ − R₁  ∈ Σ⁺
```
(atributul comun determină funcțional restul uneia dintre scheme).

**Exemplu:** R[A,B,C], Σ = {A→B}.
- ρ₁ = {[A,B], [A,C]} — **fără pierdere** (A∩ = A, A→B ∈ Σ⁺). ✓
- ρ₂ = {[A,B], [B,C]} — **cu pierdere** (B∩ = B, dar B→A ∉ Σ⁺ și B→C ∉ Σ⁺). ✗

Se folosește în algoritmul de descompunere în **BCNF/4NF** (descompui până toate schemele sunt în BCNF/4NF, păstrând proprietatea fără pierdere).

---

## 12. Declanșatoare (triggers) ⭐
> *„Triggere."*

**Declanșator (trigger)** = constrângere **dinamică**; monitorizează schimbările în BD, verifică o condiție și inițiază o acțiune. Regulă **eveniment–condiție–acțiune**. Introduce logica aplicației în SGBD; forțează constrângeri ce nu pot fi exprimate altfel.
```sql
CREATE TRIGGER nume
  BEFORE | AFTER | INSTEAD OF  {INSERT | DELETE | UPDATE [OF ...]} ON tabel
  [REFERENCING OLD/NEW ROW/TABLE AS var]
  [FOR EACH ROW]                 -- tip row (per linie) vs statement (per comandă)
  [WHEN (condiție)]
  acțiune;                        -- comandă SQL / bloc procedural
```
*Exemplu (ștergere în cascadă):*
```sql
CREATE TRIGGER Cascade AFTER DELETE ON S
  REFERENCING OLD ROW AS O FOR EACH ROW
  DELETE FROM R WHERE A = O.B;
```
**Probleme potențiale:** ordinea când mai multe triggere se activează simultan; **înlănțuire/auto-declanșare** care poate duce la **ciclare**.

---

## Anexă — Introducere, modele de baze de date

**De ce BD și nu fișiere?** separarea/izolarea datelor (fără join-uri), duplicarea (integritate), interdependența (chei), formate incompatibile, lipsa unui limbaj de interogare, acces concurent limitat, ACID manual.

**SGBD (Sistem de Gestiune a BD)** = Hardware + Software + Utilizatori + Date. Software oferă: **DDL** (Data Definition), **DML** (Data Manipulation), **DCL** (grant/revoke), **TCL** (commit/rollback/savepoint).

**Modele de BD:** ierarhic (arbore, un părinte), rețea (CODASYL), **relațional** (Codd, 1970 — cel studiat), obiect-relațional. SGBD-uri relaționale: MySQL, MariaDB, Oracle, PostgreSQL, SQL Server, SQLite — toate folosesc **SQL**.

**Modelul relațional (Codd):** fiecare element conține informație (null e informație), fiecare atribut e unic, valorile unui atribut sunt din același domeniu, nu există linii identice, ordinea rândurilor/coloanelor e arbitrară. Bazat pe **algebra relațională**.

**Metodologia de proiectare:** 1. Analiza cerințelor → 2. Modelare conceptuală (diagrame E/A, UML) → 3. Modelare logică (normalizare, schema relațională) → 4. Modelare fizică (indexare, optimizare).

---

# 🎯 TEST GRILĂ — BD (recapitulare)
> Apasă pe „Răspuns". Distractorii = confuzii frecvente reale.

**1.** Operația de **proiecție** (π) în algebra relațională:
- A) selectează anumite **linii** (tuple) care satisfac o condiție
- B) păstrează anumite **coloane** (atribute) ale relației
- C) combină două relații după atributele comune
- D) elimină tuplele dintr-o relație

<details><summary>✅ Răspuns</summary>

**B)** — A descrie **selecția** (σ); C = join natural. Proiecția = `SELECT col1, col2 FROM ...`.
</details>

**2.** Care operație de agregare **NU** poate fi exprimată prin ceilalți operatori de bază ai algebrei relaționale?
- A) intersecția
- B) join-ul natural
- C) join-ul extern
- D) agregarea (avg, sum, count...)

<details><summary>✅ Răspuns</summary>

**D)** — Intersecția, join-ul și join-ul extern se pot exprima cu operatorii de bază; **agregarea** nu.
</details>

**3.** O schemă e în **BCNF** dacă pentru orice dependență funcțională netrivială `X → A`:
- A) A este atribut prim
- B) X este **supercheie**
- C) X este atribut neprim
- D) A nu e tranzitiv dependent

<details><summary>✅ Răspuns</summary>

**B)** — BCNF: partea stângă a oricărei FD netriviale trebuie să fie supercheie. BCNF ⟹ 3NF.
</details>

**4.** Relația corectă dintre formele normale este:
- A) 2NF ⟹ 3NF ⟹ BCNF ⟹ 4NF (mai slabă implică mai tare)
- B) 4NF ⟹ BCNF ⟹ 3NF ⟹ 2NF ⟹ 1NF (mai tare implică mai slabă)
- C) sunt independente între ele
- D) 1NF e cea mai strictă

<details><summary>✅ Răspuns</summary>

**B)** — 4NF e cea mai strictă; dacă o schemă e în 4NF, e automat în toate cele anterioare.
</details>

**5.** Un **B+ arbore** are complexitatea căutării/inserării/ștergerii:
- A) O(1)
- B) O(n)
- C) O(log n)
- D) O(n log n)

<details><summary>✅ Răspuns</summary>

**C)** — Arbore echilibrat; restructurarea la inserare/ștergere necesită timp logaritmic. Doar frunzele trimit spre datele.
</details>

**6.** Un index **bitmap** e potrivit pentru:
- A) atribute cu **multe** valori distincte
- B) interogări de tip interval
- C) atribute cu **puține** valori distincte (categoriale) și condiții compuse
- D) chei primare

<details><summary>✅ Răspuns</summary>

**C)** — Bitmap = un șir binar per valoare; interogările multiple se rezolvă cu AND/OR/NOT pe biți.
</details>

**7.** Un index **hash** NU este eficient pentru:
- A) căutări punctuale (egalitate)
- B) interogări de tip **interval**
- C) reducerea coliziunilor
- D) accesul O(1)

<details><summary>✅ Răspuns</summary>

**B)** — Valorile hash nu sunt ordonate → interogările de interval sunt ineficiente. Pentru interval → B+ arbori.
</details>

**8.** `X → Y` (dependență funcțională) înseamnă că:
- A) dacă două tuple coincid pe **X**, atunci coincid pe **Y**
- B) X și Y au aceleași valori
- C) Y determină X
- D) X și Y sunt chei

<details><summary>✅ Răspuns</summary>

**A)** — X determină funcțional Y. Axiomele lui Armstrong: reflexivitate, augmentare, tranzitivitate.
</details>

**9.** O **cheie candidat** este:
- A) orice atribut al relației
- B) o supercheie **minimală** (nicio submulțime proprie nu e supercheie)
- C) o cheie străină
- D) cheia primară aleasă

<details><summary>✅ Răspuns</summary>

**B)** — Supercheie = identifică unic; candidat = supercheie minimală; primară = candidata aleasă.
</details>

**10.** O subinterogare **corelată**:
- A) se execută o singură dată, independent de exterioară
- B) se re-execută **pentru fiecare tuplu** al interogării exterioare (depinde de ea)
- C) nu poate fi scrisă în SQL
- D) e mereu mai rapidă decât una necorelată

<details><summary>✅ Răspuns</summary>

**B)** — Corelată = depinde de exterioară → re-evaluată per tuplu (mai costisitoare). Necorelată = o dată.
</details>

**11.** Ce marchează **încheierea cu succes** a unei tranzacții?
- A) `BEGIN TRANSACTION`
- B) `ROLLBACK`
- C) `COMMIT`
- D) `SAVEPOINT`

<details><summary>✅ Răspuns</summary>

**C)** — `COMMIT` face permanente efectele (Durabilitate); `ROLLBACK` anulează; `BEGIN` marchează începutul.
</details>

**12.** Litera **I** din ACID înseamnă:
- A) Integritate
- B) Izolare (tranzacțiile nu se influențează, chiar simultane)
- C) Indexare
- D) Inserare

<details><summary>✅ Răspuns</summary>

**B)** — A=Atomicitate, C=Consistență, I=Izolare, D=Durabilitate.
</details>

**13.** Un **view** (tabel virtual) este actualizabil (updatable) în standardul SQL dacă:
- A) conține `GROUP BY` și agregare
- B) e definit cu `SELECT DISTINCT`
- C) e pe **o singură tabelă**, fără `DISTINCT`, fără `GROUP BY`/agregare
- D) referă mai multe tabele cu join

<details><summary>✅ Răspuns</summary>

**C)** — Agregarea/GROUP BY/DISTINCT/join-urile fac view-ul neactualizabil (nu se pot rescrie univoc pe tabelele de bază).
</details>

**14.** O descompunere ρ = {R₁, R₂} este **fără pierdere** dacă:
- A) R₁ și R₂ nu au atribute comune
- B) `R₁ ∩ R₂ → R₁−R₂` sau `R₁ ∩ R₂ → R₂−R₁` ∈ Σ⁺
- C) R₁ = R₂
- D) mereu (orice descompunere e fără pierdere)

<details><summary>✅ Răspuns</summary>

**B)** — Atributul comun trebuie să determine funcțional restul uneia dintre scheme.
</details>

**15.** O dependență **multivaluată** `X ↠ Y` exprimă că:
- A) X determină o singură valoare a lui Y
- B) valorile lui Y sunt **independente** de restul atributelor (Z), pentru un X fixat
- C) Y este cheie primară
- D) X și Y sunt tranzitiv dependente

<details><summary>✅ Răspuns</summary>

**B)** — MVD = independență între Y și Z pentru X fixat → apar toate combinațiile. Stă la baza 4NF.
</details>

---

> ⭐ **Strategie:** primele 3 teme (algebra relațională + SQL, forme normale, indexare) apar la aproape fiecare examen. Dependențele (funcționale + multivaluate) și cheile completează miezul. Pentru B+ arbori — exersează să desenezi unul (te pot pune la tablă).
