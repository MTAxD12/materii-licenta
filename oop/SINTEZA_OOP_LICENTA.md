# SINTEZĂ OOP — LICENȚĂ
> Prioritizat după frecvența întrebărilor din anii anteriori

---

## CUPRINS (după prioritate)
1. [Constructor & Destructor](#1-constructor--destructor) ⭐⭐⭐⭐ (4 întrebări)
2. [Clase Virtuale, Interfețe, Clase Abstracte](#2-clase-virtuale-interfețe-clase-abstracte) ⭐⭐⭐ (3 întrebări)
3. [Clase Parametrizate / Templates](#3-clase-parametrizate--templates) ⭐⭐⭐ (3 întrebări)
4. [Suprascriere Metode și Polimorfism Dinamic](#4-suprascriere-metode-și-polimorfism-dinamic) ⭐⭐⭐ (3 întrebări)
5. [Relații de Asociere](#5-relații-de-asociere) ⭐⭐ (2 întrebări)
6. [Principiul Open-Closed (SOLID)](#6-principiul-open-closed-solid) ⭐⭐ (2 întrebări)
7. [Clase, Obiecte, Modificatori de Acces](#7-clase-obiecte-modificatori-de-acces) ⭐⭐ (2 întrebări)
8. [Excepții](#8-excepții) ⭐ (1 întrebare)

---

## 1. Constructor & Destructor

### Definiție și Scop

**Constructorul** este o metodă specială a clasei, cu același nume ca și clasa, fără tip de return. Se apelează automat la crearea unui obiect. Scopul principal:
- Inițializarea datelor membre
- Alocarea resurselor
- Validarea stării inițiale

**Destructorul** este o metodă specială prefixată cu `~`, fără parametri și fără tip de return. Se apelează automat la distrugerea obiectului.

### Tipuri de Constructori

```cpp
class Date {
    int day, month, year;
public:
    Date() : day(1), month(1), year(2000) {}           // Default
    Date(int d, int m, int y) : day(d), month(m), year(y) {} // Parametrizat
    Date(const Date& d) : day(d.day), month(d.month), year(d.year) {} // Copy
    Date(Date&& d) : day(d.day), month(d.month), year(d.year) {}      // Move
    Date() : Date(1, 1, 2000) {}                        // Delegating (apelează alt ctor)
    ~Date() { /* cleanup */ }
};
```

### Lista de Inițializare (Initializer List)

- Sintaxa: `: member(value), member2(value2)`
- **Obligatorie** pentru:
  - Membri `const` (nu pot fi modificați după creare)
  - Membri de tip referință (`&`)
  - Clase fără constructor default
- Ordinea de inițializare = ordinea **declarației** în clasă (nu ordinea din lista de inițializare!)

```cpp
class Example {
    const int x;   // TREBUIE inițializat prin initializer list
    int& ref;      // TREBUIE inițializat prin initializer list
    int y;
public:
    Example(int val, int& r) : x(val), ref(r), y(0) {} // corect
};
```

### Când se Apelează Constructorii/Destructorii

| Situație | Constructor | Destructor |
|----------|-------------|------------|
| Variabilă locală | la declarare | la ieșirea din scope |
| Variabilă globală | la start program | la end program |
| `new` pe heap | la apel `new` | la apel `delete` |
| Array | pt fiecare element | pt fiecare element (invers) |
| Raw pointer `T* p` | **NU** (pointer, nu obiect) | **NU** (doar dacă faci `delete`) |

```cpp
Date d1;          // Constructor apelat
Date* p = &d1;    // NU apelează constructor!
Date* p2 = new Date(); // Constructor apelat
delete p2;        // Destructor apelat
```

### Constructor & Destructor în Contextul Derivării (Inheritance)

**Ordinea construcției** (de jos în sus):
1. Constructorii claselor de bază (stânga→dreapta pentru moștenire multiplă)
2. Constructorii datelor membre (în ordinea declarației)
3. Corpul constructorului clasei derivate

**Ordinea distrugerii** = **inversul** construcției (LIFO)

```cpp
class A {
public:
    A() { printf("ctor A\n"); }
    ~A() { printf("dtor A\n"); }
};
class B {
public:
    B() { printf("ctor B\n"); }
    ~B() { printf("dtor B\n"); }
};
class C : public B, public A {  // B înainte de A
public:
    C() : A(/* explicit */) { printf("ctor C\n"); }
    ~C() { printf("dtor C\n"); }
};

// Output: ctor B → ctor A → ctor C | dtor C → dtor A → dtor B
```

**Apelarea constructorului bazei** — dacă baza nu are constructor default, TREBUIE apelat explicit:

```cpp
class Base {
    int value;
public:
    Base(int v) : value(v) {}  // nu există default constructor
};

class Derived : public Base {
public:
    Derived() : Base(42) {}  // apel explicit obligatoriu
};
```

### Reguli `= delete` și `explicit`

```cpp
class Singleton {
    Singleton() {}                         // constructor privat
public:
    Singleton(const Singleton&) = delete;  // interzice copierea
    Singleton& operator=(const Singleton&) = delete;
    static Singleton& GetInstance() {
        static Singleton inst;
        return inst;
    }
};
```

```cpp
class MyClass {
public:
    explicit MyClass(int x) {}  // interzice conversia implicită
};
// MyClass obj = 5;   // EROARE
// MyClass obj(5);    // OK
```

### Singleton Pattern

```cpp
class Database {
    static Database* instance;
    Database() {}  // constructor privat
public:
    static Database* GetInstance() {
        if (instance == nullptr)
            instance = new Database();
        return instance;
    }
};
Database* Database::instance = nullptr;
```

---

## 2. Clase Virtuale, Interfețe, Clase Abstracte

### Metode Virtuale

Keyword-ul `virtual` permite **polimorfismul dinamic**: apelul metodei se rezolvă la runtime în funcție de tipul real al obiectului, nu de tipul pointerului.

```cpp
class Animal {
public:
    virtual void sound() { printf("..."); }  // virtual - poate fi suprascrisă
};
class Dog : public Animal {
public:
    void sound() override { printf("Ham!"); } // suprascrie
};

Animal* a = new Dog();
a->sound();  // Afișează "Ham!" — polimorfism dinamic
```

### Mecanismul vfptr / vtable

- Orice clasă cu cel puțin o metodă virtuală primește un pointer ascuns: **vfptr** (Virtual Function Pointer)
- **vtable** (Virtual Function Table) = tabel cu pointeri la metodele virtuale ale clasei
- Dimensiune vfptr: 4 bytes (x86), 8 bytes (x64)
- **Fiecare constructor** setează vfptr la vtable-ul propriu
- Apelul virtual se face DOAR prin pointer/referință la clasa de bază

```
[vfptr] → vtable Dog: [&Dog::sound, &Dog::eat, ...]
```

### `override` și `final`

```cpp
class A {
    virtual void foo(int x) {}
};
class B : public A {
    void foo(int x) override {}   // OK
    // void foo(float x) override {} // EROARE compilare — semnătura nu coincide
};
class C : public B {
    void foo(int x) final {}  // nimeni nu mai poate suprascrie
};
class D final : public A {   // nimeni nu mai poate moșteni din D
};
```

### Pure Virtual — Metode Pure Virtuale

```cpp
class Shape {
public:
    virtual double area() = 0;  // metodă pură virtuală (= 0)
};
```

- O clasă cu **cel puțin o** metodă pură virtuală = **clasă abstractă**
- Clasa abstractă **NU poate fi instanțiată** direct
- Clasa derivată TREBUIE să implementeze TOATE metodele pure pentru a fi instanțiabilă

### Diferența: Clasă Abstractă vs Interfață

| Caracteristică | Clasă Abstractă | Interfață |
|----------------|-----------------|-----------|
| Metode pure virtuale | cel puțin una | TOATE |
| Date membre | poate avea | NU are |
| Constructori | poate avea | NU are |
| Metode nevirtu. | poate avea | NU are |

```cpp
// Interfață (DOAR metode pure, fără date)
struct IDrawable {
    virtual void draw() = 0;
    virtual void resize(float factor) = 0;
    virtual ~IDrawable() = default;
};

// Clasă abstractă (date membre + cel puțin o metodă pură)
class AbstractShape {
protected:
    string color;  // are date membre
public:
    AbstractShape(string c) : color(c) {}  // are constructor
    virtual double area() = 0;  // metodă pură — face clasa abstractă
    void setColor(string c) { color = c; }  // metodă non-virtuală
};
```

### Covarianță

O metodă virtuală suprascrisă poate schimba tipul de return **dacă noul tip este derivat din cel original**:

```cpp
class Base {
    virtual Base* clone() { return new Base(); }
};
class Derived : public Base {
    virtual Derived* clone() override { return new Derived(); }  // OK — covarianță
};
```

### Diamond Problem & Virtual Inheritance

```cpp
class A { public: int x; };
class B : public virtual A {};  // virtual inheritance
class C : public virtual A {};
class D : public B, public C {};  // A apare o singură dată

D d;
d.x = 5;  // OK — nu e ambiguu
```

Fără `virtual`: `d.x` ar fi ambiguu (B::A::x vs C::A::x).

---

## 3. Clase Parametrizate / Templates

### Funcții Template

```cpp
template <class T>
T Sum(T a, T b) {
    return a + b;
}

// Utilizare
int x = Sum<int>(3, 4);      // explicit
double y = Sum(1.5, 2.5);    // dedus automat de compilator
```

### Clase Template

```cpp
template <class T>
class Stack {
    T data[100];
    int count = 0;
public:
    void push(T value) { data[count++] = value; }
    T pop() { return data[--count]; }
    int size() const { return count; }
};

// Utilizare
Stack<int> intStack;
Stack<string> strStack;
intStack.push(10);
```

### Parametri Multipli și Valori Implicite

```cpp
template <class T, class U>
class Pair {
public:
    T first;
    U second;
};

template <class T, int Size = 10>  // valoare implicită pentru Size
class Array {
    T data[Size];
};

Array<int> a1;      // Size=10 implicit
Array<int, 20> a2;  // Size=20
```

### Specializare Template

```cpp
template <class T>
class Printer {
public:
    void print(T val) { cout << val; }
};

// Specializare pentru char*
template <>
class Printer<char*> {
public:
    void print(char* val) { printf("String: %s", val); }
};
```

### static_assert — Constrângeri la Compilare

```cpp
template <int Size>
class FixedArray {
    static_assert(Size > 0, "Size must be positive!");
    int data[Size];
};

// FixedArray<0> a; // EROARE la compilare: "Size must be positive!"
```

### Exemple Canonice de Clase Parametrizate

- **Stack\<T\>**: push, pop, top, empty
- **Queue\<T\>**: enqueue, dequeue, front
- **Pair\<T,U\>**: first, second
- **Vector\<T\>**: dynamic array cu resize
- **SmartPointer\<T\>**: gestionare automată memorie

---

## 4. Suprascriere Metode și Polimorfism Dinamic

### Method Hiding vs Overriding

```cpp
class A {
public:
    virtual void foo() { printf("A::foo"); }  // virtual
    void bar() { printf("A::bar"); }           // non-virtual
};
class B : public A {
public:
    void foo() override { printf("B::foo"); }  // SUPRASCRIERE (overriding)
    void bar() { printf("B::bar"); }            // ASCUNDERE (hiding) — nu polimorfism!
};

A* ptr = new B();
ptr->foo();  // "B::foo" — polimorfism dinamic (virtual)
ptr->bar();  // "A::bar" — hiding, tipul static contează
```

### Cum Funcționează Polimorfismul Dinamic

1. Compilatorul pune un **vfptr** în fiecare obiect cu metode virtuale
2. vfptr pointează la **vtable-ul clasei reale** (setată în constructor)
3. La apelul `ptr->foo()`, compilatorul generează: `ptr->vfptr[index_foo]()`
4. Se apelează funcția din vtable-ul obiectului real → polimorfism

```
Obiect de tip Dog:
[vfptr] → vtable_Dog: [&Dog::sound, &Dog::eat]
          ptable_Animal: [&Animal::sound, &Animal::eat]  // suprascris!
```

### RTTI și dynamic_cast

**RTTI** (Run-Time Type Information) este stocat în vtable și permite identificarea tipului real la runtime.

```cpp
Animal* a = new Dog();

// dynamic_cast — sigur, returnează nullptr dacă tipul nu coincide
Dog* d = dynamic_cast<Dog*>(a);  // OK — a este de fapt Dog
Cat* c = dynamic_cast<Cat*>(a);  // nullptr — a nu este Cat

if (d != nullptr) {
    d->fetch();  // sigur
}
```

**Cerință**: clasa trebuie să aibă cel puțin o metodă virtuală pentru dynamic_cast.

### Tipuri de Cast

| Cast | Comportament | Când se folosește |
|------|-------------|-------------------|
| `static_cast<T*>` | compilare, poate schimba adresa | upcast/downcast sigur, conversii numerice |
| `dynamic_cast<T*>` | runtime, RTTI, nullptr la eșec | downcast cu verificare tip |
| `reinterpret_cast<T*>` | compilare, NU schimbă adresa | reinterpretare brută (unsafe) |
| `const_cast<T*>` | compilare | adaugă/elimină const |

### Upcast vs Downcast

```cpp
class Animal {};
class Dog : public Animal {};

// Upcast (derived → base): AUTOMAT, SIGUR
Dog d;
Animal* a = &d;  // implicit, OK

// Downcast (base → derived): EXPLICIT, PERICULOS
Animal* a2 = new Dog();
Dog* d2 = static_cast<Dog*>(a2);   // fără verificare
Dog* d3 = dynamic_cast<Dog*>(a2);  // cu verificare RTTI
```

### Object Slicing

```cpp
void show(Animal a) {  // prin VALOARE — slicing!
    a.sound();  // apelează Animal::sound, nu Dog::sound
}

Dog d;
show(d);  // câmpurile specifice Dog sunt "tăiate"
// Soluție: folosiți referință sau pointer
void show(Animal& a) { a.sound(); }  // OK — polimorfism
```

---

## 5. Relații de Asociere

### Definiții și Clasificări

**Asocierea** reprezintă relația dintre clase/obiecte. Există trei forme principale:

#### 1. Asociere simplă (Association)
- Relație slabă: obiectele există independent unul de altul
- A "cunoaște" B, dar nu este "responsabil" de B
- Implementare: pointer sau referință care nu gestionează ownership

```cpp
class Student {};
class Professor {
    Student* students[30];  // profesor cunoaște studenți, dar nu îi deține
public:
    void addStudent(Student* s) { /* ... */ }
};
```

#### 2. Agregare (Aggregation) — "HAS-A" (slab)
- Obiectul compus **conține** alte obiecte, dar acestea **pot exista independent**
- Dacă se distruge containerul, componentele supraviețuiesc
- Implementare: **pointer** (nu owned)

```cpp
class Engine {};
class Car {
    Engine* engine;  // Car conține Engine, dar Engine poate exista fără Car
public:
    Car(Engine* e) : engine(e) {}
    // ~Car() nu face delete engine!
};
```

#### 3. Compoziție (Composition) — "HAS-A" (puternic)
- Obiectul compus **deține** componentele; ciclul de viață este legat
- Dacă se distruge containerul, componentele se distrug și ele
- Implementare: **date membre directe** sau **unique_ptr**

```cpp
class Engine {};
class Car {
    Engine engine;  // Engine este PARTE INTEGRANTĂ din Car
    // sau: unique_ptr<Engine> engine;
public:
    Car() : engine() {}
    // ~Car() distruge automat engine
};
```

### Rezumat Comparativ

| | Asociere | Agregare | Compoziție |
|--|----------|----------|------------|
| Ownership | Nu | Parțial | Total |
| Ciclu de viață | Independent | Independent | Dependent |
| Implementare | raw pointer/ref | pointer (non-owned) | valoare/unique_ptr |
| Notație UML | linie simplă | diamant gol | diamant plin |

### Implementare cu Smart Pointers

```cpp
class Room {};

// Compoziție — House deține Room-urile
class House {
    vector<unique_ptr<Room>> rooms;  // owner exclusiv
public:
    void addRoom() { rooms.push_back(make_unique<Room>()); }
};

// Asociere — Student nu deține Course
class Course {};
class Student {
    vector<Course*> courses;  // nu owned, nu face delete
};
```

---

## 6. Principiul Open-Closed (SOLID)

### Definiție
> "Entitățile software (clase, module, funcții) trebuie să fie **deschise pentru extensie** dar **închise pentru modificare**."

Adaugi funcționalitate nouă fără să modifici codul existent.

### Exemplu care VIOLEAZĂ principiul

```cpp
enum class ShapeType { Circle, Rectangle };

class Shape {
    ShapeType type;
    double dim1, dim2;
public:
    double area() {
        switch (type) {   // La adăugare de Square → trebuie modificat switch-ul!
            case ShapeType::Circle: return 3.14 * dim1 * dim1;
            case ShapeType::Rectangle: return dim1 * dim2;
        }
    }
};
```

Problema: la adăugarea unui nou tip de formă, trebuie modificat switch-ul → **clasă existentă modificată**.

### Soluție Corectă (cu polimorfism)

```cpp
struct Shape {
    virtual double area() = 0;  // interfață / clasă abstractă
    virtual ~Shape() = default;
};

struct Circle : public Shape {
    double radius;
    Circle(double r) : radius(r) {}
    double area() override { return 3.14 * radius * radius; }
};

struct Rectangle : public Shape {
    double w, h;
    Rectangle(double w, double h) : w(w), h(h) {}
    double area() override { return w * h; }
};

// Adăugare Square nu modifică nimic existent:
struct Square : public Shape {
    double side;
    Square(double s) : side(s) {}
    double area() override { return side * side; }
};
```

### Toate Principiile SOLID

| Acronim | Principiu | Scurt |
|---------|-----------|-------|
| **S** | Single Responsibility | O clasă = un singur motiv de schimbare |
| **O** | Open-Closed | Deschis extensiei, închis modificării |
| **L** | Liskov Substitution | Subclasele înlocuiesc baza fără a rupe logica |
| **I** | Interface Segregation | Interfețe mici și specifice, nu una mare |
| **D** | Dependency Inversion | Modulele de nivel înalt nu depind de cele de nivel jos; ambele depind de abstracțiuni |

### LSP — Exemplu

```cpp
// VIOLEAZĂ LSP:
struct Bird { virtual void fly() = 0; };
struct Penguin : public Bird {
    void fly() override { throw exception("Penguins can't fly!"); }
    // Penguin înlocuiește Bird dar rupe comportamentul așteptat
};

// CORECT:
struct Bird {};
struct FlyingBird : public Bird { virtual void fly() = 0; };
struct Eagle : public FlyingBird { void fly() override { /*...*/ } };
struct Penguin : public Bird {};  // Penguin nu implementează fly
```

---

## 7. Clase, Obiecte, Modificatori de Acces

### Clasă vs Struct

| | class | struct |
|--|-------|--------|
| Acces implicit | private | public |
| Altfel | identice | identice |

### Modificatori de Acces

```cpp
class MyClass {
public:      // accesibil din orice loc
    int x;
protected:   // accesibil din clasă + clase derivate
    int y;
private:     // accesibil DOAR din clasă (default pentru class)
    int z;
};
```

### Transformarea Accesului prin Moștenire

| Membrul original | public inherit | protected inherit | private inherit |
|-----------------|----------------|-------------------|-----------------|
| public | public | protected | private |
| protected | protected | protected | private |
| private | inaccesibil | inaccesibil | inaccesibil |

### Structura unui Obiect în Memorie

```cpp
class Simple {
    int x;    // 4 bytes
    double y; // 8 bytes
    char z;   // 1 byte + padding
};
// sizeof(Simple) = 24 (cu padding pentru aliniere la 8 bytes)
```

Cu moștenire — câmpurile bazei vin **primele**:
```
[Base fields][Derived fields]
```

Cu metode virtuale — primul câmp este **vfptr**:
```
[vfptr][fields...]
```

### Membri Statici

```cpp
class Counter {
    static int count;  // o singură copie, partajată de toate instanțele
    int id;
public:
    Counter() : id(++count) {}
    static int getCount() { return count; }  // metodă statică — nu are 'this'
};
int Counter::count = 0;  // definire în afara clasei (obligatoriu)
```

---

## 8. Excepții

### Sintaxa de Bază

```cpp
try {
    // cod care poate genera excepție
    if (condition) throw MyException("mesaj");
}
catch (const MyException& e) {
    cout << e.what();
}
catch (const std::exception& e) {
    cout << "Generic: " << e.what();
}
catch (...) {  // prinde orice tip, TREBUIE să fie ultimul
    cout << "Unknown exception";
}
```

### Ierarhia Excepțiilor Standard

```
std::exception
├── bad_alloc          (new eșuează)
├── bad_cast           (dynamic_cast eșuează)
├── logic_error
│   ├── invalid_argument
│   ├── out_of_range
│   └── length_error
└── runtime_error
    ├── overflow_error
    └── range_error
```

### Excepție Personalizată

```cpp
struct DivisionByZero : public std::exception {
    int numerator;
    DivisionByZero(int n) : numerator(n) {}
    const char* what() const override { return "Division by zero!"; }
};

int divide(int a, int b) {
    if (b == 0) throw DivisionByZero(a);
    return a / b;
}

try {
    int result = divide(10, 0);
} catch (const DivisionByZero& e) {
    cout << e.what() << " numerator was: " << e.numerator;
} catch (const std::exception& e) {  // prinde orice derivat din exception
    cout << e.what();
}
```

### noexcept

```cpp
void safeFunction() noexcept { /* nu aruncă niciodată */ }

// std::vector folosește move ctor dacă e noexcept (altfel copy ctor — mai lent)
struct MyData {
    MyData(MyData&& other) noexcept { /* move rapid */ }
};
```

### RAII (Resource Acquisition Is Initialization)

Principiu: resursele sunt gestionate prin ciclul de viață al obiectelor (destructor). Asigură cleanup chiar și în caz de excepție.

```cpp
// Fără RAII — memory leak la excepție:
void bad() {
    int* p = new int[100];
    riskyOperation();  // dacă aruncă excepție → leak!
    delete[] p;
}

// Cu RAII — cleanup automat:
void good() {
    auto p = make_unique<int[]>(100);
    riskyOperation();  // dacă aruncă → destructor unique_ptr curăță automat
}
```

**Reguli importante:**
- **NU** arunca excepții din destructori (comportament nedefinit → crash)
- Folosiți `unique_ptr`/`shared_ptr` pentru a evita leak-uri

---

## BONUS: Design Patterns Cheie

### Singleton
Clasă cu o singură instanță. Constructor privat + static `GetInstance()`.

### Factory
Encapsulează crearea obiectelor. Constructor privat în produse + `friend class Factory`.

### Observer
One-to-many: când sursa se schimbă, notifică toți subscriberii.

### Open-Closed prin Polimorfism
Baza stabilă cu metode virtuale pure; extensia se face prin derivare.

---

## CHEAT SHEET RAPID

```
Constructor order:   baze (stânga→dreapta) → date membre → corp
Destructor order:    invers (LIFO)

virtual + pointer:   polimorfism dinamic (vfptr → vtable)
= 0:                 metodă pură → clasă abstractă
Interfață:           DOAR metode pure, fără date
Clasă abstractă:     cel puțin o metodă pură, poate avea date

template <class T>:  clasă/funcție parametrizată
template <>:         specializare completă

Compoziție:          date membre / unique_ptr (ownership)
Agregare:            pointer neowned (fără delete)
Asociere:            pointer/referință, obiecte independente

OCP:                 extinde prin derivare, nu modifica baza
RAII:                resursele → distruse în destructor
noexcept:            permite move în vector (mai rapid decât copy)
```
