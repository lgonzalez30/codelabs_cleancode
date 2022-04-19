summary: How to Write a Clean Code
id: how-to-write-a-cleancode
categories: CleanCode
environments: Web
tags: medium
status: Published 
authors: Luis Gonzalez




<!-- ------------------------ -->

# Introducción a Clean Code


<!-- ------------------------ -->
## Images
![alt-text-here](assets/cleancode.jpg)

<!-- ------------------------ -->
## Principios de ingeniería de software
Duration: 5

### Tomado del libro Clean Code de Robert C. Martin, adaptado para Java
Esto es una guía para producir software legible, reutilizable y refactorizable en Java.

No todos los principios de este documento tienen que seguirse estrictamente, e incluso menos serán aceptados universalmente. Estas son pautas y nada más, pero están codificadas durante muchos años de experiencia colectiva por los autores de Clean Code.

Nuestro oficio de ingeniería de software tiene poco más de 50 años y todavía estamos aprendiendo mucho. Cuando la arquitectura del software sea tan antigua como la arquitectura misma, quizás tengamos reglas más difíciles de seguir. Por ahora, deje que estas pautas sirvan como piedra de toque para evaluar la calidad del código Java que usted y su equipo producen.

Una cosa más: saber esto no lo convertirá inmediatamente en un mejor desarrollador de software, y trabajar con ellos durante muchos años no significa que no cometerá errores. Cada pieza de código comienza como un primer borrador, como arcilla húmeda a la que se le da forma final. Finalmente, eliminamos las imperfecciones cuando lo revisamos con nuestros compañeros. No se castigue por los primeros borradores que necesitan mejorar. ¡Mejora el código en su lugar!

<!-- ------------------------ -->
## Variables
Duration: 5

### Use nombres de variables significativos y pronunciables 

### Malo:

```java
  String yyyymmdstr = new SimpleDateFormat("YYYY/MM/DD").format(new Date());  

```

### Bueno:

```java 
  String currentDate = new SimpleDateFormat("YYYY/MM/DD").format(new Date());  

```


<!-- ------------------------ -->
## Usar el mismo vocabulario para el mismo tipo de variable
Duration: 5

### Malo:

```java 
  getUserInfo();
  getClientData();
  getCustomerRecord();  

```

### Bueno:

```java
  getUser();  

```

<!-- ------------------------ -->
## Usar nombres buscables
Duration: 5

Leeremos más código del que jamás escribiremos. Es importante que el código que escribimos sea legible y buscable. Al no nombrar variables que terminan siendo significativas para entender nuestro programa, lastimamos a nuestros lectores. Haga que sus nombres se puedan buscar.

### Malo:

```java 
  // What the heck is 86400000 for?
  setTimeout(blastOff, 86400000);  

```

### Bueno:

```java 
  // Declare them as capitalized `const` globals.
  public static final int MILLISECONDS_IN_A_DAY = 86400000;

  setTimeout(blastOff, MILLISECONDS_IN_A_DAY);  

```

<!-- ------------------------ -->
## Usar variables explicativas
Duration: 5

### Malo:

```java 
  String address = "One Infinite Loop, Cupertino 95014";
  String cityZipCodeRegex = "/^[^,\\\\]+[,\\\\\\s]+(.+?)\\s*(\\d{5})?$/";

  saveCityZipCode(address.split(cityZipCodeRegex)[0],
                address.split(cityZipCodeRegex)[1]);

```

### Bueno:

```java 
  String address = "One Infinite Loop, Cupertino 95014";
  String cityZipCodeRegex = "/^[^,\\\\]+[,\\\\\\s]+(.+?)\\s*(\\d{5})?$/";

  String city = address.split(cityZipCodeRegex)[0];
  String zipCode = address.split(cityZipCodeRegex)[1];

  saveCityZipCode(city, zipCode);  

```
<!-- ------------------------ -->
## Evite el mapeo mental
Duration: 5

No fuerce al lector de su código a traducir lo que significa la variable. Explícito es mejor que implícito.

### Malo:

```java
  String [] l = {"Austin", "New York", "San Francisco"};

  for (int i = 0; i < l.length; i++) {
    String li = l[i];
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    // Wait, what is `$li` for again?
    dispatch(li);
  }  
  
```

### Bueno:

```java
  String[] locations = {"Austin", "New York", "San Francisco"};

  for (String location : locations) {
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    dispatch(location);
  }
    

```

<!-- ------------------------ -->
## No agregue contexto innecesario
Duration: 5

Si el nombre de su clase/objeto le dice algo, no lo repita en su nombre de variable.

### Malo:

```java
  class Car {
  public String carMake = "Honda";
  public String carModel = "Accord";
  public String carColor = "Blue";
  }

  void paintCar(Car car) {
  car.carColor = "Red";
  }
  
```

### Bueno:

```java
  class Car {
  public String make = "Honda";
  public String model = "Accord";
  public String color = "Blue";
  }

  void paintCar(Car car) {
  car.color = "Red";
  }

```

<!-- ------------------------ -->
## Funciones
Duration: 5

Limitar la cantidad de parámetros de función es increíblemente importante porque facilita la prueba de su función. Tener más de tres conduce a una explosión combinatoria en la que tienes que probar toneladas de casos diferentes con cada argumento por separado.

Uno o dos argumentos es el caso ideal, y tres deben evitarse si es posible. Cualquier cosa más que eso debe consolidarse. Por lo general, si tiene más de dos argumentos, su función está tratando de hacer demasiado. En los casos en que no lo sea, la mayoría de las veces un objeto de nivel superior será suficiente como argumento.

<!-- ------------------------ -->
## Las funciones deben hacer una cosa
Duration: 5

Esta es, con mucho, la regla más importante en la ingeniería de software. Cuando las funciones hacen más de una cosa, son más difíciles de componer, probar y razonar. Cuando puede aislar una función en una sola acción, se pueden refactorizar fácilmente y su código se leerá mucho más claro. Si no saca nada más de esta guía aparte de esto, estará por delante de muchos desarrolladores.

### Malo:

```java
  public void emailClients(List<Client> clients) {
    for (Client client : clients) {
        Client clientRecord = repository.findOne(client.getId());
        if (clientRecord.isActive()){
            email(client);
        }
    }
  }
  
```

### Bueno:

```java
  public void emailClients(List<Client> clients) {
    for (Client client : clients) {
        if (isActiveClient(client)) {
            email(client);
        }
    }
  }

  private boolean isActiveClient(Client client) {
    Client clientRecord = repository.findOne(client.getId());
    return clientRecord.isActive();
  }

```

<!-- ------------------------ -->
## Los nombres de las funciones deben decir lo que hacen
Duration: 5

### Malo:

```java
  private void addToDate(Date date, int month){
    //..
  }

  Date date = new Date();

  // It's hard to to tell from the method name what is added
  addToDate(date, 1);
  
```

### Bueno:

```java
  private void addMonthToDate(Date date, int month){
    //..
  }

  Date date = new Date();
  addMonthToDate(1, date)

```

<!-- ------------------------ -->
## Las funciones solo deben tener un nivel de abstracción
Duration: 5

Cuando tiene más de un nivel de abstracción, su función suele hacer demasiado. La división de funciones conduce a la reutilización y a pruebas más sencillas.

<!-- ------------------------ -->
## Eliminar código duplicado
Duration: 3

Haga todo lo posible para evitar el código duplicado. El código duplicado es malo porque significa que hay más de un lugar para modificar algo si necesita cambiar alguna lógica.

Imagínese si dirige un restaurante y realiza un seguimiento de su inventario: todos sus tomates, cebollas, ajo, especias, etc. Si tiene varias listas en las que mantiene esto, entonces todas deben actualizarse cuando sirve un plato con tomates. en ellos. Si solo tiene una lista, ¡solo hay un lugar para actualizar!


<!-- ------------------------ -->
## No use banderas como parámetros de función
Duration: 3

Las banderas le dicen a su usuario que esta función hace más de una cosa. Las funciones deben hacer una cosa. Divide tus funciones si están siguiendo diferentes rutas de código basadas en un booleano.


<!-- ------------------------ -->
## Evite los efectos secundarios (part 1)
Duration: 3

Una función produce un efecto secundario si hace algo más que tomar un valor y devolver otro valor o valores. Un efecto secundario podría ser escribir en un archivo, modificar alguna variable global o transferir accidentalmente todo su dinero a un extraño.

Ahora, usted necesita tener efectos secundarios en un programa de vez en cuando. Al igual que en el ejemplo anterior, es posible que deba escribir en un archivo. Lo que quieres hacer es centralizar dónde estás haciendo esto. No tenga varias funciones y clases que escriban en un archivo en particular. Tenga un servicio que lo haga. Uno y solo uno.

El punto principal es evitar errores comunes como compartir el estado entre objetos sin ninguna estructura, usar tipos de datos mutables en los que cualquier cosa puede escribir y no centralizar dónde ocurren los efectos secundarios. Si puede hacer esto, será más feliz que la gran mayoría de los otros programadores.


<!-- ------------------------ -->
## Evite los efectos secundarios (part 2)
Duration: 5

En JavaScript, las primitivas se pasan por valor y los objetos/matrices se pasan por referencia. En el caso de objetos y matrices, si su función realiza un cambio en una matriz de carrito de compras, por ejemplo, agregando un artículo para comprar, cualquier otra función que use esa matriz de carrito se verá afectada por esta adición. Eso puede ser genial, sin embargo, también puede ser malo. Imaginemos una mala situación:

El usuario hace clic en el botón "Comprar", que llama a una función de compra que genera una solicitud de red y envía la matriz del carrito al servidor. Debido a una mala conexión de red, la función de compra tiene que volver a intentar la solicitud. Ahora, ¿qué pasa si, mientras tanto, el usuario hace clic accidentalmente en el botón "Agregar al carrito" en un artículo que en realidad no quiere antes de que comience la solicitud de red? Si eso sucede y comienza la solicitud de red, entonces esa función de compra enviará el artículo agregado accidentalmente porque tiene una referencia a una matriz de carrito de compras que la función addItemToCart modificó al agregar un artículo no deseado.

Una gran solución sería que addItemToCart siempre clone el carrito, lo edite y devuelva el clon. Esto asegura que ninguna otra función que retiene una referencia del carrito de compras se verá afectada por ningún cambio.

Dos advertencias para mencionar a este enfoque:

Puede haber casos en los que realmente desee modificar el objeto de entrada, pero cuando adopte esta práctica de programación, encontrará que esos casos son bastante raros. ¡La mayoría de las cosas se pueden refactorizar para que no tengan efectos secundarios!


<!-- ------------------------ -->
## No escribas en funciones globales
Duration: 3

Contaminar globales es una mala práctica en JavaScript porque podría entrar en conflicto con otra biblioteca y el usuario de su API no se daría cuenta hasta que obtenga una excepción en producción. Pensemos en un ejemplo: ¿qué pasaría si quisiera extender el método Array nativo de JavaScript para tener un método diff que pudiera mostrar la diferencia entre dos matrices? Podría escribir su nueva función en Array.prototype, pero podría chocar con otra biblioteca que intentara hacer lo mismo. ¿Qué pasaría si esa otra biblioteca solo estuviera usando diff para encontrar la diferencia entre el primer y el último elemento de una matriz? Es por eso que sería mucho mejor usar las clases ES2015/ES6 y simplemente extender el Array global.


<!-- ------------------------ -->
## Favorecer la programación funcional sobre la programación imperativa
Duration: 3

JavaScript no es un lenguaje funcional en la forma en que lo es Haskell, pero tiene un sabor funcional. Los lenguajes funcionales son más limpios y fáciles de probar. Favorezca este estilo de programación cuando pueda.



<!-- ------------------------ -->
## Evitar condicionales
Duration: 3

Esto parece una tarea imposible. Al escuchar esto por primera vez, la mayoría de la gente dice: "¿cómo se supone que debo hacer algo sin una declaración if?" La respuesta es que puede usar el polimorfismo para lograr la misma tarea en muchos casos. La segunda pregunta suele ser, "bueno, eso es genial, pero ¿por qué querría hacer eso?" La respuesta es un concepto de código limpio anterior que aprendimos: una función solo debe hacer una cosa. Cuando tiene clases y funciones que tienen declaraciones if, le está diciendo a su usuario que su función hace más de una cosa. Recuerda, solo haz una cosa.

<!-- ------------------------ -->
## No sobreoptimices
Duration: 3

Los navegadores modernos realizan una gran cantidad de optimización bajo el capó en tiempo de ejecución. Muchas veces, si está optimizando, simplemente está perdiendo el tiempo. Hay buenos recursos para ver dónde falta la optimización. Apunta a aquellos mientras tanto, hasta que se arreglen si es posible.

<!-- ------------------------ -->
## Eliminar código muerto
Duration: 3

El código muerto es tan malo como el código duplicado. No hay razón para mantenerlo en su base de código. Si no está siendo llamado, ¡deshazte de él! Todavía estará seguro en su historial de versiones si todavía lo necesita.


<!-- ------------------------ -->
## Objetos y estructuras de datos
Duration: 3

Usar getters y setters
El uso de getters y setters para acceder a los datos de los objetos podría ser mejor que simplemente buscar una propiedad en un objeto. "¿Por qué?" podrías preguntar. Bueno, aquí hay una lista desorganizada de razones por las cuales:

- Cuando desee hacer algo más que obtener una propiedad de objeto, no tiene que buscar y cambiar cada elemento de acceso en su base de código. 
- Simplifica la adición de validación al hacer un conjunto. 
- Encapsula la representación interna. 
- Fácil de agregar registro y manejo de errores al obtener y configurar.
- Puede cargar de forma diferida las propiedades de su objeto, digamos obtenerlo de un servidor.

<!-- ------------------------ -->
## Hacer que los objetos tengan miembros privados
Duration: 3

Esto se puede lograr a través de cierres (para ES5 e inferior).



<!-- ------------------------ -->
## Preferir la composición a la herencia
Duration: 3

Como se afirma en Design Patterns by the Gang of Four, debes preferir la composición a la herencia siempre que puedas. Hay muchas buenas razones para usar la herencia y muchas buenas razones para usar la composición. El punto principal de esta máxima es que si tu mente busca instintivamente la herencia, trata de pensar si la composición podría modelar mejor tu problema. En algunos casos puede.

<!-- ------------------------ -->
## Solido
Duration: 3

### Principio de responsabilidad única (SRP)

Como se indica en Clean Code, "Nunca debe haber más de una razón para cambiar una clase". Es tentador abarrotar una clase con mucha funcionalidad, como cuando solo puedes llevar una maleta en tu vuelo. El problema con esto es que su clase no será conceptualmente cohesiva y le dará muchas razones para cambiar. Es importante minimizar la cantidad de veces que necesita cambiar una clase. Es importante porque si hay demasiada funcionalidad en una clase y modifica una parte de ella, puede ser difícil entender cómo afectará eso a otros módulos dependientes en su base de código.


<!-- ------------------------ -->
## Principio abierto/cerrado (OCP)
Duration: 3

Como afirma Bertrand Meyer, "las entidades de software (clases, módulos, funciones, etc.) deben estar abiertas a la extensión, pero cerradas a la modificación". Pero, ¿qué significa eso? Este principio básicamente establece que debe permitir a los usuarios agregar nuevas funcionalidades sin cambiar el código existente.

<!-- ------------------------ -->
## Principio de sustitución de Liskov (LSP)
Duration: 3

Este es un término aterrador para un concepto muy simple. Se define formalmente como "Si S es un subtipo de T, entonces los objetos de tipo T pueden reemplazarse con objetos de tipo S (es decir, los objetos de tipo S pueden sustituir a los objetos de tipo T) sin alterar ninguna de las propiedades deseables de ese programa (corrección, tarea realizada, etc.)". Esa es una definición aún más aterradora.

La mejor explicación para esto es que si tiene una clase principal y una clase secundaria, entonces la clase base y la clase secundaria se pueden usar indistintamente sin obtener resultados incorrectos. Esto aún puede ser confuso, así que echemos un vistazo al ejemplo clásico de Square-Rectangle. Matemáticamente, un cuadrado es un rectángulo, pero si lo modela utilizando la relación "es-un" a través de la herencia, rápidamente se mete en problemas.



<!-- ------------------------ -->
## Principio de segregación de interfaz (ISP)
Duration: 3

JavaScript no tiene interfaces, por lo que este principio no se aplica tan estrictamente como otros. Sin embargo, es importante y relevante incluso con la falta de sistema de tipos de JavaScript.

El ISP afirma que "no se debe obligar a los clientes a depender de interfaces que no utilizan". Las interfaces son contratos implícitos en JavaScript debido a la tipificación pato.

Un buen ejemplo para observar que demuestra este principio en JavaScript es para clases que requieren objetos de configuración grandes. Es beneficioso no requerir que los clientes configuren grandes cantidades de opciones, porque la mayoría de las veces no necesitarán todas las configuraciones. Hacerlos opcionales ayuda a evitar tener una "interfaz gorda".

<!-- ------------------------ -->
## Principio de inversión de dependencia (DIP)
Duration: 3

Este principio establece dos cosas esenciales:

- Los módulos de alto nivel no deben depender de los módulos de bajo nivel. Ambos deberían depender de abstracciones. 
- Las abstracciones no deben depender de los detalles. Los detalles deben depender de las abstracciones.

Esto puede ser difícil de entender al principio, pero si ha trabajado con AngularJS, habrá visto una implementación de este principio en forma de inyección de dependencia (DI). Si bien no son conceptos idénticos, DIP evita que los módulos de alto nivel conozcan los detalles de sus módulos de bajo nivel y los configuren. Puede lograr esto a través de DI. Un gran beneficio de esto es que reduce el acoplamiento entre módulos. El acoplamiento es un patrón de desarrollo muy malo porque hace que su código sea difícil de refactorizar.

Como se indicó anteriormente, JavaScript no tiene interfaces, por lo que las abstracciones de las que se depende son contratos implícitos. Es decir, los métodos y propiedades que un objeto/clase expone a otro objeto/clase. En el ejemplo a continuación, el contrato implícito es que cualquier módulo de solicitud para InventoryTracker tendrá un método de elementos de solicitud.

<!-- ------------------------ -->
## Pruebas
Duration: 3

La prueba es más importante que el envío. Si no tiene pruebas o una cantidad inadecuada, entonces cada vez que envíe el código no estará seguro de que no haya roto nada. Decidir qué constituye una cantidad adecuada depende de su equipo, pero tener una cobertura del 100 % (todos los extractos y sucursales) es la forma en que logra una confianza muy alta y la tranquilidad del desarrollador. Esto significa que además de tener un gran marco de prueba, también necesita usar una buena herramienta de cobertura.


<!-- ------------------------ -->
## Manejo de errores
Duration: 3

¡Los errores lanzados son algo bueno! Significan que el tiempo de ejecución identificó con éxito cuando algo en su programa salió mal y le informa deteniendo la ejecución de la función en la pila actual, matando el proceso (en el nodo) y notificándoselo en la consola con un seguimiento de la pila.


<!-- ------------------------ -->
## Formateo
Duration: 3

El formateo es subjetivo. Como muchas reglas en este documento, no hay una regla estricta que deba seguir. El punto principal es NO ARGUMENTAR sobre el formato. Hay toneladas de herramientas para automatizar esto. ¡Usa uno! Es una pérdida de tiempo y dinero que los ingenieros discutan sobre el formateo.

Para las cosas que no se incluyen en el ámbito del formato automático (sangría, tabulaciones frente a espacios, comillas dobles frente a simples, etc.), consulte aquí para obtener orientación.


<!-- ------------------------ -->
## Las personas que llaman y los llamados a funciones deben estar cerca
Duration: 3

Si una función llama a otra, mantenga esas funciones verticalmente cerca en el archivo fuente. Idealmente, mantenga a la persona que llama justo encima de la persona a la que llama. Tendemos a leer el código de arriba a abajo, como un periódico. Debido a esto, haga que su código se lea de esa manera.


<!-- ------------------------ -->
## Comentarios
Duration: 5

### Solo comente cosas que tengan complejidad de lógica empresarial

Los comentarios son una disculpa, no un requisito. El buen código se documenta principalmente a sí mismo.

### Malo:

```java
  // Creating a List of customer names 
  List<String> customerNames = Arrays.asList('Bob', 'Linda', 'Steve', 'Mary'); 

  // Using Stream findFirst() 
  Optional<String> firstCustomer = customerNames.stream().findFirst(); 

  // if the stream is empty, an empty 
  // Optional is returned. 
  if (firstCustomer.isPresent()) { 
    System.out.println(firstCustomer.get()); 
  } 
  else { 
    System.out.println("no value"); 
  }
  
```

### Bueno:

```java
  List<String> customerNames = Arrays.asList('Bob', 'Linda', 'Steve', 'Mary'); 

  Optional<String> firstCustomer = customerNames.stream().findFirst(); 

  if (firstCustomer.isPresent()) { 
    System.out.println(firstCustomer.get()); 
  } 
  else { 
    System.out.println("no value"); 
  } 

```

<!-- ------------------------ -->
## No use un comentario cuando puede usar una función o una variable
Duration: 5

El mejor comentario es sin comentarios.

### Malo:

```java
  //Check to see if order is eligible to ship
  if((order.isPaid & order.isLabeled) && CUSTOMER_FLAG) {
  // ...
  }
  
```

### Bueno:

```java
  if(order.isEligibleToShip()) {
  // ...
  } 

```

<!-- ------------------------ -->
## No deje código comentado en su base de código
Duration: 5

El control de versiones existe por una razón. Deje el código antiguo en su historial.

### Malo:

```java
  doStuff();
  // doOtherStuff();
  // doSomeMoreStuff();
  // doSoMuchStuff();
  
```

### Bueno:

```java
  doStuff(); 

```

<!-- ------------------------ -->
## No tengo comentarios de diario
Duration: 5

¡Recuerde, use el control de versiones! No hay necesidad de código inactivo, código comentado y especialmente comentarios de revistas. ¡Usa git log para obtener el historial!

### Malo:

```java
  /**
  * 2021-03-06: Renamed clean to cleanCode (DL)
  * 2020-01-03: Changed return value (LB)
  * 2019-05-12: Added clean method (DL)
  */
  cleanCode(String code) {
   return null;
  }
  
```

### Bueno:

```java
   cleanCode(String code) {
   return null;
   }

```

<!-- ------------------------ -->
## Evite los marcadores de posición
Duration: 5

Por lo general, solo agregan ruido. Deje que las funciones y los nombres de las variables, junto con la sangría y el formato adecuados, le den la estructura visual a su código.

### Malo:

```java
  ////////////////////////////////////////////////////////////////////////////////
  // Instantiate Order List
  ////////////////////////////////////////////////////////////////////////////////
  List<Order> orders = new ArrayList();

  ////////////////////////////////////////////////////////////////////////////////
  // Ship Orders that are eligible
  ////////////////////////////////////////////////////////////////////////////////
 
  orders.filter(Order::isEligibleToShip).forEach(x -> ship(x));
  
```

### Bueno:

```java
   List<Order> orders = new ArrayList();

   orders.filter(Order::isEligibleToShip).forEach(x -> ship(x));

```

<!-- ------------------------ -->
## Traducción
Duration: 1

Abierto para traducciones.