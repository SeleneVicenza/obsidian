**Formati di rappresentazione dei dati**, spesso usati nei database non relazionali

## JSON (JavaScript Object Notation)

- **Testuale**.
- **Formato standardizzato** e indipendente dalla piattaforma.
- **Utilizzato per scambio dati tra sistemi** (API REST, file di configurazione, ecc.).
- Supporta tipi base: stringa, numero, booleano, null, array, oggetto.

``` json

{
  "nome": "Selene",
  "eta": 30,
  "skills": ["PHP", "Node.js", "Kotlin"],
  "attivo": true
}

```

## BSON (Binary JSON)

- **Binario** → non leggibile dall’uomo, ottimizzato per la macchina.
- È un’estensione di JSON sviluppata per **MongoDB**.
- Supporta tipi di dato **aggiuntivi** che JSON non ha:
    - `Date` (data nativa)
    - `ObjectId` (identificativo univoco a 12 byte)
    - `BinData` (dati binari)
    - `Decimal128` (numeri a precisione estesa)
    
- **Più veloce** per lettura/scrittura in database perché non deve fare parsing testuale.
- **Occupa più spazio** rispetto a JSON (include metadati come la lunghezza dei campi).


``` js
<document>
  nome: "Selene"
  eta: 30
  skills: ["PHP", "Node.js", "Kotlin"]
  attivo: true
</document>

```




## JSON vs BSON

|Caratteristica|JSON (testuale)|BSON (binario)|
|---|---|---|
|**Formato**|Testo leggibile dall’uomo|Binario leggibile solo dalla macchina|
|**Tipi supportati**|Limitati (stringa, numero, booleano, array, oggetto, null)|Estesi (Date, ObjectId, Decimal128, BinData, ecc.)|
|**Efficienza**|Parsing più lento|Parsing più veloce, ottimizzato per DB|
|**Dimensione**|Più compatto|Più grande (include metadati e lunghezze campi)|
|**Uso tipico**|API, file di configurazione, scambio dati|Storage interno in MongoDB, sistemi che richiedono velocità|
