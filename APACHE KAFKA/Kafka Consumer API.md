Consente di leggere i dati da uno o più [[TOPIC]]. 

I consumatori Kafka sono raggruppati in gruppi (**consumer group**). Questo consente di scalare l'elaborazione dei messaggi in modo efficiente.

Ogni gruppo di consumatori garantisce che ogni messaggio venga letto una sola volta per gruppo, ma più gruppi possono leggere dallo stesso [[TOPIC]] indipendentemente. 
#### Funzionamento della Kafka Consumer API
Un consumatore Kafka si iscrive a uno o più [[TOPIC]] e legge i messaggi da una o più partizioni. Kafka assegna automaticamente le partizioni ai consumatori, garantendo che ogni partizione venga assegnata a un solo consumatore per gruppo. 

Il consumatore traccia l'**[[offset]]** dei messaggi che ha letto in una partizione, cioè la posizione corrente di lettura. Kafka memorizza l'[[offset]] per ogni consumatore, in modo che, in caso di riavvio del consumatore, possa riprendere a leggere dall'offset corretto.


#### Funzionalità aggiuntive della Kafka Consumer API

- **Commit degli offset**: 
	I consumatori Kafka possono eseguire il commit automatico o manuale degli offset. Il commit manuale è utile quando si vuole garantire che i messaggi siano stati elaborati correttamente prima di avanzare l'offset. 

	```java props.put("enable.auto.commit", "true"); // Commit automatico ``` 
	Per il commit manuale degli offset: 
	```java consumer.commitSync(); // Commit manuale ```

-  **Consumer Group**: 
	I consumatori possono far parte di un gruppo di consumatori. 
	Ogni gruppo di consumatori condivide il carico di lettura dei messaggi da un topic. Se ci sono più consumatori in un gruppo rispetto alle partizioni, alcuni consumatori rimarranno inattivi. 
	
- **Rebilanciamento dei consumatori**: 
	Kafka gestisce automaticamente il rebilanciamento dei consumatori in un gruppo se un consumatore si aggiunge o si disconnette. Durante il rebilanciamento, le partizioni vengono riassegnate ai consumatori disponibili.

