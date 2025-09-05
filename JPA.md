**JPA** (<span style="color:rgb(242, 98, 2)">Java Persistence API</span>) è una specifica Java che definisce un'API per la gestione della persistenza dei dati in applicazioni Java. In altre parole, JPA è un [[ORM]] per Java, che consente di mappare oggetti Java (entità) su tabelle di un database relazionale, semplificando l'interazione tra il codice dell'applicazione e il database stesso.

JPA fornisce un modo standardizzato di lavorare con i dati, senza dover scrivere manualmente SQL per ogni operazione. Le operazioni di persistenza (come il salvataggio, la lettura, l'aggiornamento e l'eliminazione di dati) possono essere eseguite tramite l'uso di oggetti Java.

### Caratteristiche principali di JPA:

1. **<span style="color:rgb(238, 150, 226)">Entità</span>**: Le entità sono le classi Java che rappresentano le tabelle del database. Ogni entità è associata a una tabella e ogni istanza della classe rappresenta una riga della tabella.
    
2. **<span style="color:rgb(238, 150, 226)">Entity</span> <span style="color:rgb(238, 150, 226)">Manager</span>**: L'`EntityManager` è l'interfaccia principale di JPA per interagire con il database. Gestisce il ciclo di vita delle entità e fornisce metodi per persistere, caricare, aggiornare ed eliminare oggetti.
    
3. <span style="color:rgb(238, 150, 226)">**Query JPQL</span> (Java Persistence Query Language)**: JPQL è un linguaggio di query orientato agli oggetti che JPA utilizza per eseguire ricerche su entità, simile a SQL ma progettato per lavorare con oggetti Java. Le query sono eseguite su entità e non direttamente sulle tabelle del database.
    
4. **<span style="color:rgb(238, 150, 226)">Mapping delle Entità</span>**: JPA usa annotazioni (come `@Entity`, `@Id`, `@Column`, ecc.) per definire come le classi Java sono mappate alle tabelle del database e come i campi delle classi sono mappati alle colonne.
    
5. **<span style="color:rgb(238, 150, 226)">Gestione delle Transazioni</span>**: JPA gestisce le transazioni automaticamente o tramite il codice, permettendo di eseguire operazioni di lettura e scrittura in modo sicuro.
    

### Esempio di uso di JPA:

Supponiamo di avere una classe `Person` che rappresenta una tabella "person" in un database:

```java
import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class Person {

    @Id
    private Long id;
    private String name;
    private int age;

    // Getters and setters
}
```

In questo caso, la classe `Person` è un'entità che rappresenta una tabella nel database. Con JPA, possiamo utilizzare `EntityManager` per salvare un'istanza della classe `Person` nel database:

```java
import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;

public class Main {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("myJpaUnit");
        EntityManager em = emf.createEntityManager();

        em.getTransaction().begin();
        
        // Creiamo un nuovo oggetto Person
        Person person = new Person();
        person.setId(1L);
        person.setName("John");
        person.setAge(30);
        
        // Persistiamo l'oggetto nel database
        em.persist(person);
        
        em.getTransaction().commit();
        em.close();
    }
}
```

In questo esempio, `EntityManager` gestisce il ciclo di vita dell'oggetto `Person`, persiste l'oggetto nel database e gestisce le transazioni.

### Framework che implementano JPA:

- **Hibernate**: Il più popolare framework che implementa JPA. Molti progetti Java, come quelli basati su Spring, utilizzano Hibernate come implementazione di JPA.
- **EclipseLink**: framework di implementazione JPA.
- **OpenJPA**: framework  sviluppato da Apache.

JPA fornisce quindi un approccio standardizzato per la persistenza dei dati, riducendo la complessità rispetto alla gestione manuale delle query SQL e rendendo il codice Java più elegante e facilmente manutenibile.