# Hibernate: Mapatge de Relacions amb Anotacions  

## Introducció  
Les anotacions Java permeten definir el mapatge d’una entitat Hibernate sense necessitat d’un fitxer `.hbm.xml`.  

## Exemple de Classe amb Anotacions  
```java
package com.hibernate.iticbcn.model;

import java.io.Serializable;
import java.time.LocalDate;
import jakarta.persistence.*;

@Entity
@Table(name="employees")
public class Empleats implements Serializable {
    @Id
    @GeneratedValue(strategy= GenerationType.IDENTITY)
    @Column(name="emp_no")
    private int numeroEmpleat;
    
    @Column(name="birth_date")
    private LocalDate dataNaixement;
    
    @Column(name="first_name")
    private String nom;
    
    @Column(name="last_name")
    private String cognoms;
    
    @Column(name="gender")
    private char genere;
    
    @Column(name="hire_date")
    private LocalDate dataAlta;

    public Empleats(LocalDate dataNaixement, String nom, String cognoms, char genere, LocalDate dataAlta) {
        this.dataNaixement = dataNaixement;
        this.nom = nom;
        this.cognoms = cognoms;
        this.genere = genere;
        this.dataAlta = dataAlta;
    }
}
```

## **Fitxer `hibernateUtil.java`**
```java
return new Configuration().configure().addAnnotatedClass(Empleats.class)
    .buildSessionFactory(new StandardServiceRegistryBuilder().configure().build());
```
## **Fitxer `hibernate.cfg.xml`**
```xml
<session-factory>
    <mapping class="com.hibernate.iticbcn.model.Empleats"/>
</session-factory>
```
## **`Inserir un Empleat`**
```java
SessionFactory sesion = HibernateUtil.getSessionFactory();
Session session = sesion.openSession();
session.beginTransaction();

Empleats e = new Empleats(LocalDate.of(1970, Month.APRIL,3),"Pere", "Martinez", 'M', LocalDate.now());

session.persist(e);
session.getTransaction().commit();
session.close();

```
## **`Resultat de l'Execució`**
```sql
Hibernate: create table employees (
    emp_no integer not null auto_increment, 
    birth_date date, 
    last_name varchar(255), 
    hire_date date, 
    gender char(1), 
    first_name varchar(255), 
    primary key (emp_no)
) engine=InnoDB;

Hibernate: insert into employees (birth_date,last_name,hire_date,gender,first_name) values (?,?,?,?,?);

```
##  **`Base de Dades`**
```sql
MariaDB [tasks]> select * from employees;
+--------+------------+-----------+------------+--------+------------+
| emp_no | birth_date | last_name | hire_date  | gender | first_name |
+--------+------------+-----------+------------+--------+------------+
|      1 | 1980-04-03 | Martin    | 2024-03-05 | M      | Pablo      |
|      2 | 1970-04-03 | Martinez  | 2024-03-11 | M      | Pere       |
+--------+------------+-----------+------------+--------+------------+
```

