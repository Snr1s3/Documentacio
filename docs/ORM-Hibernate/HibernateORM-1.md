# ORM's: Hibernate  

## Què són els ORM?  
Els **ORM (Object Relational Mapping)** són tècniques que permeten establir una correspondència entre les estructures d'una base de dades relacional i els objectes d'un llenguatge de programació.  

### Correspondències habituals:  
- **Taules** ↔ **Classes/Objectes**  
- **Camps** ↔ **Atributs**  
- **Claus principals** ↔ **Identificadors**  

Els ORM faciliten la persistència d'objectes, permetent que aquests siguin rastrejats i que qualsevol canvi en ells es reflecteixi de manera permanent a la BD.  

## Exemples d'ORM  
- **PHP**: Doctrine, Eloquent (Laravel), Propel  
- **Java**: Hibernate  
- **Python**: SQLAlchemy  

---

## Avantatges i inconvenients dels ORM  
### ✅ Avantatges  
- Redueixen el temps de desenvolupament.  
- Separació de la lògica de negoci de l'accés a dades.  
- Independència del **SGDB** utilitzat.  
- Llenguatge propi per a consultes sobre objectes persistits.  
- Reutilització, escalabilitat i portabilitat del codi.  

### ❌ Inconvenients  
- **Rendiment**: Les consultes han de ser traduïdes i processades, fet que pot afectar la velocitat.  

---

# Hibernate  
Hibernate és un **ORM per Java** que permet la persistència d'objectes a bases de dades mitjançant:  
- **Fitxers XML declaratius**  
- **Anotacions a les classes Java (JavaBeans)**  

Hibernate disposa d'un llenguatge propi anomenat **HQL (Hibernate Query Language)** que permet realitzar consultes sobre objectes, sent el mateix ORM qui les tradueix al SQL específic de cada BD.  

---

## Arquitectura de Hibernate  
1. L'aplicació crea **objectes transitoris** en memòria.  
2. Hibernate utilitza **Sessions** per gestionar la persistència d'aquests objectes.  
3. Les sessions es connecten amb el motor d'Hibernate per emmagatzemar i recuperar objectes de la BD.  
4. Hibernate utilitza **JDBC, JNDI i JTA** per comunicar-se amb la base de dades.  

### Estats de persistència dels objectes  
- **Transient**: Objectes que només existeixen en memòria.  
- **Persistent**: Objectes gestionats per Hibernate i emmagatzemats a la BD.  
- **Detached**: Objectes que han estat persistits però ja no estan associats a una sessió.  
- **Removed**: Objectes eliminats de la BD.  

---

# Configuració de Hibernate amb Maven  
Per començar amb Hibernate, es recomana crear un **projecte Maven** i afegir les següents dependències al `pom.xml`:  

### Hibernate  
```xml
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>6.4.4.Final</version>
</dependency>

<dependency>
    <groupId>org.mariadb.jdbc</groupId>
    <artifactId>mariadb-java-client</artifactId>
    <version>3.5.1</version>
</dependency>
```

# Parts d'un projecte amb Hibernate

## 1. **Estructura general d'un projecte Hibernate**
Un projecte Hibernate conté diversos elements essencials:

- **`HibernateUtil`**: Classe per gestionar la sessió i la configuració d'Hibernate.
- **`hibernate.cfg.xml`**: Fitxer XML amb la configuració de connexió a la base de dades.
- **Classes POJO (Plain Old Java Object)**: Representen les entitats del model de dades.
- **Arxius de mapatge (`*.hbm.xml`)**: Defineixen la relació entre les classes i la base de dades (opcional si s'utilitzen anotacions).
- **Classe principal (`Main` o `App`)**: Executa l'aplicació i gestiona les transaccions.

---

## 2. **Fitxer `hibernate.cfg.xml`**
Aquest fitxer XML configura la connexió a la base de dades. Exemple:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <property name="connection.driver_class">org.mariadb.jdbc.Driver</property>
        <property name="connection.url">jdbc:mariadb://localhost:3306/tasks</property>
        <property name="connection.username">root</property>
        <property name="connection.password">1234</property>
        <property name="dialect">org.hibernate.dialect.MySQLDialect</property>
        <property name="show_sql">true</property>
        <property name="hbm2ddl.auto">update</property>
        <mapping resource="tasks.hbm.xml"/>
    </session-factory>
</hibernate-configuration>
```

## 3. **Fitxer `hibernateUtil.java`**
```java
package com.iticbcn.hibernate;

import org.hibernate.SessionFactory;
import org.hibernate.boot.registry.StandardServiceRegistryBuilder;
import org.hibernate.cfg.Configuration;

public class HibernateUtil {
    private static final SessionFactory sessionFactory = buildSessionFactory();

    private static SessionFactory buildSessionFactory(){
        try {
            return new Configuration().configure().buildSessionFactory(
                new StandardServiceRegistryBuilder().configure().build()
            );
        } catch (Throwable ex) {
            System.err.println("Error inicialitzant Hibernate: " + ex);
            throw new ExceptionInInitializerError(ex);
        }
    }

    public static SessionFactory getSessionFactory() {
        return sessionFactory;
    }
}

```
## 4. **Fitxer `main.java`**
```java
package com.iticbcn.hibernate;

import org.hibernate.Session;
import org.hibernate.SessionFactory;

public class Main {
    public static void main(String[] args) {
        SessionFactory sesion = HibernateUtil.getSessionFactory();
        Session session = sesion.openSession();
        session.beginTransaction();
        
        System.out.println("Hola des de Hibernate");

        session.getTransaction().commit();
        session.close();
    }
}

```

## 5.1. **Fitxer `Tasks.java`**
```java
package com.iticbcn.hibernate.model;
import java.io.Serializable;

public class Tasks implements Serializable {
    private int idTask;
    private String descTask;
    private int numHours;
    private boolean finalitzada;
    
    public Tasks() {}

    public Tasks(String descTask, int numHours, boolean finalitzada) {
        this.descTask = descTask;
        this.numHours = numHours;
        this.finalitzada = finalitzada;
    }

    public int getIdTask() { return idTask; }
    public void setIdTask(int idTask) { this.idTask = idTask; }

    public String getDescTask() { return descTask; }
    public void setDescTask(String descTask) { this.descTask = descTask; }

    public int getNumHours() { return numHours; }
    public void setNumHours(int numHours) { this.numHours = numHours; }

    public boolean isFinalitzada() { return finalitzada; }
    public void setFinalitzada(boolean finalitzada) { this.finalitzada = finalitzada; }

    @Override
    public String toString() {
        return "Tasks [idTask=" + idTask + ", descTask=" + descTask + ", numHours=" + numHours + ", finalitzada=" + finalitzada + "]";
    }
}

```
## 5.2. **Fitxer `tasks.hbm.xml`**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
    <class name="com.iticbcn.hibernate.model.Tasks" table="tasks">
        <id name="idTask" column="idTask">
            <generator class="identity"/>
        </id>
        <property column="descTask" name="descTask" />
        <property column="numHours" name="numHours" />
        <property column="finalitzada" name="finalitzada" />
    </class>
</hibernate-mapping>

```
## 6. **Fitxer `main.java amb persist`**
```java
package com.iticbcn.hibernate;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import com.iticbcn.hibernate.model.Tasks;

public class App {
    public static void main(String[] args) {
        SessionFactory sesion = HibernateUtil.getSessionFactory();
        Session session = sesion.openSession();
        session.beginTransaction();

        Tasks t = new Tasks("Donar la Benvinguda", 1, true);
        session.persist(t);

        session.getTransaction().commit();
        session.close();
    }
}
```
## 7. **Fitxer `Output`**
```SQL
Hibernate: create table tasks (
    idTask integer not null auto_increment, 
    descTask varchar(255), 
    numHours integer, 
    finalitzada bit, 
    primary key (idTask)
) engine=InnoDB
tasks [idTask=0, descTask=Donar la Benvinguda, numHours=1, finalitzada=true]
Hibernate: insert into tasks (descTask, finalitzada, numHours) values (?, ?, ?)

```
