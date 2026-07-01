# SINTEZĂ COMPLETĂ OOP — LICENȚĂ
> Acoperă toate cele 12 cursuri. Prioritizat după frecvența la exam (⭐).
> **Stelele actualizate cu întrebări din 2021, 2022, 2023, 2024.**

---

## CUPRINS
0. [**Cele 4 principii fundamentale POO** (Încapsulare, Abstractizare, Moștenire, Polimorfism) ⭐⭐⭐](#0-cele-4-principii-fundamentale-ale-poo-)
1. [Compilare, Memorie, Biblioteci](#1-compilare-memorie-biblioteci)
2. [Clase și Obiecte — Fundamente ⭐⭐](#2-clase-și-obiecte--fundamente)
3. [Pointeri, Referințe, const, friend](#3-pointeri-referințe-const-friend)
4. [Supraîncărcarea Metodelor + Polimorfism static vs dinamic ⭐⭐⭐⭐](#4-supraîncărcarea-metodelor)
5. [Constructori ⭐⭐⭐⭐](#5-constructori-)
6. [Destructori și Supraîncărcarea Operatorilor ⭐⭐⭐⭐](#6-destructori-și-supraîncărcarea-operatorilor-)
7. [Moștenire / Derivare ⭐⭐⭐](#7-moștenire-)
8. [Metode Virtuale, Interfețe, Clase Abstracte, Polimorfism dinamic ⭐⭐⭐⭐⭐](#8-metode-virtuale-interfețe-clase-abstracte-)
9. [Cast-uri și RTTI (reguli de conversie) ⭐⭐⭐](#9-cast-uri-și-rtti-)
10. [Macro-uri și Preprocesare](#10-macro-uri-și-preprocesare)
11. [Template-uri (Clase Parametrizate) ⭐⭐⭐⭐](#11-template-uri-clase-parametrizate-)
12. [STL — Containere](#12-stl--containere)
13. [Smart Pointers](#13-smart-pointers)
14. [C++11/14/17: auto, constexpr, lambda, structured binding](#14-c111417-auto-constexpr-lambda-structured-binding)
15. [Excepții ⭐⭐⭐](#15-excepții-)
16. [Relații de Asociere și Agregare ⭐⭐⭐⭐](#16-relații-de-asociere-)
17. [Principii SOLID (mai ales DIP/D, OCP/O, LSP/L) ⭐⭐⭐⭐⭐](#17-principii-solid-)
18. [Design Patterns — GoF (Composite, Visitor, Factory) ⭐⭐⭐⭐](#18-design-patterns--gang-of-four)

> **Top recurente OOP (toți anii 2021–2024):**
> - **SOLID — Principiul Dependențelor Inversate (DIP/D)** — cel mai frecvent! (definiție, exemplu unde NU se respectă, soluție); apar și O (Open-Closed) și L (Liskov).
> - **Suprascriere (overriding) + polimorfism dinamic** — în fiecare an (atenție: și **static vs dinamic** — vezi cap. 4).
> - **Relații de asociere / agregare** — în fiecare an (definiție, clasificare, implementare).
> - **Clase abstracte vs interfețe + metode virtuale** — în fiecare an.
> - **Clase parametrizate (template-uri)** — în fiecare an.
> - **Constructori și destructori** — frecvent.
> - **Design patterns: Composite, Visitor, (Object) Factory** — în fiecare an.
> - **Excepții** (mecanismul în POO), **reguli de conversie (cast)**, **derivare/moștenire**, **clase/obiecte/modificatori de acces** — recurente.

---

## 0. Cele 4 principii fundamentale ale POO ⭐⭐⭐
> Cei **4 piloni** ai Programării Orientate-Obiect. Apar frecvent ca întrebare directă ("ce este încapsularea/abstractizarea/...") sau ca parte din alte subiecte.

```
┌───────────────────────────────────────────────────────────────┐
│ 1. ÎNCAPSULARE (Encapsulation)                                 │
│ 2. ABSTRACTIZARE (Abstraction)                                 │
│ 3. MOȘTENIRE (Inheritance)                                     │
│ 4. POLIMORFISM (Polymorphism)                                  │
└───────────────────────────────────────────────────────────────┘
```

### 1. Încapsularea (Encapsulation)
> Gruparea **datelor** și a **metodelor** care operează pe ele într-o singură unitate (clasa) + **ascunderea detaliilor interne** (data hiding).

- Datele membre se fac `private`/`protected`; accesul se face controlat prin metode publice (**getters/setters**).
- Protejează invarianții obiectului (starea nu poate fi modificată direct din exterior).
- Realizată prin **modificatori de acces**: `private`, `protected`, `public`.

```cpp
class ContBancar {
    double sold;                          // ascuns (private)
public:
    void depune(double s) { if (s > 0) sold += s; }   // acces controlat
    double getSold() const { return sold; }
};
// cont.sold = -1000;  ❌ imposibil din exterior → starea e protejată
```

### 2. Abstractizarea (Abstraction)
> Expunerea doar a **esențialului** (CE face un obiect) și ascunderea **complexității** (CUM face).

- Te concentrezi pe **comportament**, nu pe implementare.
- Realizată prin **clase abstracte** și **interfețe** (metode pur virtuale) — vezi cap. 8.
- Exemplu: apeși pedala de accelerație (interfața) fără să știi cum funcționează motorul (implementarea).

```cpp
struct Forma { virtual double arie() const = 0; };   // abstracție: "ce", nu "cum"
struct Cerc : Forma { double r; double arie() const override { return 3.14*r*r; } };
```

> **Încapsulare vs Abstractizare** (confuzie frecventă): încapsularea **ascunde datele** (nivel de implementare — cum sunt stocate); abstractizarea **ascunde complexitatea** (nivel de design — ce operații expune). Prima protejează starea, a doua simplifică interfața.

### 3. Moștenirea (Inheritance)
> O clasă **derivată** preia atribute și metode de la o clasă de **bază** (relație **is-a** / "este un/o") → **reutilizarea** codului.

- `class Pisica : public Animal {}` — Pisica *este un* Animal.
- Permite ierarhii de clase; baza pentru polimorfismul dinamic. Detalii (public/private/protected, diamant) — vezi cap. 7.

### 4. Polimorfismul (Polymorphism)
> "Multe forme" — același apel/interfață produce **comportamente diferite** în funcție de tipul real al obiectului.

- **Static** (compile-time): supraîncărcare (overloading), template-uri — vezi cap. 4.
- **Dinamic** (runtime): metode virtuale (overriding), prin vtable — vezi cap. 8.

```cpp
Animal* a = new Pisica();
a->sunet();   // apelează Pisica::sunet() — polimorfism dinamic
```

> **Sinteză:** Încapsularea **protejează**, Abstractizarea **simplifică**, Moștenirea **reutilizează**, Polimorfismul **generalizează**.

---

## 1. Compilare, Memorie, Biblioteci

### Tipuri de Compilatoare

| Tip | Descriere | Exemple |
|-----|-----------|---------|
| **Native** | Compilează direct în cod mașină specific procesorului | C++, C, Rust |
| **Interpreted** | Codul sursă este interpretat linie cu linie la runtime | Python, JavaScript (vechi) |
| **JIT** (Just-In-Time) | Compilat la runtime în cod mașină | Java (JVM), C# (.NET) |

### Procesul de Compilare C++

```
Sursă (.cpp) → Preprocesare → Compilare → Object file (.obj/.o)
                                                    ↓
                                               Linker
                                                    ↓
                               [Object files + Biblioteci] → Executabil (.exe)
```

### Tipuri de Biblioteci

| Tip | Extensie | Legare | Caracteristici |
|-----|----------|--------|----------------|
| **Statică** | `.lib` / `.a` | La compilare | Codul e inclus în executabil; executabil mai mare |
| **Dinamică** | `.dll` / `.so` | La încărcare | Codul e în fișier separat; partajat între procese |
| **Delayed** | — | La primul apel | Dll-ul se încarcă abia când e nevoie de el |

### Organizarea Memoriei unui Program

```
+------------------+
|   Stack          |  ← variabile locale, parametri funcții (LIFO, rapid)
+------------------+
|   Heap           |  ← alocare dinamică (new/delete), gestionat manual
+------------------+
|   Global/Static  |  ← variabile globale și statice
+------------------+
|   Constants      |  ← string literals, constante
+------------------+
|   Code (.text)   |  ← instrucțiunile programului
+------------------+
```

- **Stack**: alocare/dezalocare automată. Limitat (~1-8MB). Variabilele locale dispar la ieșirea din funcție.
- **Heap**: alocare cu `new`, dezalocare cu `delete`. Nelimitat practic. Riscuri: leak-uri, dangling pointers.

### Alinierea în Memorie

Compilatorul adaugă **padding** pentru a alinia datele la granițe de memorie (performanță hardware):

```cpp
struct Bad {    // sizeof = 12 (nu 9)
    char a;     // 1 byte + 3 padding
    int b;      // 4 bytes
    char c;     // 1 byte + 3 padding
};

struct Good {   // sizeof = 8
    int b;      // 4 bytes
    char a;     // 1 byte
    char c;     // 1 byte + 2 padding
};

#pragma pack(1)  // elimină padding-ul
struct Packed { char a; int b; };  // sizeof = 5
#pragma pack()   // resetează
```

---

## 2. Clase și Obiecte — Fundamente

### Motivația pentru Clase

Înainte de clase (C), codul suferea de:
- **Lipsa validării**: un câmp putea fi setat la orice valoare (ex: `month = 15`)
- **Lipsa inițializării**: variabilele trebuiau inițializate manual, riscând valori nedefinite
- **Lipsa encapsulării**: orice funcție putea modifica orice date

Clasele rezolvă toate acestea prin encapsulare.

### Structura unei Clase

```cpp
class Date {
private:             // implicit pentru class
    int day;
    int month;
    int year;

public:
    Date(int d, int m, int y);  // constructor
    ~Date();                     // destructor
    int getDay() const;          // metodă const — nu modifică obiectul
    void setDay(int d);
    static int instanceCount;   // câmp static — partajat de toate instanțele
    static int getCount();      // metodă statică — nu are 'this'
};
```

### `class` vs `struct`

Sunt **identice** în C++, cu o singură diferență:
- `class`: accesul implicit al membrilor este **private**
- `struct`: accesul implicit al membrilor este **public**

### Membri Statici

```cpp
class Counter {
    static int count;  // declarat în clasă
    int id;
public:
    Counter() : id(++count) {}
    static int getCount() { return count; }  // nu are 'this', nu accesează membri non-statici
};
int Counter::count = 0;  // OBLIGATORIU definit în afara clasei (.cpp)

Counter c1, c2, c3;
cout << Counter::getCount();  // 3
```

**Reguli membre statice:**
- Există o singură copie, indiferent de câte instanțe există
- Metodele statice nu au `this`, nu pot accesa membri non-statici
- Se inițializează ÎNAINTE de `main()`, în ordinea declarației

---

## 3. Pointeri, Referințe, const, friend

### Pointer vs Referință

| | Pointer | Referință |
|--|---------|-----------|
| Sintaxă | `int* p = &x` | `int& r = x` |
| Poate fi null | da (`nullptr`) | nu (trebuie inițializată) |
| Poate fi reatribuit | da | nu (alias permanent) |
| Aritmetică | da (`p++`) | nu |
| Overhead | pointer deref (`*p`) | fără overhead |

```cpp
int x = 10;
int* p = &x;      // pointer la x
int& r = x;       // referință la x (alias)

*p = 20;   // x devine 20
r = 30;    // x devine 30 (r este x)
p = nullptr;  // OK
// r = nullptr;  // EROARE — referința nu poate fi reatribuită
```

### `const` — Variantele

```cpp
int x = 5;

const int* p1 = &x;      // pointer la const int: nu poți modifica *p1
int* const p2 = &x;      // const pointer la int: nu poți modifica p2 (adresa)
const int* const p3 = &x; // const pointer la const int: nimic nu se poate modifica

void show(const Date& d); // metodă care primește referință const — nu modifică obiectul
int getValue() const;     // metodă const — nu poate modifica datele membre ale clasei
```

### `mutable`

Permite modificarea unui câmp chiar și în metode `const`:

```cpp
class Cache {
    mutable int cachedValue;  // poate fi modificat în metode const
    mutable bool dirty;
public:
    int getValue() const {
        if (dirty) { cachedValue = computeExpensive(); dirty = false; }
        return cachedValue;
    }
};
```

### `friend`

Permite unei funcții sau clase externe să acceseze membrii privați:

```cpp
class Vector2D {
    float x, y;
public:
    // friend function — poate accesa x și y private
    friend Vector2D operator+(const Vector2D& a, const Vector2D& b) {
        return {a.x + b.x, a.y + b.y};
    }
    // friend class — clasa Editor are acces complet la Vector2D
    friend class Editor;
};
```

**Limitări `friend`:**
- Prietenia nu se moștenește
- Prietenia nu este reciprocă
- Restrânge encapsularea — folosiți cu grijă

### NULL vs nullptr

```cpp
void foo(int x) { }
void foo(int* p) { }

foo(NULL);     // AMBIGUU — NULL e 0 (int), poate apela orice variantă
foo(nullptr);  // CORECT — nullptr e de tip void*, apelează foo(int*)
```

---

## 4. Supraîncărcarea Metodelor

### Overloading — Reguli

Metodele pot fi supraîncărcate dacă diferă prin:
- Numărul de parametri
- Tipul parametrilor
- Calificatorul `const`

**Nu** pot fi supraîncărcate după tipul de return.

```cpp
class Printer {
public:
    void print(int x) { }
    void print(double x) { }
    void print(const char* s) { }
    void print(int x) const { }  // versiunea const
    // int print(int x) { }   // EROARE — diferă doar prin return
};
```

### Rezoluția Supraîncărcării

Compilatorul alege în ordinea:
1. **Potrivire exactă** (`int` → `int`)
2. **Promovare** (`char` → `int`, `float` → `double`)
3. **Conversie standard** (`int` → `double`)
4. **Cast / conversie definită de utilizator**
5. **Parametri variadici** (`...`)

```cpp
void foo(int x) { }
void foo(double x) { }

foo(5);    // foo(int) — potrivire exactă
foo(5.0);  // foo(double) — potrivire exactă
foo('A');  // foo(int) — promovare char→int
```

### Polimorfism STATIC vs DINAMIC ⭐⭐⭐⭐ (întrebare frecventă!)
> *"Suprascrierea, polimorfism dinamic — ce este asta?"* apare în fiecare an. Distincția cheie:

```
POLIMORFISM STATIC (compile-time / early binding)
  • se rezolvă la COMPILARE
  • se face prin OVERLOADING (supraîncărcare de metode/operatori) și TEMPLATE-uri
  • exemplu: void print(int) vs void print(double) — compilatorul alege după tip

POLIMORFISM DINAMIC (run-time / late binding)
  • se rezolvă la EXECUȚIE (runtime), prin vfptr/vtable
  • se face prin METODE VIRTUALE = OVERRIDING (suprascriere)
  • apelat prin pointer/referință la clasa de bază → se execută metoda obiectului REAL
```

| | Polimorfism STATIC | Polimorfism DINAMIC |
|--|--------------------|---------------------|
| Când se rezolvă | la compilare | la runtime |
| Mecanism | **overloading** (supraîncărcare) + template | **overriding** (suprascriere) + `virtual` |
| Legare (binding) | early binding | late binding (prin vtable) |
| Cuvânt cheie | — | `virtual` / `override` |

> ⚠️ Nu confunda **overloading** (supraîncărcare = polimorfism static, aceeași metodă cu semnături diferite) cu **overriding** (suprascriere = polimorfism dinamic, redefinirea unei metode virtuale în clasa derivată). Detalii complete despre overriding și vtable — vezi **cap. 8**.

---

## 5. Constructori ⭐⭐⭐⭐

### Tipuri de Constructori

```cpp
class Date {
    int day, month, year;
public:
    // 1. Default constructor — fără parametri
    Date() : day(1), month(1), year(2000) {}

    // 2. Parametrizat
    Date(int d, int m, int y) : day(d), month(m), year(y) {}

    // 3. Copy constructor — primește referință const la același tip
    Date(const Date& other) : day(other.day), month(other.month), year(other.year) {}

    // 4. Move constructor — primește rvalue reference (&&)
    Date(Date&& other) noexcept : day(other.day), month(other.month), year(other.year) {}

    // 5. Delegating constructor — apelează alt constructor al aceleiași clase
    Date() : Date(1, 1, 2000) {}
};
```

### Lista de Inițializare (Initializer List)

Sintaxa `: member(value)` după lista de parametri.

**Obligatorie pentru:**
- Membri `const` (nu pot fi atribuiți după creare)
- Membri de tip referință (`&`)
- Clase de bază fără constructor default
- Clase membre fără constructor default

```cpp
class Example {
    const int id;    // TREBUIE inițializat în initializer list
    int& ref;        // TREBUIE inițializat în initializer list
    string name;
public:
    Example(int i, int& r, string n) : id(i), ref(r), name(n) {}
    // Nu se poate: this->id = i; // const!
};
```

**Ordinea de inițializare = ordinea declarației în clasă**, nu ordinea din lista de inițializare:

```cpp
class Trap {
    int y;   // declarat înainte de x!
    int x;
public:
    Trap(int val) : x(val), y(x) {}  // PERICOL: y se inițializează primul (din x neinițializat)
};
```

### Când Se Apelează Constructorii

```cpp
Date d1;                    // ctor: variabilă locală → la declarare
Date d2(1, 1, 2023);       // ctor: cu parametri
Date* p1 = nullptr;        // NU apelează constructor (e un pointer)
Date* p2 = new Date();     // ctor: alocare pe heap
Date arr[3];               // ctor x3: array de obiecte (apelat pt fiecare)
Date d3 = d1;              // copy ctor: inițializare prin copiere
Date d4 = move(d1);        // move ctor
```

**Variabilele globale** sunt construite înainte de `main()`, în ordinea declarației.

### Singleton Pattern

```cpp
class Config {
    Config() { /* load config */ }          // constructor privat
    Config(const Config&) = delete;          // interzice copierea
    Config& operator=(const Config&) = delete;
public:
    static Config& getInstance() {
        static Config instance;  // creat la primul apel, distrus la end of program
        return instance;
    }
    // metode publice...
};

Config& cfg = Config::getInstance();
```

### `explicit` — Interzice Conversia Implicită

```cpp
class MyString {
public:
    explicit MyString(int size) { /* aloc size bytes */ }
};

MyString s1(10);    // OK — constructor explicit
MyString s2 = 10;   // EROARE — conversie implicită interzisă de explicit
```

### `= delete`

```cpp
class NonCopyable {
public:
    NonCopyable() = default;
    NonCopyable(const NonCopyable&) = delete;           // interzice copy ctor
    NonCopyable& operator=(const NonCopyable&) = delete; // interzice copy assign
};
```

### lvalue vs rvalue

- **lvalue**: are adresă, poate fi pe stânga `=` (variabile, referințe)
- **rvalue**: temporar, nu are adresă persistentă (literale, rezultate expresii)
- **rvalue reference** (`&&`): referință la un rvalue, permite mutarea resurselor

```cpp
int x = 5;        // x = lvalue, 5 = rvalue
int& r = x;       // r = lvalue reference
int&& rr = 5;     // rr = rvalue reference (prinde temporare)
int&& rr2 = move(x); // move() convertește lvalue → rvalue
```

---

## 6. Destructori și Supraîncărcarea Operatorilor ⭐⭐⭐⭐

### Destructori

```cpp
class Buffer {
    char* data;
    int size;
public:
    Buffer(int n) : size(n), data(new char[n]) {}
    ~Buffer() { delete[] data; }  // eliberare resurse
};
```

**Când se apelează destructorul:**
- Variabilă locală: la ieșirea din scope
- Variabilă globală: la terminarea programului
- Obiect pe heap: la `delete`
- Obiect în container: la distrugerea containerului

**Ordinea distrugerii = inversul construcției (LIFO).**

**`delete` vs `delete[]`:**
```cpp
int* p = new int;       delete p;    // pentru un singur obiect
int* arr = new int[10]; delete[] arr; // OBLIGATORIU [] pentru array
// delete arr;  // UNDEFINED BEHAVIOR — nu tot array-ul e distrus
```

### Supraîncărcarea Operatorilor

Aproape orice operator poate fi supraîncărcat. Regulile:
- **Operanzii trebuie să fie de tip user-defined** (nu poți suprascrie `int + int`)
- Operatorii `=`, `[]`, `()`, `->` **nu pot fi funcții friend** (trebuie să fie metode)
- Nu se pot crea operatori noi

```cpp
class Vector2D {
public:
    float x, y;
    Vector2D(float x, float y) : x(x), y(y) {}

    // Operator de adunare — metodă
    Vector2D operator+(const Vector2D& other) const {
        return {x + other.x, y + other.y};
    }

    // Operator de atribuire — metodă (NU friend)
    Vector2D& operator=(const Vector2D& other) {
        if (this != &other) { x = other.x; y = other.y; }
        return *this;  // returnează referință pentru chaining (a = b = c)
    }

    // Operator de index
    float& operator[](int i) { return i == 0 ? x : y; }

    // Operator apel funcție ()
    float operator()(float scale) const { return x * scale + y * scale; }

    // Prefix ++
    Vector2D& operator++() { ++x; ++y; return *this; }

    // Postfix ++ (primește int dummy)
    Vector2D operator++(int) { Vector2D tmp = *this; ++(*this); return tmp; }

    // friend — pentru operatori care nu pot fi metode (ex: cout <<)
    friend ostream& operator<<(ostream& os, const Vector2D& v) {
        return os << "(" << v.x << "," << v.y << ")";
    }

    // Operator move assignment
    Vector2D& operator=(Vector2D&& other) noexcept {
        x = other.x; y = other.y; return *this;
    }
};
```

### Transmiterea Obiectelor ca Parametri

```cpp
void byValue(Date d) { }     // apelează copy ctor (sau move ctor din rvalue)
void byRef(Date& d) { }      // fără copiere, poate modifica originalul
void byConstRef(const Date& d) { } // fără copiere, sigur (nu modifică)
void byPointer(Date* d) { }  // explicit, poate fi nullptr

Date getDate() { return Date(1,1,2023); } // return by value → poate triggera NRVO
```

**Return Value Optimization (RVO/NRVO)**: compilatorul poate elimina copierea la return. Nu te baza pe apelarea copy ctor la return.

---

## 7. Moștenire ⭐⭐⭐⭐

### Sintaxă și Tipuri

```cpp
class Animal {            // clasă de bază
    string name;
public:
    Animal(string n) : name(n) {}
    void eat() { cout << "eating"; }
};

class Dog : public Animal {    // moștenire publică (cel mai comun)
    string breed;
public:
    Dog(string n, string b) : Animal(n), breed(b) {}  // apel explicit ctor bază
    void bark() { cout << "Ham!"; }
};
```

### Transformarea Modificatorilor de Acces prin Moștenire

| Membrul bazei | `public` inherit | `protected` inherit | `private` inherit |
|---------------|-----------------|---------------------|-------------------|
| `public` | `public` | `protected` | `private` |
| `protected` | `protected` | `protected` | `private` |
| `private` | inaccesibil | inaccesibil | inaccesibil |

**Moștenire publică** = "IS-A" — Dog IS-A Animal (cel mai comun)
**Moștenire privată** = "IMPLEMENTED-IN-TERMS-OF" — rareori folosit

### Ordinea Construcției și Distrugerii

```
Construcție: Baze (stânga→dreapta) → Date membre (top→bottom) → Corp constructor
Distrugere:  Corp destructor → Date membre (bottom→top) → Baze (dreapta→stânga)
```

```cpp
class A { public: A() { puts("ctor A"); } ~A() { puts("dtor A"); } };
class B { public: B() { puts("ctor B"); } ~B() { puts("dtor B"); } };
class C : public A, public B {   // A înainte de B
    int x;
public:
    C() : B(), A() { puts("ctor C"); }  // ATENȚIE: ordinea în initializer list nu contează
    ~C() { puts("dtor C"); }           // se construiesc în ordinea declarării (A, B)
};
// Output construcție: ctor A → ctor B → ctor C
// Output distrugere: dtor C → dtor B → dtor A
```

### Apelul Explicit al Constructorului Bazei

Dacă baza nu are constructor default, derivata **TREBUIE** să apeleze explicit constructorul:

```cpp
class Base {
public:
    Base(int x) { }  // nu există default constructor
};
class Derived : public Base {
public:
    Derived() : Base(42) { }  // obligatoriu
    Derived(int v) : Base(v) { }
};
```

### Moștenire Multiplă

```cpp
class Flyable { public: virtual void fly() = 0; };
class Swimmable { public: virtual void swim() = 0; };

class Duck : public Flyable, public Swimmable {
public:
    void fly() override { cout << "Duck flying"; }
    void swim() override { cout << "Duck swimming"; }
};
```

### Diamond Problem și Virtual Inheritance

```cpp
class A { public: int x = 0; };
class B : public A { };
class C : public A { };
class D : public B, public C { };  // PROBLEMA: D are A de două ori → d.x ambiguu

// SOLUȚIA: virtual inheritance
class B : public virtual A { };
class C : public virtual A { };
class D : public B, public C { };  // acum A există o singură dată în D
D d;
d.x = 5;  // OK, fără ambiguitate
```

---

## 8. Metode Virtuale, Interfețe, Clase Abstracte ⭐⭐⭐

### Metode Virtuale și Polimorfism Dinamic

Fără `virtual`, apelul metodei se rezolvă la **compile time** (tipul pointerului):
Cu `virtual`, apelul se rezolvă la **runtime** (tipul real al obiectului):

```cpp
class Animal {
public:
    void sound_nv() { cout << "..."; }          // non-virtual
    virtual void sound() { cout << "..."; }     // virtual
};
class Dog : public Animal {
public:
    void sound_nv() { cout << "Ham"; }
    void sound() override { cout << "Ham"; }
};

Animal* a = new Dog();
a->sound_nv();  // "..." — rezoluție la compile time (tipul Animal*)
a->sound();     // "Ham" — rezoluție la runtime (obiect Dog)
```

### Mecanismul vfptr / vtable

Fiecare clasă cu metode virtuale primește:
- **vfptr** (Virtual Function Pointer): câmp ascuns, primul în obiect (4/8 bytes)
- **vtable**: tabel global al clasei cu pointeri la implementările virtuale

```
Obiect Dog:  [vfptr | name | breed]
                ↓
          vtable_Dog: [&Dog::sound, &Dog::eat, ...]
```

**Reguli importante:**
- Fiecare constructor setează vfptr la vtable-ul **propriei** clase
- Operatorul `=` (atribuire) **NU** setează vfptr — dacă faci `*base = *derived`, vfptr rămâne al bazei
- Apelul virtual se face DOAR prin pointer/referință. Prin obiect direct, compilatorul știe tipul exact și apelează direct:

```cpp
Dog d;
d.sound();          // apel direct, fără vfptr (compilatorul știe e Dog)
Animal* a = &d;
a->sound();         // apel prin vfptr (polimorfism)
```

### `override` și `final`

```cpp
class A {
    virtual void foo(int x) {}
    virtual void bar() {}
};
class B : public A {
    void foo(int x) override { }      // OK — compilatorul verifică semnătura
    void foo(float x) override { }    // EROARE — semnătura diferă, nu suprascrie nimic
    void bar() final { }              // nu mai poate fi suprascrisă în clase derivate din B
};
class C final : public B { };         // nimeni nu mai poate deriva din C
class D : public C { };              // EROARE — C este final
```

### Metode Pure Virtuale `= 0`

```cpp
class Shape {
public:
    virtual double area() = 0;       // metodă pură virtuală
    virtual void draw() = 0;
    virtual ~Shape() = default;      // destructor virtual — IMPORTANT!
};
// Shape s; // EROARE — clasă abstractă nu poate fi instanțiată
```

**De ce destructor virtual?**
```cpp
Animal* a = new Dog();
delete a;  // fără virtual ~Animal(): apelează ~Animal(), nu ~Dog() → leak!
           // cu virtual ~Animal(): apelează ~Dog(), apoi ~Animal() → corect
```

### Clasă Abstractă vs Interfață

**Clasă abstractă**: cel puțin o metodă pură virtuală; poate avea date membre, constructori, metode concrete.

**Interfață** (convenție în C++): NUMAI metode pure virtuale, fără date membre, fără constructori.

```cpp
// Interfață — contract pur
struct ISerializable {
    virtual void serialize(ostream& out) const = 0;
    virtual void deserialize(istream& in) = 0;
    virtual ~ISerializable() = default;
};

// Clasă abstractă — oferă și implementare parțială
class AbstractAnimal {
protected:
    string name;        // are date membre
    int age;
public:
    AbstractAnimal(string n, int a) : name(n), age(a) {}  // are constructor
    virtual void sound() = 0;                              // metodă pură
    void breathe() { cout << "breathing"; }                // metodă concretă
};
```

**Când folosești una sau cealaltă (întrebare la examen — tip util):**
```
INTERFAȚĂ    → când NU știi nimic despre CUM trebuie implementat
                (doar contractul, "ce" trebuie făcut)
CLASĂ ABSTRACTĂ → când ai deja o IDEE de funcționalitate / comportament comun
                (implementare parțială + lași pași abstracți)
```
> Perspectiva Java (apare la examen): *"Extinderea unei clase abstracte impune o relație puternică între două clase. Implementarea unei interfețe e mult mai ușoară: doar specifică faptul că o clasă respectă un anumit contract."* Nu se exclud — se folosesc împreună:
> `List` = **contractul** (interfață) → `AbstractList` = **comportamentul comun** (clasă abstractă) → `ArrayList`, `LinkedList` = **comportament specific**.
>
> Implementare: în **Java** — `interface I {}` + `class C implements I {}`; `abstract class A {}` + `class C extends A {}`. În **C++** — interfața = clasă cu doar metode pure virtuale; moștenire cu `:`.

### Covarianță

Un override poate returna un tip **derivat** din tipul original de return:

```cpp
class Base {
    virtual Base* clone() const { return new Base(*this); }
};
class Derived : public Base {
    virtual Derived* clone() const override { return new Derived(*this); }  // OK — covarianță
};
```

---

## 9. Cast-uri și RTTI ⭐⭐⭐

### Cele 4 Cast-uri C++

#### `static_cast`
- Verificat la compilare; poate schimba adresa (moștenire multiplă)
- Folosit pentru: conversii numerice, upcast/downcast fără RTTI

```cpp
double d = 3.7;
int i = static_cast<int>(d);      // 3 (trunchiere)
Base* b = static_cast<Base*>(derived_ptr);  // upcast
Derived* d2 = static_cast<Derived*>(base_ptr); // downcast fără verificare runtime
```

#### `dynamic_cast`
- Verificat la **runtime** prin RTTI; returnează `nullptr` dacă tipul nu coincide
- Cerință: clasa trebuie să aibă cel puțin o metodă virtuală (RTTI activat)

```cpp
Animal* a = new Dog();
Dog* d = dynamic_cast<Dog*>(a);   // OK — returnează pointer valid
Cat* c = dynamic_cast<Cat*>(a);   // nullptr — a nu e Cat

if (d) d->fetch();   // sigur
```

#### `reinterpret_cast`
- **Nu** schimbă adresa, nu verifică nimic. Pur reinterpretare de biți. Unsafe.

```cpp
int x = 65;
char* c = reinterpret_cast<char*>(&x);  // vede int ca și char*
// *c == 'A' pe little-endian
```

#### `const_cast`
- Elimină sau adaugă `const`. Singurul cast care modifică calificatorul const.

```cpp
const int x = 5;
int* p = const_cast<int*>(&x);  // atenție: undefined behavior dacă x e const real
*p = 10;
```

### Upcast vs Downcast

```cpp
// Upcast (derived → base): IMPLICIT și SIGUR
Dog* d = new Dog();
Animal* a = d;           // implicit, OK

// Downcast (base → derived): EXPLICIT, periculos fără verificare
Animal* a2 = new Dog();
Dog* d2 = static_cast<Dog*>(a2);   // OK dacă știm sigur că e Dog
Dog* d3 = dynamic_cast<Dog*>(a2);  // RECOMANDAT — verificare la runtime
```

### Object Slicing

Apare când se copiază un obiect derivat într-o variabilă de tip bază (prin valoare):

```cpp
void show(Animal a) {        // PRIN VALOARE — slicing!
    a.sound();               // apelează Animal::sound, chiar dacă a venit un Dog
}
Dog d;
show(d);  // Dog e "tăiat" la Animal — câmpurile specifice Dog sunt pierdute

// Soluție: mereu pointer sau referință
void showSafe(Animal& a) { a.sound(); }   // polimorfism corect
void showSafe(Animal* a) { a->sound(); }  // polimorfism corect
```

---

## 10. Macro-uri și Preprocesare

### `#define` — Macro-uri Simple

```cpp
#define PI 3.14159
#define MAX_SIZE 100
```

Preprocesorul face **substitutie textuală** — nu e o variabilă, nu are tip.

### Macro-uri Funcționale — Capcane

```cpp
#define SQUARE(x) x * x          // GREȘIT
#define SQUARE(x) ((x) * (x))    // CORECT — paranteze la fiecare apariție

SQUARE(3+1)     // fără paranteze: 3+1 * 3+1 = 7 (greșit!)
SQUARE(3+1)     // cu paranteze: (3+1) * (3+1) = 16 (corect)

#define MAX(a,b) ((a) > (b) ? (a) : (b))
MAX(i++, j)  // PERICOL: i++ evaluat de două ori!
```

### Macro-uri Variadice

```cpp
#define LOG(format, ...) { printf("[LOG] "); printf(format, __VA_ARGS__); }
LOG("x=%d, y=%d", 3, 5);  // [LOG] x=3, y=5
```

### Operatori Speciali în Macro-uri

```cpp
#define STRINGIFY(x) #x           // # = stringification
#define CONCAT(a, b) a##b         // ## = concatenare token

STRINGIFY(Hello)    // → "Hello"
CONCAT(var, 1)      // → var1
```

### Macro-uri Predefinite

```cpp
__FILE__     // numele fișierului curent (string)
__LINE__     // numărul liniei curente (int)
__DATE__     // data compilării (string)
__TIME__     // ora compilării (string)
__COUNTER__  // contor unic, incrementat la fiecare apel
```

### `inline` vs Macro

```cpp
// Macro — garantat inlining, dar unsafe (nu are tip)
#define SQ(x) ((x)*(x))

// inline — sugestie pentru compilator, are tip, safe
inline int sq(int x) { return x * x; }
// În Release/optimized: compilatorul chiar face inline
// În Debug: poate fi apelat normal
```

---

## 11. Template-uri (Clase Parametrizate) ⭐⭐⭐

### Funcții Template

```cpp
template <class T>         // sau template <typename T>
T maximum(T a, T b) {
    return a > b ? a : b;
}

maximum<int>(3, 5);        // explicit — T=int
maximum(3.0, 5.0);         // dedus automat — T=double
maximum('A', 'Z');         // T=char
```

### Clase Template

```cpp
template <class T>
class Stack {
    T data[100];
    int top = -1;
public:
    void push(const T& val) { data[++top] = val; }
    T pop() { return data[top--]; }
    T peek() const { return data[top]; }
    bool empty() const { return top == -1; }
    int size() const { return top + 1; }
};

Stack<int> intStack;
Stack<string> strStack;
intStack.push(42);
```

### Parametri Multipli

```cpp
template <class K, class V>
class Dictionary {
    K keys[100];
    V values[100];
    int count = 0;
public:
    void add(const K& k, const V& v) { keys[count] = k; values[count++] = v; }
    V get(const K& k) const { /* ... */ }
};

Dictionary<string, int> grades;
grades.add("Popescu", 10);
```

### Valori Implicite pentru Parametri Template

```cpp
template <class T, int Size = 10>
class FixedArray {
    T data[Size];
public:
    T& operator[](int i) { return data[i]; }
};

FixedArray<int> a1;      // Size=10 (implicit)
FixedArray<int, 50> a2;  // Size=50
```

### Specializare Completă

```cpp
template <class T>
class Box {
public:
    void print(T val) { cout << "Box: " << val; }
};

// Specializare completă pentru bool
template <>
class Box<bool> {
public:
    void print(bool val) { cout << "Bool Box: " << (val ? "true" : "false"); }
};
```

### `static_assert` — Constrângeri la Compilare

```cpp
template <class T, int N>
class SafeArray {
    static_assert(N > 0, "Size must be positive");
    static_assert(sizeof(T) <= 8, "Type too large");
    T data[N];
};

// SafeArray<int, 0> a;  // EROARE compilare: "Size must be positive"
```

### Exemple Canonice de Clase Parametrizate

```cpp
// Stack<T> — LIFO
template <class T>
class Stack { void push(T); T pop(); bool empty(); };

// Queue<T> — FIFO
template <class T>
class Queue { void enqueue(T); T dequeue(); };

// Pair<T,U> — pereche de valori
template <class T, class U>
struct Pair { T first; U second; };

// SmartPointer<T> — gestionare automată
template <class T>
class SmartPointer {
    T* ptr;
public:
    SmartPointer(T* p) : ptr(p) {}
    ~SmartPointer() { delete ptr; }
    T* operator->() { return ptr; }
    T& operator*() { return *ptr; }
};
```

---

## 12. STL — Containere

### Containere Secvențiale

| Container | Acces | Insert/Delete front | Insert/Delete back | Insert middle | Memorie |
|-----------|-------|--------------------|--------------------|---------------|---------|
| `vector<T>` | O(1) random | O(n) | O(1) amortizat | O(n) | contiguu |
| `deque<T>` | O(1) random | O(1) | O(1) | O(n) | blocuri |
| `array<T,N>` | O(1) random | nu | nu | nu | contiguu, fix |
| `list<T>` | O(n) | O(1) | O(1) | O(1) (cu iterator) | noduri |
| `forward_list<T>` | O(n) | O(1) | O(n) | O(1) (cu iterator) | noduri, single |

```cpp
#include <vector>
vector<int> v = {1, 2, 3, 4, 5};
v.push_back(6);           // adaugă la sfârșit
v.pop_back();             // elimină ultimul
v.insert(v.begin(), 0);   // insert la poziție (iterator)
v.erase(v.begin());       // ștergere
v[2];                     // acces fără verificare bounds
v.at(2);                  // acces cu verificare bounds (aruncă out_of_range)

// emplace_back vs push_back:
v.push_back(MyObj(1, 2));    // construiește temporar, apoi copiază/mută
v.emplace_back(1, 2);        // construiește direct în container — mai eficient
```

### Adaptori de Containere

```cpp
#include <stack>
stack<int> s;       // LIFO, implicit bazat pe deque
s.push(1); s.push(2);
s.top();            // accesează vârful
s.pop();            // elimină vârful

#include <queue>
queue<int> q;       // FIFO, implicit bazat pe deque
q.push(1); q.push(2);
q.front();          // primul element
q.pop();            // elimină primul

#include <queue>
priority_queue<int> pq;   // maxim la vârf (implicit)
priority_queue<int, vector<int>, greater<int>> minPQ; // minim la vârf
pq.push(3); pq.push(1); pq.push(5);
pq.top();  // 5
```

### Containere Asociative (Arbore Roșu-Negru, sortate)

```cpp
#include <map>
map<string, int> grades;           // cheie unică, sortat după cheie
grades["Popescu"] = 10;
grades.insert({"Ionescu", 9});
grades.find("Popescu");            // returnează iterator
grades.count("Ionescu");           // 0 sau 1

multimap<string, int> multi;       // chei repetate permise
multi.insert({"Popescu", 10});
multi.insert({"Popescu", 8});      // OK — cheie duplicată
auto range = multi.equal_range("Popescu");  // toate valorile pt cheie

set<int> s = {3, 1, 4, 1, 5};    // {1, 3, 4, 5} — unice, sortate
multiset<int> ms;                  // permite duplicate
```

### Containere Neordonate (Hash-based, O(1) mediu)

```cpp
#include <unordered_map>
unordered_map<string, int> umap;  // hash, fără ordine garantată, mai rapid
unordered_map<string, int> umap;
umap["key"] = 42;

unordered_set<int> uset = {1, 2, 3};
```

### Strings

```cpp
#include <string>
string s = "Hello";
s += " World";              // concatenare
s.length();                 // dimensiune
s.substr(0, 5);             // substring
s.find("World");            // caută (returnează index sau string::npos)
s.replace(6, 5, "C++");    // înlocuiește

wstring ws = L"Salut";     // wide string (Unicode)

// string_view (C++17) — referință non-owning, fără alocare
#include <string_view>
string_view sv = s;         // nu copiază șirul
```

### File I/O

```cpp
#include <fstream>
ofstream fout("output.txt");
fout << "Hello" << endl;
fout.close();

ifstream fin("input.txt");
int x; string line;
fin >> x;             // citire cu whitespace skip
getline(fin, line);   // citire linie întreagă
fin.close();
```

---

## 13. Smart Pointers

### Problema — Raw Pointers sunt Periculoși

```cpp
void bad() {
    int* p = new int(5);
    if (error_condition) return;  // LEAK — delete nu e apelat niciodată
    delete p;
}
```

### `unique_ptr` — Ownership Exclusiv

```cpp
#include <memory>
auto p = make_unique<int>(42);     // creare (preferată față de new)
*p = 100;                           // dereferențiere
p.get();                            // pointer raw (fără transfer ownership)
p.reset();                          // eliberare explicită
auto p2 = move(p);                  // transfer ownership (p devine nullptr)

// unique_ptr<T> = delete copy
unique_ptr<int> p3 = p2;           // EROARE — copy interzis
unique_ptr<int> p3 = move(p2);     // OK — move

// Folosire cu array
auto arr = make_unique<int[]>(10);
arr[0] = 5;
```

### `shared_ptr` — Ownership Partajat (Reference Counting)

```cpp
auto t1 = make_shared<Test>(10);   // count=1
auto t2 = t1;                       // count=2 (copiere permisă)
auto t3 = t1;                       // count=3
t1.reset();                         // count=2
// t2, t3 distrus → count=0 → obiect șters
t1.use_count();                     // returnează numărul de referințe
```

Counter-ul de referințe este **thread-safe**.

### `weak_ptr` — Referință Slabă (fără ownership)

Nu incrementează contorul. Rezolvă problema **referințelor circulare**:

```cpp
// PROBLEMA: A deține B, B deține A → niciodată nu ajunge count la 0 → LEAK
struct A { shared_ptr<B> b; };
struct B { shared_ptr<A> a; };  // circular!

// SOLUȚIE: weak_ptr
struct A { shared_ptr<B> b; };
struct B { weak_ptr<A> a; };    // nu incrementează count

// Acces sigur prin weak_ptr
if (auto locked = weak.lock()) {
    locked->doSomething();  // valid
} // altfel locked e nullptr (obiectul a fost distrus)
```

---

## 14. C++11/14/17: auto, constexpr, lambda, structured binding

### `auto` — Deducerea Tipului

```cpp
auto x = 10;              // int
auto y = 3.14;            // double
auto s = string("hi");    // string
auto p = new Test();      // Test*
auto it = v.begin();      // vector<int>::iterator (foarte util)

// Cu referință
auto& r = x;              // int&
const auto& cr = x;       // const int&

// Cu funcții
auto f = sum;             // pointer la funcție
```

### `decltype` — Tipul unei Expresii

```cpp
int x = 5;
decltype(x) y;            // y este de tip int
decltype(x + 1.0) z;      // z este double (tipul expresiei x + 1.0)
decltype(v[0]) elem = v[0]; // int& (v[0] returnează referință)
```

### `constexpr` — Expresii Constante

Spune compilatorului că valoarea poate fi calculată la **compile time**:

```cpp
constexpr int SIZE = 100;
constexpr double PI = 3.14159;

constexpr int factorial(int n) {
    return n <= 1 ? 1 : n * factorial(n-1);
}
constexpr int f5 = factorial(5);  // calculat la compilare → 120

// constexpr în clase
class Point {
public:
    int x, y;
    constexpr Point(int x, int y) : x(x), y(y) {}
};
constexpr Point p(3, 4);  // creat la compilare, fără apel constructor la runtime
```

**Diferența `const` vs `constexpr`:**
- `const`: valoarea nu poate fi modificată după inițializare
- `constexpr`: valoarea este calculată la compilare; implică și `const`

### Range-based `for`

```cpp
vector<int> v = {1, 2, 3, 4, 5};

for (int x : v) { cout << x; }          // copie (nu modifică v)
for (int& x : v) { x *= 2; }            // referință (modifică v)
for (const int& x : v) { cout << x; }   // referință const (read-only, eficient)
for (auto& x : v) { x++; }              // auto cu referință

// Funcționează și cu array, initializer_list, orice are begin()/end()
int arr[] = {1, 2, 3};
for (int x : arr) { }
for (int x : {1, 2, 3}) { }
```

### Structured Binding (C++17) — Destructuring

```cpp
// Cu array
int a[2] = {1, 2};
auto [x, y] = a;       // x=1, y=2 (copii)
auto& [rx, ry] = a;    // rx, ry sunt referințe la a[0], a[1]

// Cu struct
struct Point { int x, y, z; };
auto [px, py, pz] = Point{1, 2, 3};

// Cu map — very useful!
map<string, int> grades = {{"Popescu", 10}};
for (auto& [name, grade] : grades) {
    cout << name << ": " << grade;
}

// Cu make_tuple / tie (C++11)
auto [a, b] = make_pair(1, 2.5);
```

### Lambda Expressions

O funcție anonimă care poate captura variabile din context:

```cpp
// Sintaxa: [capture](params) -> return_type { body }
auto add = [](int a, int b) -> int { return a + b; };
auto add = [](int a, int b) { return a + b; };  // return type dedus
int result = add(3, 4);  // 7
```

#### Moduri de Captare

```cpp
int x = 10, y = 20;

auto f1 = [x, y]() { return x + y; };    // captură prin COPIE — x, y sunt freeze
auto f2 = [&x, &y]() { return x + y; };  // captură prin REFERINȚĂ
auto f3 = [=]() { return x + y; };       // toate prin copie (doar cele folosite)
auto f4 = [&]() { return x + y; };       // toate prin referință
auto f5 = [=, &x]() { return x + y; };   // y prin copie, x prin referință
auto f6 = [this]() { return member; };    // captură this (în metode ale clasei)
```

#### Lambda și STL

```cpp
vector<int> v = {3, 1, 4, 1, 5, 9};

sort(v.begin(), v.end(), [](int a, int b) { return a < b; });

auto count = count_if(v.begin(), v.end(), [](int x) { return x % 2 == 0; });

for_each(v.begin(), v.end(), [](int& x) { x *= 2; });

// Cu captură — sort după valoare absolută
int pivot = 5;
sort(v.begin(), v.end(), [pivot](int a, int b) {
    return abs(a - pivot) < abs(b - pivot);
});
```

#### `mutable` Lambda

Lambdele care captează prin copie nu pot modifica copiile (operator() este const). `mutable` elimină această restricție:

```cpp
int count = 0;
auto counter = [count]() mutable { return count++; };  // modifică COPIA lui count
// count original rămâne 0
```

#### Lambda este tradusă de compilator ca o clasă cu `operator()`

```cpp
auto f = [x](int a) { return a + x; };
// echivalent cu:
class lambda_xyz {
    int x;
public:
    lambda_xyz(int x) : x(x) {}
    int operator()(int a) const { return a + x; }
};
```

#### Generic Lambda (C++14)

```cpp
auto add = [](auto a, auto b) { return a + b; };
add(1, 2);       // int
add(1.5, 2.5);   // double
```

### CRTP — Static Polymorphism

Curiously Recurring Template Pattern: o clasă derivată este pasată ca template la baza sa. Permite polimorfism **fără overhead de vtable**:

```cpp
template <typename T>
struct Base {
    void interface() {
        static_cast<T*>(this)->implementation();  // apel la derived, fără virtual
    }
};
struct Derived : public Base<Derived> {
    void implementation() { cout << "Derived!"; }
};

Derived d;
d.interface();  // "Derived!" — fără virtual, fără vtable

// Avantaj: performanță (apel direct, inlineable)
// Dezavantaj: nu poți pune Base<A> și Base<B> în același container (nu au bază comună)
```

---

## 15. Excepții ⭐⭐

### Mecanismul de Bază

```cpp
try {
    int x = -1;
    if (x < 0) throw invalid_argument("x must be positive");
    // cod care poate eșua
}
catch (const invalid_argument& e) {
    cerr << "Error: " << e.what();
}
catch (const exception& e) {    // prinde orice derivat din std::exception
    cerr << "Generic: " << e.what();
}
catch (...) {                   // prinde ORICE — mereu ultimul
    cerr << "Unknown exception";
}
```

**Reguli:**
- `catch` pentru tipuri mai specifice vine **înaintea** celor generice
- `catch(...)` este **ultimul** dintotdeauna
- `throw;` (fără argument) re-aruncă excepția curentă
- Excepțiile nu urmează regulile de promovare (un `char` aruncat nu este prins de `catch(int)`)

### Ierarhia Excepțiilor Standard

```
std::exception
├── bad_alloc            new nu poate aloca memorie
├── bad_cast             dynamic_cast eșuează pentru referință
├── logic_error
│   ├── invalid_argument
│   ├── out_of_range     (ex: vector::at cu index invalid)
│   ├── length_error
│   └── domain_error
└── runtime_error
    ├── overflow_error
    ├── underflow_error
    ├── range_error
    └── system_error
```

### Excepție Personalizată

```cpp
struct DatabaseError : public runtime_error {
    int errorCode;
    DatabaseError(int code, const string& msg)
        : runtime_error(msg), errorCode(code) {}
    // what() moștenit din runtime_error
};

void query(const string& sql) {
    if (sql.empty()) throw DatabaseError(404, "Empty query");
}

try {
    query("");
} catch (const DatabaseError& e) {
    cout << "DB Error " << e.errorCode << ": " << e.what();
} catch (const exception& e) {  // prinde orice alt std::exception
    cout << e.what();
}
```

### `noexcept`

```cpp
void safeOp() noexcept { }       // garantează că nu aruncă
void riskyOp() noexcept(false) { } // poate arunca (default)

// Verificare la compilare
static_assert(noexcept(safeOp()), "safeOp must not throw");

// IMPORTANT pentru vector: dacă move ctor e noexcept, vector îl folosește la resize
struct MyObj {
    MyObj(MyObj&& other) noexcept { }  // vector va folosi move (rapid)
    // fără noexcept → vector folosește copy ctor (mai lent, dar safe)
};
```

### RAII — Resource Acquisition Is Initialization

Principiu: resursele sunt legate de ciclul de viață al obiectelor. Destructorul garantează cleanup indiferent de cum iese funcția (normal sau excepție):

```cpp
// Fără RAII — leak la excepție
void bad() {
    FILE* f = fopen("file.txt", "r");
    riskyOp();        // dacă aruncă → fclose nu e apelat!
    fclose(f);
}

// Cu RAII
struct FileGuard {
    FILE* f;
    FileGuard(const char* name) : f(fopen(name, "r")) {}
    ~FileGuard() { if(f) fclose(f); }  // garantat apelat
};
void good() {
    FileGuard guard("file.txt");
    riskyOp();        // dacă aruncă → ~FileGuard() curăță automat
}

// Sau folosiți unique_ptr:
auto p = make_unique<MyClass>();  // eliberat automat la orice exit
```

**NU arunca din destructor** — dacă o excepție se propagă și destructorul aruncă altă excepție → `std::terminate()` → crash.

### SEH — Structured Exception Handling (Windows)

Excepțiile hardware (division by zero, access violation, stack overflow) nu sunt prinse de `try/catch` standard. Pe Windows, se folosesc `__try/__except`:

```cpp
#include <windows.h>
int x = 0;
__try {
    int result = 10 / x;  // division by zero — hardware exception
}
__except (EXCEPTION_EXECUTE_HANDLER) {
    printf("Division by zero caught!");
}
```

---

## 16. Relații de Asociere ⭐⭐

### Asociere (Association)
Relație slabă: obiectele există independent. A "cunoaște" B dar nu îl deține.

```cpp
class Student;
class Course {
    vector<Student*> students;  // cunoaște studenți, nu îi deține
public:
    void enroll(Student* s) { students.push_back(s); }
};
```

### Agregare (Aggregation) — HAS-A slab
Containerul conține componentele, dar ele pot exista independent:

```cpp
class Engine {};
class Car {
    Engine* engine;       // pointer — nu owned
public:
    Car(Engine* e) : engine(e) {}
    ~Car() { /* NU face delete engine */ }
};

Engine e;
{ Car car(&e); }  // car distrus, dar e supraviețuiește
```

### Compoziție (Composition) — HAS-A puternic
Containerul deține componentele; ciclul de viață e legat:

```cpp
class Engine {};
class Car {
    Engine engine;              // valoare — parte integrantă
    // sau: unique_ptr<Engine> engine;
public:
    Car() : engine() {}
    ~Car() { /* engine distrus automat */ }
};
```

### Tabel Comparativ

| Aspect | Asociere | Agregare | Compoziție |
|--------|----------|----------|------------|
| Ownership | Nu | Parțial | Total |
| Ciclu de viață | Independent | Independent | Dependent (copilul moare cu părintele) |
| Implementare | raw pointer / referință | pointer (neowned) | valoare / `unique_ptr` |
| Dacă containerul e distrus | componentele ok | componentele ok | componentele distruse |

---

## 17. Principii SOLID ⭐⭐

### S — Single Responsibility Principle
**O clasă = un singur motiv de schimbare.**

```cpp
// GREȘIT — Car face prea multe lucruri
class Car {
    void drive() { }
    void save(string file) { }         // serializare
    void print() { }                   // afișare
    double computeEfficiency() { }     // calcul
};

// CORECT — fiecare clasă cu responsabilitate unică
class Car { void drive() { } };
class CarSerializer { void save(Car& c, string f) { } };
class CarPrinter { void print(const Car& c) { } };
class EfficiencyCalculator { double calculate(const Car& c) { } };
```

### O — Open-Closed Principle
**Deschis extensiei, închis modificării.**

```cpp
// GREȘIT — adăugarea unui shape necesită modificarea clasei existente
class Shape {
    double area() {
        switch(type) {  // trebuie modificat la fiecare shape nou!
            case CIRCLE: return PI*r*r;
            case RECT: return w*h;
        }
    }
};

// CORECT — extensie prin derivare, Shape nu se modifică
struct Shape { virtual double area() = 0; };
struct Circle : Shape { double area() override { return PI*r*r; } };
struct Rectangle : Shape { double area() override { return w*h; } };
struct Triangle : Shape { double area() override { return b*h/2; } }; // adăugat fără a modifica nimic
```

### L — Liskov Substitution Principle
**Obiectele subclaselor trebuie să poată înlocui obiectele clasei de bază fără a strica comportamentul așteptat.**

```cpp
// VIOLEAZĂ LSP — Penguin nu poate zbura, dar Bird cere asta
struct Bird { virtual void fly() = 0; };
struct Penguin : Bird { void fly() override { throw exception("can't fly"); } };

// CORECT
struct Bird {};
struct FlyingBird : Bird { virtual void fly() = 0; };
struct Eagle : FlyingBird { void fly() override { } };
struct Penguin : Bird { };  // nu implementează fly
```

### I — Interface Segregation Principle
**Clasele nu trebuie forțate să implementeze interfețe pe care nu le folosesc.**

```cpp
// GREȘIT — Animal impune fly și swim tuturor animalelor
struct Animal {
    virtual void fly() = 0;
    virtual void swim() = 0;
    virtual void eat() = 0;
};

// CORECT — interfețe mici și specifice
struct IEater { virtual void eat() = 0; };
struct IFlyer { virtual void fly() = 0; };
struct ISwimmer { virtual void swim() = 0; };

struct Dog : IEater, ISwimmer { void eat() override {}; void swim() override {}; };
struct Eagle : IEater, IFlyer { void eat() override {}; void fly() override {}; };
```

### D — Dependency Inversion Principle
**Modulele de nivel înalt nu depind de cele de nivel jos. Ambele depind de abstracțiuni.**

```cpp
// GREȘIT — Computer depinde de Keyboard (concret)
class Computer {
    Keyboard keyboard;  // depinde direct de o implementare
public:
    void processInput() { auto s = keyboard.getInput(); }
};

// CORECT — Computer depinde de interfața Input
struct Input { virtual string getInput() = 0; };
struct Keyboard : Input { string getInput() override { return "key"; } };
struct Mouse : Input { string getInput() override { return "click"; } };

class Computer {
    Input* input;      // depinde de abstracțiune
public:
    Computer(Input* i) : input(i) {}
    void processInput() { auto s = input->getInput(); }
};
// Poți schimba Keyboard cu Mouse fără a modifica Computer
```

---

## 18. Design Patterns — Gang of Four

GoF (Gang of Four) = Gamma, Helm, Johnson, Vlissides — autori ai cărții "Design Patterns" (1994). Definit 23 de pattern-uri clasice, grupate în 3 categorii.

### Creational Patterns

#### Singleton
O singură instanță a clasei, accesibil global:

```cpp
class Database {
    Database() { }                              // private
    Database(const Database&) = delete;
    Database& operator=(const Database&) = delete;
public:
    static Database& getInstance() {
        static Database inst;                   // thread-safe din C++11
        return inst;
    }
    void query(string sql) { }
};

Database::getInstance().query("SELECT ...");
```

#### Multiton
Extensie a Singleton: mai multe instanțe, fiecare identificată printr-o cheie:

```cpp
class Connection {
    Connection(string url) { }
    static unordered_map<string, shared_ptr<Connection>> pool;
public:
    static shared_ptr<Connection> get(const string& url) {
        if (!pool.count(url))
            pool[url] = shared_ptr<Connection>(new Connection(url));
        return pool[url];
    }
};

auto db1 = Connection::get("server1");
auto db2 = Connection::get("server2");
auto db1_again = Connection::get("server1");  // aceeași instanță ca db1
```

#### Factory Method
Centralizează crearea obiectelor. Decuplează codul client de clasele concrete:

```cpp
struct Animal { virtual void sound() = 0; };
struct Dog : Animal {
    void sound() override { cout << "Ham"; }
private:
    Dog() {}
    friend class AnimalFactory;
};
struct Cat : Animal {
    void sound() override { cout << "Miau"; }
private:
    Cat() {}
    friend class AnimalFactory;
};

struct AnimalFactory {
    static Animal* create(const string& type) {
        if (type == "dog") return new Dog();
        if (type == "cat") return new Cat();
        return nullptr;
    }
};

auto a = AnimalFactory::create("dog");  // nu știm că e Dog, îl folosim prin interfață
a->sound();
```

#### Builder
Construiește obiecte complexe pas cu pas. Fiecare setter returnează `*this` pentru chaining:

```cpp
class Car {
    string name, color;
    int speed, hp;
    Car() {}
    friend class CarBuilder;
public:
    void show() const { cout << name << " " << color << " " << speed << "km/h"; }
};

class CarBuilder {
    string name = "Unknown", color = "Gray";
    int speed = 0, hp = 0;
public:
    CarBuilder& setName(string n) { name = n; return *this; }
    CarBuilder& setColor(string c) { color = c; return *this; }
    CarBuilder& setSpeed(int s) { speed = s; return *this; }
    CarBuilder& setHp(int h) { hp = h; return *this; }
    Car build() {
        Car c; c.name = name; c.color = color; c.speed = speed; c.hp = hp;
        return c;
    }
};

auto car = CarBuilder()
    .setName("Dacia")
    .setColor("Red")
    .setSpeed(120)
    .setHp(75)
    .build();
```

---

### Structural Patterns

#### Adapter
Convertește interfața unei clase incompatibile la ce așteptă clientul:

```cpp
struct Car { virtual void drive() = 0; };
void testDrive(Car* c) { c->drive(); }

struct Bicycle {          // nu derivă din Car
    void pedal() { cout << "Cycling"; }
};

// Adapter face Bicycle compatibil cu interfața Car
struct BicycleAdapter : public Car {
    Bicycle* b;
    BicycleAdapter(Bicycle* bicycle) : b(bicycle) {}
    void drive() override { b->pedal(); }  // traduce interfața
};

Bicycle bike;
BicycleAdapter adapter(&bike);
testDrive(&adapter);     // funcționează!
```

#### Composite
Organizează obiecte în structuri arbore; tratează uniform frunzele și nodurile:

```cpp
struct Widget { virtual void paint() = 0; };

struct Button : Widget {
    string name;
    Button(string n) : name(n) {}
    void paint() override { cout << name << "::paint\n"; }
};

struct Window : Widget {
    vector<Widget*> children;
    string name;
    Window(string n) : name(n) {}
    void add(Widget* w) { children.push_back(w); }
    void paint() override {
        cout << name << "::paint\n";
        for (auto* c : children) c->paint();  // recursiv
    }
};

Window w("MainWindow");
w.add(new Button("OK"));
w.add(new Button("Cancel"));
w.paint();
// MainWindow::paint → OK::paint → Cancel::paint
```

---

### Behavioral Patterns

#### Visitor
Adaugă operații noi la clase existente fără a le modifica:

```cpp
struct Dog; struct Cat;

struct Visitor {
    virtual void visit(Dog& d) = 0;
    virtual void visit(Cat& c) = 0;
};
struct Animal { virtual void accept(Visitor& v) = 0; };

struct Dog : Animal {
    void bark() { cout << "Ham!"; }
    void accept(Visitor& v) override { v.visit(*this); }
};
struct Cat : Animal {
    void miau() { cout << "Miau!"; }
    void accept(Visitor& v) override { v.visit(*this); }
};

// Nouă operație adăugată fără a modifica Dog sau Cat:
struct SoundVisitor : Visitor {
    void visit(Dog& d) override { d.bark(); }
    void visit(Cat& c) override { c.miau(); }
};

Dog dog; Cat cat;
SoundVisitor sv;
dog.accept(sv);   // Ham!
cat.accept(sv);   // Miau!
```

#### Observer
One-to-many: când sursa se schimbă, toți subscriberii sunt notificați automat:

```cpp
struct Observer {
    virtual void update(const string& event) = 0;
};

struct EventSource {
    vector<Observer*> observers;
    void subscribe(Observer* o) { observers.push_back(o); }
    void unsubscribe(Observer* o) {
        observers.erase(remove(observers.begin(), observers.end(), o), observers.end());
    }
    void notify(const string& event) {
        for (auto* o : observers) o->update(event);
    }
};

struct Logger : Observer {
    void update(const string& e) override { cout << "[LOG] " << e; }
};
struct UI : Observer {
    void update(const string& e) override { cout << "[UI] refresh: " << e; }
};

EventSource src;
Logger log; UI ui;
src.subscribe(&log);
src.subscribe(&ui);
src.notify("data changed");   // Ambii primesc notificarea
src.unsubscribe(&log);
src.notify("more changes");   // Doar UI primește
```

#### Chain of Responsibility
Lanț de handlere; fiecare procesează cererea sau o pasează mai departe:

```cpp
struct Widget {
    Widget* parent = nullptr;
    virtual void processKey(string key) = 0;
    void passToParent(string key) {
        if (parent) parent->processKey(key);
    }
};

struct Button : Widget {
    void processKey(string key) override {
        if (key == "Enter") cout << "Button clicked!";
        else passToParent(key);
    }
};

struct Window : Widget {
    void processKey(string key) override {
        if (key == "Alt+F4") cout << "Close window!";
        else passToParent(key);
    }
};

struct Desktop : Widget {
    void processKey(string key) override {
        cout << "Desktop handles: " << key;
    }
};

Desktop desktop;
Window window; window.parent = &desktop;
Button button; button.parent = &window;

button.processKey("Enter");    // Button handled
button.processKey("Alt+F4");   // Button → Window handled
button.processKey("X");        // Button → Window → Desktop handled
```

---

## CHEAT SHEET RAPID

### Constructor / Destructor
```
Ordine construcție: Baze (stg→dr) → Date membre (sus→jos) → Corp
Ordine distrugere:  Corp → Date membre (jos→sus) → Baze (dr→stg)
Initializer list:   OBLIGATORIE pentru const, referințe, clase fără default ctor
Ordine init:        = ordinea DECLARAȚIEI, nu a initializer list!
```

### Virtual / Polimorfism
```
virtual + pointer:  polimorfism dinamic prin vfptr → vtable
= 0:               metodă pură → clasă abstractă (nu poate fi instanțiată)
Interfață:          DOAR metode pure, fără date, fără constructori
override:           verificare semnătură la compilare
final:             blochează suprascriere / moștenire
virtual destructor: OBLIGATORIU dacă ai polimorfism și delete prin pointer bază
```

### Cast-uri
```
static_cast:       compilare, safe, poate schimba adresa
dynamic_cast:      runtime + RTTI, nullptr la eșec (necesită cel puțin un virtual)
reinterpret_cast:  periculos, nu schimbă adresa
const_cast:        adaugă/elimină const
```

### Templates
```
template <class T>:   parametru tip
template <int N>:     parametru valoare
template <>:          specializare completă
static_assert:        verificare la compilare
```

### Smart Pointers
```
unique_ptr:  ownership exclusiv, move-only
shared_ptr:  reference counting, copiabil
weak_ptr:    non-owning, previne circular ref, acces prin .lock()
```

### Lambda
```
[=]:   captură toate prin copie
[&]:   captură toate prin referință
[x]:   captură x prin copie
[&x]:  captură x prin referință
mutable: permite modificarea copiilor capturate
```

### SOLID (memorare rapidă)
```
S: O responsabilitate per clasă
O: Extinde prin derivare, nu modifica baza
L: Subclasa înlocuiește baza fără a rupe logica
I: Interfețe mici și specifice
D: Depinde de abstracțiuni, nu de concrete
```

### Design Patterns (scurt)
```
Singleton:    1 instanță, ctor privat + static getter
Factory:      creare centralizată, ctor privat + friend factory
Builder:      construire pas cu pas, metode returnează *this
Adapter:      convertește interfața incompatibilă
Composite:    structură arbore, tratare uniformă nod/frunză
Observer:     subscribe/notify
Visitor:      operație nouă fără modificarea clasei
Chain of Resp: pasează cererea de-a lungul unui lanț
```

---

# 🎯 TEST GRILĂ — OOP (subiecte reale de licență 2021–2025)
> Fiecare grilă pornește de la un **subiect dat efectiv la examen**. Apasă pe **„Răspuns"** pentru a-l dezvălui. Variantele greșite sunt **greșeli frecvente reale**; răspunsul corect este distribuit uniform pe A/B/C/D.

**1.** *(«Principiul dependințelor inversate. Definiție»)* — DIP afirmă că:
- A) O clasă trebuie să aibă un singur motiv de schimbare
- B) Entitățile trebuie să fie deschise extensiei și închise modificării
- C) Atât modulele de nivel înalt, cât și cele de nivel jos trebuie să depindă de abstracții (nu de detalii)
- D) Modulele de nivel înalt trebuie să depindă direct de cele de nivel jos, pentru eficiență

<details><summary>✅ Răspuns</summary>

**C)** — A = SRP, B = OCP, D = exact inversul greșit al DIP.
</details>

**2.** *(«DIP: exemplu unde nu e respectat și soluția»)* — Care e violarea tipică + soluția?
- A) O clasă instanțiază direct cu `new` o clasă concretă → soluția: injectarea unei abstracții (interfețe) prin constructor
- B) O clasă are prea multe metode → soluția: împărțirea în interfețe mici
- C) O subclasă schimbă comportamentul bazei → soluția: să nu suprascrii metoda
- D) O funcție folosește `switch` pe tip → soluția: polimorfism

<details><summary>✅ Răspuns</summary>

**A)** — Dependency Injection pe o abstracție. B = ISP, C = LSP, D = OCP.
</details>

**3.** *(«Constructor și destructor: apelare în contextul derivării»)* — Ordinea de apelare a CONSTRUCTORILOR la un obiect derivat este:
- A) Derivat → bază → membri
- B) Membri → bază → derivat
- C) Derivat → membri → bază
- D) Bază → membri (în ordinea declarării) → corpul constructorului derivat

<details><summary>✅ Răspuns</summary>

**D)** — Întâi baza, apoi membrii, apoi corpul derivatului. Destructorii merg exact invers.
</details>

**4.** *(«Suprascrierea și polimorfismul dinamic»)* — Polimorfismul dinamic se realizează prin ... și are loc la ...:
- A) supraîncărcare (overloading), la compilare
- B) template-uri, la compilare
- C) metode virtuale (overriding), la execuție (runtime)
- D) supraîncărcarea operatorilor, la execuție

<details><summary>✅ Răspuns</summary>

**C)** — A și B descriu polimorfismul **static** (compile-time). Dinamic = runtime, prin vtable.
</details>

**5.** *(«Supraîncărcarea și suprascrierea metodelor»)* — Diferența corectă:
- A) Overloading = redefinirea unei metode virtuale în derivată; overriding = aceeași metodă cu semnături diferite
- B) Overloading = aceeași metodă cu **semnături diferite** (compile-time); overriding = redefinirea unei **metode virtuale** în derivată (runtime)
- C) Sunt sinonime
- D) Overloading necesită `virtual`, overriding nu

<details><summary>✅ Răspuns</summary>

**B)** — A e inversat; D e inversat. Overriding cere `virtual`; overloading nu.
</details>

**6.** *(«Design pattern-ul Composite»)* — Composite se folosește pentru a:
- A) Adăuga dinamic responsabilități unui obiect
- B) Oferi o interfață simplificată unui subsistem
- C) Converti o interfață incompatibilă
- D) Compune obiecte în structuri arborescente parte-întreg, tratate uniform (nod/frunză)

<details><summary>✅ Răspuns</summary>

**D)** — A = Decorator, B = Facade, C = Adapter.
</details>

**7.** *(«Relația de agregare. Definiție, clasificare, exemple»)* — Agregarea (față de compoziție) presupune că:
- A) Partea poate exista **independent** de întreg (ciclu de viață independent)
- B) Partea nu poate exista fără întreg
- C) Este o relație de moștenire (is-a)
- D) Întregul și partea sunt aceeași clasă

<details><summary>✅ Răspuns</summary>

**A)** — La **compoziție** partea moare cu întregul (B); agregarea e mai „slabă".
</details>

**8.** *(«Relații de asociere între clase. Definiție, tipuri»)* — Cazuri particulare ale asocierii sunt:
- A) Moștenirea și derivarea
- B) Supraîncărcarea și suprascrierea
- C) Agregarea și compoziția
- D) Interfața și clasa abstractă

<details><summary>✅ Răspuns</summary>

**C)** — Agregarea/compoziția = forme de asociere (part-whole). Moștenirea e relație distinctă (is-a).
</details>

**9.** *(«Design pattern-ul Object-Factory»)* — Factory rezolvă problema:
- A) Garantării unei singure instanțe a unei clase
- B) Creării de obiecte fără a expune/specifica clasa concretă instanțiată
- C) Parcurgerii secvențiale a unei colecții
- D) Notificării automate a mai multor obiecte

<details><summary>✅ Răspuns</summary>

**B)** — A = Singleton, C = Iterator, D = Observer.
</details>

**10.** *(«Clase abstracte vs interfețe. Asemănări, deosebiri»)* — Deosebire corectă (în C++):
- A) Interfața poate avea date membre și constructori, clasa abstractă nu
- B) O clasă poate moșteni mai multe clase abstracte, dar o singură interfață
- C) Interfața se poate instanția direct, clasa abstractă nu
- D) Clasa abstractă poate avea date membre, constructori și metode concrete; interfața (convenție) are doar metode pur virtuale

<details><summary>✅ Răspuns</summary>

**D)** — A e inversat; B fals (în C++ poți moșteni multiple de ambele); C fals (niciuna nu se instanțiază).
</details>

**11.** *(întrebarea Rădulescu: «când folosești interfață, când clasă abstractă»)* — Interfața e mai potrivită:
- A) Când vrei doar să impui un contract, fără a ști cum se va implementa
- B) Când ai deja o implementare parțială comună
- C) Când vrei o singură instanță
- D) Când clasele au date membre comune

<details><summary>✅ Răspuns</summary>

**A)** — Interfață = „ce", nu „cum". B și D indică o clasă abstractă; C = Singleton.
</details>

**12.** *(«Clase parametrizate. Definiție, exemple canonice, implementare»)* — Un template C++:
- A) Este instanțiat o singură dată, indiferent de tip
- B) Generează cod separat pentru fiecare tip cu care e instanțiat, la compilare
- C) Rezolvă tipurile la runtime
- D) Necesită moștenire dintr-o clasă de bază comună

<details><summary>✅ Răspuns</summary>

**B)** — Mecanism de compile-time (polimorfism static); o instanțiere per tip.
</details>

**13.** *(«Design pattern-ul Visitor»)* — Visitor permite:
- A) Restaurarea unui obiect la o stare anterioară
- B) Încapsularea unei cereri ca obiect
- C) Definirea unei operații noi asupra unei structuri de obiecte, fără a modifica clasele elementelor
- D) Decuplarea unei abstracții de implementarea ei

<details><summary>✅ Răspuns</summary>

**C)** — A = Memento, B = Command, D = Bridge. Dezavantaj: la o clasă nouă, toți vizitatorii se actualizează.
</details>

**14.** *(«Excepții. Mecanismul de administrare în POO»)* — Mecanismul (C++) folosește:
- A) `try` / `catch` / `throw`, cu potrivire pe tip; `catch(...)` trebuie plasat ultimul
- B) `try` / `except` / `raise`
- C) coduri de retur verificate manual după fiecare apel
- D) `catch(...)` plasat primul

<details><summary>✅ Răspuns</summary>

**A)** — B e Python; C nu e mecanism de excepții; `catch(...)` e mereu **ultimul** (D greșit).
</details>

**15.** *(nota examen: excepțiile ≈ un design pattern)* — Tratarea excepțiilor seamănă cel mai bine cu:
- A) Singleton — o singură excepție globală
- B) Observer — excepția notifică toți handlerii
- C) Factory — excepția creează obiecte
- D) Chain of Responsibility — excepția e pasată din `catch` în `catch` până e tratată

<details><summary>✅ Răspuns</summary>

**D)** — Handler-ele formează un lanț; primul care „se potrivește" tratează excepția.
</details>

**16.** *(«O din SOLID»)* — Principiul Open-Closed spune că entitățile trebuie să fie:
- A) Deschise modificării, închise extensiei
- B) Deschise extensiei, închise modificării
- C) Cu o singură responsabilitate
- D) Substituibile prin subtipurile lor

<details><summary>✅ Răspuns</summary>

**B)** — A e inversul greșit; C = SRP; D = LSP. Se obține prin abstractizare + polimorfism.
</details>

**17.** *(«O din SOLID: când e încălcat»)* — Open-Closed este ÎNCĂLCAT când:
- A) Adaugi o clasă nouă fără a modifica nimic existent
- B) Folosești o interfață/clasă abstractă
- C) Adăugarea unui caz nou cere modificarea unei clase existente (ex. `switch`/`if` pe tip)
- D) Folosești polimorfism dinamic

<details><summary>✅ Răspuns</summary>

**C)** — A, B, D respectă OCP; violarea tipică e un `switch` pe tip care se modifică la fiecare caz nou.
</details>

**18.** *(«Principiul lui Liskov (L)»)* — LSP este încălcat când:
- A) O subclasă schimbă comportamentul așteptat al bazei (ex. `Square` redefinește `setWidth/setHeight` incompatibil cu `Rectangle`)
- B) O subclasă adaugă metode noi
- C) O clasă depinde de o abstracție
- D) O interfață este prea mare

<details><summary>✅ Răspuns</summary>

**A)** — B e permis; C e bun (DIP); D = ISP. LSP cere substituibilitate fără surprize.
</details>

**19.** *(«Metode virtuale»)* — Apelul unei metode virtuale la runtime se face prin:
- A) Căutarea numelui în tabela de simboluri
- B) Rezolvare la compilare de către compilator
- C) `dynamic_cast` / RTTI
- D) Pointerul `vfptr` al obiectului către `vtable`-ul clasei sale reale

<details><summary>✅ Răspuns</summary>

**D)** — Fiecare obiect polimorfic are un vfptr spre vtable; apelul e indirect prin acel tabel.
</details>

**20.** *(«Clase și obiecte. Gruparea obiectelor în clase»)* — O clasă reprezintă:
- A) O instanță concretă a unui obiect
- B) Un șablon care descrie trăsăturile comune (atribute + metode) ale unui grup de obiecte
- C) O funcție cu stare internă
- D) Un modificator de acces

<details><summary>✅ Răspuns</summary>

**B)** — Obiectul e o instanță a clasei; A e inversat.
</details>

**21.** *(«Modificatori de acces pentru atribute»)* — Membrii `protected` sunt accesibili:
- A) De oriunde
- B) Doar din interiorul clasei
- C) Din clasă și din clasele derivate
- D) Doar din funcții `friend`

<details><summary>✅ Răspuns</summary>

**C)** — `private` = doar clasa (B); `public` = oriunde (A); `protected` = clasa + derivate.
</details>

**22.** *(«Reguli de conversie (cast)»)* — Care cast face verificare **la runtime** și dă `nullptr` la eșec (pentru pointeri)?
- A) `dynamic_cast`
- B) `static_cast`
- C) `reinterpret_cast`
- D) `const_cast`

<details><summary>✅ Răspuns</summary>

**A)** — `static_cast` nu verifică; `reinterpret_cast` reinterpretează biții; `const_cast` schimbă doar `const`/`volatile`.
</details>

**23.** *(«Clasificarea relației de derivare»)* — La `class D : private B`, membrii **publici** ai lui B devin în D:
- A) publici
- B) protejați
- C) complet inaccesibili
- D) privați

<details><summary>✅ Răspuns</summary>

**D)** — La moștenire privată, `public` și `protected` din B devin **private** în D. (`protected` moștenire → protected; `public` → neschimbați.)
</details>

**24.** *(«Cele 4 principii POO»)* — Cei 4 piloni fundamentali ai POO sunt:
- A) Single Responsibility, Open-Closed, Liskov, Dependency Inversion
- B) Încapsulare, Abstractizare, Moștenire, Polimorfism
- C) Clase, Obiecte, Metode, Atribute
- D) Constructor, Destructor, Copiere, Mutare

<details><summary>✅ Răspuns</summary>

**B)** — A sunt (parte din) principiile **SOLID** (design); C și D sunt concepte, nu piloni.
</details>

**25.** *(«Cele 4 principii POO: încapsulare vs abstractizare»)* — Încapsularea înseamnă în primul rând:
- A) Gruparea datelor + metodelor într-o clasă și **ascunderea datelor** (data hiding) prin modificatori de acces
- B) Expunerea doar a esențialului și ascunderea complexității de implementare
- C) Preluarea atributelor de la o clasă de bază
- D) Același apel cu comportamente diferite

<details><summary>✅ Răspuns</summary>

**A)** — B descrie **abstractizarea** (confuzie frecventă!); C = moștenire; D = polimorfism.
</details>

---

> 💡 **Distribuția răspunsurilor** e echilibrată (A/B/C/D). Reține **de ce** variantele greșite par corecte — acolo sunt confuziile testate: overloading↔overriding, agregare↔compoziție, interfață↔clasă abstractă, cele 5 litere SOLID, cei 4 piloni POO.
