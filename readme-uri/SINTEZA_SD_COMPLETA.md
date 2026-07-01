# SINTEZĂ COMPLETĂ — STRUCTURI DE DATE (SD)
> Acoperă toate cele 12 cursuri. Organizat după frecvența la examen (⭐).
> Lângă fiecare capitol scrie la ce întrebări din anul trecut corespunde.

---

## CUPRINS (după prioritatea la examen)
> **Prioritate actualizată cu întrebări din 2021, 2022, 2023, 2024.** "×N" = de câte ori a apărut.

| # | Temă | Recurență 2021–2024 | Prioritate |
|---|------|---------------------|------------|
| 1 | [Complexitate & Analiza eficienței (caz fav/nefav, O/Ω/Θ, ordine de creștere)](#1-complexitate--analiza-eficienței-) | în fiecare an (×4+) | ⭐⭐⭐⭐⭐ |
| 2 | [Complexitatea funcțiilor recursive (4 metode + Teorema Master)](#2-complexitatea-funcțiilor-recursive-) | în fiecare an (Master ×4) | ⭐⭐⭐⭐⭐ |
| 7bis | [**Union-Find** (colecții de mulțimi disjuncte)](#7bis-colecții-de-mulțimi-disjuncte-union-find) | **foarte frecvent (×3 într-un an!)** | ⭐⭐⭐⭐⭐ |
| 3 | [Tipurile abstracte Stivă și Coadă](#3-tipurile-abstracte-stivă-și-coadă-) | recurent | ⭐⭐⭐ |
| 4 | [Sortare prin numărare și distribuire](#4-sortare-prin-numărare-și-distribuire-) | recurent | ⭐⭐⭐ |
| 5 | [Grafuri, digrafuri, parcurgeri, componente (tare) conexe](#5-grafuri-digrafuri-parcurgeri-) | **grafuri ×3 într-un an** | ⭐⭐⭐⭐ |
| 6 | [Arbori binari de căutare (ABC)](#6-arbori-binari-de-căutare-abc-) | în fiecare an | ⭐⭐⭐⭐ |
| 7 | [Coadă cu priorități / Heap (min/max)](#7-coadă-cu-priorități--heap-) | recurent | ⭐⭐⭐ |
| 8 | [Arbori echilibrați (AVL, Roșu-Negru)](#8-arbori-echilibrați-avl-roșu-negru-) | în fiecare an (AVL + RB) | ⭐⭐⭐⭐ |
| 9 | [Arbori digitali (Trie)](#9-arbori-digitali-trie-) | rar | ⭐ |
| A,B,C | [Anexe: **Tipuri de date/clasificare**, **Tablouri/structuri**, **Liste liniare**, Sortări comparative, Hash](#anexe) | **toate recurente** (vezi mai jos) | ⭐⭐⭐ |

> **Top recurente SD (toți anii 2021–2024):**
> - **Union-Find** — printre cele mai întrebate (apare de mai multe ori chiar în același an); **învață și LA CE folosește** (componente conexe, Kruskal) — profesorii întreabă explicit.
> - **Complexitate** (caz fav/nefav cu **exemplu de algoritm dat de tine**, O/Ω/Θ, clasificarea claselor O) + **funcții recursive** (substituție, iterație, arbori de recursie, **Teorema Master**).
> - **Grafuri/digrafuri** (reprezentare: liste/matrice de adiacență; parcurgeri DFS/BFS) — foarte frecvent.
> - **ABC** + **arbori echilibrați AVL și Roșu-Negru** — în fiecare an.
> - **Liste liniare** (definiție + implementări), **Stiva și Coada**, **Coada cu priorități/Heap** — recurente.
> - **Sortări** (prin comparație; numărare/distribuire) — recurente.
> - **Tipuri de date — definiție și clasificare** și **Tablouri și structuri** — recurente (în Anexa A; vezi acolo).
> - **Tabele de dispersie / coliziuni** — recurent (Anexa E).

---

## 1. Complexitate & Analiza eficienței ⭐⭐⭐⭐⭐
> Întrebări: *"Eficiență și complexitate în timp. Caz favorabil/nefavorabil. Exemple. Ordinul de creștere."* / *"Complexități. Notația asimptotică."*

### Ce înseamnă analiza eficienței

Estimează volumul de **resurse** consumate de un algoritm:
- **Timp** de execuție (cel mai important)
- **Spațiu** de memorie

**Modelul de calcul = RAM (Random Access Machine):**
- prelucrările se execută secvențial;
- fiecare operație elementară (atribuire, comparație, operație aritmetică/logică) costă **o unitate de timp**;
- timpul de acces la memorie este același indiferent de locație.

**Dimensiunea problemei** = volumul datelor de intrare (ex: `n` = numărul de elemente dintr-un tablou).

### Cele 3 cazuri de analiză

Timpul de execuție depinde de **dimensiunea problemei** ȘI de **proprietățile datelor de intrare**. De aceea analizăm:

| Caz | Ce reprezintă | La ce folosește |
|-----|---------------|-----------------|
| **Favorabil** (best case) | cel mai mic timp posibil | margine inferioară; identifică algoritmi ineficienți |
| **Nefavorabil** (worst case) | cel mai mare timp posibil | **margine superioară — cea mai importantă** (garanție) |
| **Mediu** (average case) | media pe toate intrările | comportament realist; necesită distribuția de probabilitate |

**Exemplu — căutarea secvențială** a unei valori `v` într-un tablou de `n` elemente:
- **Favorabil:** `v` este pe prima poziție → `T(n) = O(1)`
- **Nefavorabil:** `v` lipsește sau e ultimul → `T(n) = O(n)`
- **Mediu** (dacă `v` e prezent cu probabilitate egală pe orice poziție): `T(n) ≈ n/2 = O(n)`

> **Observație importantă:** timpul mediu NU este neapărat media aritmetică între caz favorabil și nefavorabil.

### Ordinul de creștere

Ne interesează cum **crește** timpul când crește `n`, nu valoarea exactă. Păstrăm doar **termenul dominant** și ignorăm constantele:

```
T(n) = 3n² + 100n + 50   →   ordin de creștere O(n²)
                              (n² domină când n e mare)
```

Ce se întâmplă când dublăm `n` (k=2):

| Ordin | T(n) | T(2n) | Efect |
|-------|------|-------|-------|
| liniar | `an` | `2an` | se dublează |
| logaritmic | `a·log n` | `a·log n + a` | crește puțin |
| pătratic | `an²` | `4an²` | crește de 4× |
| exponențial | `aⁿ` | `(aⁿ)²` | se ridică la pătrat (catastrofal) |

### Notațiile asimptotice O, Ω, Θ

Acestea descriu **clase de funcții**. Fie `f, g : N → R⁺`.

**O (O-mare) — margine SUPERIOARĂ** ("cel mult la fel de mare"):
```
f(n) = O(g(n))  ⟺  ∃ c>0, n₀  astfel încât  0 ≤ f(n) ≤ c·g(n),  ∀n ≥ n₀
```
Reprezentare grafică:
```
valoare
  │           c·g(n)  ← plafonul
  │         ╱
  │       ╱ ___ f(n)   (f rămâne SUB c·g)
  │     ╱ ╱
  │   ╱_╱
  │ ╱╱
  └──────┼──────────► n
         n₀
```
Exemplu: `3n + 3 = O(n)` (cu c=4, n₀=3, fiindcă `4n ≥ 3n+3` pentru n≥3).

**Ω (Omega) — margine INFERIOARĂ** ("cel puțin la fel de mare"):
```
f(n) = Ω(g(n))  ⟺  ∃ c>0, n₀  astfel încât  f(n) ≥ c·g(n) ≥ 0,  ∀n ≥ n₀
```
```
valoare
  │         ___ f(n)   (f rămâne DEASUPRA c·g)
  │       ╱╱
  │     ╱ ╱
  │   ╱  ╱  c·g(n)  ← podeaua
  │ ╱  ╱
  └──────┼──────────► n
         n₀
```

**Θ (Theta) — margine STRÂNSĂ** ("exact același ordin"):
```
f(n) = Θ(g(n))  ⟺  ∃ c₁,c₂>0, n₀  astfel încât  c₁·g(n) ≤ f(n) ≤ c₂·g(n),  ∀n ≥ n₀
```
```
valoare
  │          c₂·g(n)  ← plafon
  │        ╱___
  │      ╱_╱ f(n)   (f e PRINSĂ între c₁·g și c₂·g)
  │    ╱╱╱
  │  ╱╱  c₁·g(n)  ← podea
  └──────┼──────────► n
         n₀
```

**Relația fundamentală:**
```
Θ(g(n)) = O(g(n)) ∩ Ω(g(n))
```
adică `f = Θ(g)` ⟺ `f = O(g)` ȘI `f = Ω(g)`.

### Proprietățile notațiilor asimptotice

| Proprietate | O | Ω | Θ |
|-------------|---|---|---|
| Reflexivitate `f ∈ X(f)` | ✓ | ✓ | ✓ |
| Tranzitivitate `f∈X(g), g∈X(h) ⟹ f∈X(h)` | ✓ | ✓ | ✓ |
| Simetrie `f∈Θ(g) ⟹ g∈Θ(f)` | — | — | ✓ |

**Reguli utile (Θ):**
- Pentru un polinom `T(n) = a_d·nᵈ + ... + a₁n + a₀` → `T(n) = Θ(nᵈ)` (contează doar gradul).
- `Θ(c·g(n)) = Θ(g(n))` (constantele se ignoră).
- `Θ(f(n) + g(n)) = Θ(max{f(n), g(n)})` (domină termenul mai mare).
- `Θ(log_a n) = Θ(log_b n)` (baza logaritmului nu contează).

**Compararea a doi timpi prin limită** — calculezi `lim_{n→∞} T1(n)/T2(n)`:
```
= 0      → T1 are ordin de creștere MAI MIC decât T2
= c > 0  → T1 și T2 au ACELAȘI ordin
= ∞      → T1 are ordin MAI MARE decât T2
```

### Exemple de calcul al complexității (Curs 2)

**Ex. 1 — Suma primelor n numere:** un singur `for` de la 1 la n → `T(n) = Θ(n)` (liniar).

**Ex. 2 — Înmulțirea a două matrici** (m×n cu n×p): trei `for`-uri imbricate; operația dominantă `a[i,k]*b[k,j]` se execută `m·n·p` ori → `T(m,n,p) = Θ(mnp)` (pentru matrici n×n: **Θ(n³)**, cubic).

**Ex. 3 — Minimul unui tablou:** o singură parcurgere → `Θ(n)`. (favorabil 3n, nefavorabil 4n−1, ambele liniare)

**Ex. 4 — Căutarea secvențială:** favorabil O(1) (primul element), nefavorabil O(n) (lipsește/ultimul).

**Ex. 5 — Sortarea prin inserție:** favorabil O(n) (deja sortat), nefavorabil O(n²) (sortat invers), mediu O(n²).

### Analiza în cazul mediu

Se bazează pe **distribuția de probabilitate** a datelor de intrare. Dacă datele se grupează în `m` clase cu probabilitățile `P₁,...,Pₘ` și timpii `T₁,...,Tₘ`:
```
T_mediu(n) = P₁·T₁(n) + P₂·T₂(n) + ... + Pₘ·Tₘ(n)
```
> Timpul mediu NU este neapărat media aritmetică a cazurilor extreme.

**Analiza empirică** (când cea teoretică e dificilă): implementezi algoritmul, generezi date de intrare, măsori (nr. operații/timp) și analizezi rezultatele.

### Clasificarea algoritmilor (de la rapid la lent)

```
O(1) ⊂ O(log n) ⊂ O(n) ⊂ O(n log n) ⊂ O(n²) ⊂ O(n³) ⊂ O(2ⁿ) ⊂ O(n!)
constant  log    liniar  liniaritmic  pătr.  cubic  expon.  factorial
```

| Clasă | Exemplu tipic |
|-------|---------------|
| O(log n) | căutare binară |
| O(n) | căutare secvențială, minim într-un tablou |
| O(n log n) | merge sort, heap sort, quick sort (mediu) |
| O(n²) | sortare prin inserție/selecție/bubble |
| O(n³) | înmulțirea a două matrici n×n (naiv), Warshall |
| O(2ⁿ) | submulțimile unei mulțimi; Fibonacci recursiv |
| O(n!) | permutările de ordin n |

### Etapele analizei unui algoritm

1. Identifică **dimensiunea problemei**.
2. Identifică **operația dominantă** (cea mai frecventă/costisitoare).
3. Numără de câte ori se execută operația dominantă.
4. Dacă timpul depinde de date → analizează favorabil / nefavorabil / mediu.
5. Stabilește ordinul (clasa) de complexitate.

---

## 2. Complexitatea funcțiilor recursive ⭐⭐⭐⭐⭐
> Întrebări (cele mai frecvente!): *"cele 4 metode pt complexitatea recursivă (LE-AU PUS LA TOATE)"*, *"metoda substituției și iterației"*, *"Arbori de recursie, Teorema Master"*.

### Ce e o funcție recursivă

O funcție care **se auto-apelează** (direct sau indirect). Definiția are mereu:
- **Cazul de bază** — condiția de oprire (fără el → recursie infinită);
- **Cazul general** — apelul recursiv pe o problemă mai mică.

```
Function factorial(n)
  if n <= 1 then return 1          // caz de bază
  else return n * factorial(n-1)   // caz general
```

Pentru a analiza timpul, scriem o **relație de recurență**. Pentru factorial:
```
T(n) = T(n-1) + 1,   T(1) = 0     (un apel + o operație)
```

> **Cost ascuns al recursiei:** la fiecare apel se salvează date pe **stiva programului** → consum de memorie suplimentar.

### Cele 4 metode de rezolvare a recurențelor

```
┌─────────────────────────────────────────────────────────┐
│ 1. SUBSTITUȚIA  → ghicești soluția, demonstrezi prin inducție │
│ 2. ITERAȚIA     → desfășori recurența până la cazul de bază  │
│ 3. ARBORE RECURSIE → vizualizezi costurile pe niveluri        │
│ 4. TEOREMA MASTER → formulă directă pt T(n)=aT(n/b)+f(n)      │
└─────────────────────────────────────────────────────────┘
```

---

### Metoda 1 — Substituția

**Pași:** (a) *ghicești* forma soluției; (b) demonstrezi prin **inducție matematică** și determini constantele.

**Exemplu:** `T(n) = 2T(n/2) + n`
- Ghicim: `T(n) = O(n log n)`, adică `T(n) ≤ c·n·log n`.
- Inducție (presupunem adevărat pentru n/2):
```
T(n) ≤ 2·(c·(n/2)·log(n/2)) + n
     = c·n·log(n/2) + n
     = c·n·log n − c·n·log 2 + n
     = c·n·log n − c·n + n
     ≤ c·n·log n        (pentru c ≥ 1)  ✓
```

> ⚠️ Capcană: trebuie demonstrată **forma exactă** a ipotezei. Dacă "ghicești" `T(n)=O(n)` pentru `2T(n/2)+n`, demonstrația pare să meargă (`cn+n`) dar e **falsă** — nu obții exact `cn`.

---

### Metoda 2 — Iterația

Desfășori recurența înlocuind termenii succesiv, până ajungi la cazul de bază; apoi aduni.

**Exemplu:** `T(n) = T(n-1) + 1`, `T(1) = 0`
```
T(n)   = T(n-1) + 1
T(n-1) = T(n-2) + 1
...
T(2)   = T(1)  + 1
T(1)   = 0
───────────────────  (aduni toate)
T(n) = n − 1   →   O(n)
```

**Exemplu cu serie geometrică:** `T(n) = 3T(n/4) + n`
```
T(n) = n + 3(n/4) + 9(n/16) + 27(n/64) + ...
     = n · [1 + 3/4 + (3/4)² + (3/4)³ + ...]
     ≤ n · 1/(1 − 3/4) = 4n   →   O(n)
```
(folosind suma seriei geometrice `1 + x + x² + ... = 1/(1−x)` pentru |x|<1)

---

### Metoda 3 — Arborele de recursie

Fiecare nod = costul unei subprobleme. Aduni costurile **pe niveluri**, apoi totul.

**Exemplu:** `T(n) = 3T(n/4) + cn²`
```
Nivel 0:                cn²                         cost: cn²
                     ╱   │   ╲
Nivel 1:        c(n/4)² c(n/4)² c(n/4)²            cost: 3·c(n/4)² = (3/16)cn²
                ╱│╲    ╱│╲    ╱│╲
Nivel 2:    9 noduri de c(n/16)²                   cost: (3/16)²cn²
                ...
Nivel i:    3ⁱ noduri, fiecare c(n/4ⁱ)²            cost: (3/16)ⁱ cn²
```
- Înălțimea arborelui: `n/4ⁱ = 1 ⟹ i = log₄ n` → sunt `log₄ n + 1` niveluri.
- Cost total:
```
T(n) = Σ (3/16)ⁱ cn²  ≤  cn² · 1/(1 − 3/16)  =  (16/13)cn²  →  O(n²)
```

---

### Metoda 4 — Teorema Master

Rezolvă **direct** recurențe de forma:
```
T(n) = a·T(n/b) + f(n),   cu a ≥ 1, b > 1
```
unde: `a` = numărul de subprobleme, `n/b` = dimensiunea unei subprobleme, `f(n)` = costul de divizare+combinare.

Compari `f(n)` cu `n^(log_b a)`:

```
┌──────────────────────────────────────────────────────────────┐
│ CAZ 1: f(n) crește MAI ÎNCET decât n^(log_b a)                 │
│        f(n) = O(n^(log_b a − ε))                               │
│        ⟹  T(n) = Θ(n^(log_b a))        [recursia domină]      │
├──────────────────────────────────────────────────────────────┤
│ CAZ 2: f(n) crește LA FEL ca n^(log_b a)                       │
│        f(n) = Θ(n^(log_b a))                                   │
│        ⟹  T(n) = Θ(n^(log_b a) · log n)  [echilibru]          │
├──────────────────────────────────────────────────────────────┤
│ CAZ 3: f(n) crește MAI REPEDE decât n^(log_b a)               │
│        f(n) = Ω(n^(log_b a + ε))  + condiția de regularitate   │
│        ⟹  T(n) = Θ(f(n))               [f(n) domină]          │
└──────────────────────────────────────────────────────────────┘
```

**Exemple:**

| Recurență | a | b | f(n) | n^(log_b a) | Caz | Rezultat |
|-----------|---|---|------|-------------|-----|----------|
| `9T(n/3)+n` | 9 | 3 | n | n²  | 1 | Θ(n²) |
| `T(2n/3)+1` | 1 | 3/2 | 1 | n⁰=1 | 2 | Θ(log n) |
| `3T(n/4)+n log n` | 3 | 4 | n log n | n^0.79 | 3 | Θ(n log n) |

> ⚠️ Teorema **NU se aplică** dacă `f(n)` e doar **logaritmic** mai mare (nu polinomial). Ex: `T(n)=2T(n/2)+n log n` — aici `f(n)/n^(log_b a) = log n`, nu e `n^ε`, deci Master nu funcționează.

### Recursie vs. iterație — exemplu Fibonacci

```
Fibonacci RECURSIV:  fib(n) = fib(n-1) + fib(n-2)
                     → O(φⁿ) EXPONENȚIAL (recalculează aceleași valori!)

         fib(5)
        ╱      ╲
    fib(4)    fib(3)         ← fib(3) calculat de 2 ori
    ╱   ╲      ╱   ╲
 fib(3) fib(2) ...           ← fib(2) calculat de mai multe ori

Fibonacci ITERATIV:  un singur for de la 2 la n
                     → O(n) LINIAR
```

---

## 3. Tipurile abstracte Stivă și Coadă ⭐⭐⭐
> Întrebări: *"Tipurile abstracte de date Stiva si Coada"*, *"Ce e un tip abstract?"*

### Ce este un tip abstract de date (TAD)?

Un **TAD** definește:
- **Obiectele** (ce date reprezintă);
- **Operațiile** permise asupra lor (CE fac, nu CUM).

Separă **specificarea** (interfața) de **implementare**. Aceeași stivă poate fi implementată cu tablouri SAU cu liste înlănțuite — utilizatorul nu trebuie să știe care.

---

### STIVA (Stack) — LIFO

**LIFO = Last In, First Out** (ultimul intrat, primul ieșit).

```
        push(e) ↓   ↑ pop()/top()
              ┌─────┐
              │  e₃ │ ← vârf (top) — ultimul introdus
              ├─────┤
              │  e₂ │
              ├─────┤
              │  e₁ │ ← baza (primul introdus)
              └─────┘
```
> Analogie: un teanc de farfurii — pui și iei mereu de deasupra.

**Operații:**
| Operație | Efect |
|----------|-------|
| `stivaVida()` | creează stivă goală |
| `esteVida(S)` | true dacă e goală |
| `push(S, e)` | adaugă `e` în vârf |
| `pop(S)` | scoate elementul din vârf (eroare dacă vidă) |
| `top(S)` | citește vârful fără a-l scoate |

**Aplicații:** istoric browser (back), undo în editor, apeluri recursive (stiva programului), evaluare expresii.

**Implementare cu tablou:**
```
S.tab: [e₀][e₁][e₂][ ][ ]      S.varf = 2
                    ↑ varf
push: varf++; tab[varf]=e        pop: varf--
```

**Implementare cu listă înlănțuită** (inserare/ștergere la început):
```
S → [e₃|•] → [e₂|•] → [e₁|•] → NULL
    push/pop se fac AICI (la cap) → O(1)
```

---

### COADA (Queue) — FIFO

**FIFO = First In, First Out** (primul intrat, primul ieșit).

```
   elimina()        insereaza(e)
   citeste()              ↓
      ↑     ┌────┬────┬────┬────┐
   ◄──── e₀ │ e₁ │ e₂ │ e₃ │ ... │ ◄──── e (intră la coadă)
      prim  └────┴────┴────┴────┘ ultim
   (cel mai vechi)        (cel mai nou)
```
> Analogie: rândul la casă — cine vine primul, pleacă primul.

**Operații:**
| Operație | Efect |
|----------|-------|
| `coadaVida()` | creează coadă goală |
| `esteVida(C)` | true dacă e goală |
| `insereaza(C, e)` | adaugă `e` la sfârșit (ultim) |
| `elimina(C)` | scoate primul element (cel mai vechi) |
| `citeste(C)` | citește primul element fără a-l scoate |

**Aplicații:** fire de așteptare, acces la resurse partajate (imprimante), parcurgerea BFS.

**Implementare cu tablou circular** (ca să nu se "consume" tabloul):
```
indici calculați modulo Max:
   C.ultim ← (C.ultim + 1) % Max
```

**Implementare cu listă** (insert la coadă, scoatere de la cap):
```
C.prim → [e₀|•] → [e₁|•] → [e₂|•] → NULL ← C.ultim
         scoatere                  inserare
```

---

### Comparație rapidă Stivă vs Coadă

| | Stivă | Coadă |
|--|-------|-------|
| Disciplină | LIFO | FIFO |
| Inserare | vârf | sfârșit (ultim) |
| Extragere | vârf | început (prim) |
| Folosită în | DFS, undo, recursie | BFS, fire de așteptare |

### Aplicație: conversie expresii infixate → postfixate (cu stivă)

Notația postfixată (poloneză inversă) elimină nevoia de paranteze.
```
infixat:    a + b * (c + d) + e
postfixat:  a b c d + * + e +
```
Algoritmul folosește o stivă pentru operatori, comparând prioritățile. Evaluarea expresiei postfixate se face tot cu o stivă (operanzii se pun pe stivă, la operator se scot doi și se pune rezultatul).

---

## 4. Sortare prin numărare și distribuire ⭐⭐⭐
> Întrebări: *"Sortarea prin numărare. Sortarea prin distribuție."* (apare de 4 ori)

Acestea sunt sortări **NEbazate pe comparații** → pot depăși limita `O(n log n)` și ajung la **O(n)** în anumite condiții.

---

### Sortarea prin NUMĂRARE (Counting Sort)

**Ipoteză:** elementele sunt întregi într-un interval mic `{1, 2, ..., k}`.

**Idee:** pentru fiecare valoare, numără câte elemente sunt **mai mici sau egale** → asta îți dă direct poziția în tabloul sortat.

**Pași:**
1. Numără aparițiile fiecărei valori în vectorul `c[]` (`c[v]` = de câte ori apare `v`).
2. Transformă `c[]` în sume cumulate (`c[i] += c[i-1]`) → `c[v]` = câte elemente ≤ v.
3. Parcurge intrarea de la dreapta la stânga și pune fiecare element pe poziția `c[a[j]]-1`, decrementând `c`.

**Exemplu** (k=6), intrare `a = [2, 5, 3, 2, 3, 1]`:
```
Pas 1 — numărare:        valoare:  1  2  3  4  5  6
                         c[]:      1  2  2  0  1  0

Pas 2 — sume cumulate:   c[]:      1  3  5  5  6  6
                                   ↑ câte elemente ≤ valoarea respectivă

Pas 3 — plasare:  rezultat sortat → [1, 2, 2, 3, 3, 5]
```

**Complexitate:** `O(n + k)`. Eficient doar dacă `k = O(n)`. Este **stabilă**.

---

### Sortarea prin DISTRIBUIRE (Bucket Sort)

**Ipoteză:** elementele sunt distribuite **uniform** într-un interval, ex `[0, 1)`.

**Idee:** împarte intervalul în `n` "găleți" (buckets) egale, distribuie elementele, sortează fiecare găleată, apoi concatenează.

**Pași:**
1. Creează `n` găleți goale.
2. Pune `a[i]` în găleata `⌊n · a[i]⌋`.
3. Sortează fiecare găleată (cu altă metodă, ex. insertion sort).
4. Concatenează gălețile în ordine.

**Exemplu** (n=5), intrare `[0.12, 0.83, 0.09, 0.45, 0.42]`:
```
găleți:   B0[0,0.2)   B1[0.2,0.4)  B2[0.4,0.6)   B3[0.6,0.8)  B4[0.8,1)
          0.12, 0.09     —          0.45, 0.42       —          0.83
            │                          │                          │
         sortat:                    sortat:                     sortat:
          0.09,0.12                  0.42,0.45                   0.83

concatenare → [0.09, 0.12, 0.42, 0.45, 0.83]
```

**Complexitate medie:** `O(n)` (dacă distribuția e uniformă). În cel mai nefavorabil (toate într-o găleată) degenerează.

---

### Comparație numărare vs distribuire

| | Counting Sort | Bucket Sort |
|--|---------------|-------------|
| Ipoteză | întregi în {1..k} | valori uniform distribuite |
| Complexitate | O(n + k) | O(n) mediu |
| Tehnică | numără aparițiile | distribuie în găleți + sortează |
| Stabilă | da | da |

> Tabel complet de complexități al tuturor sortărilor — vezi [Anexa: Sortări comparativ](#anexă-c--sortări-comparativ).

---

## 5. Grafuri, digrafuri, parcurgeri ⭐⭐⭐
> Întrebări: *"Grafuri. Digrafuri. Reprezentare. Parcurgere."*, *"Componente conexe și componente tare conexe și alg pt a le afla."*

### Definiții

**Graf neorientat** `G = (V, E)`:
- `V` = mulțime de **vârfuri** (noduri);
- `E` = mulțime de **muchii** (perechi **neordonate**): `{i,j} = {j,i}`.

**Digraf (graf orientat)** `D = (V, A)`:
- `A` = mulțime de **arce** (perechi **ordonate**): `(i,j) ≠ (j,i)`.

```
   GRAF (neorientat)              DIGRAF (orientat)
                                  
    0 ──── 1                       0 ───► 1
    │    ╱                         ▲      │
    │   ╱                          │      ▼
    2 ──── 3                       2 ◄─── 3

  E={{0,1},{0,2},{1,2},{2,3}}   A={(0,1),(2,0),(1,2),(3,2)}
```

**Terminologie:**
- **drum (path):** secvență de vârfuri distincte legate prin muchii/arce;
- **ciclu (cycle):** drum închis (i₀ = ik);
- vârfuri **adiacente (vecine):** legate printr-o muchie.

---

### Reprezentare (2 metode)

**1) Matrice de adiacență** `a[i][j]`:
```
a[i][j] = 1 dacă există muchie/arc i→j, altfel 0

       0  1  2  3
    0 [0  1  0  0]
    1 [0  0  1  0]    ← pentru digraful de mai sus
    2 [1  0  0  0]
    3 [0  0  1  0]
```
- La graf neorientat matricea e **simetrică**.
- Spațiu: `O(n²)`. Test "există arc i→j?": `O(1)`.
- Inserare arc: O(1); eliminare vârf: O(n²).

**2) Liste de adiacență** — tablou de liste înlănțuite:
```
a[0] → 1 → 2          (vecinii lui 0)
a[1] → 2
a[2] → 0
a[3] → 2
```
- Spațiu: `O(n + m)` (m = nr. de arce). Mai bun pentru grafuri **rare**.
- Inserare vârf/arc: O(1); eliminare vârf: O(n+m).

| | Matrice | Liste |
|--|---------|-------|
| Spațiu | O(n²) | O(n+m) |
| "Există i→j?" | O(1) | O(grad) |
| Bună pentru | grafuri dense | grafuri rare |

---

### Parcurgeri (explorare sistematică)

Se gestionează: `S` = vârfuri vizitate, `SB` = vârfuri de procesat. Diferența DFS/BFS = **structura folosită pentru SB**:

**DFS (Depth First Search) — în adâncime → STIVĂ**
```
Merge cât poate în adâncime, apoi se întoarce (backtrack).

    1 ──► 2 ──► 4          Ordine DFS de la 1: 1, 2, 4, 3, 5
    │     │                (folosește stiva — LIFO)
    ▼     ▼
    3 ──► 5
```

**BFS (Breadth First Search) — în lățime → COADĂ**
```
Vizitează nivel cu nivel (toți vecinii direcți întâi).

    1 ──► 2 ──► 4          Ordine BFS de la 1: 1, 2, 3, 4, 5
    │     │                (folosește coada — FIFO)
    ▼     ▼
    3 ──► 5
```

| | DFS | BFS |
|--|-----|-----|
| Structură | **stivă** | **coadă** |
| Strategie | adâncime | lățime/niveluri |
| Complexitate | O(n+m) | O(n+m) |

---

### Componente conexe (graf neorientat)

O **componentă conexă** = submulțime maximală de vârfuri între care există drumuri.
Un graf e **conex** dacă are o **singură** componentă conexă.

```
   Component 1      Component 2
    0 ── 1            4 ── 5
    │                 
    2                 6
                      
   → 2 componente conexe: {0,1,2} și {4,5,6}
```

**Algoritm (DFS cu colorare):** pornești un DFS din fiecare vârf necolorat; fiecare DFS colorează o componentă întreagă. Numărul de DFS-uri pornite = numărul de componente.
```
k ← 0
pentru fiecare vârf i necolorat:
    k ← k + 1
    DFS(i, culoare=k)     // colorează toată componenta
return k                  // numărul de componente
```
Complexitate: `O(n + m)`.

---

### Componente tare conexe (digraf)

Într-un **digraf**, o **componentă tare conexă** = mulțime maximală de vârfuri în care de la **oricare** vârf `u` la oricare `v` există drum `u→v` ȘI drum `v→u` (în ambele sensuri).

```
    0 ───► 1                {0,1,2} = componentă tare conexă
    ▲      │                (0→1→2→0 ciclu, deci dus-întors între toate)
    │      ▼
    2 ◄────┘                {3} = componentă tare conexă separată
    
    3 ───► 2                (din 3 ajungi la 2, dar din 2 NU ajungi la 3)
```

**Algoritm (2 DFS-uri — bazat pe Kosaraju):**
```
1. DFS pe D → reține timpii finali de vizitare ai vârfurilor
2. Calculează Dᵀ (graful TRANSPUS: inversează toate arcele)
3. DFS pe Dᵀ, luând vârfurile în ordinea DESCRESCĂTOARE a timpilor finali
4. Fiecare arbore DFS rezultat la pasul 3 = o componentă tare conexă
```
Complexitate totală: `O(n + m)`.

**Aplicații grafuri:** Google PageRank, Google Maps (GIS), rețele sociale, planificare (sortare topologică), problema podurilor din Königsberg.

---

## 6. Arbori binari de căutare (ABC) ⭐⭐
> Întrebări: *"Arbori binari de căutare. Reprezentare și operații."*

### Definiție

Un **arbore binar de căutare (ABC)** este un arbore binar în care, pentru **orice** nod `v`:
- toate valorile din **subarborele stâng** < `v`;
- valoarea `v` < toate valorile din **subarborele drept**.

```
            18              Pentru fiecare nod:
          ╱    ╲            stânga < nod < dreapta
        10      30
       ╱  ╲    ╱  ╲
      5   15  25   40
```
> **Consecință cheie:** parcurgerea **în inordine** (stânga → rădăcină → dreapta) dă valorile **sortate crescător**: `5,10,15,18,25,30,40`.

> ABC asociat unei mulțimi **NU e unic** — depinde de ordinea inserării.

### Operații (toate proporționale cu înălțimea h)

**Căutare** `x`: pornești din rădăcină; dacă `x < nod` mergi stânga, dacă `x > nod` mergi dreapta.
```
Caută 15:   18 → (15<18, stânga) → 10 → (15>10, dreapta) → 15 ✓
```
Complexitate: `O(h)`.

**Inserare** `x`: cauți locul (ca la căutare) și adaugi noul nod ca frunză.
```
Inserează 12:  18→10→15→(12<15, stânga, gol) → pune 12 aici
```

**Eliminare** `x` — 3 cazuri:
```
Caz 1: nodul nu are fii        → îl ștergi direct
       
       ...   →   ...
        │
       [x]      (șters)

Caz 2: nodul are UN fiu        → îl înlocuiești cu fiul

       [x]            [fiu]
        │      →
      [fiu]

Caz 3: nodul are DOI fii       → îl înlocuiești cu predecesorul
       (cea mai mare valoare din subarborele stâng:
        cobori stânga o dată, apoi dreapta cât poți),
       apoi ștergi acel nod (care e în cazul 1 sau 2)
```
Complexitate eliminare: `O(h)`.

### Complexitate — importanța echilibrului

```
ABC ECHILIBRAT          ABC DEGENERAT (inserare 1,2,3,4,5 în ordine)
       3                    1
     ╱   ╲                    ╲
    2     4                    2
   ╱       ╲                    ╲
  1         5                    3       ← devine o LISTĂ!
                                  ╲
  h = O(log n)                     4      h = O(n)
  căutare O(log n)                  ╲
                                     5    căutare O(n) — la fel de lent ca o listă
```

| Caz | Înălțime | Căutare/Inserare/Ștergere |
|-----|----------|---------------------------|
| Mediu (echilibrat) | O(log n) | O(log n) |
| Nefavorabil (degenerat) | O(n) | O(n) |

→ De aici nevoia de **arbori echilibrați** (cap. 8).

---

## 7. Coadă cu priorități / Heap ⭐⭐
> Întrebări: *"Coada cu priorități. Max/min heap."*, *"Tipul abstract coadă cu priorități. Structuri Min/Max Heap."*

### TAD Coadă cu priorități

Spre deosebire de coada FIFO obișnuită, aici elementele (numite **atomi**) au o **prioritate (cheie)**, și se extrage mereu **cel mai prioritar**, nu cel mai vechi.

**Operații:**
| Operație | Efect |
|----------|-------|
| `inserează(C, atom)` | adaugă un atom |
| `citește(C)` | atomul cu prioritatea maximă |
| `elimină(C)` | scoate atomul cu prioritatea maximă |

**Aplicații:** pasagerii unui avion (business/urgențe), avioane care aterizează (urgență/carburant), planificarea proceselor.

### Max-Heap (implementarea cozii cu priorități)

Un **max-heap** este un arbore binar **complet** cu proprietatea de heap:
> **Cheia oricărui nod ≥ cheile fiilor săi.** (la min-heap: ≤)

Consecință: **maximul e mereu în rădăcină**.

```
            12          ← maximul, mereu în vârf
          ╱    ╲
         9      8
        ╱ ╲    ╱ ╲
       7   1  3   4
      ╱ ╲
     5   2

Fiecare părinte ≥ fiii săi. Arbore COMPLET (umplut de la stânga).
```

**"Arbore complet"** = toate nivelurile pline, ultimul nivel umplut de la stânga la dreapta. → înălțimea e mereu `O(log n)`.

### Reprezentare cu tablou (foarte importantă!)

Heap-ul complet se stochează compact într-un tablou, fără pointeri:
```
index:    0   1   2   3   4   5   6   7   8
tablou: [12,  9,  8,  7,  1,  3,  4,  5,  2]

Pentru nodul de la indexul i:
   fiu stâng  = 2i + 1
   fiu drept  = 2i + 2
   părinte    = (i − 1) / 2
```
```
            12 (i=0)
          ╱        ╲
      9 (i=1)      8 (i=2)
       ╱ ╲          ╱ ╲
   7(3) 1(4)     3(5) 4(6)
    ╱ ╲
 5(7) 2(8)
```

### Inserare (urcare / sift-up)

1. Adaugi noul nod la **sfârșit** (prima poziție liberă pe ultimul nivel).
2. **Urci** (swap cu părintele) cât timp e mai mare decât părintele.

```
Inserează 10 în heap:           urcă 10 (10>9, dar 10<12 → stop):

      12                              12
    ╱    ╲                          ╱    ╲
   9      8        adaugă la        10     8
  ╱ ╲    ╱ ╲       sfârșit →       ╱ ╲    ╱ ╲
 7  1   3  4                      7   9  3  4    (10 a urcat peste 9)
╱ ╲ ╱                            ╱ ╲ ╱
5 2 [10]                        5  2 1
```
Complexitate: `O(log n)`.

### Eliminare (a maximului) (coborâre / sift-down)

1. Înlocuiești rădăcina cu **ultimul** nod.
2. Ștergi ultimul nod.
3. **Cobori** rădăcina (swap cu cel mai mare fiu) cât timp e mai mică decât un fiu.

```
Elimină maximul (12):       pune ultimul (2) în vârf:    coboară 2:

      12                          2                          9
    ╱    ╲                      ╱    ╲                      ╱    ╲
   9      8       →            9      8        →           7      8
  ╱ ╲    ╱ ╲                  ╱ ╲    ╱ ╲                  ╱ ╲    ╱ ╲
 7  1   3  4                 7  1   3  4                 5   1  3  4
╱ ╲                         ╱                           ╱
5 [2]                       5                           2
```
Complexitate: `O(log n)`.

> **Teoremă:** un max-heap cu `n` chei are înălțimea `O(log₂ n)`, deci inserarea și eliminarea sunt `O(log n)`.

### Min-Heap vs Max-Heap

| | Max-Heap | Min-Heap |
|--|----------|----------|
| Proprietate | părinte ≥ fii | părinte ≤ fii |
| În rădăcină | **maximul** | **minimul** |
| Folosit pt | extragere maxim | extragere minim |

> Heap-ul stă și la baza algoritmului **Heap Sort** — vezi [Anexa C](#anexă-c--sortări-comparativ).

---

## 7bis. Colecții de mulțimi disjuncte (Union-Find)
> Tot din Cursul 6, alături de heap. TAD pentru gestionarea partițiilor.

### TAD Colecții de mulțimi disjuncte

**Obiecte:** o colecție de submulțimi **disjuncte** (o **partiție**) a unei mulțimi univers `{0, 1, ..., n-1}`.

**Operații:**
| Operație | Efect |
|----------|-------|
| `singleton(C, i)` | creează o submulțime cu unicul element `i` |
| `find(C, i)` | întoarce **reprezentantul** submulțimii care conține `i` |
| `union(C, i, j)` | **reunește** submulțimile care conțin pe `i` și `j` |

**Aplicații:** rețele de calculatoare (sunt conectate?), pixeli într-o imagine (regiuni), componente conexe, algoritmul lui Kruskal.

### Reprezentarea ca "pădure" de arbori

Fiecare submulțime e un **arbore**; reprezentantul ei e **rădăcina**. Toată colecția = o pădure de arbori. Se stochează cu **relația "părinte"** într-un singur tablou `parinte[]`:
- `parinte[i] = rădăcina părinte` a lui `i`;
- `parinte[rădăcină] = -1` (semn că e rădăcină).

```
Exemplu: n=10, C = {{1,2,6}, {3}, {0,4,5,8}, {7,9}}

   6        3        5          9        ← rădăcini (reprezentanți)
  ╱ ╲                ╲          │
 2   1              0  8        7
                    │
                    4

tablou parinte:
 index:    0  1  2  3  4  5  6  7  8  9
 parinte: [5, 6, 6,-1, 0,-1,-1, 9, 5,-1]
           ↑ 0 are părinte 5;  3,5,6,9 sunt rădăcini (-1)
```

### Operațiile — implementare

**singleton** — `i` devine propria rădăcină:
```
procedure singleton(C, i)
    C.parinte[i] ← -1
```

**find** — urci din `i` până la rădăcină (unde `parinte = -1`):
```
procedure find(C, i)
    temp ← i
    while (C.parinte[temp] >= 0) do
        temp ← C.parinte[temp]
    return temp                        // rădăcina = reprezentantul
```

**union** — legi rădăcina unui arbore sub rădăcina celuilalt:
```
procedure union(C, i, j)
    ri ← find(C, i)
    rj ← find(C, j)
    if ri != rj then
        C.parinte[rj] ← ri             // arborele lui j atârnă sub rădăcina lui i
```
```
union(1, 0):  find(1)=6, find(0)=5  →  parinte[5] ← 6

      6                            6
     ╱ ╲          devine          ╱│╲
    2   1                        2  1  5      ← arborele lui 0 atârnă sub 6
                                      │╲
                                     0  8
                                     │
                                     4
```

### Problema: arbori dezechilibrați

Dacă faci mereu `union` legând la întâmplare, arborii pot degenera într-un **lanț lung** → `find` devine `O(n)`. Două optimizări rezolvă asta:

**1. Union ponderat (weighted union):** legi mereu arborele **mai mic** sub cel **mai mare** (reții dimensiunea în rădăcină, cu semn negativ). Ține arborii joși.
```
parinte[rădăcină] = -(număr de noduri din arbore)
La union: arborele cu mai puține noduri atârnă sub cel cu mai multe.
```

**2. Aplatizarea (path compression):** la `find`, după ce găsești rădăcina, **relegi direct** toate nodurile de pe drum la rădăcină. Următoarele `find` devin instant.
```
find(9) înainte:          find(9) după aplatizare:

      0                          0
      │                        ╱ │ ╲ ╲
      3                       3  6  9 ...     ← toate atârnă direct de rădăcină
      │                       
      6                       
      │                       
      9                       
```

**Complexitate (cu ambele optimizări):**
> **Teoremă:** o secvență de `m` operații `union`/`find` pe `n` elemente are complexitatea `O(n + m·log* n)`, unde `log* n` (logaritm iterat) este numărul de logaritmări succesive până ajungi la 1 — practic o constantă (`log* n ≤ 5` pentru orice `n` realist).

---

## 8. Arbori echilibrați (AVL, Roșu-Negru) ⭐⭐
> Întrebări: *"Arbori binari de căutare echilibrați - red black trees"*, *"...AVL"*.

### De ce echilibrare?

Un ABC poate degenera în listă (`O(n)`). Arborii echilibrați **garantează** înălțimea `O(log n)` → toate operațiile `O(log n)`.

> **Definiție:** o clasă de arbori e **echilibrată** dacă `h(t) ≤ c·log n`. E **O(log n)-stabilă** dacă căutarea/inserarea/ștergerea se fac în O(log n) și rezultatul rămâne în clasă.

---

### Arbori AVL (Adelson-Velskii & Landis, 1962)

**Condiția AVL:** pentru **orice** nod `v`:
```
| înălțime(subarbore stâng) − înălțime(subarbore drept) | ≤ 1
```
Această diferență se numește **factor de echilibrare** (poate fi −1, 0, +1).

```
AVL VALID:              NU e AVL (nodul 8 dezechilibrat):
       8 (fe=0)              8        ← stâng h=2, drept h=0
     ╱   ╲                 ╱
    4     12              4           diferență = 2 > 1  ✗
   ╱ ╲   ╱ ╲            ╱
  2   6 10  14         2
```

**Reechilibrare prin ROTAȚII** (după inserare/ștergere):

Există 4 tipuri: stânga simplă, dreapta simplă, stânga-dreapta dublă, dreapta-stânga dublă.

**Rotație simplă la stânga** (când dezechilibrul e pe partea dreaptă-dreapta):
```
     x                          y
      ╲                       ╱   ╲
       y        ─────►       x     z
        ╲                     ╲
         z                    (T)

   rotatieStanga(x):
     y = x→drp
     x→drp = y→stg
     y→stg = x
     return y          // O(1)
```

**Rotație dublă** = două rotații simple consecutive (când dezechilibrul e "în zig-zag", ex. stânga-dreapta).

**Algoritm inserare AVL:**
1. Inserează ca într-un ABC obișnuit.
2. Memorează drumul de la rădăcină la nodul nou (într-o stivă).
3. Parcurge drumul invers și reechilibrează nodurile dezechilibrate cu rotații.

Complexitate: `O(log n)`.

**Avantaje/dezavantaje AVL:**
- ✓ Operații garantate `O(log n)`, arbore foarte bine echilibrat.
- ✗ Spațiu suplimentar (factor echilibrare), reechilibrări costisitoare.
- → preferați când sunt **multe căutări** și **puține** inserări/ștergeri.

---

### Arbori Roșu-Negru (Red-Black, Bayer 1972)

Arbore binar de căutare în care fiecare nod are o **culoare** (roșu/negru), respectând **4 proprietăți**:

```
1. Fiecare nod e ROȘU sau NEGRU.
2. Rădăcina și frunzele (nil) sunt NEGRE.
3. Un nod ROȘU are ambii fii NEGRI (nu există doi roșii consecutivi).
4. Orice drum de la un nod la frunzele sale are ACELAȘI număr de noduri NEGRE.
```

```
            20(N)
          ╱      ╲
       10(N)     35(N)
      ╱   ╲      ╱   ╲
   3(R)  15(R) 21(R) 50(R)
   
   (N)=negru, (R)=roșu
   Niciun roșu nu are fiu roșu; drumurile au același nr. de noduri negre.
```

Aceste reguli garantează:
> **Teoremă:** un arbore roșu-negru cu `n` noduri are înălțimea `h ≤ 2·log₂(n+1)`.
→ căutare/inserare/ștergere în `O(log n)`.

**Inserare:**
1. Inserezi ca în ABC obișnuit, **colorezi nodul nou ROȘU**.
2. Dacă părintele e roșu (încalcă prop. 3) → repari prin **recolorări** și **rotații**, în funcție de culoarea "unchiului":
```
Caz 1: unchiul ROȘU  → recolorezi părinte+unchi în negru, bunic în roșu
                       (muți problema mai sus)
Caz 2: unchi NEGRU, nod = fiu "interior" → rotație care îl aduce în Caz 3
Caz 3: unchi NEGRU, nod = fiu "exterior" → rotație pe bunic + recolorare
```

**Utilizări (foarte frecvente în practică!):**
- C++ STL: `map`, `multimap`, `set`, `multiset`
- Java: `TreeMap`, `TreeSet`
- Kernel Linux (Completely Fair Scheduler)

---

### AVL vs Roșu-Negru

| | AVL | Roșu-Negru |
|--|-----|-----------|
| Echilibrare | strictă (diferență ≤ 1) | mai relaxată (h ≤ 2log(n+1)) |
| Înălțime | mai mică (căutări rapide) | puțin mai mare |
| Inserări/ștergeri | mai multe rotații | mai puține rotații |
| Bun pentru | multe căutări | multe inserări/ștergeri |
| Operații | O(log n) | O(log n) |

---

## 9. Arbori digitali (Trie) ⭐
> Întrebare: *"Arbori digitali. Arbori digitali compactați."*

### Ce sunt

Structură pentru **șiruri de caractere** (information retrieval). Cheile sunt secvențe de cifre/litere; **drumul de la rădăcină** descrie cheia, nu valoarea din nod.

- Arbore **k-ar** ordonat (k = mărimea alfabetului). Fiecare nod are până la `k` fii.
- **Economie de memorie** când există multe **prefixe comune** (prefixul comun e stocat o singură dată).

```
Trie cu cuvintele: "10", "100", "11" (alfabet {0,1})

            (rădăcină)
            ╱       ╲
          0           1
          │          ╱ ╲
          1*        0   1*       (* = sfârșit de cuvânt/cheie)
          │
          0*
          
Drumul rădăcină→nod = cheia.  Prefixul "1" e partajat de "10","100","11".
```

### Operații

| Operație | Cum | Complexitate |
|----------|-----|--------------|
| **Căutare** `a` | parcurgi drumul descris de literele `a[0..m-1]` | O(m) |
| **Inserare** `x` | parcurgi drumul; unde nu există nod, adaugi unul nou | O(m) |
| **Ștergere** `x` | parcurgi drumul; la întoarcere ștergi nodurile cu toți succesorii nil | O(m) |

unde `m` = lungimea cheii. **Nu depinde de numărul de chei `n`!**

**Proprietăți:** orice nod intern are cel mult `k` fii; arborele are `n` noduri externe; înălțimea = lungimea celui mai lung cuvânt.

**Arbori digitali compactați (Patricia/radix trie):** lanțurile de noduri cu un singur fiu se "comprimă" într-un singur nod → economie de spațiu.

---

# ANEXE
> Material din cursurile 1, 4, 5, 8, 9, 11 — mai puțin probabil la examen ca temă principală, dar util pentru completitudine și pentru întrebări de tip "definiție".

## Anexă A — Algoritmi și limbaj algoritmic (Curs 1)

### Ce este un algoritm

**Algoritm:** metodă (secvență finită de instrucțiuni bine definite) de rezolvare a unei probleme.
**Structură de date:** metodă de a păstra/reprezenta informația.
> *Algorithms + Data Structures = Programs* — Niklaus Wirth.

O **problemă** = pereche (input, output): descrierea datelor de intrare și a rezultatului dorit.

**Proprietățile unui algoritm:**
- **input** — zero sau mai multe date din exterior;
- **output** — produce informație;
- **terminare** — pentru orice intrare se termină într-un **număr finit de pași**;
- **corectitudine** — produce rezultatul corect pentru orice intrare.

**Eficiență:** un algoritm trebuie să folosească un volum rezonabil de resurse (timp, memorie). Contează pentru scalabilitate și soluții optimizate. (Ex: Fibonacci recursiv e corect dar ineficient — exponențial.)

### Modelul de calcul

- **Memoria** = structură liniară de celule (variabile, pointeri).
- O **variabilă** are: nume, adresă, atribute (tip), instanță (valoarea curentă).

### Tipuri de date

- **Elementare:** întregi, reale, booleene (and/or/not/xor), caractere, **pointeri** (valori = adrese sau NULL; dereferențiere `*p`).
- **Structurate de nivel jos:** operații la nivel de componentă.
- **De nivel înalt:** operații implementate de algoritmi.

**Cost uniform vs cost logaritmic:**
| Model | Idee | Cost atribuire |
|-------|------|----------------|
| **uniform** | fiecare operație = 1 unitate, indiferent de mărimea valorii | O(1) |
| **logaritmic** | costul depinde de numărul de biți ai valorii | O(log valoare) |
> În analiza obișnuită folosim **costul uniform**.

### Instrucțiuni

- **Atribuirea** `variabilă ← expresie` — singura care modifică memoria; cost O(1).
- **Condiționale:** `if` / `if-else`.
- **Iterative:** `while`, `repeat...until`, `for`.
  - `repeat S until e` execută `S` **cel puțin o dată** (testul e la final).
  - `for i ← e1 to e2 do S` ≡ while cu contor incrementat.
- **return** — întrerupe secvența.

### Subprograme

- **Procedură:** `Procedure nume(parametri) begin ... end` — nu întoarce valoare direct.
- **Funcție:** conține cel puțin un `return <expr>`; se folosește într-o expresie.
- Interfața cu modulul apelant se face prin **parametri** și variabile globale.

```
Procedure SWAP(x, y)        Function max3(x, y, z)
begin                       begin
  aux ← x                     temp ← x
  x ← y                       if y > temp then temp ← y
  y ← aux                     if z > temp then temp ← z
end                           return temp
                            end
```

### Tablouri și structuri

- **Tablou:** ansamblu **omogen** (toate componentele de același tip), identificate prin indici, memorie **contiguă**, acces `a[i]` în O(1).
  - **Unidimensional:** `a[0..n-1]`.
  - **Bidimensional:** `a[m,n]`, memorat în ordine lexicografică a indicilor (linie cu linie). Poate fi văzut ca tablou de tablouri.
- **Șir de caractere:** tablou unidimensional de caractere; operația de concatenare (`+`).
- **Structură:** ansamblu **eterogen** de **câmpuri** (tipuri diferite), fiecare cu nume propriu. Acces: `S.câmp`, sau `p->câmp` dacă `p` e pointer. Memorie contiguă, în ordinea declarării.

### Execuția unui algoritm

- **Calcul** = succesiunea de pași elementari produsă de execuție.
- **Configurație** = starea memoriei + instrucțiunea curentă. Calculul = secvență de configurații `c₀ → c₁ → ... → cₙ`.

## Anexă B — Liste liniare (Curs 4)

### TAD LLin (listă liniară)

**Obiecte:** `L = (e₀, ..., e_{n-1})`, n ≥ 0. `e₀` = primul, `e_{n-1}` = ultimul, `eᵢ` = predecesorul lui `eᵢ₊₁`.

**Operații:**
| Operație | Efect |
|----------|-------|
| `listaVida()` | listă goală |
| `insereaza(L, k, e)` | inserează `e` pe poziția `k` (eroare dacă k invalid) |
| `elimina(L, k)` | elimină elementul de pe poziția `k` |
| `alKlea(L, k)` | întoarce elementul de pe poziția `k` |
| `poz(L, e)` | prima poziție pe care apare `e`, sau −1 |
| `lung(L)` | numărul de elemente |
| `parcurge(L, viziteaza)` | aplică `viziteaza()` fiecărui element |
| `elimTotE(L, e)` | elimină toate aparițiile lui `e` |

### Implementare cu tablou vs listă înlănțuită

```
Tablou:    L.tab=[e₀][e₁][e₂][e₃]   L.ultim=3
                                     acces alKlea(k): O(1)
                                     inserare/ștergere: O(n) (deplasări)

Listă:     L.prim→[e₀|•]→[e₁|•]→[e₂|•]→NULL ←L.ultim
           un nod: [elt | succ]  (info + adresa succesorului)
                                     acces alKlea(k): O(n)
                                     inserare/ștergere (cu poziția dată): O(1)
```
> Compromis clasic: **tabloul** e bun la **acces aleator**, **lista** e bună la **inserări/ștergeri**.

### Listă liniară ordonată (LLinOrd)

Elementele sunt mereu sortate (`e₀ ≤ e₁ ≤ ... ≤ e_{n-1}`). Inserarea pune elementul la locul potrivit ca să păstreze ordinea. Căutarea (`poz`):
- cu tablou → **căutare binară**: `O(log n)`;
- cu listă înlănțuită → `O(n)` (nu poți sări la mijloc).

### Aplicație: conversie infixat → postfixat (cu stivă)

Notația postfixată (poloneză inversă) nu are nevoie de paranteze.
```
infixat:    a + b * (c + d) + e
postfixat:  a b c d + * + e +
```
**Algoritm** (parcurgi expresia infixată, folosești o stivă `S` pentru operatori):
```
pentru fiecare simbol x din infix:
   dacă x e operand        → îl pui direct în rezultat (postfix)
   dacă x e '('            → push(S, x)
   dacă x e ')'            → scoți din S în postfix până la '(', apoi arunci '('
   dacă x e operator       → scoți din S operatorii cu prioritate ≥ x,
                             apoi push(S, x)
la final: golești S în postfix
```
**Evaluarea** expresiei postfixate (tot cu stivă): operanzii se pun pe stivă; la fiecare operator scoți doi operanzi, aplici operația, pui rezultatul înapoi.

## Anexă C — Sortări comparativ (Curs 8)

| Algoritm | Favorabil | Mediu | Nefavorabil | Stabilă? | Spațiu extra |
|----------|-----------|-------|-------------|----------|--------------|
| Bubble (interschimbare) | n | n² | n² | da | O(1) |
| Insertion (inserție) | n | n² | n² | da | O(1) |
| Selection (selecție naivă) | n² | n² | n² | nu | O(1) |
| **Heap sort** | n log n | n log n | n log n | nu | O(1) |
| **Merge sort** | n log n | n log n | n log n | **da** | O(n) |
| **Quick sort** | n log n | n log n | **n²** | nu | O(log n) |
| Counting sort | — | n+k | n+k | da | O(k) |
| Bucket sort | — | n | — | da | O(n) |

**Recomandări:**
- **Quick sort:** rapid în medie, când stabilitatea nu contează (cel mai folosit în practică).
- **Merge sort:** când e nevoie de **stabilitate** (dar consumă O(n) spațiu).
- **Heap sort:** când contează garanția în cazul **nefavorabil** și spațiul O(1).
- **Insertion sort:** când `n` e mic.
- **Counting/Bucket:** sub anumite ipoteze pe date → `O(n)`.

**Paradigma divide-et-impera** (merge sort, quick sort):
```
1. DIVIDE: împarte problema în subprobleme mai mici
2. IMPERA: rezolvă recursiv subproblemele
3. COMBINĂ: asamblează soluțiile
```
- **Merge sort:** divizarea e banală (la mijloc), munca e la **combinare** (interclasare).
- **Quick sort:** munca e la **divizare** (partiționare în jurul pivotului), combinarea e banală. Alegerea pivotului decide eficiența (pivot prost → O(n²)).

**Sortare stabilă** = păstrează ordinea relativă a elementelor cu chei egale.

## Anexă D — Arbori binari și parcurgeri (Curs 5)

### Definiție și terminologie

**Arbore (definiție recursivă):** fie arborele vid `Λ`, fie o rădăcină `r` cu subarbori `A₁,...,Aₖ`.

**Arbore binar:** fiecare nod are 0, 1 sau 2 fii, **ordonați** (fiu stâng / fiu drept — dacă are un singur fiu trebuie precizat care).

**Terminologie:**
- **rădăcină** — nodul fără părinte;
- **nod intern** — are cel puțin un fiu; **frunză (nod extern)** — fără fii;
- **descendenți / frați / subarbore**;
- **adâncimea** unui nod = distanța la rădăcină (rădăcina are adâncime 0);
- **înălțimea** arborelui = adâncimea maximă a unui nod.

### Proprietăți (arbore binar cu înălțime h, n noduri)

```
h + 1 ≤ n ≤ 2^(h+1) − 1
log₂(n+1) − 1 ≤ h ≤ n − 1
```
- **Arbore propriu:** fiecare nod intern are **exact 2 fii** → `n_externe = n_interne + 1`.
- **Arbore complet:** arbore propriu cu toate frunzele la aceeași adâncime → `n = 2^(h+1) − 1`, nivelul `i` are `2ⁱ` noduri.

### Implementări

```
1. Structuri înlănțuite — fiecare nod: [stg | inf | drp]
   (cel mai folosit; doi pointeri spre fii)

2. Tablou de părinți — parinte[i] = indexul părintelui
   Avantaj: acces ușor spre rădăcină, economic.
   Dezavantaj: acces dificil de la rădăcină spre fii.

3. Tablou cu indexare implicită (ca la heap):
   index(rădăcină) = 0
   fiu stâng al lui x  = 2·index(x) + 1
   fiu drept al lui x  = 2·index(x) + 2
   Bun pentru arbori (aproape) compleți; risipă la arbori rari.
```

### Cele 4 parcurgeri (pe arborele exemplu):
```
            C
          ╱   ╲
         E      G
        ╱ ╲    ╱ ╲
       K   A  M   D
```
| Parcurgere | Ordine | Rezultat |
|-----------|--------|----------|
| **Preordine** | Rădăcină→Stâng→Drept | C E K A G M D |
| **Inordine** | Stâng→Rădăcină→Drept | K E A C M G D |
| **Postordine** | Stâng→Drept→Rădăcină | K A E M D G C |
| **BFS (pe niveluri)** | nivel cu nivel (cu o **coadă**) | C E G K A M D |

> La un ABC, **inordinea** dă valorile sortate. Pre/postordinea pe arborele unei expresii dau notația prefixată/postfixată.

**Aplicație — expresii ca arbori:** `−12 + 17*5` → operatorii în noduri interne, operanzii în frunze. Postordine → notație postfixată; preordine → notație prefixată.

## Anexă E — Tabele de dispersie / Hash (Curs 11)

**Tabelă cu adresare directă:** `T[k] = x` direct. Operații `Θ(1)`, dar spațiu `Θ(|U|)` — irosit dacă universul cheilor e uriaș.

**Tabelă de dispersie (hash):** o **funcție hash** `h(k)` mapează cheia la o poziție `{0,..,m-1}`.
- **Coliziune:** două chei diferite ajung la aceeași poziție.

**Rezolvarea coliziunilor:**
```
1. ÎNLĂNȚUIRE (dispersie externă): fiecare slot = o listă de elemente
   T[0] → 
   T[1] → [k1] → [k7] → NULL       (k1,k7 au h(k)=1)
   T[2] → [k3] → NULL

2. ADRESARE DESCHISĂ (dispersie internă): toate elementele în tablou;
   la coliziune cauți altă poziție liberă (examinare):
   - liniară:   h(k,i) = (h'(k) + i) mod m
   - pătratică: h(k,i) = (h'(k) + c₁i + c₂i²) mod m
   - dublă:     h(k,i) = (h₁(k) + i·h₂(k)) mod m
```

**Factorul de încărcare** `α = n/m` (nr. elemente / nr. sloturi). Dacă `α = O(1)`, căutarea e în medie `O(1)`.

**Funcții de dispersie:**
- **Diviziune:** `h(k) = k mod m` (alege `m` prim, nu aproape de o putere a lui 2).
- **Înmulțire:** `h(k) = ⌊m·(k·A − ⌊k·A⌋)⌋`.

**Utilizări:** indexare în baze de date, tabele de simboluri în compilatoare, cache.

## Anexă F — Căutare: aspect static și predecesor/succesor (Curs 9)

### Problema căutării — două aspecte

- **Static:** mulțimea `S` e fixă; doar întrebi "a ∈ S?".
- **Dinamic:** `S` se schimbă în timp (inserări/ștergeri) → de aici nevoia de ABC/arbori echilibrați.

**Complexitatea căutării în structuri liniare:**
| Structură | Căutare | Inserare | Ștergere |
|-----------|---------|----------|----------|
| Tablou neordonat | O(n) | O(1) | O(n) |
| Listă înlănțuită | O(n) | O(1) | O(1) |
| Tablou ordonat | **O(log n)** | O(n) | O(n) |
| Listă ordonată | O(n) | O(n) | O(1) |

### Căutarea binară (aspect static)

Pe un tablou **sortat** `s[0..n-1]`, compari cu elementul din mijloc și elimini jumătate la fiecare pas:
```
Function poz(s, n, a)
   p ← 0; q ← n-1
   m ← (p+q)/2
   while (s[m] != a and p < q) do
       if (a < s[m]) then q ← m-1      // caut în jumătatea stângă
       else p ← m+1                     // caut în jumătatea dreaptă
       m ← (p+q)/2
   if (s[m] == a) then return m else return -1
```
Complexitate: **O(log n)** (la fiecare pas dimensiunea se înjumătățește).

### Predecesor / Succesor (în ABC)

Modifică operația de căutare: dacă valoarea `x` **nu se găsește**, întoarce:
- **predecesorul** = cea mai mare valoare `< x`, sau
- **succesorul** = cea mai mică valoare `> x`.

**Succesorul unui nod** într-un ABC:
- dacă nodul are subarbore **drept** → succesorul e **minimul** din subarborele drept (cobori dreapta o dată, apoi stânga cât poți);
- altfel → urci spre părinți până găsești primul strămoș al cărui fiu **stâng** e pe drumul tău.

---

# CHEAT SHEET FINAL (recapitulare rapidă)

### Complexitate
```
O = margine superioară   Ω = margine inferioară   Θ = ambele (exact)
Ierarhie: 1 < log n < n < n log n < n² < n³ < 2ⁿ < n!
Caz: favorabil (best) / nefavorabil (worst, cel mai important) / mediu
```

### 4 metode recurențe
```
1. Substituție: ghicești + inducție
2. Iterație: desfășori până la cazul de bază
3. Arbore recursie: aduni costuri pe niveluri
4. Master: T(n)=aT(n/b)+f(n), compari f(n) cu n^(log_b a)
   - f mai mic → Θ(n^log_b a)
   - f egal    → Θ(n^log_b a · log n)
   - f mai mare→ Θ(f(n))
```

### Structuri
```
STIVĂ: LIFO, push/pop/top la vârf → DFS, undo, recursie
COADĂ: FIFO, insert la coadă, scoatere la cap → BFS, fire așteptare
HEAP (max): părinte ≥ fii, maxim în rădăcină, O(log n) insert/elimină
  tablou: fiu stâng=2i+1, fiu drept=2i+2, părinte=(i-1)/2
```

### Arbori
```
ABC:  stânga < nod < dreapta; inordine = sortat; operații O(h)
      degenerat → O(n), echilibrat → O(log n)
AVL:  |h_stâng − h_drept| ≤ 1; rotații; foarte echilibrat
R-N:  4 reguli culori; h ≤ 2log(n+1); STL map/set, Java TreeMap
TRIE: pt șiruri; drum = cheie; căutare O(m) (m=lungime cuvânt)
```

### Grafuri
```
Reprezentare: matrice adiacență O(n²) / liste adiacență O(n+m)
DFS = stivă (adâncime)    BFS = coadă (lățime)    ambele O(n+m)
Comp. conexe (graf): DFS din fiecare vârf necolorat
Comp. tare conexe (digraf): DFS + transpus Dᵀ + DFS → O(n+m)
```

### Sortări de reținut
```
n log n: merge (stabil, O(n) spațiu), quick (mediu; O(n²) worst), heap (O(1) spațiu)
n²:      bubble, insertion, selection
O(n):    counting (întregi în interval), bucket (uniform distribuit)
```

---

# 🎯 TEST GRILĂ — SD (subiecte reale de licență 2021–2025)
> Fiecare grilă pornește de la un **subiect dat efectiv la examen**. Apasă pe **„Răspuns"**. Răspunsul corect e distribuit uniform pe A/B/C/D.

**1.** *(«Notații asimptotice»)* — Notația O(g(n)) reprezintă:
- A) o margine inferioară (cel puțin la fel de mare ca g)
- B) marginea exactă (și superioară, și inferioară)
- C) timpul mediu de execuție
- D) o margine superioară (cel mult la fel de mare ca g)

<details><summary>✅ Răspuns</summary>

**D)** — A = Ω, B = Θ. O = plafon superior.
</details>

**2.** *(«Notații asimptotice»)* — `f(n) = Ω(g(n))` înseamnă că f crește:
- A) cel puțin la fel de repede ca g (margine inferioară)
- B) cel mult la fel de repede ca g
- C) exact ca g
- D) egal cu g

<details><summary>✅ Răspuns</summary>

**A)** — B = O; C = Θ. Omega = podea inferioară.
</details>

**3.** *(«Big O, Theta, Omega»)* — `Θ(g(n)) = O(g(n)) ∩ Ω(g(n))` înseamnă că f are:
- A) ordin cel mult egal cu g
- B) ordin cel puțin egal cu g
- C) exact același ordin de creștere ca g
- D) niciun raport cu g

<details><summary>✅ Răspuns</summary>

**C)** — Theta = ordin strâns (tight bound).
</details>

**4.** *(«Caz favorabil/nefavorabil, exemplu de algoritm»)* — La căutarea secvențială într-un tablou de n elemente, cazul cel mai nefavorabil are complexitatea:
- A) O(1)
- B) O(n)
- C) O(log n)
- D) O(n²)

<details><summary>✅ Răspuns</summary>

**B)** — Nefavorabil = valoarea lipsește/e ultima. Favorabil = O(1). O(log n) ar fi căutarea binară.
</details>

**5.** *(«Clasificarea algoritmilor folosind O»)* — Ordinea crescătoare după ordinul de creștere:
- A) O(n) < O(log n) < O(n²) < O(2ⁿ)
- B) O(1) < O(n²) < O(n) < O(log n)
- C) O(2ⁿ) < O(n²) < O(n) < O(log n)
- D) O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ)

<details><summary>✅ Răspuns</summary>

**D)** — În A, log e greșit după n; B și C sunt dezordonate/inversate.
</details>

**6.** *(«Teorema Master»)* — Teorema Master se folosește pentru:
- A) recurențe de forma T(n) = a·T(n/b) + f(n) (complexitatea algoritmilor recursivi divide-et-impera)
- B) a sorta un tablou
- C) complexitatea algoritmilor iterativi
- D) a echilibra un arbore

<details><summary>✅ Răspuns</summary>

**A)** — Compari f(n) cu n^(log_b a) și alegi unul din 3 cazuri.
</details>

**7.** *(«Teorema Master»)* — Teorema Master NU se poate aplica când:
- A) a ≥ 1 și b > 1
- B) f(n) este un polinom
- C) f(n) nu e polinomial comparabilă cu n^(log_b a) (ex. `T(n)=2T(n/2)+n log n`)
- D) subproblemele au dimensiuni egale

<details><summary>✅ Răspuns</summary>

**C)** — A sunt exact condițiile de aplicare. Master cere diferență polinomială.
</details>

**8.** *(«Metoda substituției»)* — Metoda substituției presupune:
- A) desfășurarea recurenței până la cazul de bază și însumare
- B) ghicirea soluției și demonstrarea prin inducție matematică
- C) construirea unui arbore de costuri pe niveluri
- D) aplicarea unei formule directe

<details><summary>✅ Răspuns</summary>

**B)** — A = metoda iterației; C = arborele de recursie; D = Teorema Master.
</details>

**9.** *(«Metoda iterației»)* — Metoda iterației presupune:
- A) ghicirea soluției și inducție matematică
- B) aplicarea Teoremei Master
- C) colorarea nodurilor
- D) desfășurarea (iterarea) recurenței până la cazul de bază și însumarea termenilor

<details><summary>✅ Răspuns</summary>

**D)** — A = substituția; B = Master. Iterația „desface" recurența pas cu pas.
</details>

**10.** *(«Structuri union-find. La ce sunt folositoare?»)* — Union-Find e folosită pentru:
- A) gestionarea partițiilor — a afla din ce submulțime face parte un element (find) și a reuni submulțimi (union); ex. componente conexe, algoritmul lui Kruskal
- B) sortarea rapidă a datelor
- C) căutarea binară
- D) parcurgerea în lățime a unui graf

<details><summary>✅ Răspuns</summary>

**A)** — ⚠️ profesorii întreabă explicit **la ce folosește**: componente conexe, arbore parțial minim (Kruskal), rețele.
</details>

**11.** *(«Structuri union-find»)* — Cele două operații de bază sunt:
- A) push și pop
- B) insert și delete
- C) find și union
- D) enqueue și dequeue

<details><summary>✅ Răspuns</summary>

**C)** — A = stivă, D = coadă. `find(i)` = reprezentantul; `union(i,j)` = reunire.
</details>

**12.** *(«Tipurile Stiva și Coada»)* — Stiva este o structură de tip:
- A) FIFO (primul intrat, primul ieșit)
- B) LIFO (ultimul intrat, primul ieșit)
- C) cu acces aleator
- D) ordonată după prioritate

<details><summary>✅ Răspuns</summary>

**B)** — A = coada; D = coada cu priorități.
</details>

**13.** *(«Tipurile Stiva și Coada»)* — Operația de eliminare dintr-o coadă (FIFO) scoate:
- A) ultimul element introdus
- B) elementul cu prioritatea maximă
- C) elementul din mijloc
- D) primul element introdus (cel mai vechi)

<details><summary>✅ Răspuns</summary>

**D)** — A ar fi stiva (LIFO); B ar fi coada cu priorități.
</details>

**14.** *(«Coada cu priorități. Min/max heap»)* — Într-un max-heap, rădăcina conține:
- A) maximul
- B) minimul
- C) mediana
- D) ultimul inserat

<details><summary>✅ Răspuns</summary>

**A)** — B ar fi un min-heap. Proprietatea: părinte ≥ fii.
</details>

**15.** *(«Min/max heap, reprezentare»)* — În heap-ul reprezentat cu tablou, fiul stâng al nodului i este la indexul:
- A) i/2
- B) 2i + 2
- C) 2i + 1
- D) i + 1

<details><summary>✅ Răspuns</summary>

**C)** — Fiu stâng = 2i+1, fiu drept = 2i+2, părinte = (i−1)/2. (A e părintele.)
</details>

**16.** *(«Arbori binari de căutare, reprezentare, operații»)* — Într-un ABC, pentru orice nod:
- A) subarborele stâng conține valori mai mari, cel drept mai mici
- B) subarborele stâng conține valori mai mici, cel drept mai mari decât nodul
- C) toți fiii au aceeași valoare
- D) frunzele sunt mereu la aceeași adâncime

<details><summary>✅ Răspuns</summary>

**B)** — A e inversat; D descrie un arbore complet. Parcurgerea în inordine dă valorile sortate.
</details>

**17.** *(«Arbori binari de căutare»)* — Căutarea într-un ABC degenerat (caz nefavorabil) are complexitatea:
- A) O(log n)
- B) O(1)
- C) O(n log n)
- D) O(n)

<details><summary>✅ Răspuns</summary>

**D)** — Un ABC degenerat devine o listă. De aceea folosim arbori echilibrați (O(log n)).
</details>

**18.** *(«Arbori binari de căutare echilibrați AVL»)* — Un AVL e echilibrat dacă pentru orice nod:
- A) diferența de înălțime dintre subarborele stâng și drept este cel mult 1
- B) numărul de noduri din stânga = numărul din dreapta
- C) toate frunzele au aceeași culoare
- D) rădăcina e cea mai mare valoare

<details><summary>✅ Răspuns</summary>

**A)** — Factor de echilibrare ∈ {−1,0,+1}. C ține de roșu-negru; D de heap.
</details>

**19.** *(«Arbori roșu-negru»)* — O proprietate corectă a arborilor roșu-negru:
- A) un nod roșu are ambii fii roșii
- B) rădăcina este roșie
- C) orice drum de la un nod la frunze are același număr de noduri negre
- D) toate nodurile sunt roșii

<details><summary>✅ Răspuns</summary>

**C)** — Un nod roșu are fiii negri (A inversat); rădăcina e neagră (B fals). Garantează h ≤ 2·log₂(n+1).
</details>

**20.** *(«Grafuri. Reprezentare: liste, matrice de adiacență»)* — Matricea de adiacență ocupă spațiu:
- A) O(n + m)
- B) O(n²)
- C) O(m)
- D) O(log n)

<details><summary>✅ Răspuns</summary>

**B)** — Listele de adiacență ocupă O(n+m) (A) — mai bune pentru grafuri rare.
</details>

**21.** *(«Grafuri. Algoritmi de parcurgere»)* — DFS folosește o ..., iar BFS o ...:
- A) coadă / stivă
- B) heap / listă
- C) ambele o coadă
- D) stivă / coadă

<details><summary>✅ Răspuns</summary>

**D)** — DFS (adâncime) = stivă; BFS (lățime) = coadă. A e inversat.
</details>

**22.** *(«Componente conexe și tare conexe»)* — O componentă tare conexă într-un digraf este o mulțime maximală de vârfuri în care:
- A) între oricare două vârfuri u, v există drum u→v ȘI v→u
- B) există drum într-un singur sens
- C) toate vârfurile au același grad
- D) nu există cicluri

<details><summary>✅ Răspuns</summary>

**A)** — „Tare" = în ambele sensuri. B descrie doar conexitatea (graf neorientat).
</details>

**23.** *(«Sortare prin numărare și distribuire»)* — Sortarea prin numărare (counting sort):
- A) se bazează pe comparații între elemente
- B) are complexitate O(n log n)
- C) presupune valori întregi într-un interval mic {1..k}, complexitate O(n+k), fără comparații
- D) funcționează doar pe date uniform distribuite în [0,1)

<details><summary>✅ Răspuns</summary>

**C)** — D descrie **bucket sort** (distribuire). Counting NU compară elemente.
</details>

**24.** *(«Sortare bazată pe comparații; ce algoritm pt set foarte mare»)* — Pentru un set foarte mare cu garanție O(n log n) ȘI stabilitate, alegi:
- A) bubble sort
- B) merge sort
- C) quick sort
- D) insertion sort

<details><summary>✅ Răspuns</summary>

**B)** — Merge sort: O(n log n) garantat + stabil (dar O(n) spațiu). Quick e O(n²) în worst-case și nu e stabil.
</details>

**25.** *(«Tabele de dispersie. Ce se întâmplă în caz de coliziune»)* — O coliziune apare când:
- A) tabela este plină
- B) o cheie nu există în tabelă
- C) funcția hash returnează 0
- D) două chei diferite produc aceeași valoare hash (aceeași poziție)

<details><summary>✅ Răspuns</summary>

**D)** — Rezolvare: înlănțuire (dispersie externă) sau adresare deschisă (internă).
</details>

**26.** *(«Coliziune: rezolvare»)* — Rezolvarea prin înlănțuire (dispersie externă) înseamnă:
- A) fiecare slot păstrează o listă cu toate elementele care au aceeași valoare hash
- B) căutarea unei alte poziții libere în tabelă
- C) mărirea automată a tabelei
- D) ignorarea celei de-a doua chei

<details><summary>✅ Răspuns</summary>

**A)** — B descrie adresarea deschisă (dispersie internă) — confuzie frecventă.
</details>

**27.** *(«Liste liniare. Moduri de implementare»)* — Accesul la al k-lea element este O(1) la ... și O(n) la ...:
- A) listă înlănțuită / tablou
- B) ambele O(1)
- C) tablou / listă înlănțuită
- D) ambele O(n)

<details><summary>✅ Răspuns</summary>

**C)** — Tabloul are acces aleator O(1); lista se parcurge O(n). Lista e mai bună la inserări/ștergeri.
</details>

**28.** *(«Tipuri de date. Definiție. Clasificare»)* — Un tip de date abstract (TAD) definește:
- A) doar implementarea concretă a structurii
- B) obiectele și operațiile permise (CE face), separat de implementare (CUM)
- C) doar sintaxa unui limbaj de programare
- D) modul de alocare a memoriei

<details><summary>✅ Răspuns</summary>

**B)** — Ex: Stiva/Coada = TAD-uri, implementabile cu tablou SAU listă.
</details>

**29.** *(«Algoritmi. Proprietăți»)* — Care sunt proprietăți obligatorii ale unui algoritm?
- A) să fie scris într-un limbaj orientat-obiect
- B) să folosească recursie
- C) să aibă interfață grafică
- D) input/output, terminare (număr finit de pași) și corectitudine

<details><summary>✅ Răspuns</summary>

**D)** — Un algoritm trebuie să se termine și să producă rezultatul corect pentru orice intrare.
</details>

---

> 💡 **Distribuția răspunsurilor** e echilibrată. Confuzii cheie SD: O vs Ω vs Θ, stivă vs coadă, fiu stâng `2i+1` vs părinte `(i−1)/2`, înlănțuire vs adresare deschisă, degenerat O(n) vs echilibrat O(log n), counting (întregi) vs bucket (uniform).
