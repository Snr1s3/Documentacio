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

# Mapatge de Relacions a Hibernate

A Hibernate, les relacions entre entitats poden ser **unidireccionals** o **bidireccionals**.

## 1. Relacions Unidireccionals: `@OneToOne`

En una relació unidireccional, només una entitat coneix l'altra.  
Exemple: Un empleat té assignat un despatx, però el despatx no coneix l'empleat.

### **Classe `Despatx` (Entitat sense referència a `Empleats`)**
```java
@Entity
@Table(name="despatxos")
public class Despatx implements Serializable {
    @Id
    @Column(name="off_no")
    private int idDespatx;

    @Column(name="location")
    private String lloc;

    public Despatx() {}

    public Despatx(int idDespatx, String lloc) {
        this.idDespatx = idDespatx;
        this.lloc = lloc;
    }

    public int getIdDespatx() { return idDespatx; }
    public void setIdDespatx(int idDespatx) { this.idDespatx = idDespatx; }
    public String getLloc() { return lloc; }
    public void setLloc(String lloc) { this.lloc = lloc; }
}
```
### **Classe `Empleats` (Amb referència a `Despatx`)**

```java
@Entity
@Table(name="empleats")
public class Empleats implements Serializable {
    @Id
    @GeneratedValue(strategy= GenerationType.IDENTITY)
    @Column(name="emp_no")
    private int numeroEmpleat;

    @Column(name="emp_name")
    private String nomEmpleat;

    @OneToOne(cascade=CascadeType.ALL)
    @JoinColumn(name="despatx", referencedColumnName="off_no", updatable=false)
    private Despatx despatx;

    public Empleats() {}

    public Empleats(String nomEmpleat, Despatx despatx) {
        this.nomEmpleat = nomEmpleat;
        this.despatx = despatx;
    }

    public int getNumeroEmpleat() { return numeroEmpleat; }
    public void setNumeroEmpleat(int numeroEmpleat) { this.numeroEmpleat = numeroEmpleat; }
    public String getNomEmpleat() { return nomEmpleat; }
    public void setNomEmpleat(String nomEmpleat) { this.nomEmpleat = nomEmpleat; }
    public Despatx getDespatx() { return despatx; }
    public void setDespatx(Despatx despatx) { this.despatx = despatx; }
}
```
## 2. Propietat `cascade`

Defineix les accions permeses sobre l'entitat relacionada:

- `CascadeType.ALL`: Aplica totes les accions (`INSERT`, `UPDATE`, `DELETE`).
- `CascadeType.PERSIST`: Només permet insercions.
- `CascadeType.MERGE`: Només permet actualitzacions.
- `CascadeType.REMOVE`: Esborra l'entitat relacionada.
- `CascadeType.DETACH`: Desvincula l'entitat sense esborrar-la.

---

## 3. Relacions Bidireccionals

En una relació bidireccional, ambdues entitats tenen referències l'una a l'altra.

### **Exemple `@OneToOne`: Empleat i Vehicle**
Un empleat té un vehicle d'empresa, però aquest vehicle pot canviar d'empleat.

### **Classe `Empleat`**
```java
@Entity
@Table(name="employees")
public class Empleat implements Serializable {
   @Id
   @GeneratedValue(strategy= GenerationType.IDENTITY)
   @Column(name="emp_no")
   private int numeroEmpleat;

   @OneToOne(cascade=CascadeType.ALL)
   @JoinColumn(name="vehicle", referencedColumnName="matricula", updatable=false)
   private Vehicle vehicle;

   public Empleat() {}

   public Empleat(int numeroEmpleat, Vehicle vehicle) {
       this.numeroEmpleat = numeroEmpleat;
       this.vehicle = vehicle;
   }
}
```
### **Classe `Vehicle`**
```java
@Entity
@Table(name="cars")
public class Vehicle {
   @Id
   private String matricula;

   @OneToOne(mappedBy="vehicle")
   private Empleat empleat;

   public Vehicle() {}

   public Vehicle(String matricula, Empleat empleat) {
       this.matricula = matricula;
       this.empleat = empleat;
   }
}
```
### **Persist**
```java
Session session = HibernateUtil.getSessionFactory().openSession();
session.beginTransaction();

Empleats e = new Empleats("Pere", "Martinez");
Vehicles v = new Vehicles("1234KJK", "Peugeot 3008", e);

session.persist(v); // Persistim vehicle, que conté l'empleat

session.getTransaction().commit();
session.close();

```

# OneToMany i ManyToOne

Podeu trobar el codi complet a `tasks-OneToMany`.

Si bé es poden emprar de forma unidireccional, anem a veure-les com bidireccionals.

En aquest cas considerem l'autor que ha escrit molts llibres:

## **Exemple OneToMany i ManyToOne**

### **Classe `Llibre`**
```java
@Entity
@Table(name="Books")
public class Llibre implements Serializable {
   @Id
   @GeneratedValue(strategy= GenerationType.IDENTITY)
   private int idLlibre;

   @ManyToOne(cascade=CascadeType.PERSIST)
   @JoinColumn(name="idAutor", foreignKey = @ForeignKey(name="FK_LIB_AUT"))
   private Autor autor;
}
```

### **Classe `Autor`**
```java
@Entity
@Table(name="Authors")
public class Autor implements Serializable {
   @OneToMany(mappedBy="autor", cascade=CascadeType.PERSIST, fetch=FetchType.LAZY)
   private Set<Llibre> llibres;
}
```
# **Implementació dins de l'aplicació**

## **OneToMany i ManyToOne**

En aquest exemple, es creen i persisteixen un autor i els seus llibres utilitzant Hibernate.

```java
public class App {
    public static void main( String[] args ) {
        SessionFactory sesion = HibernateUtil.getSessionFactory();
        Session session = sesion.openSession();
        session.beginTransaction();

        Autor a = new Autor("Juan Gomez-Jurado", "ESP");

        Llibre l1 = new Llibre("El paciente", "Thriller", a);
        Llibre l2 = new Llibre("Amanda Black 1 - Una herencia peligrosa", "Thriller", a);

        session.persist(l1);
        session.persist(l2);

        session.getTransaction().commit();
        session.close();
    }
}
```
## **ManyToMany**

```java
@Entity
@Table(name="Teachers")
public class Professor implements Serializable {
   @ManyToMany(cascade=CascadeType.PERSIST, fetch=FetchType.LAZY)
   @JoinTable(
       name="Docencia",
       joinColumns = @JoinColumn(name="professor_id"),
       inverseJoinColumns = @JoinColumn(name="modul_id")
   )
   private Set<Moduls> moduls = new HashSet<>();

   public void addModul(Moduls m) {
      this.moduls.add(m);
      m.getProfessors().add(this);
   }
}


@Entity
@Table(name="Modules")
public class Moduls implements Serializable {
   @ManyToMany(mappedBy = "moduls", cascade=CascadeType.PERSIST, fetch=FetchType.LAZY)
   private Set<Professor> professors = new HashSet<>();

   public void addProfessor(Professor p) {
      this.professors.add(p);
      p.getModuls().add(this);
   }
}


public class App {
    public static void main( String[] args ) {
        SessionFactory sesion = HibernateUtil.getSessionFactory();
        Session session = sesion.openSession();
        session.beginTransaction();

        Professor p1 = new Professor("Antonio Talens"); 
        Professor p2 = new Professor("Roger Sobrino");

        Modul m1 = new Modul("Acces a Dades"); 
        Modul m2 = new Modul("Programacio"); 
        Modul m3 = new Modul("Sistemes");

        p1.addModul(m1); 
        p1.addModul(m2); 

        p2.addModul(m1); 
        p2.addModul(m3);

        session.persist(p1);
        session.persist(p2);

        session.getTransaction().commit();
        session.close();
    }
}
```