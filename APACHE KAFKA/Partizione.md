- **Definizione:** unità logiche di divisione dei dati per supportare la scalabilità e il parallelismo.
- **Funzione:** consentono di distribuire i dati tra più broker nel cluster Kafka.


- Ogni messaggio che arriva in un [[TOPIC]] viene assegnato a una specifica partizione. Le partizioni garantiscono l'**ordinamento dei messaggi**, ma solo all'interno di ciascuna partizione. Se un [[TOPIC]] ha più partizioni, non esiste un ordine globale dei messaggi tra di esse, ma ogni partizione ha un proprio ordine.



- **scalabilità orizzontale**: aggiungendo nuove partizioni, possiamo distribuire il carico tra più broker Kafka e ottenere un sistema che scala in modo dinamico con l'aumento del volume di dati.

==**NB:**== Il partizionamento dei dati dipende da una funzione di hash della chiave del messaggio o può essere distribuito in modo [[round-robin]], se non è specificata una chiave.

 