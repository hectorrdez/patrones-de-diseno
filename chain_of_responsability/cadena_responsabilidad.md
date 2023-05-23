# Chain Of Responsability

La cadena de responsabilidad o _Chain Of Responsability_ en Ingles, es un patron de Comportamiento del programa que trata de establecer __Controladores__ que se iran llamando dependiendo de la __Responsabilidad__ que tengan asignada. Esto resuelve el problema de las peticiones que se hagan a distintos metodos y clases, si no que este patron los centra en uno, para luego dependiendo de su responsabilidad, destinarlo a donde debe.

![Diagrama Chain_Of_Responsability](cadena_responsabilidad.png "Diagrama Cadena de Responsabilidad")

Este es el diagrama de clases del ejemplo del __Restaurante__ propuesto para el uso del patrón _Chain Of Responsability_

## Implementación

Para la implementación de este patrón de diseño es necesario una super clase de la que hereden todos los __Controladores__ que puedan gestionar las solicitudes. Los nuevos __Controladores__ cambiaran el funcionamiento del metodo _ManejarSolicitud_ dependiendo de lo que necesiten y configurarán en cada clase el siguiente _Controlador_ al que se llamará si la solicitud que le llega no entra dentro de su responsabilidad.

## Ventajas y Desventajas

| Ventajas ✔                                                        | Desventajas ❌                                                                                                   |
| ----------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Se pueden gestionar directamente <br/> las solicitudes | Algunas solicitudes pueden quedar <br/> fuera de la gestion de los controladores |
| Cumple el principio de responsabilidad <br> Unica. Se pueden separar las clases que invocan <br/> operaciones de las que las realizan. | |
| Cumple el principio de Abierto/Cerrado. Se <br> pueden crear mas controladores en la aplicacion <br> sin romper el codigo ya existente |                                                                                      

## Ejemplo

Para mostrar el funcionamiento de este patron, hemos realizado un ejemplo de un Restaurante. Dentro de este tenemos la clase __Camarero__ y la clase __Cocinero__. Todos estos son __Empleado__ e implementan la interfaz __Controlador__. La interfaz obliga a que todos los __Empleados__ tengan los metodos _SetSiguiente_ y _ManejarSolicitud_.
````java
public interface Controlador {
    public void SetSiguiente(Controlador siguiente);
    public void ManejarSolicitud(Solicitud solicitud);
}
````

La clase __Empleado__ que implementa la interfaz y de la que heredaran todos los trabajadores.

````java
public class Empleado implements Controlador{
    private Controlador siguiente;
    public void SetSiguiente(Controlador siguiente){
        this.siguiente = siguiente;
    }
    public void ManejarSolicitud(Solicitud solicitud){
        if(siguiente != null){
            siguiente.ManejarSolicitud(solicitud);
        }
    }
}
````

Las clases __Camarero__ y __Cocinero__ gestionaran las solicitudes que entren dentro de su responsabilidad y las que no, se la enviaran al siguiente __Empleado__

### Camarero
````java
public class Camarero extends Empleado{
    public void ManejarSolicitud(Solicitud solicitud){
        if(solicitud.GetTipo().equals("LLevar comida")){
            System.out.println("Camarero lleva: " + solicitud.GetDescripcion());
        }else{
            super.ManejarSolicitud(solicitud);
        }
    }
}
````
### Cocinero
````java
public class Cocinero extends Empleado{
    public void ManejarSolicitud(Solicitud solicitud){
        if(solicitud.GetTipo().equals("Cocina")){
            System.out.println("Cocinero prepara: " + solicitud.GetDescripcion());
        }else{
            super.ManejarSolicitud(solicitud);
        }
    }
}
````


La clase de apoyo como __Solicitud__ y el archivo principal del programa __Main__.

### Solicitud
````java
public class Solicitud {
    private String tipo;
    private String descripcion;
    public Solicitud(String tipo, String descripcion){
        this.tipo = tipo;
        this.descripcion = descripcion;
    }
    public String GetTipo(){
        return this.tipo;
    }
    public String GetDescripcion(){
        return this.descripcion;
    }
}
````

### Main

````java
public class CadenaResponsabilidad {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        Empleado cocinero = new Cocinero();
        Empleado camarero = new Camarero();
        cocinero.SetSiguiente(camarero);
        
        String tipo = "Cocina";
        String descripcion = "Pollo al horno con patatas";
        Solicitud solicitud = new Solicitud(tipo, descripcion);
        
        cocinero.ManejarSolicitud(solicitud);
       
        tipo = "LLevar comida";
        descripcion = "Pollo al horno con patatas";
        solicitud = new Solicitud(tipo, descripcion);
        
        cocinero.ManejarSolicitud(solicitud);
    }
    
}
````

## Webgrafia

- [Refactoring guru](https://refactoring.guru/design-patterns/chain-of-responsibility "refactoring.guru")
- [Digital Ocean](https://www.digitalocean.com/community/tutorials/chain-of-responsibility-design-pattern-in-java "digitalocean.com")
- [Junta de Andalucia](https://www.juntadeandalucia.es/servicios/madeja/contenido/recurso/182 "juntadeandalucia.es")
- [Reactive Programming](https://reactiveprogramming.io/blog/es/patrones-de-diseno/chain-of-responsability "reactiveprogramming.io")
