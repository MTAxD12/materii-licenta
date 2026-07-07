# GHIDUL DE 2 ORE — înțelege tot sistemul înainte de prezentare

> Cum citești: de sus în jos, fără să sari. E scris în ordinea în care se construiește
> înțelegerea: întâi imaginea de ansamblu, apoi fiecare piesă, apoi cum vorbesc piesele
> între ele, apoi ML-ul, apoi întrebările comisiei. Ce e în casete `┌─┐` sunt diagrame,
> ce e în **bold** sunt lucruri de reținut, ce e în tabele sunt comparații.
>
> Regula de aur când răspunzi comisiei: **nu recita definiții — spune ce problemă rezolvă
> piesa respectivă ÎN SISTEMUL TĂU.** Toate explicațiile de aici sunt construite așa.

---

# PARTEA 0 — Imaginea de ansamblu (10 min)

## Ce ai construit, într-o frază

Un sistem complet de management pentru un restaurant: clientul/chelnerul comandă → bucătăria
vede comanda instant → stocul scade singur → iar un serviciu de ML (Machine Learning = învățare
automată) prezice ce se va vinde în zilele următoare și recomandă produse la comandă.

## Cele 8 servicii (piesele de LEGO)

Sistemul NU e un singur program. E format din **8 programe separate**, fiecare rulând în
propriul container Docker, care comunică între ele prin rețea:

```
             UTILIZATORII (oameni, în browser)
                                                     
  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
  │   POS    │ │Bucătărie │ │Management│ │  Client  │   4 interfețe web
  │  :3000   │ │  :3001   │ │  :3002   │ │  :3003   │   (React)
  └────┬─────┘ └────┬─────┘ └────┬─────┘ └────┬─────┘
       │            │            │            │
       └────────────┴─────┬──────┴────────────┘
                          │  REST (cereri HTTP) + SignalR (push în timp real)
                          ▼
               ┌─────────────────────┐
               │   API — ASP.NET     │  5. creierul sistemului (C#)
               │   Core   :8081      │
               └──────┬───────┬──────┘
          SQL         │       │  publică evenimente
          (EF Core)   │       │
                      ▼       ▼
        ┌────────────────┐  ┌──────────────┐
        │  PostgreSQL    │  │   RabbitMQ   │   6. baza de date
        │    :5432       │  │   :5672      │   7. poștașul (broker de mesaje)
        └───────▲────────┘  └──────┬───────┘
                │                  │ consumă evenimente
                │ citește istoric  ▼
                │          ┌──────────────┐
                └──────────│  Serviciu ML │   8. creierul „inteligent" (Python)
                           │    :8001     │      recomandări + predicția cererii
                           └──────────────┘
```

**De reținut ca pe apă:** 4 interfețe React + API C# + PostgreSQL + RabbitMQ + serviciu ML Python
= **8 servicii**. Toate pornesc cu o singură comandă: `docker compose up`.

## Cine ce rol are (analogia cu restaurantul real)

| Serviciu | În restaurantul real ar fi... | Tehnologie |
|---|---|---|
| POS (Point of Sale = punct de vânzare) | Carnețelul chelnerului | React |
| Ecran bucătărie | Panoul cu comenzi din bucătărie | React |
| Panou management | Biroul managerului (rapoarte, stocuri) | React |
| Portal client | Meniul + comanda de pe telefonul clientului | React |
| API (Application Programming Interface) | Managerul care coordonează tot | C# / ASP.NET Core |
| PostgreSQL | Registrul contabil + caietul de stocuri | Bază de date relațională |
| RabbitMQ | Poștașul care duce bilețele între departamente | Broker de mesaje |
| Serviciu ML | Analistul care prezice vânzările de mâine | Python / FastAPI |

## De ce e spart în bucăți (microservicii) și nu un singur program (monolit)?

Două motive **concrete** (astea le spui comisiei, nu definiții din carte):

1. **Limbaje diferite pentru treburi diferite.** Logica de business e în C# (tipuri stricte,
   Entity Framework — îmi prinde erorile la compilare). ML-ul cere Python (numpy, scikit-learn
   nu au echivalent serios în C#). Două limbaje nu pot trăi în același proces → două servicii.
2. **Izolarea defectelor.** Dacă serviciul ML moare, restaurantul funcționează normal în
   continuare: comenzile se plasează, bucătăria le vede, doar recomandările lipsesc.
   Recomandarea NU e pe „calea critică" a comenzii.

Prețul plătit: mai multe piese de pornit și configurat → rezolvat cu Docker (o comandă pornește tot).

---

# PARTEA 1 — Frontend-ul: cele 4 interfețe React (15 min)

## Ce e React, pe scurt

**React** = bibliotecă JavaScript pentru interfețe. Ideea centrală: **interfața e o funcție a
stării**. Tu nu spui „adaugă un rând în tabel", tu spui „starea = lista de comenzi" și descrii
cum arată ecranul pentru orice listă. Când lista se schimbă, React redesenează SINGUR doar
bucățile care s-au schimbat.

```
   stare (date)          funcția ta           ce vede omul
  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐
  │ comenzi:    │ ───► │  <OrderCard>│ ───► │ ┌─────────┐ │
  │  [#12, #13] │      │  pentru     │      │ │Comanda12│ │
  │             │      │  fiecare    │      │ │Comanda13│ │
  └─────────────┘      └─────────────┘      │ └─────────┘ │
        ▲                                   └─────────────┘
        │ sosește comanda #14 (prin SignalR)
        └── starea se schimbă → React redesenează automat lista
```

De ce se potrivește aici: ecranul bucătăriei primește evenimente în timp real și trebuie să se
actualizeze singur, fără refresh. Exact modelul „stare → interfață".

## Noțiuni pe care le poți primi ca întrebare

- **SPA (Single-Page Application)** = aplicația se încarcă O DATĂ în browser (un singur HTML),
  apoi tot ce se schimbă pe ecran se face din JavaScript, fără reîncărcarea paginii.
  Navigarea „între pagini" e simulată. Opusul: site clasic unde fiecare click aduce alt HTML de la server.
- **Componentă** = o bucată de interfață reutilizabilă (ex: `OrderCard.jsx` = cardul unei
  comenzi). Compui interfața din componente ca din cărămizi.
- **Hook** = funcție specială React care dă componentelor „puteri": `useState` (ține minte o
  stare), `useEffect` (fă ceva la montare, ex: adu datele de la API). Eu mi-am scris hook-uri
  proprii: `useCart` (coșul), `useMenu` (meniul), `useKitchenOrders` (comenzile + SignalR).
- **Vite** = unealta de dezvoltare/împachetare. Pornire instant a serverului de dev + la final
  „build" = împachetează totul în fișiere statice (HTML+JS+CSS).
- **Tailwind CSS** = stilizare prin clase utilitare direct în marcaj (`bg-gray-900 rounded-xl`),
  în loc de fișiere CSS separate. Mi-a ținut tema dark consistentă pe toate 4 aplicațiile.
- **nginx** = în producție (în Docker), fișierele statice generate de Vite sunt servite de un
  server web mic numit nginx. Fiecare frontend = un container nginx cu fișierele lui.
- **axios** = biblioteca cu care JavaScript-ul trimite cereri HTTP către API. Am pus un
  „interceptor" care atașează automat token-ul JWT la fiecare cerere.

## Ce face fiecare interfață (posibil să te pună să le descrii)

| Interfață | Utilizator | Funcții cheie |
|---|---|---|
| **POS** :3000 | chelnerul | meniu pe categorii → coș → selectare masă → note → modal de confirmare **cu 3 recomandări ML** → trimite comanda |
| **Bucătărie** :3001 | bucătarul | 2 coloane („Comenzi noi" / „În preparare"), comenzile apar în timp real prin SignalR, butoane de schimbare status |
| **Management** :3002 | managerul | KPI-uri (indicatori) zilnice, stocuri live + reaprovizionare, predicția cererii + necesar ingrediente, meniu (CRUD = Create/Read/Update/Delete), rapoarte, costuri și marje, livrări, rezervări |
| **Portal client** :3003 | clientul | cont + login, meniu cu căutare și filtru de alergeni, comandă (la masă sau livrare), plată cash/card (Stripe), tracking în timp real, istoricul comenzilor, buton „Am primit comanda" |

---

# PARTEA 2 — Backend-ul: API-ul ASP.NET Core (20 min)

## Ce e un API și ce e REST

**API (Application Programming Interface)** = un program care nu are interfață pentru oameni,
ci pentru ALTE programe. Frontend-urile vorbesc cu el prin cereri HTTP.

**REST (Representational State Transfer)** = convenția după care sunt organizate aceste cereri:
resursele au adrese (URL-uri), iar verbele HTTP spun ce vrei să faci cu ele:

```
GET    /api/menu            →  „dă-mi meniul"            (citire)
POST   /api/orders          →  „creează o comandă"       (creare)
PATCH  /api/orders/5/status →  „schimbă statusul la #5"  (modificare parțială)
DELETE /api/menu/items/3    →  „șterge produsul 3"       (ștergere)
```

Răspunsurile vin ca **JSON (JavaScript Object Notation)** = format text de date, ex:
`{ "id": 5, "status": 2, "items": [...] }`.

## Cum e organizat codul: lanțul Controller → Service → DB

```
  cerere HTTP           CONTROLLER              SERVICE               EF CORE → SQL
 ────────────►  ┌─────────────────────┐  ┌──────────────────┐  ┌─────────────────────┐
 POST /api/     │ OrdersController    │  │ OrderService     │  │ db.Orders.Add(...)  │
 orders         │ - primește cererea  │─►│ - logica reală:  │─►│ SaveChangesAsync()  │
                │ - verifică cine ești│  │   validare, stoc,│  │  → INSERT INTO ...  │
                │ - deleagă mai jos   │  │   notificări     │  │    (PostgreSQL)     │
                └─────────────────────┘  └──────────────────┘  └─────────────────────┘
```

- **Controller** = „recepționerul": primește cererea HTTP, verifică drepturile, deleagă.
  Controllerele mele: Auth, Menu, Categories, Orders, Ingredients, Stats, Payment,
  Reservations, Tables, Webhooks.
- **Service** = logica de business (OrderService, AuthService, MenuService, StatsService...).
- **DI (Dependency Injection = injecție de dependențe)** = serviciile nu se creează manual;
  le declari în `Program.cs` și framework-ul le „injectează" unde e nevoie. Avantaj: cuplare
  slabă, ușor de înlocuit/testat.

## Entity Framework Core și „code-first"

**ORM (Object-Relational Mapper)** = traducător între obiecte C# și tabele SQL.
**EF Core (Entity Framework Core)** = ORM-ul de la Microsoft. În loc să scriu SQL de mână:

```csharp
// eu scriu C#:
var order = await db.Orders.Include(o => o.Items).FirstAsync(o => o.Id == 5);
// EF Core generează SQL-ul: SELECT ... FROM "Orders" JOIN "OrderItems" ... WHERE "Id"=5
```

**Code-first** = clasele C# sunt sursa de adevăr; schema bazei de date se generează din ele prin
**migrări** (migrations) = pași versionați de modificare a schemei („adaugă coloana CostPerUnit").
La pornirea API-ului, migrările nerulate se aplică automat.

## DTO — de ce nu trimit entitățile direct

**DTO (Data Transfer Object)** = obiect „plat" creat special pentru a fi trimis în exterior.
Problema pe care o rezolvă: `Order` conține lista `OrderItems`, iar fiecare `OrderItem` are
referință înapoi la `Order` → serializarea directă în JSON ar intra în **buclă infinită**
(Order → Items → Order → Items...). Soluția: fiecare entitate are o metodă `ToResponse()`
care produce un obiect fără referințe circulare. **Nicio entitate nu iese direct pe ușă.**

## Autentificarea: JWT pas cu pas

**JWT (JSON Web Token)** = un „ecuson" digital semnat criptografic. Cum funcționează:

```
 1. LOGIN                              2. ORICE CERERE ULTERIOARĂ
 ┌────────┐  user+parolă  ┌─────┐     ┌────────┐ Authorization:      ┌─────┐
 │ client │ ────────────► │ API │     │ client │ Bearer eyJhbG...    │ API │
 │        │ ◄──────────── │     │     │        │ ──────────────────► │     │
 └────────┘   token JWT   └─────┘     └────────┘  API VERIFICĂ       └─────┘
                                                  SEMNĂTURA (nu ține
    parola e comparată cu                         sesiuni în memorie!)
    hash-ul BCrypt din DB
```

- Token-ul conține **claims** (afirmații): `sub` = id-ul utilizatorului, `unique_name` = numele,
  `role` = rolul. E semnat cu o cheie secretă → nu poate fi falsificat (orice modificare strică semnătura).
- **BCrypt** = algoritm de hash pentru parole: parola NU se stochează niciodată; se stochează un
  hash lent-de-calculat, cu „salt" (sare) — două parole identice dau hash-uri diferite.
- **Stateless (fără stare)** = serverul nu ține minte cine e logat; validează semnătura la fiecare
  cerere. Avantaj: poți porni 5 copii ale API-ului și oricare validează orice token.
- **4 roluri**: Admin, Kitchen, Waiter, Client. Fiecare endpoint declară cine îl poate apela:
  `[Authorize(Roles = "Admin,Kitchen")]`.
- Capcana pe care am pățit-o (poveste bună pentru comisie): ASP.NET „traduce" implicit numele
  claim-urilor în URI-uri lungi → căutarea după `"sub"` nu găsea nimic → eroare 500. Fix:
  `MapInboundClaims = false` (claim-urile rămân cum le-am pus) + căutare defensivă în ambele forme.

## CORS — de ce a durut

**CORS (Cross-Origin Resource Sharing)** = mecanism de securitate al BROWSERULUI: o pagină de pe
`localhost:3000` nu are voie implicit să cheme un API de pe `localhost:8081` (alt „origin").
API-ul trebuie să declare explicit cine are voie. Problema mea: SignalR cere
`AllowCredentials()`, dar asta e incompatibilă cu `AllowAnyOrigin()`. Soluția:
`SetIsOriginAllowed(_ => true)` — permite orice origine într-o formă compatibilă cu credențialele.
(În producție aș restrânge lista la domeniile reale.)

---

# PARTEA 3 — Baza de date: PostgreSQL (10 min)

## De ce bază RELAȚIONALĂ

Datele unui restaurant sunt prin natura lor legate între ele: o comandă ARE linii, o linie
REFERĂ un produs, un produs ARE o rețetă din ingrediente. Un model relațional cu **chei străine**
(foreign keys = coloana care arată spre rândul din alt tabel) exprimă exact asta și garantează
integritatea (nu poți avea o linie de comandă care arată spre un produs inexistent).

## Schema, desenată

```
 Category ──1:N── MenuItem ──1:N── RecipeItem ──N:1── Ingredient
 (categorie)      (produs)         (cât consumă         (stoc curent,
                     │              o porție)            prag minim,
                     │ N:1                               cost/unitate)
                     │
 Order ──────1:N── OrderItem
 (comandă)          (linie: produs, cantitate, PREȚ ÎNGHEȚAT)
   │
   ├── N:1 → Table (masa; pentru comenzi la masă)
   ├── N:1 → User  (UserId  = clientul care a comandat din portal)  ← ambele opționale!
   └── N:1 → User  (StaffId = chelnerul care a introdus-o din POS)

 User (username, hash BCrypt, rol)          Reservation (masă, interval, depozit Stripe)
```

## Detalii de modelare pe care comisia le poate întreba

- **De ce `OrderItem.UnitPrice` copiat, nu citit din produs?** Prețul produsului se poate
  schimba mâine; comanda de azi trebuie să rămână la prețul de azi. „Îngheț" prețul la plasare.
- **De ce `decimal` și nu `float` pentru bani?** `float` = virgulă mobilă binară → erori de
  rotunjire (0.1 + 0.2 ≠ 0.3). `decimal` = exact pentru zecimale. Configurat explicit
  `decimal(10,2)` (10 cifre, 2 zecimale).
- **Statusul comenzii** = enumerare (număr cu nume): 0 Pending (în așteptare) → 1 InProgress
  (în preparare) → 2 Ready (gata) → 3 Served (servită/livrată); 4 Cancelled (anulată) e terminală.
- **De ce Order are DOI utilizatori (UserId + StaffId)?** Clientul care a comandat din portal ≠
  chelnerul care a introdus-o din POS. Ambele nullable (comandă anonimă e permisă).
- **De ce PostgreSQL și nu MongoDB (NoSQL)?** Datele sunt puternic relaționale, am nevoie de
  tranzacții și chei străine; NoSQL n-ar aduce nimic aici, doar ar slăbi garanțiile de integritate.

---

# PARTEA 4 — Cele 3 feluri de comunicare: REST vs SignalR vs RabbitMQ (20 min)

**Asta e inima arhitecturii tale. Dacă înțelegi tabelul ăsta, poți răspunde la jumătate din întrebări.**

| | REST | SignalR | RabbitMQ |
|---|---|---|---|
| Analogie | telefon: întrebi și aștepți răspunsul | difuzorul din bucătărie: serverul STRIGĂ când e ceva nou | cutia poștală: lași scrisoarea, pleci; poștașul o livrează când poate |
| Direcție | client → server (cerere-răspuns) | server → client (push) | serviciu → serviciu (evenimente) |
| Sincron? | DA — aștepți răspunsul | conexiune permanentă deschisă | NU — trimiți și uiți (fire-and-forget) |
| În sistemul meu | tot ce e cerere-răspuns: meniu, plasare comandă, statistici | comanda nouă apare instant în bucătărie/dashboard | API-ul anunță serviciul ML de fiecare comandă |
| Dacă destinatarul e căzut | eroare imediată | clientul se reconectează automat | mesajul AȘTEAPTĂ în coadă până își revine |

## SignalR în detaliu

**SignalR** = biblioteca Microsoft pentru comunicare în timp real. Ideal folosește **WebSocket** =
conexiune bidirecțională permanentă între browser și server (nu se închide după fiecare mesaj,
ca la HTTP clasic). Prin ea serverul poate ÎMPINGE mesaje către browser oricând.

Alternativa naivă ar fi **polling** = browserul întreabă serverul în buclă la fiecare X secunde
(„ai ceva nou? ai ceva nou?"). Risipă de cereri + întârziere de până la X secunde. Cu SignalR:
zero cereri inutile, latență practic nulă.

```
                        HUB (OrderHub, pe server)
                    ┌───────────────────────────────┐
                    │  grupul "kitchen"   grupul    │
                    │  ┌──────────────┐  "waiters"  │
   comandă nouă ───►│  │ ecran        │ ┌─────────┐ │
   NewOrder         │  │ bucătărie #1 │ │ POS-uri │ │
   (doar către      │  │ ecran        │ └─────────┘ │
    grupul kitchen) │  │ bucătărie #2 │             │
                    │  └──────────────┘             │
                    └───────────────────────────────┘
```

- **Hub** = punctul central pe server prin care circulă mesajele.
- **Grupuri**: la conectare, ecranul de bucătărie cheamă `JoinGroup("kitchen")`. Serverul trimite
  `NewOrder` DOAR grupului „kitchen", nu tuturor browserelor conectate.
- Evenimentele mele: `NewOrder` (comandă nouă) și `OrderStatusChanged` (status schimbat —
  îl ascultă și dashboard-ul ca să-și actualizeze cifrele).
- Detaliu tehnic bun de menționat: WebSocket nu acceptă antete (headers) HTTP personalizate la
  handshake → token-ul JWT se trimite prin parametru de query (`?access_token=...`), iar serverul
  îl preia special pentru căile care încep cu `/hubs`.
- `withAutomaticReconnect()` pe client + indicator vizual conectat/deconectat pe ecranul bucătăriei.

## RabbitMQ în detaliu

**RabbitMQ** = **broker de mesaje** (message broker) = un program intermediar specializat în
primit, ținut și livrat mesaje între alte programe. Vocabular:

- **Producer (producător)** = cine publică mesaje → API-ul C#, la fiecare comandă plasată.
- **Consumer (consumator)** = cine le primește → serviciul ML (Python, biblioteca `pika`).
- **Exchange** = „sortatorul" în care publici. Tipul folosit: **fanout** = distribuie fiecare
  mesaj către TOATE cozile legate de el (ca un difuzor, nu ca o adresă).
- **Queue (coadă)** = cutia poștală din care consumatorul citește. Dacă consumatorul e oprit,
  mesajele așteaptă în coadă.

```
   API C#                    RabbitMQ                        Python ML
  ┌────────┐    publish   ┌──────────────┐   coada ML    ┌──────────────┐
  │comandă │ ───────────► │  exchange    │ ────────────► │ consumer.py  │
  │plasată │              │  "orders"    │               │ (thread      │
  └────────┘              │  (fanout)    │──► (coadă     │  separat)    │
                          └──────────────┘    viitoare:  └──────────────┘
                                              analytics?)
```

**De ce broker și nu apel HTTP direct de la API la ML?** Trei argumente (le zici în ordinea asta):
1. **API-ul nu așteaptă.** La plasarea comenzii, clientul vrea răspuns imediat; reantrenarea
   modelului poate dura. API-ul publică evenimentul și își vede de treabă (asincron).
2. **Toleranță la căderi.** Dacă serviciul ML e oprit, mesajele așteaptă în coadă; când își
   revine, le primește. Cu HTTP direct, cererile s-ar fi pierdut.
3. **Extensibilitate.** Fanout = pot lega mâine o coadă nouă (ex: modul de analytics) fără să
   ating o linie din API.

**Ce face ML-ul cu mesajele:** NU se reantrenează la fiecare comandă (risipă). Comenzile intră
într-un **buffer**; reantrenarea completă din baza de date se declanșează la **10 comenzi noi SAU
10 minute**, ce vine primul. Modelul nou înlocuiește modelul vechi printr-o atribuire atomică
(cererile în curs nu văd niciodată un model pe jumătate antrenat).

Detaliu de război (autenticitate): C# serializează JSON cu **PascalCase** (`TableId`), Python-ul
căuta **camelCase** (`tableId`) → mesajele soseau dar păreau goale. Fix: citesc ambele variante.

---

# PARTEA 5 — Fluxul complet al unei comenzi (10 min)

**Povestea asta leagă TOT. Dacă o poți spune cursiv, ai demonstrat că înțelegi sistemul.**

```
 1  Chelnerul apasă „Confirmă" în POS
 2  POS → POST /api/orders (REST, cu JSON: masă, produse, cantități, note)
 3  OrdersController: cine plasează? client autentificat → UserId;
                      personal → StaffId; nimeni → comandă anonimă
 4  OrderService.PlaceOrderAsync:
      a) încarcă produsele din DB, calculează prețurile (le ÎNGHEAȚĂ în OrderItem)
      b) salvează Order + OrderItems  →  SaveChangesAsync()   ← comanda EXISTĂ acum
      c) DeductStockAsync: pentru fiecare produs comandat ia rețeta;
         scădere = cantitate_rețetă × cantitate_comandată, per ingredient;
         dacă un ingredient scade sub pragul minim → TOATE produsele care îl
         folosesc devin IsAvailable = false (dispar din meniu)
      d) SignalR: hub.Clients.Group("kitchen").SendAsync("NewOrder", dto)
      e) RabbitMQ: publish { OrderId, Items... } pe exchange-ul "orders"
 5  Ecranul bucătăriei (abonat la grupul "kitchen") afișează comanda INSTANT
 6  Serviciul ML primește evenimentul → buffer → (la 10 comenzi/10 min) reantrenare
 7  Bucătarul: „În preparare" → „Gata"  → PATCH /api/orders/{id}/status
      → SignalR OrderStatusChanged → POS/dashboard/tracking văd statusul nou
```

**De ce ordinea b) → d)?** Întâi salvez, abia apoi anunț. Dacă salvarea eșuează, nu am anunțat
o comandă care nu există. (Răspuns bun dacă te întreabă de consistență.)

**Plata cu cardul (Stripe):** API-ul creează o sesiune Stripe Checkout cu produsele comenzii →
clientul e redirecționat pe pagina securizată Stripe → la revenire, API-ul **verifică direct cu
Stripe** că sesiunea e plătită (nu se încrede în parametrii din URL-ul de retur, care pot fi
falsificați) → abia atunci marchează PaymentStatus = Paid. Cheia secretă stă în variabilă de
mediu, nu în cod.

---

# PARTEA 6 — ML #1: Recomandările (ALS) — explicat ca la grădiniță (20 min)

## Problema

Chelnerul are în coș pizza + bere. Ce al treilea produs ar avea sens să sugerez? Vreau să învăț
asta DIN ISTORICUL COMENZILOR, nu din reguli scrise de mână.

## Pas 1: feedback implicit (de ce nu e ca la Netflix)

- **Feedback explicit** = utilizatorul îți spune direct ce-i place (stele 1–5).
- **Feedback implicit** = doar OBSERVI comportamentul: a comandat / n-a comandat.
  Nimeni nu dă stele la șaorma. Subtilitatea crucială: **absența comenzii NU înseamnă „nu-i
  place"** — poate nici n-a văzut produsul în meniu. Deci nu pot trata zerourile ca note negative.

## Pas 2: matricea de interacțiuni

Construiesc un tabel mare: rânduri = utilizatori (sau comenzi anonime), coloane = produse,
valoare = de câte ori a comandat.

```
              Pizza  Bere  Tiramisu  Salată ...
  Ion           3     2       0        1
  Maria         0     0       4        2
  comanda#517   1     1       0        0        ← comenzile anonime sunt și ele „utilizatori"
```

Matricea e **rară (sparse)**: majoritatea celulelor sunt 0 (nimeni nu comandă tot meniul).

## Pas 3: ideea factorilor latenți (inima algoritmului)

Vreau să descriu FIECARE utilizator și FIECARE produs prin câte **32 de numere** (un vector).
Aceste numere nu le aleg eu — le ÎNVAȚĂ modelul, și ajung să codifice concepte nescrise nicăieri:
„cât de italian e produsul", „cât de dulce", „e băutură?"...

**Scorul potrivirii = produsul scalar** dintre vectorul utilizatorului și al produsului
(înmulțești pozițiile între ele și aduni). Vector-utilizator „aliniat" cu vector-produs → scor
mare → recomandă.

```
  Ion      = [ 0.9 italian,  0.1 dulce, 0.7 bere-lover, ...]
  Tiramisu = [ 0.8 italian,  0.9 dulce, 0.0 bere,       ...]
  scor(Ion, Tiramisu) = 0.9·0.8 + 0.1·0.9 + 0.7·0.0 + ... = ridicat → recomandă
```

(Etichetele „italian/dulce" sunt doar pentru intuiție — în realitate sunt dimensiuni anonime.)

## Pas 4: preferință și încredere (specificul feedback-ului implicit)

Din numărul brut de comenzi derivez două lucruri:
- **Preferința p** = binar: 1 dacă a comandat vreodată, 0 dacă nu.
- **Încrederea c = 1 + α·r** (α=40, r = nr. de comenzi): a comandat de 5 ori → sunt FOARTE sigur
  că-i place; n-a comandat → am încredere minimă în acel 0 (poate doar nu l-a văzut).

Modelul încearcă să ghicească preferințele, dar greșelile pe celulele cu încredere mare „dor"
mai tare la antrenare. Așa tratez corect zerourile nesigure.

## Pas 5: de ce „Alternating Least Squares"

Trebuie găsite SIMULTAN toate vectorele de utilizatori (X) și de produse (Y) care aproximează
matricea. Problema cu ambele necunoscute deodată e grea. Trucul:

```
  repetă de 15 ori:
    1. ÎNGHEAȚĂ produsele Y  → pentru fiecare utilizator, vectorul lui optim
       are FORMULĂ EXACTĂ (least squares = cele mai mici pătrate)
    2. ÎNGHEAȚĂ utilizatorii X → analog pentru fiecare produs
  → de aceea „alternating" (alternez ce țin fix)
```

Fiecare pas rezolvă un sistem liniar mic (`numpy.linalg.solve` — mai stabil numeric decât
inversarea explicită de matrice). Plus **regularizare λ=0.1** = penalizez vectorii cu valori
mari, ca modelul să nu memoreze datele (protecție anti-supraînvățare / overfitting).

Algoritmul e din lucrarea **Hu, Koren & Volinsky, 2008** (folosit la Netflix-era de aur a
factorizării de matrice). **L-am implementat manual** cu numpy + scipy.sparse, pentru că
biblioteca planificată inițial (LightFM) nu se instala pe Python 3.12/Windows (cerea compilator
C). Avantaj real: îl pot explica și controla linie cu linie.

## Pas 6: coșul anonim — fold-in

La POS nu știu cine comandă. Am doar coșul curent. Trucul **fold-in**: tratez coșul ca pe un
„utilizator virtual" nou, îi calculez vectorul cu ACEEAȘI formulă exactă (un singur pas), apoi
scorez toate produsele și returnez top 3 — excluzând ce e deja în coș și produsele indisponibile.

## Pas 7: fallback-ul

Dacă modelul nu e încă antrenat sau coșul conține doar produse necunoscute → **recomand după
popularitate** (cele mai vândute). Sistemul NU dă eroare niciodată — „degradare elegantă".
Răspunsul conține câmpul `source`: `als` / `mixed` / `popularity` (m-a ajutat la depanare).

## Cifrele de evaluare (să le știi pe de rost)

Protocol: comenzile ordonate CRONOLOGIC, primele 80% antrenare, ultimele 20% test (563 comenzi).
Din fiecare comandă de test **ascund ultimul produs**, dau modelului restul coșului și verific
dacă produsul ascuns e în top-3 recomandări (leave-one-out).

| Metrică (la top-3) | Ce măsoară | **ALS** | Popularitate |
|---|---|---|---|
| HitRate@3 | în ce % din comenzi produsul ascuns e în top 3 | **0,316** | 0,270 |
| Precision@3 | câte din cele 3 sugestii sunt corecte (max 1/3 aici) | **0,105** | 0,090 |
| MAP@3 (Mean Average Precision) | premiază poziția: corect pe locul 1 > locul 3 | **0,275** | 0,160 |

Interpretare de spus: șansa pură ar fi 3/44 ≈ 0,068. ALS nimerește produsul exact în ~1 din 3
cazuri. Diferența cea mai mare e la MAP@3 → ALS pune produsul corect MAI SUS în listă, ceea ce
contează când afișezi doar 3 sugestii.

---

# PARTEA 7 — ML #2: Predicția cererii (gradient boosting) (15 min)

## Problema

Câte porții din fiecare produs se vor vinde în următoarele zile? Dacă știu asta + rețetele,
știu CE și CÂT să cumpăr → nu cumpăr în exces → mai puțină risipă. **Ăsta e obiectivul #1 al lucrării.**

## Transformarea cheie: din serie temporală → tabel de regresie

În loc de modele clasice de serii temporale, transform problema într-un tabel:
**fiecare rând = (produs, zi)**, ținta = porții vândute în acea zi.

```
 produs   zi        ziua_săpt  weekend  preț  vândut_ieri  vândut_acum_7z  medie_7z │ ȚINTA
 Pizza M  vin 12.06     5        1      38        4             6             4,3   │   7
 Pizza M  sâm 13.06     6        1      38        7             8             4,9   │   9
 Tiramisu vin 12.06     5        1      32        2             3             2,1   │   3
```

Două capcane evitate (astea impresionează, zi-le):

1. **Panel dens**: în baza de date există DOAR zilele cu vânzări. Zilele cu 0 vânzări lipsesc!
   Dacă antrenez pe date așa, modelul „crede" că nu există zile proaste → supraestimează tot.
   Fix: completez explicit cu 0 fiecare zi fără vânzări, pentru fiecare produs.
2. **Anti-leakage (anti-scurgere de informație)**: caracteristicile istorice (lag-uri = vânzările
   de acum 1/7/14 zile; medii mobile pe 7/28 zile) sunt calculate cu **shift(1)** = decalate cu o
   zi, ca media zilei să NU includă chiar ziua pe care o prezic. Fără asta, modelul „vede
   răspunsul" la antrenare → metrici frumoase dar FALSE în exploatare.

## Ce e gradient boosting, ca la grădiniță

O echipă de **arbori de decizie** construiți PE RÂND, în care fiecare arbore nou se antrenează
să corecteze **greșelile rămase** de la cei dinainte:

```
  arbore 1: prezice grosier          → erori mari
  arbore 2: învață să prezică ERORILE arborelui 1 → le corectează parțial
  arbore 3: corectează ce-a rămas    → ...
  ...
  predicția finală = suma contribuțiilor tuturor arborilor
```

Folosesc **HGBR (HistGradientBoostingRegressor)** din scikit-learn — varianta „histogram":
grupează valorile în intervale (bins) → antrenare rapidă. **UN SINGUR model pentru toate
produsele** (identitatea și categoria produsului sunt caracteristici de intrare) — nu 44 de
modele de întreținut.

**De ce nu Prophet/SARIMA (modele clasice de serii temporale)?** Prophet = dependență fragilă,
greu de instalat în container + câte un model per produs. SARIMA = reglare manuală per serie →
zeci de modele. HGBR: un model, e în scikit-learn (deja în proiect), tratează nativ valori lipsă.

## Predicția pe mai multe zile: recursiv

Ziua 1 o prezic din date reale. Pentru ziua 2, lag-ul de ieri = **predicția zilei 1** (n-am
realitate din viitor). Și tot așa. Erorile se acumulează cu orizontul → am limitat la max 30 zile.

## De la predicție la stoc (stock_planner)

`porții prezise × rețete = necesar de ingrediente` + estimare `days_until_stockout` (în câte
zile se termină fiecare ingredient în ritmul prezis). Asta vede managerul în dashboard: o listă
de cumpărături bazată pe viitorul estimat, nu doar pe trecut. **Stocul devine anticipativ, nu reactiv.**

## Cifrele de evaluare (pe de rost)

Test = ultimele 28 de zile (1 232 observații produs×zi). Metrici de EROARE (mai mic = mai bine):
**MAE (Mean Absolute Error)** = eroarea medie în porții; **RMSE (Root Mean Squared Error)** =
penalizează mai tare greșelile mari.

| Model | MAE | RMSE |
|---|---|---|
| **HGBR (al meu)** | **1,805** | **2,477** |
| lag-7 („cât s-a vândut aceeași zi săpt. trecută") | 2,235 | 3,279 |
| medie-7 (media ultimelor 7 zile) | 1,838 | 2,519 |

Interpretarea ONESTĂ (comisiei îi place onestitatea): față de regula naivă lag-7, câștig clar.
Față de media-7, câștigul la MAE e mic (1,805 vs 1,838) — o medie simplă e un competitor serios
pe volumul meu de date. Dar la RMSE diferența crește → modelul greșește mai puțin exact în
**zilele atipice** (vârfurile de weekend), unde media netezită ratează. Practic: eroare medie
sub 2 porții/produs/zi — suficient pentru planificarea aprovizionării.

## Datele de antrenare (întrebare garantată: „de unde ai date?")

Un restaurant nou nu are istoric → am scris un **generator de comenzi sintetice realiste**
(`generate_history.py`): 180 de zile, 3 215 comenzi, 9 070 linii, 44 produse. Reproduce tipare
reale: volum mai mare vineri–duminică (multiplicatori 1,3–1,5), distribuție Zipf a popularității
(puține produse foarte cerute, coadă lungă rar cerută), 8 perechi de produse care apar des
împreună, clienți cu preferințe persistente. **SEED=42** → totul reproductibil (aceeași rulare →
aceleași cifre). Limitarea (o recunoști TU primul): generatorul conține tiparele pe care modelele
le caută → experimentul arată că modelele POT recupera tiparele, nu că ele există garantat în
orice restaurant real.

---

# PARTEA 8 — Securitate + Docker (10 min)

## Securitate — ce ai făcut concret (audit + 4 remedieri)

1. **Bonul PDF era public** (oricine cu ID-ul comenzii vedea nume/telefon/adresă) →
   restrâns la roluri autorizate + clientul își poate lua DOAR bonul lui; descărcarea se face cu
   token în header (fetch + blob), nu prin URL simplu (un URL simplu poate fi copiat oriunde).
2. **Ownership check (verificare de proprietate)**: clientul autentificat care cere
   `GET /api/orders/{id}` trebuie să FIE proprietarul comenzii — altfel putea citi comenzile
   altora ghicind ID-uri secvențiale (1, 2, 3...).
3. **Confirm-delivery** — același ownership check.
4. **Username enumeration**: mesaje de eroare DIFERITE la login („user inexistent" vs „parolă
   greșită") permit unui atacator să afle ce conturi există. Fix: un singur mesaj generic
   „Credentiale incorecte." indiferent ce a fost greșit.

Alte măsuri: BCrypt pentru parole, JWT cu roluri pe fiecare endpoint, cheia Stripe în variabilă
de mediu (nu în cod), verificarea plății direct cu Stripe (nu din parametrii URL).

## Docker în 5 fraze

**Container** = pachet izolat care conține aplicația + TOATE dependențele ei (runtime, biblioteci),
și rulează identic pe orice mașină. NU e mașină virtuală (nu duce un sistem de operare întreg —
împarte kernelul gazdei → pornește în secunde, consumă puțin). **Imagine** = șablonul; container =
instanța care rulează. **Dockerfile** = rețeta de construire a imaginii (ex: „ia node:22,
copiază codul, npm build, servește cu nginx"). **docker-compose.yml** = fișierul care descrie
toate cele 8 servicii + rețeaua dintre ele + ordinea de pornire: `docker compose up` pornește tot.
**Healthcheck** = testul „ești sănătos?" — API-ul așteaptă ca PostgreSQL să fie healthy înainte
să ruleze migrările (altfel ar crăpa la pornire încercând să se conecteze prea devreme).

---

# PARTEA 9 — Fișa cu cifre de memorat (5 min, recitește-o și înainte să intri)

| Ce | Valoarea |
|---|---|
| Servicii containerizate | **8** (4 React + API + PostgreSQL + RabbitMQ + ML) |
| Porturi | POS 3000 · Kitchen 3001 · Dashboard 3002 · Client 3003 · API **8081** · ML 8001 |
| Roluri | Admin, Kitchen, Waiter, Client |
| Date sintetice | **180 zile · 3 215 comenzi · 9 070 linii · 44 produse** · SEED=42 |
| Split evaluare | temporal 80/20; recomandări: 2 249 train / 563 test; forecast: test = ultimele 28 zile |
| ALS parametri | k=**32** factori, α=**40**, λ=**0.1**, **15** iterații |
| ALS vs popularitate | HitRate@3: **0,316** vs 0,270 · MAP@3: **0,275** vs 0,160 |
| HGBR vs baselines | MAE **1,805** vs 2,235 (lag-7) / 1,838 (medie-7) |
| Retrain ML | buffer: **10 comenzi SAU 10 minute** |
| Ciclu comandă | Pending → InProgress → Ready → Served (Cancelled terminal) |
| Anul algoritmului ALS | Hu, Koren & Volinsky, **2008** |

---

# PARTEA 10 — Întrebările probabile ale comisiei + răspunsuri scurte

> Regulă: răspuns în 2–4 fraze, întâi ideea, apoi UN detaliu concret. Nu turui.

## Despre arhitectură

**„De ce microservicii pentru un singur restaurant? Nu e overkill?"**
Parțial da, și o recunosc — un monolit ar fi mers. Dar separarea mi-a fost impusă practic de
limbaje (C# pentru business, Python pentru ML — nu conviețuiesc într-un proces) și mi-a adus
izolarea defectelor: ML-ul poate cădea fără să oprească comenzile. Costul operațional l-am
plătit o singură dată, cu Docker Compose.

**„De ce C# ȘI Python în același proiect?"**
C# pentru API: tipare puternică, EF Core matur, îl cunoșteam bine. Python exclusiv pentru ML:
numpy/scikit-learn nu au echivalent viabil în C#. Comunică prin RabbitMQ, complet decuplate.

**„De ce RabbitMQ și nu un apel HTTP direct între API și ML?"**
Trei motive: API-ul nu blochează la plasarea comenzii (asincron); mesajele nu se pierd dacă ML-ul
e căzut (coada le ține); fanout îmi permite consumatori noi fără să modific API-ul.

**„Ce se întâmplă dacă RabbitMQ însuși cade?"**
Comenzile continuă normal — publish-ul e best-effort, iar recomandările/forecastul funcționează
cu modelul deja antrenat în memorie. Se pierde doar reantrenarea incrementală până revine
brokerul; la următorul retrain se citește oricum tot istoricul din PostgreSQL, deci nimic nu
rămâne pierdut definitiv.

**„Diferența dintre SignalR și RabbitMQ? Ambele-s «mesaje»..."**
Direcția și destinatarul: SignalR = server → BROWSERE (oameni care se uită la ecran, timp real);
RabbitMQ = serviciu → SERVICIU (evenimente între programe, asincron, cu persistență în coadă).
Push rapid către oameni vs. scrisori garantate între mașini.

**„Cum scalează sistemul?"**
API-ul e stateless (JWT, fără sesiuni) → pot rula N copii în spatele unui load balancer.
PostgreSQL: read replicas pentru rapoarte. RabbitMQ: clustering. ML: serviciu independent,
înlocuibil fără să ating restul. Pentru SignalR pe mai multe instanțe aș adăuga un backplane
(ex. Redis) — nu a fost necesar la scara demo-ului.

**„De ce PostgreSQL și nu MongoDB?"**
Datele sunt intrinsec relaționale (comandă→linii→produse→rețete→ingrediente); am nevoie de chei
străine și tranzacții. Un document store nu mi-ar aduce nimic și aș pierde garanțiile de integritate.

## Despre backend / securitate

**„Ce e un JWT și de ce nu sesiuni clasice?"**
Un token semnat criptografic ce conține identitatea și rolul; serverul validează semnătura la
fiecare cerere, fără să țină stare. Avantaj: orice instanță a API-ului validează orice token →
scalare orizontală simplă.

**„Autentificare vs autorizare?"**
Autentificare = CINE ești (login, token). Autorizare = CE AI VOIE (rolul din token vs.
`[Authorize(Roles=...)]` pe endpoint + verificările de ownership).

**„Cum stochezi parolele?"**
Niciodată în clar — hash BCrypt cu salt. La login compar hash-ul; BCrypt e intenționat lent,
ceea ce face brute-force-ul scump.

**„Ce vulnerabilități ai găsit la audit?"**
Patru: bon PDF accesibil anonim (date personale), lipsa verificării de proprietate la citirea
comenzii și la confirmarea livrării (ID-uri secvențiale ghicibile), și username enumeration la
login. Toate remediate; le pot detalia pe oricare.

**„De ce POST /api/orders e public (fără autentificare)?"**
Decizie asumată: clienții pot comanda fără cont (ca în realitate). Riscul de comenzi false există
și l-am documentat ca risc acceptat pentru demo; în producție aș adăuga rate limiting/CAPTCHA.

## Despre ML — recomandări

**„De ce ALS și nu o rețea neuronală / ceva mai modern?"**
Volumul de date e mic (mii de comenzi, 44 produse) — o rețea ar fi supradimensionată și greu de
justificat. ALS e metoda consacrată exact pentru feedback implicit, e explicabilă, se antrenează
în secunde și am putut-o implementa și controla integral. Întâi baseline-uri solide, apoi
complexitate — și evaluarea arată că și față de popularitate câștigul există dar e moderat.

**„De ce ai implementat ALS manual și n-ai luat o bibliotecă?"**
Inițial am ales LightFM, dar nu se instala pe Python 3.12/Windows (cere compilator C). Am
implementat formularea Hu–Koren–Volinsky cu numpy/scipy.sparse. Bonus real: pot explica fiecare
linie — nu e cutie neagră.

**„Ce înseamnă că feedback-ul e implicit și de ce contează?"**
Nu am note explicite, doar comportament observat (a comandat/nu). Absența nu e respingere —
poate nu a văzut produsul. De asta modelul folosește preferință binară + ÎNCREDERE proporțională
cu numărul de comenzi, în loc să trateze zerourile ca note negative.

**„Cum recomanzi pentru cineva complet nou (cold start)?"**
Coș anonim cunoscut → fold-in (utilizator virtual din coș). Coș gol sau produse necunoscute →
fallback pe popularitate. Sistemul răspunde întotdeauna ceva rezonabil.

**„Ce e HitRate@3 / MAP@3?"**
Ascund ultimul produs din fiecare comandă de test și întreb modelul de top-3. HitRate@3 = în ce
procent din comenzi produsul ascuns apare în cele 3. MAP@3 ține cont și de poziție — corect pe
locul 1 valorează mai mult decât pe locul 3. ALS: 0,316 respectiv 0,275, ambele peste popularitate.

## Despre ML — forecasting

**„De ce nu ARIMA/Prophet, care sunt făcute pentru serii temporale?"**
Ar fi însemnat câte un model per produs (44 de modele de reglat și întreținut) plus dependențe
fragile în container. Reformulând ca regresie tabelară, un singur HGBR învață toate produsele,
iar sezonalitatea intră prin caracteristici (ziua săptămânii, lag-uri). Pentru volumul meu de
date, compromisul e favorabil — și l-am validat contra baseline-urilor.

**„Ce e data leakage și cum l-ai evitat?"**
Scurgere = modelul vede la antrenare informație care în realitate n-ar fi disponibilă la momentul
predicției. Concret la mine: media mobilă a unei zile nu are voie să includă chiar ziua respectivă
— totul e decalat cu shift(1). Plus split temporal, nu aleator: antrenez pe trecut, testez pe viitor.

**„Modelul tău abia bate media pe 7 zile. Merită?"**
Întrebare corectă — la MAE diferența e mică (1,805 vs 1,838), la RMSE crește (2,477 vs 2,519),
adică exact pe zilele atipice (weekend) modelul e mai bun, iar alea dor cel mai tare la
aprovizionare. Și rămâne net peste regula naivă lag-7. Dar da: pe date puține, baseline-urile
simple sunt competitive — de asta le-am și raportat, nu le-am ascuns.

**„Datele sunt sintetice — nu ți-ai «aranjat» singur rezultatele?"**
Recunosc limitarea explicit în lucrare: generatorul conține tiparele pe care modelele le caută,
deci experimentul demonstrează că modelele le POT recupera, nu că există în orice restaurant.
Validarea finală ar fi pe date reale de producție. Am ales sinteticul reproductibil (SEED=42)
pentru că un dataset public ar fi cerut altă schemă de date, iar restaurant real cu istoric nu am.

**„Cum se reantrenează modelul în timp?"**
Evenimentele de comandă vin prin RabbitMQ într-un buffer; la 10 comenzi noi sau 10 minute se
reantrenează complet din baza de date, iar modelul nou se schimbă printr-o atribuire atomică —
cererile în zbor nu văd niciodată un model parțial.

## Întrebări-capcană generale

**„Ce ai face diferit dacă ai lua-o de la capăt?"**
(Sinceritate controlată:) Aș scrie teste automate de la început — am prins prin testare manuală
bug-uri pe care un test le-ar fi prins gratis. Și aș defini de la început convenția de serializare
JSON între C# și Python — nepotrivirea PascalCase/camelCase m-a costat o seară de depanare.

**„Care a fost cea mai grea problemă tehnică?"**
Nu un algoritm, ci granițele dintre servicii: claim-urile JWT remapate silențios de ASP.NET
(eroarea apărea abia la citire, nu la validare), CORS + SignalR (AllowCredentials incompatibil cu
AllowAnyOrigin), PascalCase vs camelCase între C# și Python. Într-un monolit, niciuna n-ar fi
existat — e prețul real al arhitecturii distribuite, și lecția principală a proiectului.

**„Cât e cod scris de tine vs. generat/preluat?"**
Arhitectura, deciziile, modelarea și implementarea îmi aparțin; am folosit AI ca asistent la
depanare, la generatorul de date sintetice și la rafinarea unor texte — totul declarat explicit
în lucrare, în declarația privind utilizarea instrumentelor AI. Pot explica orice linie din proiect.

**„Sistemul e gata de producție?"**
E un prototip funcțional complet, nu un produs: i-ar lipsi CORS restrâns pe domenii reale, HTTPS,
rate limiting, backup-uri, monitorizare, teste automate și integrarea fiscală. Arhitectura însă
e gândită să suporte pașii ăștia fără rescriere.

---

## Ultimele 15 minute înainte să intri

1. Recitește **PARTEA 9** (fișa cu cifre).
2. Spune-ți în minte, o dată, povestea din **PARTEA 5** (fluxul comenzii) — e coloana vertebrală.
3. Respiră. Tu ai construit sistemul ăsta; comisia îl vede 15 minute, tu ai trăit în el luni.
   La orice întrebare la care nu știi perfect: spui ce ȘTII sigur + cum ai afla restul. E un
   răspuns de inginer, și exact asta caută.

Succes! 🎓
