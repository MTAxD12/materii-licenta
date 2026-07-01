# SINTEZĂ COMPLETĂ — TEHNOLOGII WEB
> Acoperă toate cele 10 cursuri (web01–web11, lipsește web07 din materiale).
> Organizat după frecvența la examen (⭐). Lângă fiecare capitol, întrebările din anul trecut.

> **Notă:** Cursurile sunt slide-uri cu multe imagini. Tot ce e mai jos provine din cursuri.
> **Excepție importantă:** cursul **web07 lipsește complet** din materiale (secvența sare web06→web08).
> Acesta e aproape sigur cursul de **HTML/CSS** — de aceea *Cascade CSS* (Q11) și *Responsive Design* (Q12)
> nu apar în cele 10 cursuri disponibile. Capitolul 8 le acoperă cu cunoștințe standard W3C/MDN;
> recuperează web07 pentru exemplele specifice profesorului.

---

## CUPRINS (după prioritatea la examen)
> Frecvențele includ acum întrebări din **2021, 2022, 2023, 2024**. "×N ani" = de câte ori a apărut tema.

| # | Temă | Recurență | Prioritate |
|---|------|-----------|------------|
| 1 | [Arhitectura Web: server, proxy, gateway, app server + straturi](#1-arhitectura-web-) | confirmat în fiecare an (×4) | ⭐⭐⭐⭐ |
| 2 | [HTTP, MIME/Media Types, headere, Ajax](#2-http-mime-headere-ajax-) | recurent | ⭐⭐⭐ |
| 3 | [Cookie-uri, sesiuni, stocare persistentă + securitate](#3-cookie-uri-sesiuni-stocare-) | recurent | ⭐⭐⭐ |
| 4 | [Servicii Web, SOA, Microservicii, REST, MVC](#4-servicii-web-soa-microservicii-rest-mvc-) | **SOA vs MVC apare în fiecare an (×4+)** | ⭐⭐⭐⭐⭐ |
| 5 | [DOM, parsere, well-formed vs valid, XML](#5-dom-parsere-xml-) | **DOM well-formed vs valid în fiecare an (×4)** | ⭐⭐⭐⭐ |
| 6 | [Autentificare și autorizare în APIs](#6-autentificare-și-autorizare-în-apis-) | recurent | ⭐⭐ |
| 7 | [JSON vs HTML](#7-json-vs-html-) | apare (atenție: și XML vs JSON) | ⭐⭐ |
| 8 | [HTML, Cascade CSS și Responsive Design](#8-html-cascade-css-și-responsive-design-) | CSS cascade + responsive recurent | ⭐⭐ |
| 9 | [Subiecte recurente 2021–2024 (template engines, static/dinamic, GET/POST, DRY, poliglot, SOAP/REST, design patterns web...)](#9-subiecte-recurente-din-anii-trecuți-20212024-) | multe ×3 ani | ⭐⭐⭐ |
| — | [Anexe: URI, hypertext, PHP, SPA/PWA, mashup, CORS, JAMstack, GraphQL](#anexe) | context | — |

> **Top recurente confirmate (toți anii):** SOA vs MVC; DOM (well-formed vs valid); server/proxy/gateway/app server; servicii web vs microservicii; conținut static vs dinamic; template engines; design patterns în web; DRY; mai multe limbaje în aplicații mari.

---

## 1. Arhitectura Web ⭐⭐⭐
> Întrebări: *"Ce este un server web, ce implică acesta"*, *"Asemănări și deosebiri între server web, proxy web, gateway web, server de aplicații web"*, *"(Dez)avantajele arhitecturii pe straturi (N-tier, layered)"*.

### Ce este Web-ul

**WWW (World Wide Web)** = un **serviciu al Internetului** (WWW ≠ Internet); un spațiu informațional comun în care comunicăm interconectând și partajând **resurse**. Inventat de **Tim Berners-Lee** (CERN, 1989).

Web-ul se bazează pe:
- **modelul client/server** (request/response);
- **hipertext / hipermedia** (orice se poate lega de orice);
- **3 piloni:** identificare (URI), interacțiune (HTTP), reprezentare (formate de date: HTML, XML, JSON, RDF...).

> **Resursă** = orice "lucru" cu o identitate. **Reprezentarea** = forma concretă (HTML/XML/JSON...) prin care accesăm conținutul resursei, dată de un format deschis. Aceeași resursă (un singur URI) poate avea **mai multe reprezentări**.

### Site Web vs Aplicație Web

- **Site Web:** un sistem pe care rulează un server web, găzduind pagini (resurse) înrudite.
- **Aplicație Web:** colecție de pagini interconectate cu **conținut generat dinamic**, oferind o funcționalitate specifică, prin interfață Web. (De obicei site = aplicație.)

> **Web app = Interfață + Program + Conținut (Date)** — toate trei sunt importante (mit 1/2/3 = e doar interfața/programul/datele).

### Arhitectura generală a unei aplicații Web

```
        FRONT-END                              BACK-END
┌──────────────────────┐   HTTP        ┌──────────────────────────┐
│  Web client (browser)│ ◄──────────►  │  Web server              │
│  JavaScript          │  transfer     │  + app server / framework│
│  HTML, CSS           │  (a)sincron   │                          │
└──────────────────────┘               │   ├─ conținut static     │
                                        │   └─ conținut dinamic    │
                                        └───────┬──────────────────┘
                                                │
                                  ┌─────────────┴───────────┐
                                  │ date locale   date externe│
                                  │               (Web service)│
                                  └────────────────────────────┘
```
Prin interfața Web, utilizatorul interacționează cu **clientul (front-end)** și inițiază acțiuni (cereri HTTP (a)sincrone) executate de componente la nivel de **server (back-end)**.

### Ce este un SERVER WEB

Un **server web** este un program (daemon) care **îndeplinește cereri HTTP** de la clienți, întorcând reprezentări de resurse.
- Exemple: **Apache HTTP Server, NGINX, Lighttpd, Microsoft IIS, Eclipse Jetty, Gunicorn**.
- **Stateless**: fiecare cerere e considerată independentă de celelalte, chiar dacă vine de la același client (starea conexiunii NU este păstrată).

**Arhitecturi de implementare a serverului web:**
```
1. Pre-forked / pre-threaded (ex: Apache)
   La pornire se creează procese/fire copil; fiecare tratează un client.
   Limitare: nr. de fire e limitat → cererile simultane nu pot fi toate servite
   (operațiile I/O blocante "țin ocupat" firul).

2. Asincron / non-blocking, single-threaded (ex: NGINX, Node.js, Deno)
   Un singur fir tratează multe cereri prin I/O non-blocant → scalabilitate mai bună.
```

### Intermediari HTTP: Proxy, Gateway, Tunnel (comparație — întrebare frecventă!)

Un **intermediar** stă între client și server. Trei tipuri:

```
CLIENT ──► [ PROXY ] ──► [ GATEWAY ] ──► SERVER ORIGINE
              ▲              ▲
       aproape de client   ascunde serverul țintă
```

| Termen | Rol | Detalii |
|--------|-----|---------|
| **Proxy** | Intermediar **lângă client**; are rol și de server și de client | **forward proxy** = pentru un grup de clienți (acționează în numele lor); **reverse proxy** = pentru un grup de servere |
| **Gateway** | Intermediar care **ascunde serverul țintă** (clientul nu știe de el) | Poate face: **load balancing** (distribuire trafic), **caching**, **traducere** mesaje (ex. HTTPS→HTTP), negociere/broker |
| **Tunnel** | Retransmite mesaje HTTP (de obicei **criptate**) | Context: HTTPS prin TLS (port 443), autentificare cu certificate digitale + criptare bidirecțională |
| **Cache** | Zonă de stocare locală (memorie/disc) a mesajelor | Server- și/sau client-side → cererile viitoare sunt servite mai rapid (performanță) |

> **Exemplu real:** Cloudflare = **reverse proxy** între browserul utilizatorului și site-ul găzduit pe serverul țintă (performanță + securitate + distribuție conținut).

### Server WEB vs Server de APLICAȚII Web (comparație)

| | Server Web | Server de aplicații Web |
|--|-----------|--------------------------|
| Rol | servește resurse (static + rutează cereri) | **generează dinamic** reprezentări, găzduiește logica aplicației |
| Exemple | Apache, NGINX, IIS | Zend (PHP), ASP.NET, Node.js, Play, Spring |
| Scop | comunicație HTTP | optimizarea dezvoltării aplicațiilor complexe; impune adesea o arhitectură (ex. MVC) |

"Ingrediente" tipice ale unei arhitecturi Web: client(i), firewall, proxy, middleware, server(e) web, server(e) de aplicații, framework-uri/biblioteci, server(e) de stocare persistentă (baze de date), server(e) de conținut multimedia, CMS/wiki, sisteme legacy, eventual servicii **cloud**.

### Arhitecturi pe straturi (N-tier / layered) — (dez)avantaje

Principiul de bază: **separation of concerns** (separarea responsabilităților) — modelul datelor e separat de modelul de procesare (business logic) și de modelul de prezentare (interfață).

**Aplicație pe 3 niveluri (3-tier):**
```
┌──────────┐   ┌────────────────────┐   ┌──────────────┐
│  CLIENT  │──►│ APPLICATION SERVER │──►│   STORAGE    │
│(interfață)│   │   (aplicație)      │   │ (persistență)│
└──────────┘   └────────────────────┘   └──────────────┘
 prezentare        logica de business        date
```

**Principiu de design: layers of isolation** — modificările dintr-un strat **nu afectează** componentele din alt strat; fiecare strat oferă servicii doar straturilor vecine; un strat nu "vede" un strat ne-vecin; straturile pot încapsula ("ascunde") sisteme legacy (black box).

| Avantaje | Dezavantaje |
|----------|-------------|
| Izolarea modificărilor (un strat se schimbă fără să rupă altul) | Overhead de performanță (cererea trece prin mai multe straturi) |
| Reutilizare și mentenanță mai ușoară | Complexitate suplimentară |
| Testare independentă pe straturi | Poate deveni rigid dacă straturile sunt prea cuplate |
| Securitate (straturi de izolare, ex. firewall, gateway) | Risc de "spaghetti" dacă nu se respectă separarea |

> În REST, aplicația adoptă explicit un **layered system** (client → firewall → gateway → load balancer → server), unul dintre constrângerile REST (vezi cap. 4).

---

## 2. HTTP, MIME, headere, Ajax ⭐⭐⭐
> Întrebări: *"Media Types – MIME și Ajax"*, *"Flow-ul/legătura dintre MIME types, header-ele HTTP și Ajax + 3 exemple de atribute din header request"*.

### HTTP — protocolul

**HTTP (HyperText Transfer Protocol)** = protocol **request/response** fiabil, pe stiva TCP/IP, la nivelul **aplicație**, port standard **80** (HTTPS: 443).

```
Nivel aplicație:   HTTP  (transfer hipertext/hipermedia)
Nivel transport:   TCP   (transport fiabil prin socket-uri)
Nivel rețea:       IP    (interconectare + rutare)
Nivel acces:       MAC   (control acces la mediu)
```

| Versiune | An | Caracteristici |
|----------|-----|----------------|
| HTTP/1.1 | 1999 | standard clasic (RFC 9110-9112) |
| HTTP/2 | 2022 | mesaje **binare**, reutilizare conexiune TCP, **multiplexare** (stream-uri paralele), server push, compresie headere (HPACK) |
| HTTP/3 | — | HTTP peste **QUIC** (UDP), multiplexare, criptare, conectare rapidă |

### Mesaj HTTP = Header + Body

```
Header: câmpuri de forma   field-name ":" [ field-value ] CRLF

CERERE:   Method  Request-URI  ProtocolVersion CRLF
          [ Message-header ]
          [ CRLF  MIME-data ]

RĂSPUNS:  HTTP-version  Cod(3 cifre)  Reason CRLF
          Content
```

### Metode HTTP

| Metodă | Rol | Safe? | Idempotentă? |
|--------|-----|-------|--------------|
| **GET** | accesează reprezentarea unei resurse | ✓ | ✓ |
| **HEAD** | ca GET, dar doar **meta-date** (ex. MIME type, ultima modificare) | ✓ | ✓ |
| **PUT** | actualizează (sau creează) reprezentarea unei resurse | ✗ | ✓ |
| **POST** | creează o resursă, trimite entități (ex. date din formular) | ✗ | ✗ |
| **DELETE** | șterge o resursă | ✗ | ✓ |

- **Safe** = nu modifică starea serverului (fără side-effects). GET, HEAD sunt safe.
- **Idempotentă** = apeluri repetate dau același rezultat. GET, HEAD, PUT, DELETE sunt idempotente; POST nu.
- Tradițional, browserul permite doar **GET** și **POST**.

### Coduri de stare HTTP

| Clasă | Exemple |
|-------|---------|
| **1xx** Informational | 100 Continue, 101 Switching Protocols |
| **2xx** Success | 200 OK, 201 Created, 204 No Content, 206 Partial Content |
| **3xx** Redirection | 301 Moved Permanently, 302 Found, 303 See Other, 304 Not Modified |
| **4xx** Client Error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 405 Method Not Allowed, 429 Too Many Requests |
| **5xx** Server Error | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable |

### MIME / Media Types (Content-Type)

Tipul conținutului unei resurse e dat de **MIME (Multipurpose Internet Mail Extensions) / Media Types**, NU de extensia fișierului. Sintaxa: `Content-Type: type/subtype`.

```
text/       text/plain, text/html, text/css, text/csv, text/markdown
image/      image/gif, image/jpeg, image/png, image/webp, image/svg+xml
audio/      audio/mpeg (MP3), audio/ac3
video/      video/av1, video/h265, video/VP8
application/ application/json, application/javascript, application/pdf,
            application/zip, application/octet-stream, application/xml
multipart/  multipart/mixed, multipart/alternative
model/      model/x3d+xml
font/       font/woff2
```
> Lista oficială: IANA Media Types. **URI-urile sunt opace** — tipul resursei e dat de MIME-ul indicat de server, nu de extensie.

### Câmpuri (atribute) din header

**Atribute din header de CERERE (request)** — exemple cerute la examen:
```
Host:            adresa țintă (IP/domeniu) a mașinii care servește resursa
User-Agent:      info despre client (browser)
Accept:          tipurile de conținut acceptate (content negotiation)
Accept-Language: limbile acceptate
Accept-Encoding: codări/compresii acceptate (gzip, br, zstd)
Referer:         URI-ul resursei care a trimis la resursa curentă
Cookie:          cookie-urile trimise înapoi serverului
Authorization:   date de autentificare/autorizare
```
> Pentru întrebare: **3 atribute din header request** → ex. `Host`, `User-Agent`, `Accept` (sau `Referer`, `Accept-Language`, `Cookie`).

**Atribute din header de RĂSPUNS (response):**
```
Content-Type:     tipul MIME al reprezentării (ex. text/html; charset=UTF-8)
Content-Length:   dimensiunea în bytes
Content-Encoding: compresia folosită (gzip)
Location:         redirectare către altă reprezentare (HTTP redirect)
Set-Cookie:       setează un cookie la client
Last-Modified:    ultima modificare
Cache-Control, Expires, ETag, Age:  control cache
```

### Ajax — și legătura MIME ↔ headere ↔ Ajax (întrebare cheie!)

**Ajax (Asynchronous JavaScript and XML)** = transfer **asincron** de date între documentul HTML din browser și o aplicație pe server, **fără reîncărcarea completă a paginii**. Se face prin obiectul **XMLHttpRequest** sau **Fetch API**.

**Flow-ul MIME → headere HTTP → Ajax:**
```
1. Programul JS (în browser) creează un XMLHttpRequest / fetch()
   și deschide o cerere HTTP (open) către server.

2. Cererea poartă HEADERE HTTP — ex. Accept: application/json
   (clientul cere un anumit MIME type).

3. Serverul răspunde cu o reprezentare + header Content-Type
   care indică MIME type-ul datelor (ex. application/json, text/xml).

4. JS verifică status (200, 404...) și Content-Type, apoi
   PROCESEAZĂ datele (JSON / XML via DOM) și modifică pagina cu DOM
   — fără reload.
```
Astfel: **Ajax** folosește **headerele HTTP** (Accept / Content-Type) pentru a negocia **MIME type-ul** datelor schimbate (JSON, XML, HTML, text).

**XMLHttpRequest — esențial:**
```
Metode:    open(), send(), abort(), setRequestHeader(),
           getResponseHeader(), getAllResponseHeaders()
Proprietăți: readyState (0..4: UNSENT→OPENED→HEADERS_RECEIVED→LOADING→DONE),
           status (200, 404...), statusText, responseText, responseXML,
           onreadystatechange (funcția apelată la fiecare schimbare de stare)
```
- Datele Ajax pot fi: **JSON**, dialecte **XML** (RSS, KML), **HTML**, text, CSV.
- **Fetch API** (modern) e bazat pe **promise** (`.then()` / `.catch()`) — o operație asincronă ce se va rezolva în viitor.
- Ajax e premisa pentru invocarea asincronă a serviciilor REST.
- Alternative HTML5: **Server-Sent Events**, **WebSockets**.

---

## 3. Cookie-uri, sesiuni, stocare ⭐⭐⭐
> Întrebări: *"Tipurile de stocare persistentă într-o aplicație web"*, *"Securitatea în cookie-uri și sesiuni"*, *"Ce înseamnă pentru securitate folosirea de cookie-uri și sesiuni web. Exemple."*

### Problema: HTTP este stateless

HTTP **nu păstrează starea** — nu știe dacă mai multe cereri succesive vin de la același client. De aceea avem nevoie de mecanisme de stocare/identificare.

### Cookie-uri

Un **cookie** = mod (cvasi-)persistent de a stoca date pe mașina clientului, pentru a fi accesate ulterior de un program de pe server. Un script de pe server pune date la client (prin browser); browserul le trimite înapoi aceluiași script de pe același server.

- Un cookie = o "variabilă" `name=value` (valoarea = string URL-encoded). **Max 4KB.**
- Tipuri:
  - **persistent** — salvat într-un fișier; durata de viață setată de creator;
  - **non-persistent (volatil)** — dispare la închiderea browserului.
- **1st party** (creat de domeniul vizitat) vs **3rd party** (creat de aplicații externe — probleme de confidențialitate).

**Cum se transmite un cookie:**
```
Server → Client:  în header de RĂSPUNS:  Set-Cookie: color=green
Client → Server:  în header de CERERE:   Cookie: name1=value1; name2=value2
(doar dacă sunt îndeplinite condițiile de validitate)
```

**Atribute (opțiuni) ale unui cookie:**
| Atribut | Rol |
|---------|-----|
| `expires` | data/ora de expirare (clientul șterge cookie-urile expirate) |
| `domain` | numele serverului care a generat cookie-ul |
| `path` | submulțime de URL-uri din domeniu (distinge aplicații pe același server) |
| `secure` | cookie trimis înapoi **doar pe canal securizat (HTTPS)** |
| `httpOnly` | cookie accesibil **doar prin HTTP**, NU din JS (browser) — protecție XSS |
| `sameSite` | dacă valoarea trece în cereri cross-site: **Strict / Lax / None** |

Cookie-ul se trimite înapoi doar dacă se potrivesc: domeniu, path (virtual), timp de expirare, securitatea canalului.

**Aplicații cookie-uri:** preferințe utilizator (temă, limbă), completare automată formulare, monitorizare acces (Web analytics, user tracking → inițiativa Do Not Track), stocare info autentificare, stare tranzacție (coș cumpărături), management sesiuni Web.

### Sesiuni Web

Fiecare vizitator primește un identificator unic **SID (Session ID)**, stocat de obicei într-un **cookie** (nume implicit specific platformei: `PHPSESSID`, `JSESSIONID`, `ASP.NET_SessionId`...).
```
Server → Set-Cookie: sid=7343
La cererile următoare, clientul trimite Cookie: sid=7343
→ serverul recunoaște vizitele consecutive ale aceluiași utilizator
```
- Alternativă (nerecomandată): SID propagat prin URL.
- Variabilele de sesiune se păstrează între cereri înrudite ale aceleiași instanțe de client.
- Info de sesiune se stochează **persistent pe server**: în fișiere (cel mai des) sau în baze NoSQL (Redis, Memcached, DynamoDB).
- La CGI, managementul sesiunii e implementat integral de programator (nu există mod standard).

### Securitatea cookie-urilor și sesiunilor (întrebare frecventă!)

```
RISCURI:
• Cookie-urile 3rd party → urmărire/confidențialitate (privacy)
• Cookie accesibil din JS → poate fi furat prin XSS (cross-site scripting)
• Cookie trimis pe canal necriptat → poate fi interceptat
• SID furat → deturnarea sesiunii (session hijacking)

MĂSURI DE SECURITATE (din curs):
• secure   → cookie-ul circulă DOAR pe HTTPS (canal criptat)
• httpOnly → cookie-ul NU poate fi citit de programe client (JS) → contra XSS
• sameSite → Strict/Lax/None controlează cererile cross-site → contra CSRF
• stocarea info de sesiune pe SERVER (în cookie stă doar SID-ul)
• ștergerea cookie-ului = anularea valorii și a timpului
```
> **Exemplu de securitate (din curs):** la autentificarea webmail, răspunsul setează
> `Set-Cookie: roundcube_sessid=...; path=/; secure; HttpOnly` — cookie-ul de sesiune
> e marcat `secure` (doar HTTPS) și `HttpOnly` (inaccesibil din JS).

### Tipuri de stocare persistentă într-o aplicație Web (întrebare!)

```
LA CLIENT (browser):
• Cookie-uri          (max 4KB, trimise la fiecare cerere)
• Web Storage         localStorage / sessionStorage (perechi cheie-valoare)
• IndexedDB           stocare de obiecte, API asincron în browser

LA SERVER:
• Fișiere             (ex. sesiuni stocate în fișiere)
• Baze de date relaționale (SQL): MySQL/MariaDB, PostgreSQL, Oracle, SQLite
• Baze NoSQL          MongoDB, Cassandra, Redis, Neo4j, DynamoDB
• Modele arborescente XML (semi-structurat); RDF
```
> **Web Storage** (HTML Living Standard): `sessionStorage` și `localStorage` — liste de perechi cheie-valoare la nivel de browser. **IndexedDB** — bază de date orientată obiect, API asincron.

---

## 4. Servicii Web, SOA, Microservicii, REST, MVC ⭐⭐⭐
> Întrebări: *"Conexiunile dintre hipermedia, URI, REST și teoria automatelor"*, *"Servicii Web vs Microservicii, asemănări/deosebiri"*, *"(Dez)avantaje MVC"*, *"Interdependența între SOA și MVC"*.

### Ce este un serviciu Web

> "Un serviciu Web este un sistem software proiectat să suporte interacțiunea **interoperabilă mașină-mașină** printr-o rețea." (W3C)

- Software folosit **de la distanță** de alte aplicații/servicii, oferind o funcționalitate specifică, de obicei prin **API**.
- Implementarea **nu trebuie cunoscută** de programatorul care îl invocă (**black box**, **loosely-coupled**).
- Accesat standardizat: resurse adresate cu **URI**, transfer prin **HTTP**, formate **CSV/JSON/XML**.
- Exemple: Google Maps, Instagram, Discord, Amazon S3, IBM Watson.

**De ce servicii Web?** Soluții multi-platformă, slab cuplate, care permit integrarea în timp real a aplicațiilor; spre deosebire de un program "strâns cuplat" de aplicația-părinte (ex. spell-checker care nu poate fi reutilizat în alt context), serviciile fac specificațiile **explicite** (date de intrare/ieșire validate cu JSON Schema / XML Schema).

> Obținerea răspunsului dintr-un HTML generat de o aplicație clasică se face prin **Web scraping** (Beautiful Soup, jsoup, Scrapy...) — dar orice schimbare de markup rupe programul. Serviciile Web rezolvă asta.

### SOA — Service Oriented Architecture

**Stil arhitectural** pentru aplicații considerate **servicii** ce pot fi invocate de alte aplicații.
- Resursele sunt disponibile printr-o suită de **servicii independente** (black box).
- Componentele au **grad mare de independență** (de-coupling); serviciile interacționează fără dependențe între ele.
- Serviciile partajează un **contract formal** (interfață: operații oferite, mod de schimb de date, descoperire).
- Servicii **componibile/orchestrabile**, **reutilizabile**, **stateless**.

```
Abordare clasică: aplicația Web expune funcționalități clienților,
pe baza datelor stocate persistent (de obicei arhitectură MV*).

SOA: fiecare funcționalitate = un serviciu independent,
care poate folosi alte servicii (interne) și diverse sisteme de stocare.
```

**Orchestrare vs Coregrafie:**
| Orchestrare | Coregrafie |
|-------------|------------|
| Există un **serviciu principal (orchestrator)** care coordonează apelurile și primește răspunsurile | Descriere **globală** a serviciilor autonome care participă, definind schimbul de mesaje și regulile de interacțiune |
| Punct de vedere **centralizat** | Abordare **decentralizată** |
| Standard: WS-BPEL | Web Services Choreography Description Language |

### REST — Representational State Transfer

**Stil arhitectural** (Roy Fielding, teza de doctorat 2000) pentru dezvoltarea aplicațiilor Web, **axat pe reprezentarea datelor**.

Concepte fundamentale:
- Procesarea duce la obținerea unei **reprezentări** a unei resurse.
- Reprezentarea are un format (text/binar): HTML, JSON, CSV, PNG, SVG, PDF — indicat prin **MIME type**.
- Fiecare reprezentare are un **URL** asociat; o resursă (un URI) poate avea **mai multe** reprezentări.
- Clienții interacționează cu reprezentările prin **verbe** (metode HTTP).

**Verbele REST și maparea CRUD:**
| Operație | SQL | HTTP |
|----------|-----|------|
| Create | INSERT | PUT / POST |
| Read (Retrieve) | SELECT | GET |
| Update | UPDATE | PUT / POST / PATCH |
| Delete | DELETE | DELETE |

- **GET** safe + idempotent; **PUT** idempotent, nu safe; **PATCH** update parțial (nici safe, nici idempotent); **POST** creează (nici safe, nici idempotent); **DELETE** idempotent.

**Constrângerile (principiile) REST:**
```
• Resurse identificate prin URI; reprezentări interconectate prin URL-uri
• Client-server
• Stateless server (fiecare cerere e independentă, conține tot ce-i trebuie)
• Cache (reprezentările pot fi stocate temporar)
• Layered system (client→firewall→gateway→load balancer→server)
• HATEOAS (Hypermedia As The Engine Of Application State)
```

### Conexiunea hipermedia ↔ URI ↔ REST ↔ teoria automatelor (întrebare cheie!)

```
O aplicație Web REST = AUTOMAT FINIT NEDETERMINIST

• STĂRILE      = reprezentările resurselor
• TRANZIȚIILE  = transferuri de date prin metodele protocolului (HTTP)
• Declanșatorul tranziției = accesarea altei resurse prin URI

   resource1 ──GET──► repr.2 ──POST──► repr.3 ──GET──► repr.4
   (fiecare stare = o reprezentare; fiecare săgeată = o metodă HTTP)
```
- **URI**: fiecare resursă (stare) e adresată printr-un URI.
- **Hipermedia**: graful de legături (link-uri) între resurse — din ea, dintr-o stare (reprezentare) se pot face tranziții către alte stări. Utilizatorul/programul accesează altă resursă printr-un URI.
- **HATEOAS**: "Hypermedia as the engine of application state" — hipermedia (link-urile din reprezentări) **conduce** schimbarea stării aplicației. Fiecare reprezentare conține cel puțin un URL spre alte resurse.

> Deci: **hipermedia** oferă graful de tranziții, **URI**-urile etichetează stările/tranzițiile, iar aplicația se comportă ca un **automat** ale cărui stări sunt reprezentările accesate prin REST.

### Microservicii — și comparația Servicii Web vs Microservicii (întrebare!)

**Microserviciu** = implementează o funcționalitate specifică, disponibilă ca **un singur proces** (self-contained); componentă backend dezvoltată să fie **înlocuită, nu reutilizată**.

Caracteristici (Lewis & Fowler):
```
• mici, fiecare în propriul proces
• comunicare ușoară (de obicei HTTP)
• construite în jurul capabilităților de business
• deployabile independent
• management centralizat minim
• pot fi scrise în limbaje diferite și folosi stocări diferite
```

**Servicii Web (SOA) vs Microservicii:**
| Aspect | Servicii Web (SOA) | Microservicii |
|--------|--------------------|--------------|
| Partajare | **share-as-much-as-possible** | **share-as-little-as-possible** |
| Cuplare | contract formal, eventual middleware | de obicei FĂRĂ middleware, fără interacțiuni abstracte (contract decoupling) |
| Granularitate | servicii (pot fi mari) | foarte mici, un proces fiecare |
| Reutilizare | servicii reutilizabile | construite să fie **înlocuite** |
| Scalare | replicare/distribuire servicii | replicare individuală a fiecărui microserviciu |
| Comunicare | sincronă/asincronă, orchestrare | de obicei asincronă (point-to-point sau publish-subscribe) |

> Asemănări: ambele = software ca servicii, slab cuplate, invocate prin rețea (HTTP), interoperabile, deployment distribuit. **µSOA = Microservice Oriented Architecture.**

**Tipuri de microservicii:**
- **funcționale** — implementează operații de business (expuse consumatorului, independente);
- **de infrastructură (control)** — sarcini ne-funcționale (autentificare, autorizare, logging, monitoring); **private**, partajabile intern.

**Serverless = FaaS + BaaS:** strat de abstractizare a accesului la resurse cloud.
- **FaaS (Functions as a Service)** — funcții cloud mici (<100 linii), executate independent/asincron, declanșate de evenimente.
- **BaaS (Backend as a Service)** — încapsulează servicii de infrastructură (auth, logging...), private.

### MVC — Model-View-Controller

Majoritatea aplicațiilor Web adoptă **MVC** (Trygve Reenskaug, 1979). Principiu: **separation of concerns**.

```
        ┌─────────────┐
   (1)  │  CONTROLLER │ (2) alege un model
 cerere►│             │──────────────► ┌────────┐
        │             │◄────────────── │ MODEL  │ date + reguli (constrângeri)
        │             │ (3) date        └────────┘
        │             │
        │             │ (4) selectează    ┌────────┐
        │             │──────────────►   │  VIEW  │ moduri de prezentare a datelor
        └─────────────┘                   └────────┘
   (5) conținut ► client
```

| Componentă | Rol |
|-----------|-----|
| **Controller** | preia cererile clientului (HTTP: GET, POST din acțiunile utilizatorului), gestionează resursele necesare; apelează un model, apoi selectează un view |
| **Model** | resursele gestionate (users, products...); **date + reguli** (constrângeri); oferă controller-ului o reprezentare a datelor; validează datele de stocat |
| **View** | oferă moduri de prezentare a datelor (din model, via controller); pot exista mai multe view-uri, controller-ul alege unul |

Pași tipici: (1) cerere de la client → (2) rutare la controller → (3) alegere model → (4) furnizare date → (5) selectare view → (6) conținut spre client.

Implementat de framework-uri: ASP.NET MVC (C#), Django (Python), Express (Node.js), Laravel (PHP), Rails (Ruby), Spring (Java). **Variante derivate:** HMVC, MVP (Model-View-Presenter), MVVM (Model-View-ViewModel).

**(Dez)avantaje MVC:**
| Avantaje | Dezavantaje |
|----------|-------------|
| Separarea responsabilităților (date / logică / prezentare) | Complexitate suplimentară pentru aplicații mici |
| Mentenanță și testare mai ușoare | Curbă de învățare; structură impusă de framework |
| Mai multe view-uri pentru același model | Risc de controllere "grase" |
| Reutilizarea modelelor / view-urilor | Indirectare (mai mult cod de legătură) |

### Interdependența SOA ↔ MVC (întrebare!)

```
MVC = arhitectură INTERNĂ a unei aplicații Web
      (organizează o aplicație: model/view/controller)

SOA = arhitectură de SISTEM, între aplicații
      (funcționalitatea = servicii independente, invocabile)

LEGĂTURA:
• Abordarea clasică: aplicația Web expune funcționalități clienților
  pe baza datelor stocate, de obicei printr-o arhitectură MV*.
• Migrarea spre SOA: fiecare funcționalitate devine un SERVICIU independent
  (care poate folosi alte servicii interne + stocări proprii).
• În SOA, o aplicație MVC poate FI un serviciu, iar Controller-ul (sau Modelul)
  poate INVOCA servicii externe (proprii sau terțe) la nivel de back-end/front-end,
  sincron sau asincron.
```
Deci MVC structurează **o aplicație**, iar SOA descompune **sistemul** în servicii; o aplicație MVC poate consuma sau expune servicii SOA → cele două se completează (MVC = micro-nivel, SOA = macro-nivel).

---

## 5. DOM, parsere, XML ⭐⭐
> Întrebări: *"De ce sunt mai multe parsers, care e rolul lor și ce interdependențe pot fi între ele?"*, *"Poate fi DOM creat dacă este doar bine formatat și nu validat HTML/XML? Argumentați."*

### XML pe scurt

**XML (eXtensible Markup Language)** — meta-limbaj de adnotare derivat din SGML (standard W3C). E **o tehnologie + o familie de limbaje**.

Constituenți: **prolog**, **elemente**, **atribute**, **entități**, **secțiuni CDATA**, **instrucțiuni de procesare**.

```xml
<?xml version="1.0" encoding="UTF-8"?>   <!-- prolog -->
<products>
  <product>                              <!-- element -->
    <name>Ping Uinix</name>
    <platform type="tablet">Android</platform>  <!-- atribut -->
  </product>
</products>
```

Reguli XML (well-formed): un singur element rădăcină, elementele **corect închise și imbricate**, **case sensitive**, valorile atributelor între ghilimele/apostrofuri obligatoriu, element gol `<x/>`.

**Familia XML:** XML (sintaxă), Infoset (model de date), XLink/XPointer (legături), XSL/XSLT + XSL-FO (transformare/formatare), XQuery + XPath (interogare). Formate bazate pe XML: (X)HTML, SVG, MathML, SMIL, RSS/Atom, KML, DocBook, ODF, EPUB etc.

**Namespace XML** (`xmlns`) — vocabular pentru a califica unic elementele/atributele, denotat printr-un URI; rezolvă **conflictele** de nume (ex. `<name>` eveniment vs `<name>` persoană).

### Tipuri de procesare XML

```
• MANUALĂ            ex. expresii regulate
• ORIENTATĂ-OBIECT   DOM (arbore în memorie) / non-DOM
• PE EVENIMENTE      SAX (Simple API for XML), XPP (XML Pull Parsing)
• SIMPLIFICATĂ       SimpleXML
• SPECIFICĂ          API-uri pentru tipuri particulare (RSS, KML, SVG, MathML)
```

### Parsere XML — de ce mai multe, rolul lor, interdependențe (întrebare!)

Un **parser (procesor) XML** citește documentul și îl pune la dispoziția aplicației. Există mai multe **pentru că au roluri și compromisuri diferite:**

```
1. PARSER FĂRĂ VALIDARE
   Rol: verifică DOAR dacă documentul e BINE FORMAT (well-formed)
        — respectă regulile de sintaxă XML.
   Exemple: Expat, libxml, MSXML

2. PARSER CU VALIDARE
   Rol: verifică dacă documentul e VALID — conform unei metode de validare
        (DTD - Document Type Definition, XML Schema).
   Exemple: Apache Xerces, JAXP, libxml, MSXML

   + după modelul de acces la date:
3. PARSER DOM (orientat obiect)
   Rol: construiește în memorie ARBORELE de noduri → acces/modificare aleatoare.
        Consumă memorie (tot documentul în RAM).
4. PARSER SAX / pull (pe evenimente)
   Rol: parcurge documentul generând evenimente (start/end tag...) — rapid,
        memorie mică, dar fără acces aleator.
```

**Interdependențe între parsere:**
- Un parser **DOM** sau **SAX** poate fi **cu sau fără validare** (validarea e o etapă ortogonală: întâi well-formedness, apoi opțional validare).
- Validarea **presupune** mai întâi că documentul e bine format (nu poți valida un document care nu e well-formed).
- Bibliotecile mari (libxml, MSXML) oferă **mai multe moduri** (DOM + SAX + cu/fără validare) în același pachet → un parser DOM poate folosi intern un parser de tip eveniment pentru a construi arborele.
- Ierarhia logică: **well-formed** (obligatoriu) → **valid** (opțional) → **model de acces** (DOM/SAX).

### DOM — Document Object Model

**DOM** = API abstract, standardizat (W3C), **independent de platformă și limbaj**, pentru procesarea **orientată-obiect** a documentelor XML/HTML. Definește o structură logică de tip **arbore**: documentul = ierarhie de obiecte (noduri).

- Versiuni: DOM 1 (1998) … DOM 4 (2015); **DOM Living Standard** (WHATWG) pentru HTML.
- Interfețele sunt definite prin **IDL / WebIDL**.
- Parcurgerea arborelui: **preordine, depth-first** (tree order).

**Tipuri de noduri / interfețe fundamentale:**
```
Document    — accesul la document (doctype, documentElement)
Element     — elementele (tagName, getAttribute, setAttribute...)
Attr        — atribute (name, value)
Text, Comment, CDATASection — conținut
NodeList    — colecție de noduri (acces prin index)
NamedNodeMap — acces pe chei (ex. liste de atribute HTML)
```

**Proprietăți/metode Node:** `nodeName`, `nodeValue`, `nodeType`, `parentNode`, `childNodes`, `firstChild`, `nextSibling`; `appendChild()`, `removeChild()`, `replaceChild()`, `cloneNode()`.

**Metode Document:** `createElement()`, `createTextNode()`, `createAttribute()`, `getElementsByTagName()`, `getElementById()`.

### Poate fi creat DOM dacă documentul e doar bine format, dar nevalidat? (întrebare!)

```
DA. ✓

ARGUMENTARE:
• DOM cere ca documentul să fie DOAR BINE FORMAT (well-formed):
  marcaje corect închise/imbricate, un singur root, sintaxă XML corectă.
• VALIDAREA (conform DTD / XML Schema) este o etapă SEPARATĂ și OPȚIONALĂ —
  verifică structura/semantica față de o gramatică, NU e necesară pentru
  construirea arborelui.
• Un parser FĂRĂ validare (Expat, libxml) construiește arborele DOM atâta timp
  cât documentul e bine format.
• În browser, accesarea/procesarea HTML și XML prin DOM se face FĂRĂ validare
  (programe JavaScript interpretate de browser).

CONCLUZIE: well-formed = SUFICIENT pentru DOM; valid = NU e necesar.
(Reciproc: dacă documentul NU e bine format, arborele DOM nu poate fi construit.)
```

### DOM în browser, XPath, evenimente

- În browser, arborele DOM al documentului HTML se accesează/modifică prin **JavaScript** și obiectul `document` (interfața `HTMLDocument` extinde `Document`).
- Proprietăți utile: `innerHTML`, `textContent`; **Selectors API**: `querySelector()`, `querySelectorAll()` (selectori CSS).
- **XPath** — limbaj de adresare a nodurilor: `/html/body/article` (descendent direct), `//platform` (recursiv DFS), `*` (wildcard), `@attr` (atribut), `[expr]` (filtru/index), funcții `count()`, `contains()`, `sum()`.
- **Evenimente**: `addEventListener("event", fn, mode)`; flux **capture** (rădăcină→țintă, `mode=true`) vs **bubbling** (țintă→sus, `mode=false`); `preventDefault()`, `stopPropagation()`, `removeEventListener()`. Proprietăți Event: `type`, `target`, `currentTarget`, `bubbles`, `cancelable`.
- **Atribute custom** `data-*` accesate prin `HTMLElement.dataset`.
- **Shadow DOM** — arbore încapsulat (shadow root) atașat unui document (Web Components).

---

## 6. Autentificare și autorizare în APIs ⭐⭐
> Întrebare: *"Autentificare și autorizare în cadrul APIs, definiție, ce sunt, exemple"*.

### Definiții

- **Autentificare** = a dovedi **CINE ești** (verificarea identității).
- **Autorizare** = a stabili **CE AI VOIE** să faci/accesezi (permisiuni/politici de acces).

### Pașii uzuali pentru o aplicație care invocă un serviciu prin API public

```
(1) Înregistrarea aplicației pe site-ul furnizorului de serviciu
    → se obține o CHEIE DE ACCES (API key / consumer key / developer key)
(2) Pe baza cheii, aplicația se AUTENTIFICĂ pentru a fi AUTORIZATĂ
    → pot exista politici de acces: doar citire (read), editare etc.
(3) Autentificarea + autorizarea au loc cu CONSIMȚĂMÂNTUL utilizatorului
    (login: nume+parolă, eventual 2FA), apoi acesta autorizează aplicația
(4) Aplicația apelează funcționalitățile serviciului; sesiunea curentă e
    menținută prin TOKEN-uri (auth tokens)
```

### OAuth

**OAuth** = protocol deschis (RFC 6749) pentru a **autoriza** o aplicație să acceseze date private în mod standardizat, fără a-i da parola utilizatorului. Versiuni: OAuth 1.0 (2010), **OAuth 2.0** (2012), OAuth 2.1 (draft).
> Exemplu (Facebook): autorizare cu permisiuni — `email`, `public_profile`, `user_birthday`, `user_friends`, `user_photos`...
> Furnizori/proxy: Auth0, Okta, Ory Hydra. Servicii: GitHub, Google, Stack Exchange, WordPress.

### Metode de autentificare (din curs)

```
• pe bază de sesiune Web (SID)
• cu token-uri (token based authentication)
• fără parolă (passwordless): URL de unică folosință, TOTP
• SSO (Single Sign-On) — autentificare în mai multe aplicații înrudite
• autentificare socială (prin alte conturi/rețele sociale)
• 2FA (Two-Factor Authentication) — minim 2 factori
• passkey — ceva ce știi (PIN) + ceva ce ai (autentificator) + ceva ce ești (amprentă)
• biometrică (amprentă, facial, iris/retină, voce, ADN)
• prin dispozitiv hardware (smartcard, ceas)
```

### OpenID, OpenID Connect, JWT

- **OpenID** — autentificare descentralizată (SSO); identitatea utilizatorului = un URL dat de un **identity provider** (ex. Steam, rețea socială).
- **OpenID Connect** — strat de identitate peste **OAuth 2**; format **JWT (JSON Web Token)** (RFC 7519).
- **JWT** = obiect JSON pentru schimb "securizat" de date între două entități (de obicei client ↔ API). Folosit pentru:
  - **autentificare** (ID token după login),
  - **autorizare** (fiecare cerere include token-ul, similar sesiunilor),
  - **schimb de date** (integritate prin **semnături digitale** — HMAC + perechi de chei publice/private).

---

## 7. JSON vs HTML ⭐
> Întrebare: *"Diferența dintre JSON și HTML"*.

Ambele sunt **formate text** folosite pe Web, dar cu scopuri **diferite**:

| Aspect | **HTML** | **JSON** |
|--------|----------|----------|
| Scop | **prezentarea** conținutului către om (în browser) | **schimb/modelare de date** procesabile de software |
| Tip | limbaj de marcare (markup) | format de serializare a datelor (semi-structurat) |
| MIME | `text/html` | `application/json` |
| Sintaxă | elemente/taguri `<p>`, `<div>`, atribute | perechi `cheie: valoare`, obiecte `{}`, tablouri `[]` |
| Conține | text + meta-date de prezentare/structură | date (string, number, boolean, null, obiecte, tablouri) |
| Consumator tipic | utilizatori umani (afișare) | aplicații/servicii (procesare) |
| În arhitectura Web | reprezentarea unei resurse pentru afișare | reprezentarea unei resurse pentru API-uri/Ajax |

```
HTML (prezentare):                    JSON (date):
<section id="meteo">                  {
  <div class="weather">                "point": {
    <p lang="ro">Iași</p>                "name": "Iași",
    <span>city</span>                    "type": "city"
  </div>                               },
</section>                             "temperature": { "value": "..." }
                                       }
```
> În REST/Ajax, **JSON** e formatul preferat pentru transferul de date între client și serviciu; **HTML** e reprezentarea destinată afișării. Aceeași resursă (un URI) poate avea ambele reprezentări (HTML pentru oameni, JSON pentru programe).

---

## 8. HTML, Cascade CSS și Responsive Design ⭐
> Întrebări: *"Cascade în CSS"*, *"Definește «design Web Responsive». Cum și când se pune în aplicare."*
>
> ⚠️ **IMPORTANT — sursa acestor teme lipsește din materiale.** Cursurile disponibile sar de la
> **web06 (XML)** la **web08 (DOM)** — **cursul web07 NU există** în folder (nici PDF, nici txt).
> Întrucât examenul cere Cascade CSS și Responsive Design, iar acestea nu apar în niciunul din cele
> 10 cursuri prezente, **web07 este aproape sigur cursul de HTML/CSS** (s-ar potrivi între XML și DOM).
> Capitolul de mai jos e scris complet, cu cunoștințe standard W3C/MDN, ca să ai ce învăța pentru
> Q11 și Q12. **Recomandare:** cere/recuperează cursul web07 pentru detaliile și exemplele specifice profesorului.

### 8.1 HTML — fundamente (probabil din web07)

**HTML (HyperText Markup Language)** — limbaj de marcare pentru **structurarea** conținutului unei pagini Web (MIME `text/html`). Standardul curent: **HTML Living Standard** (WHATWG). În arhitectura Web: HTML = **reprezentarea** unei resurse destinată afișării (vezi cap. 1 și 7).

**Structura unui document HTML5:**
```html
<!DOCTYPE html>
<html lang="ro">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Titlul paginii</title>
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <h1>Conținut</h1>
    <p>Un paragraf cu <a href="...">legătură</a>.</p>
  </body>
</html>
```

**Element vs atribut:**
```
<a href="https://...">text</a>
└┬┘ └────┬──────────┘ └──┘└┬┘
 tag    atribut       conț. tag închidere
 deschidere (nume=valoare)
```
- **Element** = unitate structurală: tag de start + conținut + tag de sfârșit. Elemente goale: `<br>`, `<img>`, `<input>`, `<meta>`.
- **Atribut** = proprietate a elementului, specificat în tagul de start (`name="value"`).
- Elementele se **imbrică** (părinte/copil) → formează **arborele DOM** (vezi cap. 5).

**Elemente semantice HTML5** (structurează pagina cu sens, nu doar vizual):
```
<header> <nav> <main> <article> <section> <aside> <footer> <figure>
```
**Elemente de conținut uzuale:** titluri `<h1>`–`<h6>`, paragraf `<p>`, liste `<ul>`/`<ol>`/`<li>`, link `<a>`, imagine `<img>`, tabel `<table>`, formular `<form>` cu `<input>`, `<label>`, `<select>`, `<textarea>`, `<button>`.

> **HTML structurează, CSS stilizează, JavaScript adaugă comportament** — separarea celor trei preocupări (structură / prezentare / interacțiune).

### 8.2 CSS — Cascade (întrebare Q11)

**CSS (Cascading Style Sheets)** — limbaj de **stilizare/prezentare** a conținutului (MIME `text/css`). O regulă CSS:
```
selector { proprietate: valoare; proprietate2: valoare2; }

p.intro  { color: navy; font-size: 1.2em; }
└──┬───┘   └──────────────┬─────────────┘
 selector            declarații
```

**Cele 3 moduri de a aplica CSS:**
| Mod | Sintaxă | Observații |
|-----|---------|------------|
| **inline** | `<p style="color:red">` | atribut `style` direct pe element; cea mai mare prioritate |
| **intern (embedded)** | `<style> p {…} </style>` în `<head>` | pentru o singură pagină |
| **extern** | `<link rel="stylesheet" href="x.css">` | recomandat — reutilizabil, separat de HTML |

#### Ce înseamnă "Cascading" (cascada)

Numele vine de la mecanismul prin care, când **mai multe reguli** se aplică aceluiași element și intră în **conflict** (setează aceeași proprietate), browserul decide **care câștigă**. Stilurile "cad în cascadă" din mai multe surse și se combină după reguli precise.

**Algoritmul cascadei — ordinea de departajare (de la mare la mic):**
```
1. ORIGINE + IMPORTANȚĂ
   user-agent(browser) < utilizator < AUTOR < autor !important < utilizator !important
   (în mod normal autorul câștigă; !important inversează ordinea)

2. SPECIFICITATEA selectorului (dacă origine/importanță egale)
   se calculează ca (a, b, c):
      a = nr. de #id-uri
      b = nr. de .clase / [atribute] / :pseudo-clase
      c = nr. de elemente / ::pseudo-elemente
   stil INLINE bate orice selector;  câștigă cea mai mare (a,b,c)

3. ORDINEA în sursă (dacă și specificitatea e egală)
   câștigă regula DECLARATĂ ULTIMA
```

**Exemplu de specificitate:**
```css
p              { color: black; }   /* (0,0,1) */
.intro         { color: blue;  }   /* (0,1,0) — bate pe p          */
#main          { color: green; }   /* (1,0,0) — bate pe .intro     */
<p style="...">                    /* inline   — bate pe #main      */
```
Pentru `<p class="intro" id="main">`: câștigă regula `#main` (verde), dacă nu există inline; `!important` ar bate tot.

#### Moștenire (inheritance)

Unele proprietăți se **moștenesc** automat de la părinte la copii, altele nu:
```
SE moștenesc:    color, font-family, font-size, line-height, text-align, visibility
NU se moștenesc: margin, padding, border, width, height, background, position
```
Valori speciale: `inherit` (forțează moștenirea), `initial` (valoarea implicită), `unset`.

#### Cum lucrează împreună (rezumat cascadă)
```
Pentru fiecare element și fiecare proprietate, browserul:
1. adună toate declarațiile care se aplică (din toate sursele)
2. le sortează după origine+importanță → specificitate → ordine
3. alege "câștigătorul" (cascadă)
4. dacă nu există nicio declarație, încearcă MOȘTENIREA de la părinte
5. dacă nici așa, folosește valoarea INIȚIALĂ a proprietății
```

**Modelul cutiei (box model)** — fiecare element = o cutie:
```
┌─────────────── margin ───────────────┐
│  ┌──────────── border ────────────┐  │
│  │  ┌───────── padding ────────┐  │  │
│  │  │      CONȚINUT (content)   │  │  │
│  │  └──────────────────────────┘  │  │
│  └────────────────────────────────┘  │
└───────────────────────────────────────┘
```

### 8.3 Responsive Web Design (întrebare Q12)

> Din cursurile disponibile: realitatea Web înseamnă **moduri multiple de interacțiune**
> (mobile, laptop, PC, tabletă, smart TV, consolă, smartwatch). Mediul de execuție vizează
> **multi-device interactivity (responsive Web design)** (web04); PWA-urile au ca trăsătură
> **adaptarea la dispozitiv** (web11). Detaliile de mai jos sunt cunoștințe standard.

**Definiție:** **Responsive Web Design (RWD)** = abordare de proiectare prin care **aceeași** pagină/aplicație Web se **adaptează automat** la dispozitivul și contextul de afișare (dimensiunea ecranului, orientare, rezoluție) — arată și funcționează bine pe mobil, tabletă, desktop, smart TV. (Termen introdus de Ethan Marcotte, 2010.)

> Opusul: **MPA/site fix** sau versiuni separate (ex. `m.site.com` pentru mobil). RWD = **o singură** bază de cod care se adaptează.

**Cele 3 ingrediente de bază (Marcotte):**
```
1. LAYOUT FLUID (fluid grid)
   dimensiuni RELATIVE în loc de pixeli fificși: %, em, rem, vw, vh, fr
   → conținutul "curge" și se redimensionează cu ecranul

2. IMAGINI/MEDIA FLEXIBILE
   img { max-width: 100%; }    → imaginile nu depășesc containerul
   <picture>, srcset           → imagini diferite pentru ecrane diferite

3. MEDIA QUERIES
   reguli CSS aplicate condiționat, în funcție de caracteristicile dispozitivului
```

**Media queries — mecanismul cheie:**
```css
/* stil de bază (mobil întâi — "mobile first") */
.container { width: 100%; }

/* tabletă și mai mare */
@media (min-width: 768px) {
  .container { width: 750px; }
}

/* desktop */
@media (min-width: 1200px) {
  .container { width: 1140px; }
}
```
- **Breakpoint** = pragul de lățime la care layout-ul se schimbă.
- **Mobile-first** = pornești de la ecranul mic și adaugi reguli pentru ecrane mari (cu `min-width`).

**Meta viewport (obligatoriu pe mobil):**
```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```
fără el, mobilul "micșorează" pagina desktop în loc s-o adapteze.

**Tehnici moderne de layout:** **Flexbox** (aranjare 1D — rânduri/coloane) și **CSS Grid** (aranjare 2D — grilă), unități `fr`, `clamp()`, `minmax()`.

**Cum și când se pune în aplicare:**
```
CUM:
• meta viewport în <head>
• layout fluid cu unități relative (%, rem, fr) + Flexbox/Grid
• media queries la breakpoint-uri (mobile-first)
• imagini flexibile (max-width:100%, srcset/<picture>)
• testare pe dispozitive/rezoluții diferite (dev tools)

CÂND:
• practic la ORICE aplicație Web modernă cu utilizatori pe dispozitive eterogene
• esențial pentru PWA (vezi Anexa F) și pentru accesibilitate/UX
• când vrei o singură bază de cod în loc de versiuni separate mobil/desktop
```

> **Legătură cu PWA:** o PWA bună e **responsive** (se adaptează dispozitivului), folosește
> caching offline (Service Workers) și poate fi instalată — vezi Anexa F.

---

## 9. Subiecte recurente din anii trecuți (2021–2024) ⭐⭐⭐
> Teme apărute repetat la examen (multe de **3 ori** în 4 ani) care nu erau în întrebările din primul an analizat. Surse: cursurile web02, web04, web05, web09 + cunoștințe standard unde slide-urile sunt imagini.

### 9.1 GET vs POST — securitate și navigare ⭐⭐⭐ (2023 ×3)
> Întrebări: *"POST vs GET"*, *"Folosirea GET în defavoarea lui POST, avantaje/dezavantaje pentru securitate și navigare web."*

| Aspect | **GET** | **POST** |
|--------|---------|----------|
| Scop | **obține** reprezentarea unei resurse | **trimite** date / creează resursă |
| Unde sunt datele | în **URL** (query string) — vizibile | în **body**-ul mesajului — nu apar în URL |
| Safe / Idempotent | **da / da** (nu schimbă starea serverului) | **nu / nu** (poate schimba starea) |
| Bookmark / istoric | **da** — URL-ul poate fi salvat/refolosit | nu (datele nu-s în URL) |
| Cache | poate fi pus în cache | de regulă nu |
| Dimensiune date | limitată (lungimea URL-ului) | mare (upload de fișiere) |
| Date sensibile | **NU** (parolele ar apărea în URL/istoric/loguri) | da (parole, date mari) |

```
SECURITATE:
• GET: datele se văd în URL → apar în istoric, bookmark-uri, loguri server,
  header Referer → NU pune parole/date sensibile în GET
• POST: datele sunt în body → nu apar în URL (dar tot trebuie HTTPS pentru criptare)

NAVIGARE:
• GET e potrivit pentru navigare: URL-uri partajabile, bookmark-abile, cache-abile,
  butonul "back" merge fără reefectuarea acțiunii
• POST nu e pentru navigare: re-trimiterea unui POST cere confirmare
  ("vrei să retrimiți formularul?")
```
> Din curs (web02): **GET** se folosește pentru a obține reprezentări (HTML, imagini, PDF...); utilizatorul poate pune un **bookmark**; starea serverului NU trebuie modificată. **POST** se folosește când datele sunt **mari** (upload) sau **sensibile** (parole), sau când invocarea **schimbă starea** serverului (adăugare înregistrare, modificare fișier).

### 9.2 Conținut static vs dinamic ⭐⭐⭐ (2022, 2021 ×3)
> Întrebări: *"Avantajele și dezavantajele generării de conținut static în comparație cu generarea dinamică."*

```
CONȚINUT STATIC                      CONȚINUT DINAMIC
fișiere fixe (HTML, CSS, imagini)    generat la cerere de server (PHP, Node...)
servite direct de serverul web       generat per request, din date/BD
                                      (CGI / server de aplicații / framework)
```

| | **Static** | **Dinamic** |
|--|-----------|-------------|
| Generare | fișiere pre-existente, servite ca atare | generat la fiecare cerere (din BD/logică) |
| Performanță | **rapid** (fără procesare), ușor de pus în CDN/cache | mai lent (procesare per request) |
| Scalabilitate | foarte bună | necesită resurse server |
| Securitate | suprafață de atac mică | mai expus (cod server, BD) |
| Personalizare | **nu** (același conținut pt toți) | **da** (conținut per utilizator/context) |
| Mentenanță | simplă; greu la conținut mult/variabil | flexibil; conținut din surse heterogene |
| Exemplu | pagină de prezentare, blog static (JAMstack/SSG) | magazin online, dashboard, feed personalizat |

> Din curs: serverul de aplicații realizează **generarea dinamică, pe server, a reprezentărilor** cerute de clienți (CGI, framework-uri). JAMstack/SSG (Anexa F) pre-randează markup-ul → conținut **static** stocat redundant în CDN (performanță).

### 9.3 Template engines (sisteme de șabloane) ⭐⭐⭐ (2023, 2022, 2021 ×3)
> Întrebări: *"Cum funcționează și în ce context sunt folosite template engines / sisteme de redare a conținutului pe baza machetelor?"*

> Un **Web template system / template engine** = combină o **specificație de prezentare (un șablon/template)** cu **date persistente** (ex. din BD), folosind un **procesor (template engine)** care generează documente HTML (sau alte formate).

**Cum funcționează (din curs web05):**
```
1. Definești un TEMPLATE (machetă HTML) cu "locuri" pentru variabile:
   <h1 class="profile">[@username] profile</h1>
   <img src="[@photoURL]" />
   <div>[@firstName] [@lastName]</div>

2. Programul de pe server completează variabilele cu valori reale (din BD etc.):
   $profile = new Template('profile.tpl');
   $profile->set('username') = 'Tux';
   $profile->set('photoURL') = 'imgs/tux.svg';

3. Procesorul (engine) SUBSTITUIE [@variabilă] cu valoarea → generează HTML-ul final
   trimis clientului.
```
**Context de folosire:** separă **prezentarea** (HTML) de **logică** și **date** (separation of concerns, MVC — View-ul); reutilizare; designerii lucrează pe șabloane, programatorii pe logică.

**Exemple (din curs):**
- **Pe server:** Smarty, Blade (PHP), Twig, Mustache, Pug (Node.js), Razor (.NET), FreeMarker (Java), XSLT (XML).
- **Pe client:** Handlebars, Mustache.js, Nunjucks.

### 9.4 SOAP vs REST ⭐ (2022)
> Întrebare: *"SOAP vs REST (asemănări, diferențe)."* — Din curs (web09): "serviciile Web pot fi dezvoltate prin **SOAP și/sau REST**."

| Aspect | **SOAP** | **REST** |
|--------|----------|----------|
| Ce este | **protocol** (reguli stricte) | **stil arhitectural** |
| Format mesaje | doar **XML** (plic SOAP) | orice MIME — uzual **JSON**, XML, CSV... |
| Transport | HTTP, SMTP, TCP... | de obicei **HTTP** (folosește verbele lui) |
| Contract | formal, strict (WSDL) | nu impune (eventual OpenAPI) |
| Stare | poate fi stateful | **stateless** |
| Greutate | "greu" (verbose, overhead XML) | "ușor", simplu |
| Cache | greu | suportă caching HTTP |
| Când | enterprise, securitate/tranzacții formale (WS-Security) | API-uri web, mobile, simplitate/performanță |

> **Asemănări:** ambele permit comunicarea între aplicații prin rețea (machine-to-machine), pot folosi HTTP, suportă servicii web interoperabile.

### 9.5 Principiul DRY în web ⭐⭐⭐ (2024, 2021 ×3)
> Întrebări: *"Context unde apare principiul DRY în aplicații web."*

**DRY (Don't Repeat Yourself):** "fiecare cunoaștere trebuie să aibă o reprezentare **unică**, neambiguă în sistem" — evită duplicarea.

**Contexte unde apare DRY în aplicații web:**
```
• Template engines / componente — un layout/partial reutilizat (header, footer)
  în loc de a copia HTML pe fiecare pagină (vezi 9.3)
• CSS — clase reutilizabile, variabile CSS/preprocesoare (Sass) în loc de stiluri repetate
• MVC — logica de business o singură dată în Model, nu duplicată în view-uri/controllere
• Funcții/module/biblioteci — cod reutilizabil (DRY la nivel de cod)
• API-uri/servicii — o singură sursă de adevăr pentru date (un endpoint), consumat
  de mai mulți clienți (web, mobil) în loc de a reimplementa logica
• ORM/modele — schema datelor definită o singură dată
• Configurări — variabile de mediu/config centralizate, nu valori "magice" repetate
```
Beneficii: mentenanță ușoară (schimbi într-un singur loc), mai puține bug-uri din inconsistență.

### 9.6 Folosirea mai multor limbaje (poliglot) în aplicații web mari ⭐⭐⭐ (2024, 2023, 2021 ×3)
> Întrebări: *"Folosirea mai multor limbaje de programare în aplicații web mari (de ce) + dezavantaje."*

> Din curs (web04, studii de caz): aplicațiile mari folosesc **mai multe limbaje**, fiecare pentru ce e mai bun. Ex: **Flickr** — PHP (logică/prezentare), Perl (validare date), Java (management noduri stocare); **Netflix** — Python (Flask), Java, Node.js, React (JS); **Facebook** — Hack/PHP, Python, Java, JS.

**De ce (avantaje):**
```
• "Right tool for the job" — fiecare limbaj pentru ce excelează
  (ex. Python pt ML/data, JS pt front-end, Java/Go pt backend scalabil)
• Microservicii — fiecare serviciu poate fi în alt limbaj, independent (vezi cap. 4)
• Reutilizarea bibliotecilor/ecosistemelor specifice
• Performanță — limbaj rapid pt componente critice
• Echipe — folosesc expertiza existentă
```
**Dezavantaje:**
```
• Complexitate crescută (build, deployment, tooling pt fiecare limbaj)
• Mentenanță mai grea, integrare între componente
• Echipa trebuie să cunoască mai multe limbaje (sau mai multe echipe)
• Dificultăți de debugging cross-limbaj, duplicare de logică (anti-DRY)
• Overhead de comunicare între servicii (serializare, API-uri)
```

### 9.7 Expresii regulate pentru validarea formularelor ⭐ (2023)
> Întrebare: *"(Dez)avantaje pentru folosirea expresiilor regulate pentru validarea datelor din câmpurile unui form."*

**Avantaje:**
```
• Concise și puternice — validezi formate complexe (email, telefon, CNP) într-o linie
• Reutilizabile, standardizate (suport în orice limbaj)
• Validare rapidă pe client (instant feedback) ȘI pe server
• Atribut HTML5 pattern="..." direct pe <input>
```
**Dezavantaje:**
```
• Greu de citit/întreținut ("write-only code"), ușor de greșit
• Pot fi prea permisive sau prea stricte (ex. regex pt email "corect" e foarte complex)
• Risc de performanță — ReDoS (Regular expression Denial of Service) la regex prost scrise
• Validarea pe CLIENT nu e suficientă — trebuie REPETATĂ pe server (securitate:
  clientul poate fi ocolit)
• Nu validează logica de business (ex. "data nașterii în trecut")
```

### 9.8 Rolul codurilor HTTP în randarea paginii ⭐⭐ (2023, 2022)
> Întrebări: *"Rolul codurilor HTTP + ce rol au în randarea HTML-ului."*

Codurile de stare (vezi cap. 2) ghidează ce face **browserul** cu răspunsul:
```
1xx Informational — rar relevante pt randare (ex. 101 switch la WebSocket)
2xx Success (200 OK) — browserul PRIMEȘTE reprezentarea și o RANDEAZĂ
3xx Redirection (301/302/303) — browserul face REDIRECT automat la altă pagină
                                (304 Not Modified → folosește versiunea din cache)
4xx Client Error (404, 403) — browserul randează o pagină de eroare
                              (ex. "404 Not Found" generată automat)
5xx Server Error (500, 503) — pagină de eroare server
```
> Exemplu (din practică): când accesezi un URL inexistent, serverul întoarce **404** și browserul randează automat o pagină "Not Found"; la **300/redirect** browserul navighează automat la noua adresă; la **200** randează conținutul primit.

### 9.9 Rolul design patterns în dezvoltarea web ⭐⭐⭐ (2021 ×3)
> Întrebări: *"Rolul șabloanelor de proiectare (design patterns) în contextul dezvoltării unei aplicații web. Minim 2 exemple concrete."*

**Rol:** soluții reutilizabile, dovedite, la probleme comune → cod mai **mentenabil, extensibil, reutilizabil**; vocabular comun în echipă.

**2+ exemple concrete în web:**
```
• MVC (Model-View-Controller) — arhitectura aplicației web (vezi cap. 4):
  Controller preia cererea HTTP, Model = date, View = prezentare (template)
• Singleton — o singură conexiune la BD / un singur obiect de configurare
• Front Controller — un punct unic de intrare care rutează toate cererile (routing)
• Observer — notificări/evenimente (ex. WebSocket, pub-sub, UI reactiv)
• Factory — crearea de obiecte (ex. crearea de răspunsuri/servicii diferite)
• Decorator — middleware (adaugă funcționalitate la request/response: auth, logging)
• Proxy — reverse proxy / caching / control acces (vezi cap. 1)
• Adapter — integrarea unor servicii/API-uri externe cu interfețe diferite
```
> (Pattern-urile sunt tratate complet la materia IP — vezi sinteza IP.)

---

# ANEXE
> Material din cursuri, mai puțin probabil ca temă principală de examen, dar util pentru completitudine și întrebări de tip "definiție".

## Anexă A — URI / URL / URN (web01)

**URI (Uniform Resource Identifier)** — identifică o resursă Web (RFC 3986). **Uniformitate**: resurse eterogene denotate cu aceleași convenții sintactice.

```
http://www.penguin.info/prog/search?id=Tux
└─┬─┘  └──────┬───────┘└────┬─────┘└──┬──┘
schema    authority       path      query
```
- **URL (Locator)** — identifică prin **mecanismul de acces** (adresă rețea/domeniu). Ex: `http://...`, `mailto:...`, `ftp://...`, `tel:+40...`, `data:...`, `geo:...`.
- **URN (Name)** — identifică prin **nume**, persistent, chiar dacă resursa e abstractă. Ex: `urn:ISBN:973-681-988-4`.
- **Absolut** (cu schema + authority) vs **relativ** (doar path/query). **Fragment**: `URI#fragment`.
- Caractere rezervate codate base 16 cu `%` (spațiu → `%20`). **IRI** — permite Unicode.
- "URI-urile sunt **opace**" (tipul nu se ghicește din extensie); "**Cool URIs don't change**".

## Anexă B — Hipertext / Hipermedia (web01)

- **Hipertext** (Ted Nelson, 1965) = text neliniar, interconectat, care se ramifică la comanda cititorului. Istoric: Vannevar Bush (MEMEX, 1945), Douglas Engelbart (Augment, 1968), Ted Nelson (Xanadu).
- **Hipermedia = hipertext + multimedia** (medii continue: audio/video; discrete: text).
- Hipertextul ca **(di)graf**: noduri = concepte, muchii = relații (legături). Nod sursă (ancoră referință) → nod destinație (referent).
- Legături: **referențiale** (ne-ierarhice) vs **organizaționale** (ierarhice); **statice** (autor) vs **dinamice** (software).

## Anexă C — CGI (web02)

**CGI (Common Gateway Interface)** — interfață de programare **independentă de limbaj**, între clienți și programe invocate pe server (standard de facto, RFC 3875).
- Scriptul CGI scrie reprezentarea la **stdout**; tipul se indică prin header MIME (`Content-type: text/html`).
- Poate fi scris în orice limbaj (bash, Perl, Python, C, Rust...).
- Acces la **variabile de mediu**: `REQUEST_METHOD`, `QUERY_STRING`, `CONTENT_TYPE`, `CONTENT_LENGTH`, `HTTP_USER_AGENT`, `HTTP_COOKIE`.
- Date: la **GET** → în `QUERY_STRING`; la **POST** → la **stdin** (lungime în `CONTENT_LENGTH`).
- **FastCGI** = alternativă axată pe performanță. **SSI** (Server Side Includes) = invocare directă din HTML.

## Anexă D — Dezvoltarea aplicațiilor Web / Web Engineering (web04)

- **Web Engineering** = dezvoltare sistematică (în faze), planificare corectă, monitorizare completă.
- **Asigurarea calității**: corectitudine, fiabilitate, extensibilitate/reutilizare (modularitate), compatibilitate, eficiență, portabilitate, utilizabilitate, mentenabilitate, securitate.
- Ciclu: requirements → design → build → testing → deployment → maintenance → evolution. Metodologii **agile** preferate; **MDA** (Model-Driven Architecture).
- **Pattern** = regulă context–problemă–soluție. **Web Patterns**: MVC, Page Controller, Front Controller, Template View. **Session State Patterns**: Client/Server/Database Session State.
- Mijloace de implementare: **server de aplicații**, **framework**, **bibliotecă**, **serviciu Web**, **SDK** (încapsulează API într-o bibliotecă), **Web component**, **widget**, **(Web) app**, **add-on**.
- Studii de caz: Flickr (PHP, Perl, Java, MySQL, ImageMagick, Ajax), Netflix, Instacart.

## Anexă E — PHP (web05)

**PHP** — limbaj interpretat (Zend Engine → opcodes), inclus în HTML, **case sensitive**; procedural + OOP + funcțional. Fișiere `.php`. Stack **LAMP** (Linux, Apache, MySQL/MariaDB, PHP).

```php
<?php
$age = 21;                       // tipuri: bool, int, float, string, array, object, resource, null
$prefer["color"] = "gray";       // array asociativ
echo "Salut $nume!";             // " interpolează, ' nu
function square($n) { return $n*$n; }
```
- Superglobale: `$_GET`, `$_POST`, `$_REQUEST`, `$_COOKIE`, `$_SESSION`, `$_SERVER`, `$_FILES`, `$GLOBALS`.
- **Cookie**: `setcookie('color','tan', time()+60*60*24*10)`; acces `$_COOKIE['color']`.
- **Sesiune**: `session_start()`, apoi `$_SESSION['visits']`.
- **OOP**: `class`, `extends`, `public/private/protected`, `__construct()`/`__destruct()`, `::` (scope resolution), `abstract`, `interface`, **trait** (reutilizare metode → pseudo-moștenire multiplă), `namespace`, `try/catch/throw`.
- **Baze de date**: `mysqli`, **PDO** (PHP Data Objects — DBAL), ORM (Doctrine, Propel, RedBean).
- Unelte: framework-uri (Laravel, Symfony, CodeIgniter, CakePHP), micro-framework-uri (Slim, Flight), **Composer** (dependențe), Docker.

## Anexă F — SPA, PWA, Mashup, CORS, JAMstack (web11)

- **SPA (Single-Page Application)** — un singur document HTML, conținut rescris dinamic prin Ajax după interacțiune; starea view-ului dată de **URL** (`/path/state` sau `#state`) → **routing**; gestionarea stării globale în browser; deseori paradigma **reactivă** (FRP).
- **PWA (Progressive Web Application)** — funcționalități adaptate contextului; **responsive**; interacțiuni ca aplicațiile native; **caching offline/online** prin **Web Workers** (JS în fundal, fără acces la DOM) și **Service Workers** (proxy între app, browser și rețea); transfer securizat HTTPS; **instalabilă**.
- **Mashup (aplicație Web hibridă)** — combinație de conținut din **mai multe surse** (a)sincrone, oferind o funcționalitate nouă. Caracteristici: **combinare, agregare, vizualizare**. Ex. (din curs): HuDo (Dog API + RandomUser), DoCa (Dog API + CATAAS) — două apeluri `fetch()`.
- **CORS (Cross-Origin Resource Sharing)** — permite partajarea resurselor între **origini diferite** (domenii), depășind **Same-Origin Policy** (care restrânge accesul unui script doar la date din aceeași origine). Bazat pe câmpuri header: `Access-Control-Allow-Origin`, `Access-Control-Request-Method`, `Origin`.
- **JAMstack** = **J**avaScript + **A**PIs + **M**arkup — procesare la client (JS), funcționalități prin API-uri reutilizabile (HTTPS), markup pre-randat stocat în **CDN** (SSG / headless CMS).

## Anexă G — GraphQL (web10)

**GraphQL (Graph Query Language)** — limbaj de interogare pentru API-uri + runtime; declarativ, inspirat din JSON, **strong-typed**. Permite **query** (citire) și **mutation** (modificare). Datele = grafuri; răspunsul conține **doar** datele cerute → rezolvă **over/under-fetching**.

| | **GraphQL** | **REST** |
|--|------------|----------|
| Endpoint-uri | **unul singur** | **multiple** (independente) |
| Cine decide ce date | **clientul** | serverul |
| Tipizare | strictă (tipuri declarate) | slabă |
| Format | JSON | orice MIME (uzual JSON) |
| Documentare | self-describing | necesită soluții terțe (OpenAPI) |
| Natură | limbaj de interogare + spec + unelte | stil arhitectural |

**OpenAPI Specification** (fost Swagger) — declară platform-independent interfața publică a unui API REST (JSON/YAML).

---

# CHEAT SHEET FINAL

### Arhitectură
```
Server web: îndeplinește cereri HTTP, STATELESS (Apache, NGINX, IIS)
Proxy: lângă client (fwd/reverse)   Gateway: ascunde serverul țintă (load balance, cache)
Tunnel: retransmite mesaje criptate (HTTPS/TLS)
Server aplicații: generează dinamic (Zend/PHP, Node, ASP.NET)
Layered (N-tier): separation of concerns; izolarea modificărilor; +mentenanță/-performanță
```

### HTTP / MIME / Ajax
```
Metode: GET(safe,idemp) HEAD POST PUT(idemp) DELETE(idemp)
MIME: type/subtype (text/html, application/json, image/png) — dat de server, nu de extensie
Header request: Host, User-Agent, Accept, Referer, Cookie, Authorization
Ajax: XMLHttpRequest/Fetch → asincron, fără reload; Accept/Content-Type negociază MIME-ul
```

### Cookie / Sesiuni
```
Cookie: name=value, max 4KB; Set-Cookie (răspuns) / Cookie (cerere)
Atribute: expires, domain, path, secure(HTTPS), httpOnly(anti-XSS), sameSite(anti-CSRF)
Sesiune: SID în cookie (PHPSESSID); date pe SERVER; HTTP e stateless
Stocare: client(cookie, Web Storage, IndexedDB) / server(fișiere, SQL, NoSQL)
```

### Servicii / REST / MVC
```
REST = automat: stări=reprezentări, tranziții=metode HTTP, URI=etichete, HATEOAS=hipermedia conduce
Constrângeri REST: client-server, stateless, cache, layered, uniform, HATEOAS
CRUD↔HTTP: Create=POST/PUT, Read=GET, Update=PUT/PATCH, Delete=DELETE
SOA: servicii independente, contract, share-much; Microservicii: 1 proces, share-little, înlocuibile
MVC: Controller(cereri)→Model(date+reguli)→View(prezentare); SOA(sistem) ⊃ MVC(aplicație)
```

### DOM / XML
```
Parsere: fără validare(well-formed: Expat) / cu validare(DTD,XML Schema: Xerces); DOM(arbore) / SAX(evenimente)
DOM cere DOAR well-formed (NU valid) → DA, se poate crea DOM nevalidat
well-formed: corect închis/imbricat, 1 root, ghilimele la atribute
```

### Auth
```
Autentificare = CINE ești; Autorizare = CE AI VOIE
OAuth (autorizare delegată), OpenID Connect (identitate), JWT (token JSON semnat)
Metode: sesiune/SID, token, passwordless, SSO, 2FA, passkey, biometric
```

### JSON vs HTML
```
HTML = prezentare pt OM (text/html); JSON = date pt SOFTWARE (application/json)
Aceeași resursă poate avea ambele reprezentări (HTML pt afișare, JSON pt API/Ajax)
```

### HTML / CSS / Responsive  (din cursul web07 — LIPSĂ în materiale)
```
HTML = structură (text/html); CSS = prezentare (text/css); JS = comportament
Cascadă CSS (cine câștigă la conflict):
  1. origine+importanță (autor > user > browser; !important inversează)
  2. specificitate: inline > #id > .class/[attr]/:pseudo > element
  3. ordine: ultima declarată câștigă
  apoi: moștenire de la părinte → valoare inițială
CSS se aplică: inline (style) / intern (<style>) / extern (<link>)
Responsive (RWD): aceeași pagină se adaptează la dispozitiv
  = layout fluid (%, rem, fr, Flexbox/Grid) + imagini flexibile (max-width:100%)
    + media queries (@media (min-width:...)) + <meta viewport>; "mobile-first"
  Când: orice app cu utilizatori pe dispozitive eterogene (esențial la PWA)
```

---

# 🎯 TEST GRILĂ — Tehnologii Web (subiecte reale de licență 2021–2025)
> Fiecare grilă pornește de la un **subiect dat efectiv la examen**. Apasă pe **„Răspuns"**. Răspunsul corect e distribuit uniform pe A/B/C/D.

**1.** *(«Context unde apare principiul DRY în aplicații web»)* — DRY apare, de exemplu, prin:
- A) copierea aceluiași HTML pe fiecare pagină
- B) reutilizarea unui layout/partial (header/footer) prin template engine + o singură sursă de logică în Model (MVC)
- C) scrierea aceleiași logici în fiecare controller
- D) valori „magice" repetate în tot codul

<details><summary>✅ Răspuns</summary>

**B)** — A, C, D sunt exact **violări** ale DRY. DRY = o singură sursă de adevăr.
</details>

**2.** *(«Arborele DOM: bine-formatat sau valid?»)* — Poate fi creat arborele DOM dacă documentul e doar bine-formatat, dar NU valid?
- A) nu, e nevoie obligatoriu de validare (DTD/Schema)
- B) da, dar doar pentru HTML, nu XML
- C) da — pentru DOM e suficient să fie bine-formatat (well-formed); validarea e o etapă separată și opțională
- D) nu, DOM necesită și well-formed ȘI valid

<details><summary>✅ Răspuns</summary>

**C)** — Browserul construiește DOM-ul chiar și pentru HTML „invalid" (ignoră/repară). Well-formed = suficient.
</details>

**3.** *(«Conexiunea HTML – DOM – transfer asincron»)* — Care afirmație e corectă?
- A) browserul construiește arborele DOM din HTML; DOM-ul poate fi modificat ulterior (ex. cu date primite asincron prin Ajax), fără reîncărcarea paginii
- B) DOM-ul este imutabil după creare
- C) Ajax înlocuiește arborele DOM cu un document XML
- D) HTML-ul se regenerează din DOM la fiecare cerere

<details><summary>✅ Răspuns</summary>

**A)** — HTML → arbore DOM; JS modifică DOM-ul dinamic (inclusiv cu date Ajax), fără reload.
</details>

**4.** *(«Server web, proxy, gateway, server de aplicații: asemănări/deosebiri»)* — Care afirmație e corectă?
- A) proxy-ul ascunde serverul țintă, gateway-ul e lângă client
- B) serverul web generează conținut dinamic, app server-ul servește doar fișiere statice
- C) proxy și gateway sunt același lucru
- D) serverul web servește/rutează cereri HTTP; app server-ul generează dinamic reprezentări; proxy-ul e lângă client; gateway-ul ascunde serverul țintă

<details><summary>✅ Răspuns</summary>

**D)** — A e inversat (proxy=lângă client, gateway=ascunde serverul); B e inversat.
</details>

**5.** *(«Diferența SOA și MVC + interdependență»)* — Relația corectă:
- A) sunt sinonime
- B) MVC = arhitectură internă a unei aplicații (model/view/controller); SOA = arhitectură de sistem (funcționalitatea = servicii independente); o aplicație MVC poate consuma/expune un serviciu SOA
- C) SOA este un design pattern din MVC
- D) MVC înlocuiește SOA

<details><summary>✅ Răspuns</summary>

**B)** — MVC organizează **o aplicație**; SOA descompune **sistemul** în servicii. Controller-ul poate apela servicii SOA.
</details>

**6.** *(«Autentificare și autorizare în API-uri»)* — Diferența:
- A) autentificare = ce ai voie să faci; autorizare = cine ești
- B) sunt sinonime
- C) autentificare = cine ești (verificarea identității); autorizare = ce ai voie să faci (permisiuni)
- D) autentificarea doar cu parolă, autorizarea doar cu biometrie

<details><summary>✅ Răspuns</summary>

**C)** — A e inversat. Întâi te autentifici (login), apoi ești autorizat. În API-uri, sesiunea se poartă adesea prin token JWT.
</details>

**7.** *(«GET în defavoarea POST: securitate»)* — De ce NU pui date sensibile (parole) într-o cerere GET?
- A) datele apar în URL → ajung în istoric, bookmark-uri, loguri de server, header Referer
- B) GET e mai lent
- C) GET nu suportă HTTPS
- D) GET criptează datele automat

<details><summary>✅ Răspuns</summary>

**A)** — La POST datele sunt în body (nu în URL). C și D sunt false.
</details>

**8.** *(«GET vs POST: navigare»)* — De ce GET e potrivit pentru navigare, iar POST nu?
- A) POST e mai rapid
- B) POST nu poate trimite date
- C) GET modifică starea serverului
- D) URL-urile GET sunt bookmark-abile, partajabile, cache-abile; re-trimiterea unui POST cere confirmare

<details><summary>✅ Răspuns</summary>

**D)** — C e fals (GET e safe). GET = obținere/navigare; POST = acțiuni cu efect.
</details>

**9.** *(«Servicii web vs microservicii web»)* — O diferență corectă:
- A) SOA = share-as-little; microservicii = share-as-much
- B) SOA tinde spre share-as-much (contract formal, eventual middleware); microserviciile spre share-as-little (proces mic, independent deployabil, construit să fie înlocuit)
- C) microserviciile rulează doar în browser
- D) sunt exact același lucru

<details><summary>✅ Răspuns</summary>

**B)** — A e inversat. Asemănare: ambele = servicii slab cuplate, invocate prin rețea (HTTP).
</details>

**10.** *(«Folosirea mai multor limbaje în aplicații web mari»)* — De ce?
- A) din obligație legală
- B) pentru a face codul mai lent
- C) „right tool for the job" — fiecare limbaj pentru ce excelează; microserviciile pot fi în limbaje diferite (dezavantaj: complexitate crescută)
- D) nu se folosesc niciodată mai multe limbaje

<details><summary>✅ Răspuns</summary>

**C)** — Ex.: Flickr (PHP/Perl/Java), Netflix (Python/Java/Node/React). Dezavantaj: build/deployment/mentenanță mai grele.
</details>

**11.** *(«GraphQL vs REST»)* — Un avantaj al GraphQL față de REST:
- A) un singur endpoint; clientul cere exact datele dorite → rezolvă over/under-fetching
- B) are mai multe endpoint-uri independente
- C) nu folosește tipuri
- D) nu poate face interogări

<details><summary>✅ Răspuns</summary>

**A)** — B e specific REST; C e fals (GraphQL e strong-typed). Dezavantaj GraphQL: caching mai greu.
</details>

**12.** *(«(Dez)avantaje expresii regulate pentru validarea formularelor»)* — Un dezavantaj:
- A) sunt imposibil de scris
- B) nu pot valida adrese de email
- C) funcționează doar în PHP
- D) greu de citit/întreținut, risc de ReDoS, iar validarea pe client trebuie oricum repetată pe server

<details><summary>✅ Răspuns</summary>

**D)** — Avantaje: concise, reutilizabile, `pattern="..."` în HTML5. Validarea client-side nu e suficientă (securitate).
</details>

**13.** *(«Rolul codurilor HTTP în randarea paginii»)* — Ce face browserul la un cod HTTP 3xx (301/302)?
- A) randează o pagină de eroare
- B) face redirect automat către altă adresă
- C) închide conexiunea fără a afișa nimic
- D) retrimite aceeași cerere la infinit

<details><summary>✅ Răspuns</summary>

**B)** — 3xx = redirect (304 → folosește cache-ul). 4xx/5xx → pagină de eroare; 2xx → randează conținutul.
</details>

**14.** *(«Rolul codurilor HTTP în randare»)* — La un cod HTTP 404, browserul:
- A) face redirect automat
- B) randează normal conținutul primit
- C) randează o pagină de eroare (ex. „404 Not Found")
- D) criptează răspunsul

<details><summary>✅ Răspuns</summary>

**C)** — 4xx = eroare client → pagină de eroare. (2xx → conținut; 3xx → redirect.)
</details>

**15.** *(«Cum funcționează și în ce context sunt folosite template engines»)* — Un template engine:
- A) combină o machetă (template) cu date și substituie variabilele → generează HTML-ul final
- B) compilează cod C++
- C) este un tip de bază de date
- D) rulează în kernelul sistemului de operare

<details><summary>✅ Răspuns</summary>

**A)** — Separă prezentarea de logică/date (View-ul din MVC). Ex.: Smarty, Blade, Twig, Mustache, Handlebars.
</details>

**16.** *(«Cascada CSS»)* — Ordinea de departajare la conflict între reguli:
- A) ordinea în sursă → specificitate → importanță
- B) doar cine e scris ultimul câștigă mereu
- C) regula cu selectorul cel mai scurt câștigă
- D) origine + importanță → specificitate → ordine în sursă

<details><summary>✅ Răspuns</summary>

**D)** — A e ordinea inversată. „Ultima declarată" contează doar la **specificitate egală**.
</details>

**17.** *(«Cascada CSS: !important»)* — `!important` într-o regulă:
- A) nu are niciun efect
- B) suprascrie regulile normale (inversează ordinea importanței în cascadă)
- C) crește specificitatea la infinit pentru orice regulă
- D) e obligatoriu pentru orice stil

<details><summary>✅ Răspuns</summary>

**B)** — `!important` ridică declarația peste cele normale (a se folosi cu grijă — greu de suprascris ulterior).
</details>

**18.** *(«Diferențe și asemănări HTML și JSON»)* — Diferența esențială:
- A) JSON e pentru prezentare vizuală, HTML pentru date
- B) sunt același format
- C) HTML = marcare pentru prezentarea conținutului către om; JSON = format de schimb de date procesabile de software
- D) JSON este un limbaj de programare

<details><summary>✅ Răspuns</summary>

**C)** — A e inversat. Aceeași resursă poate avea ambele reprezentări (HTML pt afișare, JSON pt API/Ajax).
</details>

**19.** *(«Avantajele/dezavantajele conținutului static vs dinamic»)* — Un avantaj al conținutului static:
- A) performanță și scalabilitate mai bune (servit direct, ușor de pus în CDN/cache)
- B) personalizare per utilizator
- C) conținut generat din baza de date la fiecare cerere
- D) necesită un server de aplicații

<details><summary>✅ Răspuns</summary>

**A)** — B, C, D descriu conținutul **dinamic**. Static = rapid, dar fără personalizare.
</details>

**20.** *(«SOAP vs REST»)* — O diferență corectă:
- A) SOAP e un stil arhitectural, REST e un protocol
- B) ambele folosesc obligatoriu WSDL
- C) REST folosește doar XML
- D) SOAP e un protocol (doar XML, strict); REST e un stil arhitectural (orice format, uzual JSON, folosește verbele HTTP)

<details><summary>✅ Răspuns</summary>

**D)** — A e inversat. Asemănare: ambele permit comunicarea machine-to-machine prin rețea.
</details>

**21.** *(«(Dez)avantajele arhitecturii pe straturi (N-tier, layered)»)* — Un avantaj:
- A) performanță maximă, fără niciun overhead
- B) modificările dintr-un strat nu afectează celelalte (izolare, mentenanță ușoară)
- C) elimină nevoia de testare
- D) un singur strat face tot

<details><summary>✅ Răspuns</summary>

**B)** — Overhead-ul de performanță e de fapt un **dezavantaj** (A); D e monolit (opusul).
</details>

**22.** *(«Rolul design patterns în dezvoltarea unei aplicații web + exemple»)* — Un exemplu concret:
- A) bubble sort
- B) notația O-mare
- C) MVC (arhitectura aplicației) sau Singleton (o singură conexiune la BD), Front Controller (routing), Observer (evenimente)
- D) Teorema Master

<details><summary>✅ Răspuns</summary>

**C)** — A, D sunt algoritmi/noțiuni de complexitate, nu șabloane de proiectare.
</details>

**23.** *(«Design web responsive și contextul de utilizare»)* — Responsive Web Design înseamnă:
- A) aceeași pagină se adaptează automat la dispozitiv (layout fluid + media queries + imagini flexibile)
- B) o versiune separată a site-ului pentru mobil (m.site.com)
- C) un site care se încarcă rapid
- D) un site fără CSS

<details><summary>✅ Răspuns</summary>

**A)** — B e abordarea opusă (site-uri separate). Se aplică oriunde ai utilizatori pe dispozitive eterogene (esențial la PWA).
</details>

**24.** *(«Avantaje/Dezavantaje MVC în diverse tipuri de aplicații web»)* — Care e corect?
- A) avantaj: elimină nevoia de testare
- B) avantaj: face codul mai lent
- C) MVC nu are niciun avantaj
- D) avantaj: separarea responsabilităților (date/logică/prezentare), mai multe view-uri, mentenanță; dezavantaj: complexitate suplimentară pentru aplicații mici

<details><summary>✅ Răspuns</summary>

**D)** — MVC strălucește la aplicații complexe (e-commerce); la un site static simplu, overhead-ul nu se justifică.
</details>

**25.** *(«Transfer de date asincron»)* — Ce permite Ajax (XMLHttpRequest / Fetch)?
- A) compilarea codului pe server
- B) transfer asincron de date între browser și server, fără reîncărcarea completă a paginii
- C) criptarea automată a cookie-urilor
- D) rularea SQL direct în browser

<details><summary>✅ Răspuns</summary>

**B)** — Ajax folosește headerele HTTP (Accept/Content-Type) pentru a negocia MIME-ul datelor și actualizează pagina cu DOM.
</details>

---

> 💡 **Distribuția răspunsurilor** e echilibrată. Confuzii cheie Web: server web vs app server, proxy vs gateway, autentificare vs autorizare, GET vs POST, DOM well-formed vs valid, SOA (sistem) vs MVC (aplicație), static vs dinamic, SOAP (protocol) vs REST (stil).
