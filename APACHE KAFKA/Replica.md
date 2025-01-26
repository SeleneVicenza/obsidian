Kafka implementa la **replica** dei dati per garantire la disponibilità e la tolleranza ai guasti.

Ogni partizione di un topic ha una o più repliche distribuite su più broker. Il numero di repliche per una partizione è definito dalla configurazione del topic al momento della creazione. 

In un sistema di replica, Kafka distingue tra il **leader** e i **replica follower**:

-  **Leader**: Ogni partizione ha un broker leader che gestisce tutte le operazioni di lettura e scrittura per quella partizione.

-  **Follower**: Le repliche follower copiano i dati dal leader e rimangono sincronizzate. Se il leader fallisce, uno dei follower viene promosso a nuovo leader per garantire la continuità operativa. 

Tolleranza ai guasti 

 La **tolleranza ai guasti** in Kafka è garantita attraverso il meccanismo di replica e l'algoritmo di consenso [[ZooKeeper]] (o Raft, nelle versioni più recenti). 
 Se un broker fallisce, Kafka promuove un nuovo leader tra le repliche e continua a elaborare i messaggi senza interruzioni significative. 
 
 Kafka consente anche di configurare il livello di **durabilità** dei dati, permettendo di specificare quanti follower devono confermare la ricezione di un messaggio prima che Kafka consideri la scrittura completata.

