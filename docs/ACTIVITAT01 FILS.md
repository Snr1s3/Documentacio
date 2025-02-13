## ACTIVITAT01 FILS

  

### Exercici 1  Equitativament
En aquest exercici s'han de mostrar equitativament.
```java
public class Principal{
	public static void main(String[] args){
		Fil fil1 = new Fil("Juan");
		Fil fil2 = new Fil("Pepe");
		fil1.start();
		fil2.start();
		System.out.println("Termina thread main");
	}
}
```
```java
public class Fil extends Thread{
    String name;
    public Fil(String name){
        this.name = name;
    }
    @Override
    public void run() {
        for(int i = 1; i < 10; i++){
            System.out.println(name + " " + i);
        }
        System.out.println("Termina el fil " + name);
        
    }
}
```
### Exercici 2 Primer un i despres l'altre
```java
public class Principal{
	public static void main(String[] args){
		Fil fil1 = new Fil("Juan");
		Fil fil2 = new Fil("Pepe");
		fil1.start();
		fil2.start();
		System.out.println("Termina thread main");
	}
}
```
```java
public class Fil extends Thread{
    String name;
    public Fil(String name){
        this.name = name;
    }
    @Override
    public void run() {
        for(int i = 1; i < 10; i++){
            System.out.println(name + " " + i);
        }
        System.out.println("Termina el fil " + name);
        
    }
}
```
### Exercici 3 Intercalat

```java
public class Principal{
	public static void main(String[] args){
		Fil fil1 = new Fil("Juan");
		Fil fil2 = new Fil("Pepe");
		fil1.start();
		fil2.start();
		System.out.println("Termina thread main");
	}
}
```
```java
public class Fil extends Thread{
    String name;
    public Fil(String name){
        this.name = name;
    }
    @Override
    public void run() {
        for(int i = 1; i < 10; i++){
            System.out.println(name + " " + i);
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("Termina el fil " + name);
        
    }
}
```