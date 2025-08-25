Sia il browser che Node.js utilizzano JavaScript come linguaggio di programmazione. Creare app eseguibili nel browser è completamente diverso dallo sviluppare un'applicazione Node.js. Nonostante si utilizzi sempre JavaScript, ci sono alcune differenze fondamentali che rendono l'esperienza radicalmente diversa.

Dal punto di vista di uno sviluppatore frontend che utilizza ampiamente JavaScript, le app Node.js offrono un enorme vantaggio: la comodità di programmare tutto, frontend e backend, in un unico linguaggio.

Hai una grande opportunità perché sappiamo quanto sia difficile apprendere in modo completo e approfondito un linguaggio di programmazione e, utilizzando lo stesso linguaggio per svolgere tutto il tuo lavoro sul web, sia sul client che sul server, ti trovi in una posizione di vantaggio unica.

> **Ciò che cambia è l'ecosistema.**

Nel browser, la maggior parte delle volte si interagisce con il DOM o con altre API della piattaforma Web come i cookie. Questi non esistono in Node.js, ovviamente. Non sono disponibili `document`, `window`né tutti gli altri oggetti forniti dal browser.

E nel browser non abbiamo tutte le utili API che Node.js fornisce tramite i suoi moduli, come la funzionalità di accesso al file system.

Un'altra grande differenza è che in Node.js sei tu a controllare l'ambiente. A meno che tu non stia sviluppando un'applicazione open source che chiunque può distribuire ovunque, sai già su quale versione di Node.js eseguirà l'applicazione. Rispetto all'ambiente browser, dove non puoi permetterti il lusso di scegliere quale browser utilizzeranno i tuoi visitatori, questo è molto comodo.

Ciò significa che puoi scrivere tutto il codice JavaScript moderno ES2015+ supportato dalla tua versione di Node.js. Poiché JavaScript si evolve molto velocemente, ma i browser possono essere un po' lenti ad aggiornarsi, a volte sul web si è costretti a utilizzare vecchie versioni di JavaScript/ECMAScript. Puoi usare Babel per trasformare il tuo codice in modo che sia compatibile con ES5 prima di distribuirlo al browser, ma in Node.js non ne avrai bisogno.

Un'altra differenza è che Node.js supporta sia i sistemi di moduli CommonJS che ES (a partire da Node.js v12), mentre nel browser stiamo iniziando a vedere l'implementazione dello standard ES Modules.

In pratica, ciò significa che è possibile utilizzare sia `require()`e in Node.js, mentre nel browser `import`si è limitati a farlo .`import`