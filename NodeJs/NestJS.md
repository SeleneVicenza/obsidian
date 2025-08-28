**Framework backend Node.js** per la costruzione di applicazioni server-side scalabili e manutenibili.  
Scritto in **TypeScript**, adotta un’architettura **modulare e orientata agli oggetti** (Simile alla struttura di progetti Angular).

Si basa su **Express** (di default) o **Fastify** come HTTP adapter.

[[ORM]] di default $->$ **TypeORM** 

---
## Caratteristiche principali

- **TypeScript nativo** (con supporto a JavaScript plain).
- **Architettura modulare**: applicazioni suddivise in **moduli, controller, service**.
- **[[Dependency Injection]] (DI)** integrata.
- **Decorators** per definire routing, moduli e provider.
- **[[Middleware]], Guards, Interceptors e Pipes** per logica trasversale (auth, validazione, logging, trasformazioni).
- **Supporto multiprotocollo**: HTTP, WebSocket, gRPC, GraphQL, Microservizi.   
- **Testing** integrato con Jest.

---
## Vantaggi

- Ottimo supporto TypeScript e tipizzazione forte.
- Estendibile e modulare.
- Ecosistema ricco: moduli per database, GraphQL, WebSockets, CQRS, ecc.
- Supporto a **microservizi** e architetture distribuite.

---
# Autenticazione 

NestJS integra il pattern [[JWT]] tramite il pacchetto **@nestjs/jwt** e le **Guardie di autenticazione**.