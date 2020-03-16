- Uso avanzado de clases
-
	- Constructores estáticos
	- Clases anónimas
    - Clases anidadas
    - Generalización
    - Eventos 

## Constructores estáticos

Como se vio en clases anteriores, un constructor es el método especial que me permite crear un objeto para poder utilizar cualquier método de la clase. Para poder crear un constructor corriente las restricciones era: 

- tener un ámbito público
- no tener modificador de acceso

````
public class UnaClase {
    
    public UnaClase()
    {
        
    }
    
    public void unMetodo(){
        System.out.println("Ejecución de un método");
    }
    
}
````

De esta forma se crea un constructor de una clase y desde cualquier clase podrá ser utilizado para crear un objeto de la clase y utilizar sus métodos

````
public class Entrada {

    public static void main(String[] args) {
        UnaClase unaClase = new UnaClase();
        unaClase.unMetodo();
    }
}

````

Además de esta posibilidad, también existe la posibilidad de utilizar un método estático para poder crear un objeto. Si recordáis el modificador static indicaba que aquella variable o método donde fuese aplicado podría ser llamado de forma directa tan solo utilizando el nombre de la clase. Si esto lo aplicamos al ejemplo anterior y creamos un método estático, este podrá ser llamado directamente sin necesidad de tener un objeto de la clase

````
public class UnaClase {

    public UnaClase()
    {

    }
    
    public static void unMétodoEstático(){
        System.out.println("Un método estático");
    }
}
````

````
public class Entrada {

    public static void main(String[] args) {
        UnaClase.unMétodoEstático();
    }
}
````

Con esta característica, se puede utilizar el método estático para crear y deber un objeto de forma directa, sin necesidad de utilizar la palabra reservada new

````
public class UnaClase {

    public UnaClase()
    {

    }

    public static UnaClase unMétodoEstático(){
        UnaClase objeto = new UnaClase();
        return objeto;
    }

}
````

De forma que este método pueda ser llamado para generar un objeto

````
public class Entrada {

    public static void main(String[] args) {
        UnaClase objeto = UnaClase.unMétodoEstático();
    }
}
````

Esta capacidad nos permite desde una clase generar un objeto y acceder a sus métodos de forma directa. Por ejemplo imaginad una clase que tiene una colección de datos que nos interesa que sean accedido (por ejemplo una lista de equipos de fútbol)

````
public class Equipo {
    
    private String nombre, pais;
    private int ranking;

    public Equipo(String nombre, String pais, int ranking) {
        this.nombre = nombre;
        this.pais = pais;
        this.ranking = ranking;
    }

    public String getNombre() {
        return nombre;
    }

    public String getPais() {
        return pais;
    }

    public int getRanking() {
        return ranking;
    }
}
````

````
public class DataSet {

    public ArrayList<Equipo> getEquiposEspaña(){
        ArrayList<Equipo> equipos = new ArrayList<>();
        equipos.add(new Equipo("FC Barcelona",1));
        equipos.add(new Equipo("Reeal Madrid",2));
        equipos.add(new Equipo("Sevilla",3));
        equipos.add(new Equipo("Real Sociedad",4));
        equipos.add(new Equipo("Getafe",5));
        equipos.add(new Equipo("Real Sociedad",6));
        return equipos;
    }

    public ArrayList<Equipo> getEquposItalia(){
        ArrayList<Equipo> equipos = new ArrayList<>();
        equipos.add(new Equipo("Juventus",1));
        equipos.add(new Equipo("Lazio",2));
        equipos.add(new Equipo("Inter de Milán",3));
        equipos.add(new Equipo("Atalanta",4));
        equipos.add(new Equipo("Roma",5));
        equipos.add(new Equipo("Nápoles",6));
        return equipos
    }
}
````

Si en la clase DataSet se crea un método estático que genere un objeto, se podrá acceder directamente a los métodos sin necesidad de tener el objeto creado, tan solo accediendo a los métodos que nos interesa

````
public class DataSet {

    public static DataSet newInstance() {
        
        DataSet dataSet = new DataSet();
        return dataSet;
    }

    public ArrayList<Equipo> getEquiposEspaña(){
        ArrayList<Equipo> equipos = new ArrayList<>();
        equipos.add(new Equipo("FC Barcelona",1));
        equipos.add(new Equipo("Reeal Madrid",2));
        equipos.add(new Equipo("Sevilla",3));
        equipos.add(new Equipo("Real Sociedad",4));
        equipos.add(new Equipo("Getafe",5));
        equipos.add(new Equipo("Real Sociedad",6));
        return equipos;
    }

    public ArrayList<Equipo> getEquposItalia(){
        ArrayList<Equipo> equipos = new ArrayList<>();
        equipos.add(new Equipo("Juventus",1));
        equipos.add(new Equipo("Lazio",2));
        equipos.add(new Equipo("Inter de Milán",3));
        equipos.add(new Equipo("Atalanta",4));
        equipos.add(new Equipo("Roma",5));
        equipos.add(new Equipo("Nápoles",6));
        return equipos
    }
}

````

El método newInstance podrá ser llamado de forma directa desde cualquier clase

````
public class Entrada {

    public static void main(String[] args) {

        ArrayList<Equipo> listaEspaña = DataSet.newInstance().getEquiposEspaña();

    }
}
````

En este ejemplo se llama al método newInstance el cual devuelve un objeto de tipo DataSet, y al mismo tiempo se llama al método getEquiposEspaña que devuelve un arraylist, pudiendo igualarlo a una variable del mismo tipo para utilizarla como sea necesario en la clase Entrada. 

````
public class Entrada {

    public static void main(String[] args) {


        System.out.println("CLASIFICACIÓN ESPAÑA");
        System.out.println("***********");
        ArrayList<Equipo> listaEspaña = DataSet.newInstance().getEquiposEspaña();
        for (Equipo equipo: listaEspaña) {
            System.out.println(String.format("Posición %d %s",equipo.getRanking(),equipo.getNombre()));
        }

        System.out.println("\nCLASIFICACIÓN ITALIA");
        System.out.println("***********");
        ArrayList<Equipo> listaItalia = DataSet.newInstance().getEquposItalia();
        for (Equipo equipo: listaItalia) {
            System.out.println(String.format("Posición %d %s",equipo.getRanking(),equipo.getNombre()));
        }

    }
}
````


## Clases anónimas

Como su nombre indica, una clase anónima es aquella que no tiene nombre y cuyo uso es directo, sin necesidad de asignarlo a una variable cualquiera. El uso de este tipo de clase esta dedicado a aquellas operaciones que necesitan de un objeto concreto pero no está justificado su creación, de forma que con su simple uso es suficiente. Esto ahorra tanto memoria como trabajo al programa.

Lo normal es que esto se utilice mediante objetos de tipo interfaz, donde tan solo se define la firma de un método y en las clases anónimas se crea el cuerpo, sin necesidad de implementar la interfaz. Esto no quiere decir que sea uso exclusivo, ya que también se puede hacer mediante clases "normales"

### Implementación mediante clases

Imaginad la siguiente clase
````
public abstract class ClaseA {

    public void mostrarMesaje(){
        System.out.println("mensaje lanzado desde la clase A");
    }

    public abstract void mostrarMensajeAbstracto();
}

````

En este caso la clase es abstracta porque tiene un método que no tiene cuerpo, por lo que su uso está restringido a que la clase que extienda de ClaseA lo implemente. Ese sería su uso normal como se muestra en la siguiente clase: 

````
public class ClaseB extends ClaseA {
    @Override
    public void mostrarMensajeAbstracto() {
        System.out.println("Mensaje lanzado desde la clase b");
    }
}

````

Sin embargo con el uso de clases anónimas este extensión e implementación no es necesario. Se pueden crear objetos de tipo ClaseA de forma directa, con la única restricción que los métodos abstractos deben ser implementados. Imaginad la siguiente clase

````
public class ClaseC {

    public void registrarMensajeElemento(ClaseA clase){
        clase.mostrarMensaje();
        clase.mostrarMensajeAbstracto();
    }
}
````

Esta clase tiene un método que obtiene como parámetro un objeto de ClaseA, el cual llama a los métodos mostrarMensaje y mostrarMensajeAbstracto(). De estos dos métodos, el primero tiene definición y hará un sout("mensaje lanzado desde clase a"), sin embargo el método mostrarMensajeAbstracto no tienen definición por lo que aún no se sabe como se comportará. En la clase Entrada, se puede utilizar directamente mediante una clase anónima 

````
public class Entrada {

    public static void main(String[] args) {
        
        ClaseC claseC = new ClaseC();
        claseC.registrarMensajeElemento(new ClaseA() {
            @Override
            public void mostrarMensajeAbstracto() {
                
            }
        });

    }
}
````

El parámetro que admite el método registrarMensajeElemento es un objeto de ClaseA, que a su vez como es abstracta me obliga a escribir el cuerpo de todos aquellos métodos que son abstractos, como lo es el método mostrarMensajeAbstracto(). Si se escribe el cuerpo:
````
public class Entrada {

    public static void main(String[] args) {

        ClaseC claseC = new ClaseC();
        claseC.registrarMensajeElemento(new ClaseA() {
            @Override
            public void mostrarMensajeAbstracto() {
                System.out.println("mensaje lanzado desde la clase main");
            }
        });

    }
}
````

En la ejecución del main sería 
````
mensaje lanzado desde la clase A
mensaje lanzado desde la clase main
````

La primera linea se ejecuta desde el método mostraMensaje del objeto ClaseA, ya que está escrito en la propia clase y llamados desde el método registrarMensajeElemento de la ClaseC
La segunda línea se ejecuta desde el método mostrarMensajeAbstracto del objeto anónimo creado en la propia clase main, ya que ahí ha sido escrito el cuerpo

### Implementación mediante interfaces

Se realiza de la misma forma que lo anterior, con la única diferencia que todos los métodos están definidos en la interfaz y por lo tanto tendrán que ser escritos en objeto definido con la clase anónima. 

Imaginad la siguiente interfaz: 

````
public interface InterfazMensajes {

    void lanzarMensajeMinusculas();
    void lanzarMensajeMayusculas();
}
````

Esta interfaz representa la funcionalidad general que necesitan objetos de la clase, pero no es necesario que los objetos que utilizan estos métodos implementen la interfaz (porque no interesa para no "ensuciar la clase", por temas de funcionalidad, porque no se quiere que un objeto tenga varias formas, etc...)

````
public interface InterfazMensajes {

    void lanzarMensajeMinusculas();
    void lanzarMensajeMayusculas();
}
````

Al ser métodos en una interfaz, no tienen cuerpo que deberá ser definido donde se utilicen (en las clases que los implementen o en las clases anónimas que los llamen). Para poder utilizar la interfaz se crea una segunda clase que llama a estos métodos

````
public class Mensajes {

    public void lanzarMensajes(InterfazMensajes interfaz){
        interfaz.lanzarMensajeMayusculas();
        interfaz.lanzarMensajeMinusculas();
    }

}
````

Al no haber implementado la interfaz no se define el comportamiento de los métodos, tan solo se llama a los mismo para que sean ejecutados. El funcionamiento de los métodos será definido cuando se cree un objeto de tipo interfaz. En la clase entrada es donde se juntan las funcionalidades

````
public class Entrada {

    public static void main(String[] args) {

        Mensajes mensajes = new Mensajes();
        String m = "Mensaje ejemplo para Lanzarlo desde CLASE anónima";
        mensajes.lanzarMensajes(new InterfazMensajes() {
            @Override
            public void lanzarMensajeMinusculas() {
                System.out.println(m.toLowerCase());
            }
            @Override
            public void lanzarMensajeMayusculas() {
                System.out.println(m.toUpperCase());
            }
        });
    }
}
````

Al crear un objeto de tipo Mensajes y llamar al método lanzarMensajes espera un objeto de tipo InterfazMensaje (tal cual se definió en la clase). Es en este punto donde se crea el objeto, que al ser de tipo interfaz obliga a definir la funcionalidad de los métodos. La salida del main será:
````
MENSAJE EJEMPLO PARA LANZARLO DESDE CLASE ANÓNIMA
mensaje ejemplo para lanzarlo desde clase anónima
````

Ambas líneas son llamadas desde el método lanzarMensajes de la claseMensajes, pero su definición está escrita en el main cuando se crea el objeto de tipo interfaz.

Esta forma de trabajar es muy utilizada en los escuchadores de las librerías gráficas de java como awt, swing y javafx, algunas de las cuales se verán al final del curso y principios del año que viene

## Clases anidadas

Las clases anidadas en java se pueden definir cómo la creación de una clase que está dentro de otra. Por regla general, cada archivo representa una clase individual, pero como ya vimos en el tema de los Enum también es posible unos dentro de otros. El uso de este tipo de clases es el siguiente:
- Permite agrupar lógicamente clases que solo se utilizan en un lugar, sin la necesidad de tener que crear gran cantidad de archivos
- Marca una relación de dependencia, ya que la clase anidada no puede existen sin la existencia de la clase principal (a no ser que la clase anidada sea estática)
- El concepto de encapsulación se sigue manteniendo ya que una clase anidada puede acceder a todos los elementos de la clase en la que está declarada, aunque estos sean estáticos

Como regla general existen dos tipos de clases anidadas: estáticas y no estáticas o también llamadas internas

### Clases anidadas no estáticas o clases internas

Se define una clase interna como aquella que está definida dentro de una clase y cumple los siguientes requisitos:

- Puede tener acceso a todos los miembros de la clase a la que pertenece (incluso los declarados como privados)
- Solo puede crearse un objeto de la clase interna si previamente existe un objeto de la clase donde es anidada

Para poder verlo mejor vamos hacer un ejemplo:

Imaginad la siguiente clase

````
public class ClaseExterna {

    private String dato;
    private int numero;

    public ClaseExterna(String dato, int numero) {
        this.dato = dato;
        this.numero = numero;
    }

    public void mostrarDatos(){
        System.out.printf(String.format("%d %s %n",getNumero(),getDato()));
    }

    public String getDato() {
        return dato;
    }

    public void setDato(String dato) {
        this.dato = dato;
    }

    public int getNumero() {
        return numero;
    }

    public void setNumero(int numero) {
        this.numero = numero;
    }
}
````

Esta clase está representada en un archivo .java y tienen un constructor, getter y setter, y un método mostrarDatos(). Es importante fijarse en que las variables que están definidas en la clase son privadas lo que quiere decir que nadie podrá acceder a ellas, ni siquiera un objeto que está definido desde fuera (para eso están los getter // setter - concepto de encapsulación)

````
public class Entrada {

    public static void main(String[] args) {

        ClaseExterna claseExterna = new ClaseExterna("dato externo",1);
        claseExterna.mostrarDatos();
    }
}
````

En otro archivo .java queda representado el método main para poder acceder a todos aquellos elementos que no sean privados, como por ejemplo el método mostrarDatos.

Hasta aquí el uso corriente. En algunas ocasiones es interesante que la ClaseExterna tenga asociada otra clase que represente un objeto propio, y como no se va a utilizar en ningún otro sitio se puede definir dentro (concepto de clase anidada o interna). Para ello en el mismo archivo .java donde se ha definido la clase ClaseExterna se crea otra clase adicional con la palabra class seguida del nombre (cuidado con los paréntesis, ya que la clase que se está creando está dentro)

````
public class ClaseExterna {

    private String dato;
    private int numero;

    public ClaseExterna(String dato, int numero) {
        this.dato = dato;
        this.numero = numero;
    }

    public void mostrarDatos(){
        System.out.printf(String.format("%d %s %n",getNumero(),getDato()));
    }

    public String getDato() {
        return dato;
    }

    public void setDato(String dato) {
        this.dato = dato;
    }

    public int getNumero() {
        return numero;
    }

    public void setNumero(int numero) {
        this.numero = numero;
    }

    class ClaseInterna{
        // definición de todos los elementos de la clase interna
    }
}
````

De esta forma se acaba de crear una clase interna, pudiendo declarar tanto variables como métodos además de acceder a todas las partes de la clase donde se ha definido (en este caso la ClaseExterna)

````
public class ClaseExterna {

    private String dato;
    private int numero;

    public ClaseExterna(String dato, int numero) {
        this.dato = dato;
        this.numero = numero;
    }

    public void mostrarDatos(){
        System.out.printf(String.format("%d %s %n",getNumero(),getDato()));
    }

    public String getDato() {
        return dato;
    }

    public void setDato(String dato) {
        this.dato = dato;
    }

    public int getNumero() {
        return numero;
    }

    public void setNumero(int numero) {
        this.numero = numero;
    }

    class ClaseInterna{

        private int numeroInterno;
        private String datoInterno;

        public ClaseInterna(int numeroInterno, String datoInterno) {
            this.numeroInterno = numeroInterno + numero;
            this.datoInterno = dato.concat(datoInterno);
        }

        public void mostrarDatosInternos(){
            System.out.printf(String.format("%d %s %n",getNumeroInterno(),getDatoInterno()));
        }

        public int getNumeroInterno() {
            return numeroInterno;
        }

        public void setNumeroInterno(int numeroInterno) {
            this.numeroInterno = numeroInterno;
        }

        public String getDatoInterno() {
            return datoInterno;
        }

        public void setDatoInterno(String datoInterno) {
            this.datoInterno = datoInterno;
        }
    }
}
````

En la definición de la clase interna se crean tantos elementos como sean necesarios, pudiendo acceder a los elementos de la clase superior. En el ejemplo, para poder crear una clase ClaseInterna se pide por constructor un String y un int, los cuales son añadidos a los anteriores, tal y como se puede ver en el constructor. 

Una vez hecho esto se puede utilizar un objeto de ClaseInterna, siempre y cuando ya existe un objeto de la clase donde está declarada. Para ello en el la clase Entrada si se intenta crear un objeto de la forma "normal" da error:

````

// ERROR
ClaseExterna.ClaseInterna claseInterna =  new ClaseInterna(1,"asd");

````

Cuando se trabaja con clases internas, es necesario la existencia previa de un objeto de la clase que lo contiene, sino es imposible crearlas

````
// CORRECTO
ClaseExterna.ClaseInterna claseInterna =  claseExterna.new ClaseInterna(1,"dato interno");
claseInterna.mostrarDatosInternos();
````

El ejemplo completo sería

````
public class Entrada {

    public static void main(String[] args) {

        ClaseExterna claseExterna = new ClaseExterna("dato externo",1);
        claseExterna.mostrarDatos();

        ClaseExterna.ClaseInterna claseInterna =  claseExterna.new ClaseInterna(1,"dato interno");
        claseInterna.mostrarDatosInternos();
    }
}
````

Y la salida 

````
1 dato externo 
2 dato externo dato interno 
````
