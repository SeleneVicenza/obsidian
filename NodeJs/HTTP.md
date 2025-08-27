Modulo nativo di Node js che consente di:
- **Creare un server**
- **Creare un client** 

Poiché il modulo nativo utilizza un approccio **low-level**, è compito del programmatore occuparsi di:
- interpretare l’oggetto `req` (stream leggibile);
- scrivere nell’oggetto `res` (stream scrivibile);
- gestire header, status code e body.

Diversi framework, basati interamente su questo modulo, possono semplificarne l'implementazione:
- [[Express]]  
- Nest, 
- Fastify



# Creazione del server

Un server HTTP in Node:

- è un **oggetto** che **ascolta una porta TCP**;
- riceve una `request` (flusso dati in entrata, con metodo, url, header e body);
- produce una `response` (flusso dati in uscita, con header, status code e body).


``` node

const http = require('http');
const url = require('url');
const fs = require('fs');
const axios = require('axios');
const {WEATHER_API} =  require('./constants');

http.createServer((req, resp) =>{
    const {pathname, query} = url.parse(req.url, true);

     switch (pathname) {
         case '/':
          const index = fs.createReadStream('./index.html');
             resp.writeHead(200,{'Conten-Type' : 'text/html'});
             index.pipe(resp);
             break;
         case '/getWeather':
             console.log('getWeatherapi');
             const params = {};
             if(query.city && !query.zip    ){
                 params.q = query.city;
             }
             if(query.zip){
                 params.zip = query.zip + ',IT';
             }
             if(query.lang){
                 params.lang = query.lang;
             }
             console.log(params);
             axios.get(WEATHER_API,{
                  params

             } ).then(weather =>{
                 resp.writeHead(200,{'Content-Type' : 'application/json'});
                resp.end(JSON.stringify(weather.data));

             })
                 .catch( error =>{
                     console.log(error.toString());
                     resp.writeHead(500);
                     resp.end(error.response.data.message);
                 });
             break;
         default:
             resp.writeHead(400);
             resp.end('BAD REQUEST');
     }

  //  resp.end('You called me with ' + req.url)

}).listen(2000);
console.log('Listening');

```

Per le chiamate API è più semplice utilizzare [[AXIOS]]





# Definizione del client

Un client HTTP deve:
- aprire una connessione TCP;
- mandare un pacchetto con `method`, `path`, `headers` e `body`;
- ricevere la risposta e leggerla a chunk.

``` node

const request = require('request');`
`request('http://localhost:2000', (err, response, body) =>{`
  `console.log(body);`

`});

```


