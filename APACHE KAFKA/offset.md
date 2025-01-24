L'**offset** è un identificatore univoco associato a ciascun messaggio all'interno di una partizione. Kafka assegna un numero di offset crescente a ogni messaggio man mano che viene scritto in una partizione. 
Gli offset vengono utilizzati dai consumatori per tracciare la posizione di lettura corrente all'interno di una partizione. 
In altre parole, l'offset indica fino a che punto un consumatore ha letto i messaggi da una partizione. Ogni consumatore tiene traccia del proprio offset in modo indipendente. Questo consente ai consumatori di leggere dallo stesso topic senza interferire l 'uno con l'altro.  


### Commit degli offset 
Il **commit dell'offset** è il processo mediante il quale un consumatore comunica a Kafka che ha elaborato con successo un messaggio fino a un determinato offset. Esistono due modalità principali di commit degli offset: 
1. **Commit automatico**: Kafka può essere configurato per eseguire automaticamente il commit degli offset a intervalli regolari. Questo è utile per applicazioni che non hanno bisogno di controllare manualmente l'avanzamento della lettura.

Esempio di configurazione: ```java props.put("enable.auto.commit", "true"); props.put("auto.commit.interval.ms", "1000"); ``` 

3. **Commit manuale**: In alcuni casi, è necessario un maggiore controllo sul commit degli offset. Questo può essere utile quando l'applicazione deve garantire che i messaggi siano stati elaborati correttamente prima di eseguire il commit. In questo caso, il commit degli offset viene gestito manualmente.
Esempio di commit manuale: ```java consumer.commitSync(); ```

<mark style="background: #FFB8EBA6;">Il commit manuale è essenziale nei casi in cui l'applicazione deve garantire che i dati siano stati elaborati correttamente prima di aggiornare l'offset.</mark>


