Node.js è un **ambiente di runtime JavaScript** open source e multipiattaforma.
-  **esegue il motore JavaScript V8**, il cuore di Google Chrome, **esternamente al browser** $->$ molto performante.

Un'app Node.js viene eseguita in un **singolo processo**, senza creare un nuovo thread per ogni richiesta. 

- Node.js fornisce un set di **primitive di I/O asincrone** nella sua libreria standard che impediscono il blocco del codice JavaScript. Inoltre, le librerie in Node.js sono generalmente scritte utilizzando paradigmi non bloccanti. Di conseguenza, il **comportamento bloccante è l'eccezione** piuttosto che la norma in Node.js.

Quando Node.js esegue un'operazione di I/O, come la lettura dalla rete, l'accesso a un database o al file system, invece di bloccare il thread e sprecare cicli di CPU in attesa, Node.js riprenderà le operazioni quando riceverà la risposta $->$ gestire **migliaia di connessioni simultanee con un singolo server** senza dover gestire la concorrenza dei thread, che potrebbe essere una fonte significativa di bug.

## ESEMPIO di APPLICAZIONE

- Server web "Hello World"
``` js

const { createServer } = require('node:http');

const hostname = '127.0.0.1';
const port = 3000;

const server = createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});

```

Per eseguire il codice, è sufficiente salvare il file come nome.js ed lanciare da terminale il comando `node nome.js`. 

con la versione mjs del codice, è necessario salvarlo come `nome.mjs`file ed eseguirlo `node nome.mjs`nel terminale.

## Analisi del codice
Questo codice include innanzitutto il [`http`modulo](https://nodejs.org/api/http.html) Node.js.

Il `createServer()`metodo `http`crea un nuovo server HTTP e lo restituisce.

Il server è impostato per restare in ascolto sulla porta e sul nome host specificati. Quando il server è pronto, viene chiamata la funzione di callback, in questo caso per informarci che il server è in esecuzione.

Ogni volta che viene ricevuta una nuova richiesta, l'evento [`request`](https://nodejs.org/api/http.html#http_event_request) viene richiamato, fornendo due oggetti: una richiesta (un [`http.IncomingMessage`](https://nodejs.org/api/http.html#http_class_http_incomingmessage)oggetto) e una risposta (un [`http.ServerResponse`](https://nodejs.org/api/http.html#http_class_http_serverresponse)oggetto).

Questi 2 oggetti sono essenziali per gestire la chiamata HTTP.

Il primo fornisce i dettagli della richiesta. 
In questo semplice esempio, questo non viene utilizzato, ma è possibile accedere alle intestazioni della richiesta e ai dati della richiesta.

Il secondo viene utilizzato per restituire i dati al chiamante.