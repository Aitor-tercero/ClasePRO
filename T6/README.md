- Utilización de objetos, herencia, polimorfirmo y abstraccion
    - Concepto de herencia
    - Concepto de polimorfismo
    - Concepto de abstraccion
    - Concepto de interfaz. Herrencia multiple
    - Concepto y uso de los enum

### Concepto de herencia


En todo lenguaje de programación, la utilización de código que ya está escrito es una de las tareas básica, ya que así no se duplican elementos y es más fácil de codificar. Para esto existe lo que se conoce como herencia. En el tema anterior ya se introdujo el uso de la misma pero cabe recordad los siguientes aspectos básicos a la hora de utilizar herencia:
- En java una clase solo puede heredar de una clase. En concreto de la clase Object
- Cuando una clase hereda de otra, todos los métodos y atributos de la clase superior (llamada superclase) pasan a formar parte de la clase
- Solo existe una excepción en del punto anterior, que es cuando el modificador de acceso de las propiedades y/o métodos es restrictivo (private - para nadie)
- Si un método de la superase necesita ser redefinido en la clase que hereda el método, este queda sobrescrito para todas las clases que vengan debajo
- Si una superclase ha escrito un constructor (y por lo tanto ha eliminado el constructor por defecto) la subclase está obligada a utilizar como mínimo ese constructor (añadiendo más parámetros si los necesita)
- PAra poder utilizar la funcionalidad de la superclase se utiliza la para super
	- En un contructor debe ir en la primera linea. Ej. super(variables)
	- En un método puede ir en cualquier parte, seguido del nombre del nombre del método. Ej. super.nombreMétodo(variables)
- En java una clase solo puede tener una super clase
- Para poder utilizar la herencia se usa la palabra extendí seguido del nombre de la clase de la que se quiere heredar

### Usos

Sea la clase Coche

````
public class Coche {
    
    protected String marca, modelo;
    protected int bastidor;

    public Coche(String marca, String modelo, int bastidor) {
        this.marca = marca;
        this.modelo = modelo;
        this.bastidor = bastidor;
    }

    public String getMarca() {
        return marca;
    }

    public void setMarca(String marca) {
        this.marca = marca;
    }

    public String getModelo() {
        return modelo;
    }

    public void setModelo(String modelo) {
        this.modelo = modelo;
    }

    public int getBastidor() {
        return bastidor;
    }

    public void setBastidor(int bastidor) {
        this.bastidor = bastidor;
    }
}
````

En este caso la clase coche representa el molde que se quiere que tenga todas las clases que vendrán por debajo, por lo que se escriben todas las cosas comunes que tendrán todos los coches. Esto servirá como punto de unión de todas las clases que hereden de coche. Una vez se tiene la base, si se quiere crear una clase que especialice a Coche se utiliza la herencia

````
public class CocheDeportivo extends Coche {
    public CocheDeportivo(String marca, String modelo, int bastidor) {
        super(marca, modelo, bastidor);
    }
}
````

En este caso CocheDeportivo tendrá todas las características que tienen Coche y todas las que se marquen en la clase. Hay que tener en cuenta que en este ejemplo las variables y métodos de la clase Coche no son protected, por lo que se pasarán a las clases que van por debajo

En el caso de interesarnos que la clase más superior (superclase) no pueda ser instancia (no se puedan crear objetos de dicha clase), se podrá marcar como abstracta con la palabra abstract.

````
public abstract class Coche {

    String marca, modelo;
    int bastidor;

    public Coche(String marca, String modelo, int bastidor) {
        this.marca = marca;
        this.modelo = modelo;
        this.bastidor = bastidor;
    }

    public String getMarca() {
        return marca;
    }

    public void setMarca(String marca) {
        this.marca = marca;
    }

    public String getModelo() {
        return modelo;
    }

    public void setModelo(String modelo) {
        this.modelo = modelo;
    }

    public int getBastidor() {
        return bastidor;
    }

    public void setBastidor(int bastidor) {
        this.bastidor = bastidor;
    }
}
````

De esta forma la clase Coche no podrá ser utilizada para crear objetos

````
// no podrá ser utilizado
Coche coche = new Coche();

// solo podrá ser creado cuando se crea al menos uno de los métodos
Coche cocheCreado = new Coche("marca","modelo",123) {
            @Override
            public String getMarca() {
                return super.getMarca();
            }
};
````

En el lado contrario están las clases que representan los objetos que realmente se quieren instanciar. Esas clases se puede marcar como final, de forma que nadie podrá extender de ella. Para ello se utiliza la palabra final

````
public final class CocheDeportivo extends Coche {
    public CocheDeportivo(String marca, String modelo, int bastidor) {
        super(marca, modelo, bastidor);
    }
}
````

### Herencia

#### Sobrecarga y sobreescritura

#### Abstracción

En el ejemplo anterior, la clase coche (marcada como abstracta) puede tener tantos métodos como accesibles como se quiera. Estos métodos podrán ser accedidos y utilizador por la clases que extiendan de ella (CocheDeportivo)

````
public abstract class Coche {

    Protected String marca, modelo;
    Protected int bastidor, velocidad;

    public Coche(String marca, String modelo, int bastidor) {
        this.marca = marca;
        this.modelo = modelo;
        this.bastidor = bastidor;
    }

    public String getMarca() {
        return marca;
    }

    public void setMarca(String marca) {
        this.marca = marca;
    }

    public String getModelo() {
        return modelo;
    }

    public void setModelo(String modelo) {
        this.modelo = modelo;
    }

    public int getBastidor() {
        return bastidor;
    }

    public void setBastidor(int bastidor) {
        this.bastidor = bastidor;
    }

	public int getVelocidad() {
        return velocidad;
    }

    public void setVelocidad(int velocidad) {
        this.velocidad = velocidad;
    }
}
````

Adicionalmente, las clases abstractas podrán tener tantos métodos abstractos como sean necesarios. Estos métodos se caracterizan por tener tan solo firma (método de acceso, retorno, nombre y parámetros) y no tener el cuerpo definido. Esto hace que cualquier clase que extienda de ella se vea obligada a escribir el cuerpo de todos los métodos abstractos.

````
public abstract class Coche {

    protected String marca, modelo;
    protected int bastidor, velocidad;

    public Coche(String marca, String modelo, int bastidor) {
        this.marca = marca;
        this.modelo = modelo;
        this.bastidor = bastidor;
    }

    public abstract void acelerar(int vAcelerar);

    public String getMarca() {
        return marca;
    }

    public void setMarca(String marca) {
        this.marca = marca;
    }

    public String getModelo() {
        return modelo;
    }

    public void setModelo(String modelo) {
        this.modelo = modelo;
    }

    public int getBastidor() {
        return bastidor;
    }

    public void setBastidor(int bastidor) {
        this.bastidor = bastidor;
    }

    public int getVelocidad() {
        return velocidad;
    }

    public void setVelocidad(int velocidad) {
        this.velocidad = velocidad;
    }
}
````

El método abstracto acelerar no se puede definir, ya que dependerá del uso que le den los objetos que extiendan de Coche. En ese caso, cada clase que utilice la herencia se verá obligada a sobreescribir el método definiendo su cuerpo

````
public final class CocheDeportivo extends Coche {


    public CocheDeportivo(String marca, String modelo, int bastidor) {
        super(marca, modelo, bastidor);
        this.velocidad = 0;
    }

    @Override
    public void acelerar(int vAcelerar) {
        this.velocidad += vAcelerar*0.5;
    }
}

````


````
public final class CocheUtilitario extends Coche {


    public CocheUtilitario(String marca, String modelo, int bastidor) {
        super(marca, modelo, bastidor);
        this.velocidad = 0;
    }

    @Override
    public void acelerar(int vAcelerar) {
        this.velocidad = vAcelerar;
    }
}
````

De esta forma, ambas clases finales han extendido de la misma super clase y tienen los mismos métodos (aquellos que son abstractos) pero alguno de ellos con comportamiento diferente

````
public class Entrada {

    public static void main(String[] args) {
        
        CocheDeportivo deportivo = new CocheDeportivo("Ford","Focus",1234);
        CocheDeportivo utilitario = new CocheDeportivo("Ford","Mondel",2345);
        
        deportivo.acelerar(100);
        utilitario.acelerar(200);
    }
}
````


Adicionalmente, como ambos objetos pertenecen al mismo tipo genérico (Coche), se podrían juntar en una colección de tipo Coche

````
public class Entrada {

    public static void main(String[] args) {

        ArrayList<Coche> coches = new ArrayList();


        CocheDeportivo deportivo = new CocheDeportivo("Ford","Focus",1234);
        CocheUtilitario utilitario = new CocheUtilitario("Ford","Mondeo",2345);

        coches.add(deportivo);
        coches.add(utilitario);

        for (Coche c: coches) {
            c.acelerar(100);
        }

        /*
        deportivo.acelerar(100);
        utilitario.acelerar(200);*/
    }
}
````

En este ejemplo, como ambos objetos tienen la misma superclase pueden pertenecer a la colección, y esta al ser recorrida itera objetos de tipo Coche, pudiendo utilizar el método acelerar (abstracto en su definición inicial) ya que ha sido definido en las clases de CocheDeportico y CocheUtilitario