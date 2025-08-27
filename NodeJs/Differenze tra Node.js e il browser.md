## 1. Linguaggio comune, ecosistemi diversi

- Sia il browser che Node.js utilizzano **JavaScript**.
- Lo sviluppo nel browser e in Node.js è però radicalmente diverso, poiché cambia l’ecosistema.

---

## 2. Vantaggio principale di Node.js

- Utilizzo dello stesso linguaggio sia per **frontend** che per **backend**.
- Apprendimento semplificato: non è necessario padroneggiare ambienti e sintassi differenti.

---

## 3. Ecosistema del Browser

- Interazione principalmente con:
    - **DOM (Document Object Model)**
    - API della piattaforma Web (es. gestione dei **cookie**)

- Disponibilità di oggetti nativi come:
    - `document`
    - `window`

- **Limitazioni**: queste API non sono presenti in Node.js.

---

## 4. Ecosistema di Node.js

- Assenza di `document`, `window` e altre API tipiche del browser.
- Presenza di **API server-side** non disponibili nel browser, ad esempio:
    - Modulo `fs` per l’accesso al file system
    - Moduli di rete (`http`, `net`)
    - Gestione dei processi (`child_process`)

---

## 5. Controllo dell’ambiente di esecuzione

- **Node.js**: possibilità di stabilire la versione utilizzata per l’applicazione.
- **Browser**: impossibilità di controllare la versione o il tipo di browser usato dagli utenti.
- Vantaggio in Node.js: ambiente **più prevedibile e stabile**.

---

## 6. Compatibilità con ECMAScript

- **Node.js**: supporto diretto a JavaScript moderno (ES2015+), in base alla versione installata.
- **Browser**: aggiornamenti disomogenei → possibile necessità di utilizzare:
    - Versioni più vecchie del linguaggio
    - Strumenti come **Babel** per retro-compatibilità con ES5.

---

## 7. Sistemi di moduli

- **Node.js** (dalla v12): supporto sia a:
    - **CommonJS** → `require()`
    - **ES Modules** → `import`

- **Browser**: supporto in crescita per gli **ES Modules**, ma non compatibile con CommonJS.



 # In sintesi:
 
- **Browser** → ambiente orientato all’interfaccia utente, centrato sul DOM e sulle API Web.
- **Node.js** → ambiente server-side, con API dedicate e maggiore controllo sull’esecuzione.