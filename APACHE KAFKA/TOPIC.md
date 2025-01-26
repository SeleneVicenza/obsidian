Canali logici di comunicazione in cui i messaggi vengono pubblicati dai Producer e letti dai consumer. Ciascun topic è identificato da un nome univoco all'interno del cluster Kafka.



-  Divisione in PARTIZIONI ([[Partizione]]) $=>$ distribuzione dei dati r elaborazione parallela
- Durata dei messaggi: possono essere configurati per rimanere disponibili per un determinato periodo di tempo o fino al raggiungimento di una certa dimensione 


#### Creazione 

attraverso "**Kafka command-line tool**" inclusa nell'installazione.

1. **Verificare che il broker Kafka sia in esecuzione**
```bash 
	bin/kafka-server-start.sh config/server.properties 
```
2.  **Creazione $=>$  `kafka-topisc.bat` / `kafka-topisc.sh` (script per creazione, cancellazione e configurazione topic)**
```bash
kafka-topisc.bat -- create --topic [nomeTopic] --bootstrap-server [indirizzoBrokerKafka] --partition [n] --replication-factor [n]
``` 
3. **Verifica creazione**
```bash
kafka-topisc.bat --describe --topic [nomeTopic] --bootstrap-server [indirizzoBrokerKafka]
``` 


