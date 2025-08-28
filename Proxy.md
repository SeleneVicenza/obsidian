componente (server o servizio) che **intercetta le richieste tra client e server**, inoltrandole da una parte all’altra $->$ **intermediario nella comunicazione**

---
### Tipologie principali

1. **Forward Proxy** (per i client → Internet)
    - Posizionato tra il client e Internet.
	- Il client conosce l’esistenza del proxy e lo utilizza esplicitamente.
	- Usato per:
	    - **Controllo accessi** (bloccare siti, filtri di contenuti).
	    - **Anonimizzazione** (nasconde IP del client).
	    - **Caching** (riduzione traffico e tempi di risposta).

2. **Reverse Proxy** (per i server → verso i client)
    - Posizionato davanti a uno o più server.
	- Il client non è consapevole del proxy, che appare come server finale.
	- Usato per:
	    - **Bilanciamento di carico** (distribuisce richieste su più server).
	    - **Sicurezza** (maschera l’architettura interna, filtra attacchi).
	    - **Caching/Compressione** per migliorare le prestazioni.
	    - **Gestione centralizzata SSL/TLS**.

3. **Transparent Proxy**
    - Intercetta le comunicazioni senza configurazione esplicita sul client.
	- Usato spesso da ISP o reti aziendali per caching e monitoraggio.

---
## Esempi di software/proxy comuni

- **Forward Proxy**: Squid Proxy, Privoxy.

- **Reverse Proxy**: Nginx, Apache HTTP Server (mod_proxy), HAProxy, Microsoft IIS ARR.

- **Proxy Trasparenti**: utilizzati da provider di rete.

- **Servizi cloud**: Cloudflare (reverse proxy con protezione DDoS).

---

|Caratteristica|Forward Proxy|Reverse Proxy|
|---|---|---|
|**Posizionamento**|Tra client e server di destinazione|Davanti a uno o più server|
|**Visibilità**|Configurato e noto al client|Trasparente per il client|
|**Scopo principale**|Controllo e filtraggio delle richieste dei client|Gestione e protezione dei server|
|**Anonimizzazione**|Nasconde l’identità del client|Nasconde l’identità e la struttura dei server|
|**Caching**|Riduce richieste verso l’esterno|Riduce carico e latenza lato server|
|**Sicurezza**|Filtra l’accesso dei client a Internet|Protegge i server da attacchi esterni|
|**Scalabilità**|Non rilevante|Bilancia le richieste su più server|
