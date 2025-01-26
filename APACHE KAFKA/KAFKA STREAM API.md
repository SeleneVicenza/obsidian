Interfaccia potente e di alto livello che consente l'elaborazione in streaming dei dati direttamente all'interno di Kafka. 

Questa API consente di trasformare, aggregare e unire flussi di dati provenienti da uno o più topic in tempo reale. 

Kafka Streams API è progettata per essere semplice da usare e scalabile, consentendo agli sviluppatori di costruire applicazioni in streaming in modo nativo su Kafka, senza la necessità di gestire framework esterni o infrastrutture complesse. 

#### Funzionamento della Kafka Streams API 
Concetti principali: 
1. **KStream**: Rappresenta un flusso di record, dove ogni record è elaborato singolarmente e immediatamente. È simile a un normale stream di dati che viene continuamente elaborato. 
2. **KTable**: Rappresenta una tabella di dati che può essere aggiornata in tempo reale, dove ogni record rappresenta un aggiornamento di un'aggregazione o uno stato.

#### Esempio di Kafka Streams API in Java 
```java 
import org.apache.kafka.streams.KafkaStreams; 
import org.apache.kafka.streams.StreamsBuilder; 
import org.apache.kafka.streams.kstream.KStream; 
import org.apache.kafka .streams.StreamsConfig; 
import java.util.Properties; 
public class KafkaStreamsExample { 
	public static void main(String[] args) { 
	// Configurazione delle proprietà di Kafka Streams 
		Properties props = new Properties(); 
		props.put(StreamsConfig.APPLICATION_ID_CONFIG, "stream-processing-app"); 
		props.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092"); 
		props.put(StreamsConfig.DEFAULT_KEY_SERDE_CLASS_CONFIG, "org.apache.kafka.common.serialization.Serdes$StringSerde"); 
		props.put(StreamsConfig.DEFAULT_VALUE_SERDE_CLASS_CONFIG, "org.apache.kafka.common.serialization.Serdes$StringSerde"); 
		
		// Creazione di un builder per lo stream 
		StreamsBuilder builder = new StreamsBuilder(); 
		
		// Definizione del flusso di dati 
		KStream<String, String> source = builder.stream("ordine-acquisti"); 
		
		// Esempio di trasformazione: filtrare solo ordini di prodotti specifici 
		source.filter((key, value) -> value.contains("prodotti")).to("ordini-filtrati"); 
		// Costruzione e avvio del flusso 
		KafkaStreams streams = new KafkaStreams(builder.build(), props); 
		streams.start(); 
	}
}
```





#### Funzionalità avanzate di Kafka Streams API 
-  **Stato locale**: 
	Kafka Streams supporta lo stato locale per l'elaborazione stateful (con stato), che consente di mantenere dati in memoria durante l'elaborazione di flussi. 
-  **KTable aggregations**: 
	È possibile aggregare i dati in tempo reale utilizzando KTable, che permette di mantenere uno stato aggiornato dei dati aggregati. - **Joining**: Kafka Streams supporta join tra flussi e tabelle, permettendo di unire dati da diverse sorgenti in tempo reale. 
-  **Fault Tolerance**: 
	Kafka Streams è fault-tolerant per design, il che significa che, in caso di failure, può recuperare lo stato dal log Kafka stesso.
