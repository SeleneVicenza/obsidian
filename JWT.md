Standard aperto per lo scambio sicuro di informazioni tra due parti sotto forma di un oggetto JSON firmato digitalmente; abilita **autenticazione/autorizzazione stateless**.

Il server genera e cripta il token, il browser/client lo mantiene in memoria (localStorage) e lo rimanda al server ad ogni richiesta.


### 1. Autenticazione

- L’utente invia **username/password**.
- Il server valida e genera un JWT.
- Il client lo memorizza (tipicamente in **localStorage** o **cookie HTTPOnly**).

### 2. Autorizzazione

- Ad ogni richiesta protetta → il client invia il token nell’header:
``` 
    Authorization: Bearer <jwt>
```

- Il server valida firma e scadenza → se valido, autorizza.

---
# Struttura

Un token JWT è composto da **tre parti**, codificate in Base64 e separate da punti (`.`):

``` css
header.payload.signature
```
### 1. Header

Definisce il tipo di token e l’algoritmo di firma.

``` JSON
   {"alg": "HS256",   "typ": "JWT" }
```

### 2. Payload

Contiene le **claims** (informazioni):
- **Registered claims** (standard, es. `iss`, `exp`, `sub`).
- **Public claims** (definiti dall’app).
- **Private claims** (convenzioni interne).


``` JSON 
{   "sub": "1234567890",   "name": "Selene",   "role": "admin",   "iat": 1516239022,   "exp": 1516242622 }`
```

### 3. Signature

Serve a verificare integrità e autenticità.

``` SCSS

HMACSHA256(   base64UrlEncode(header) + "." + base64UrlEncode(payload),   secret ) 

```

---
