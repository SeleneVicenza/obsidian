
Express è un **framework minimalista** per Node.js che semplifica enormemente la creazione di **server web e API REST**.  
In pratica è un **wrapper** sopra il modulo `http` di Node: ti evita di scrivere a mano parsing di URL, gestione degli header, routing manuale e parsing body.

---

# Vantaggi principali

1. **Routing semplice e leggibile**  
    Definisci endpoint con metodi chiari (`app.get`, `app.post`, …) invece di fare switch su `req.url`.

2. **[[Middleware]]**  
    Puoi incatenare funzioni che elaborano la richiesta in step (logging, autenticazione, parsing JSON, validazioni, ecc.).

3. **Parsing automatico**  
    Con `express.json()` ottieni subito il `req.body` pronto (niente loop di chunk).

4. **Ecosistema enorme**  
    Migliaia di pacchetti già pronti (auth, sessioni, CORS, file upload, ecc.).

5. **Curva di apprendimento rapida**  
    API molto semplici e tutorial ovunque → ottimo per partire.

---

# Esempio

``` js

const express = require('express'); const app = express(); const PORT = 3000; 
 
// middleware globale per JSON app.use(express.json());  // Home route
app.get('/', (req, res) => {   res.send('<h1>Hello from Express</h1>'); }); 
 
// API con query string 
app.get('/echo', (req, res) => {   const name = req.query.name || 'world';   res.json({ ok: true, echo: name }); });  

// API con body JSON 
app.post('/data', (req, res) => {   res.status(201).json({ received: req.body }); });  app.listen(PORT, () => console.log(`Express server running on http://localhost:${PORT}`));

```

---

# Confronto con `http` puro

|Aspetto|`http` core|Express|
|---|---|---|
|Routing|Manuale (switch `url.parse`)|`app.get('/path')`|
|Body JSON|Leggi stream + `JSON.parse`|`express.json()` automatico|
|Middleware|Devi scriverli a mano|Struttura nativa, ecosistema enorme|
|Ecosistema plugin|Limitato|Vastissimo (auth, sessioni, cors, upload…)|
|Curva di apprendimento|Più verbosa|Più rapida|
|Controllo low-level|Massimo|Un po’ astratto (ma sufficiente per il 95% dei casi)|

---

# Quando scegliere Express

#### Ottimo per:
- MVP, prototipi rapidi, microservizi semplici.
- API REST di piccole/medie dimensioni.
- Team che vogliono imparare Node partendo dal framework più diffuso.

#### Limiti:
- Non pensato per prestazioni massime (per quelle → **Fastify**).
- Struttura lasciata “libera”: su progetti grandi può diventare caotico se non organizzi bene cartelle e [[middleware]].

---

## In sintesi: 

**Express è la scelta migliore per iniziare con server e API in Node.js**, grazie alla sua semplicità e alla community vastissima.  

Una volta acquisita dimestichezza, se serve più performance o struttura, si può valutare **Fastify** o **NestJS**.