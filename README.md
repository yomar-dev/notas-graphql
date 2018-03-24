# Notas GraphQL #


### GraphQL: ###

Para comenzar debemos entender que **GraphQL** es un query language, es decir, un lenguaje de consultas. Un lenguaje es un sistema compartido por dos partes que les permite comunicarse entre sí.

Un lenguaje de consultas como **GraphQL** nos permite *hacer consultas y esperar una respuesta predecible.* Un ejemplo de una lenguaje de consultas es **SQL**, el cual se enfoca en las consultas a una base de datos.

Aunque suene un poco confuso, **SQL** no tiene nada que ver con **GraphQL**, ya que el primero está pensado para trabajar con bases de datos, y **GraphQL** es para **_comunicar clientes y servidores._**

**GraphQL** es una herramienta que se presenta como una alternativa a **REST**. La principal mejora que propone es la optimización, además de trasladar la información del servidor al cliente.

Una de las ventajas más importantes de **GraphQL** es que es agnóstico de plataforma, lo que quiere decir que se puede implementar en más de 20 lenguajes.

El principal objetivo de **GraphQL** es evitar las múltiples consultas al servidor.


<br>
### GraphQL vs REST ###

**REST** es solo una convención, lo que quiere decir que es solo la manera en que nos ponemos de acuerdo para comunicarnos. Sin embargo, el hecho de que no hay unas reglas establecidas genera que cada uno utilice la convención de la forma que más le convenga y esto hace que no haya un orden establecido.

**GraphQL** por otro lado, es un lenguaje tipado y validable, por esto conocemos la forma en la que debemos enviar y recibir.

En **REST** el servidor expone una serie de recursos, mientras que en **GraphQL** el cliente es quien define qué quiere recibir. Además, **REST** tiene el problema del *overfetching* que significa que envía más información de la que se necesita, en **GraphQL** se envía solo lo necesario.

Al ser un lenguaje tipado, **GraphQL** es un lenguaje documentado por definición.



<br>
### Construyendo esquemas a través de tipos ###

**El Schema - Types:** Es la columna vertebral de **GraphQL** y es la manera en la que decidimos las entidades, cómo se relacionan entre ellas, cuáles son las entidades que están disponibles para cada cliente, en pocas palabras, es todo lo que el cliente puede pedir a través de **GraphQL.** <br>
*Algo que tenemos que saber es que los Schemas están compuestos de Types.*

Los **types** o tipos de datos que pueden usarse en un **schema** son los siguiente:

 - Scalar
 - Objects
 - Enums
 - Interfaces
 - Unions

<br>
**Scalars:**  nos van a permitir definir la mayoría de las propiedades de nuestras entidades.

 - Int - Números enteros
 - Float - Números decimales
 - String - Cadenas de texto
 - Boolean - Verdadero o Falso
 - ID - Identificador único

<br>
**Objects:** Son los que nos permiten definir entidades. <br>
*Ejemplo:* <br>

~~~
type Curso {
  id : ID!
  descripcion: String
  profesores: [Profesor]
  rating: Int
}

type Profesor {
  nombre: String
  edad: Int
  tieneMascota: Boolean
}
~~~

<br>
**Enums:** Es un tipo que nos permite definir algo que puede tomar el valor de una lista de opciones. <br>
*Ejemplo:* <br>

~~~
enum Genero {
	MASCULINO
	FEMENINO
}
~~~

<br>
**Interface:** Permite que los atributos especificados dentro de esta, tambien se definan en los objetos que la implementen. <br>
*Ejemplo:* <br>

~~~
interface Perfil{
  nombre: String!
  mail: String!
}

type PerfilFB implements Perfil{
  nombre: String!
  mail: String!
  amigos: [Usuario]
}

type PerfilTW implements Perfil{
  nombre: String!
  mail: String!
  handle: String
  seguidores: [Usuario]
}
~~~

<br>
**Union:** Permite definir **diferentes posibles tipos (o interfaces)** que se esperan como resultado para **diferentes tipos objetos (o entidades)** si alguna de ellas cumple con la **condición** definida para una búsqueda. <br>
*Ejemplo:* <br>

~~~
union Busqueda = Amigo | Lugar | Evento | Pagina
~~~

<br>
**Modificadores de tipo:** Existen dos modificadores de tipo. <br>

 - **Requerido:** Signo de admiración al final, y significa que ese campo no puede ser null.
 - **Corchetes:** Para denotar que tenemos una lista de lo que sea que está en medio.

~~~
String! NOT NULL
[String] Lista
[String]! Lista que no puede ser NULL pero que sin embargo sus elementos pueden ser NULL.
[String!]! Lista que no puede ser NULL ni tampoco contener ni un solo elemento NULL.
~~~

<br>
**Root Type - Query:** Podríamos verlos como una analogía a los **endpoints** que tenemos en una arquitectura **REST.** <br>
*Ejemplo:* <br>

~~~
type Query{
	cursos : [Curso]
	profesores: [Profesor]
	curso(id: String!): Curso
	profesor(id: String!, limite: Int): Profesor
}
~~~

<br>
**Root Type - Mutation:** Permite definir *insertar, modificar o eliminar elementos.* <br>
*Ejemplo:* <br>

~~~
type Mutation {
	agregarCurso(
		descripcion: String,
		profesorId: String
	): Curso
}
~~~


<br><br>

### Enlaces de Interes ###
[GraphQL](http://facebook.github.io/graphql/) <br>
[GraphQL.js](https://github.com/graphql/graphql-js) <br>
[Scalar Types](http://graphql.org/learn/schema/#scalar-types) <br>
[Custom Scalars](https://www.apollographql.com/docs/graphql-tools/scalars.html) <br>
[Graphql Starwars - Example GraphQL server using node.js](https://github.com/jahewson/graphql-starwars)