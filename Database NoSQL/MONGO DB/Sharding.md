tecnica di **partizionamento orizzontale** dei dati:

- I documenti di una collezione vengono distribuiti su più nodi (**shard**) per gestire dataset molto grandi e carichi elevati, ma l’insieme appare come **un unico database**.


## Shard Key

- campo (o campi) che decide **come i documenti sono distribuiti**.

- Va scelta con attenzione:
    - **Alta cardinalità** (molti valori distinti).
    - **Distribuzione uniforme** dei dati.
    - **Compatibilità con le query più frequenti** (per evitare [[scatter-gather]]).