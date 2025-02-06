**Tool di build e gestione dei progetti** basato su Java, utilizzato principalmente per la gestione delle dipendenze, la compilazione, il packaging e la distribuzione delle applicazioni.

È stato sviluppato dalla **Apache Software Foundation** e si basa su un concetto chiamato **Project Object Model (POM)**, che descrive la configurazione e le dipendenze del progetto in un file XML (`pom.xml`).

## **Funzionalità Principali di Maven**

- **Gestione delle dipendenze** → Automatizza il download e l'aggiornamento delle librerie.  
-  **Build automation** → Compila, testa e pacchettizza il codice automaticamente.  
-  **Gestione dei progetti multi-modulo** → Permette di organizzare grandi progetti in più moduli.  
- **Standardizzazione** → Impone una struttura di progetto chiara e predefinita.  
- **Supporto ai plugin** → Estensibile grazie a numerosi plugin ufficiali e di terze parti.


Maven segue una struttura basata su **ciclo di vita del build** e **dipendenze gestite automaticamente**.

### **Struttura Base di un Progetto Maven**

``` bash
my-project 
│── src 
│   ├── main 
│   │   ├── java          # Codice sorgente 
│   │   ├── resources     # Risorse (file di configurazione, etc.) 
│   ├── test 
│   │   ├── java          # Test JUnit 
│── pom.xml               # File di configurazione principale 
│── target                # Output della build
```


### **Il File `pom.xml` (Project Object Model)**

Il file `pom.xml` definisce:

- **Dipendenze** del progetto
- **Plugin** per estendere le funzionalità
- **Configurazioni** per la build
- **Fasi del ciclo di vita**

Esempio di un `pom.xml` base:

``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0"          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"          xsi:schemaLocation="http://maven.apache.org/POM/4.0.0           http://maven.apache.org/xsd/maven-4.0.0.xsd">         
	<modelVersion>4.0.0</modelVersion>      
	<groupId>com.example</groupId>    
	<artifactId>my-app</artifactId>     
	<version>1.0-SNAPSHOT</version>     
	<packaging>jar</packaging>      
	<dependencies>         <!-- Dipendenza per Spring Boot -->         
		<dependency>             
			<groupId>org.springframework.boot</groupId>            
			 <artifactId>spring-boot-starter-web</artifactId>            
			 <version>3.1.0</version>         
		</dependency>     
	</dependencies>  
</project>
```


## 🔹 **Comandi Base di Maven**

Maven utilizza una serie di **comandi CLI** per gestire il ciclo di vita del progetto:

|Comando|Descrizione|
|---|---|
|`mvn clean`|Pulisce i file temporanei della build|
|`mvn compile`|Compila il codice sorgente|
|`mvn test`|Esegue i test|
|`mvn package`|Crea un JAR o WAR del progetto|
|`mvn install`|Installa il pacchetto localmente|
|`mvn deploy`|Distribuisce il pacchetto in un repository remoto|

## 🔹 **Gestione delle Dipendenze in Maven**

Maven scarica automaticamente le librerie richieste da **Maven Central Repository** e le salva nella cartella locale (`~/.m2/repository`).

Esempio:  
Se vuoi aggiungere **JUnit** come dipendenza, basta aggiungere questo al `pom.xml`:

``` xml
<dependency>
	<groupId>junit</groupId>     
	<artifactId>junit</artifactId>     
	<version>4.13.2</version>     
	<scope>test</scope> 
</dependency>
```
Dopo aver aggiornato il `pom.xml`, esegui:

``` sh
`mvn clean install`
```
e Maven scaricherà automaticamente la libreria necessaria.

--------------------------------------------------------------------------

## 🔹 **Conclusione**

Maven è un potente tool per la gestione di progetti Java, rendendo più semplice il **controllo delle dipendenze, la compilazione e il deploy**. 
Grazie alla sua struttura standard e al supporto per i plugin, è uno strumento essenziale nello sviluppo Java moderno.