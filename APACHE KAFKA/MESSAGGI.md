I dati vengono rappresentati come **messaggi**. 

Ogni messaggio è composto da una **chiave** (opzionale), un **valore**, un **timestamp** (facoltativo) e metadati aggiuntivi come l'offset e le intestazioni. 
A ciascun messaggio viene assegnato un [[offset]] nella partizione.

Kafka tratta ogni messaggio come un'<mark style="background: #FF5582A6;">unità immutabile di dati</mark> scritta su una partizione di un [[TOPIC]] sotto forma di blocco di dati in formato binario     $=>$     Il [[PRODUCER]] **serializza** il messaggio prima di inviarlo, mentre il [[CONSUMER]] lo **deserializza** quando lo riceve.

##### ESEMPIO SERIALIZZAZIONE
`props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer"); props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");`

##### ESEMPIO SERIALIZZAZIONE
