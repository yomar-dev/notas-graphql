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


**Scalars:**  nos van a permitir definir la mayoría de las propiedades de nuestras entidades.

 - Int - Números enteros
 - Float - Números decimales
 - String - Cadenas de texto
 - Boolean - Verdadero o Falso
 - ID - Identificador único


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


**Enums:** Es un tipo que nos permite definir algo que puede tomar el valor de una lista de opciones.

*Ejemplo:* <br>

~~~
enum Genero {
	MASCULINO
	FEMENINO
}
~~~


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


**Union:** Permite definir **diferentes posibles tipos (o interfaces)** que se esperan como resultado para **diferentes tipos objetos (o entidades)** si alguna de ellas cumple con la **condición** definida para una búsqueda. <br>
*Ejemplo:* <br>

~~~
union Busqueda = Amigo | Lugar | Evento | Pagina
~~~


**Modificadores de tipo:** Existen dos modificadores de tipo. <br>

 - **Requerido:** Signo de admiración al final, y significa que ese campo no puede ser null.
 - **Corchetes:** Para denotar que tenemos una lista de lo que sea que está en medio.

~~~
String! NOT NULL
[String] Lista
[String]! Lista que no puede ser NULL pero que sin embargo sus elementos pueden ser NULL.
[String!]! Lista que no puede ser NULL ni tampoco contener ni un solo elemento NULL.
~~~


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
### GraphiQL ###

**Queries:** De esta forma le estamos indicando al servidor que información queremos, dentro del ***end-point cursos*** estamos indicando que queremos *titulo, descripcion y profesor*, además como el profesor es una entidad, debemos indicar que información queremos de él, en este caso *nacionalidad.* <br>
Además **GraphQL** nos permite pedir al servidor todo la información que necesitamos un mismo **Request** por eso también hemos solicitado el *nombre* de la entidad **profesores.** <br>

~~~
{
	cursos {
		titulo
		descripcion
		profesor {
			nacionalidad
		}
	}

	profesores {
		nombre
	}
}
~~~


**Argumentos:** En el siguiente ejemplo vemos como pasamos un parametro llamado **id** para obtener un curso en especifico.<br>

~~~
{
	curso(id: 2) {
		titulo
	}
}
~~~


**Variables:** Para usar variables es necesario utilizar la forma completa para realizar un *Query*, que es la siguiente:<br>

~~~
query <nombreQuery>(<$variable>: type = <valor por defecto>) {

}
~~~

*Ejemplo:* <br>
~~~
query getCurso($id: Int = 4){
	curso(id: $id){
		titulo
	}
}
~~~

*Sección de variables GraphiQL:* <br>
~~~
{
	"id": 4
}
~~~


**Aliases:** Permite asignar un nombre a una consulta para que al momento de pedir varios recursos del mismo tipo con diferentes **ID** el *query* de no ser así nos dará error.<br>

*Ejemplo:* <br>
~~~
{
  cursoMasVotado: curso(id: 1) {
    titulo
    rating
  }

  cursoMasVisto: curso(id: 2) {
    titulo
    descripcion
  }
}
~~~


**Fragments:** Son una manera que nos permite **GraphQL** en las consultas de agrupar campos para poder utilizarlos de una manera conveniente para nuestra consulta.<br>

*Ejemplo:* <br>
~~~
{
  curso(id: 1) {
    ...CamposNecesarios
  }
  cursos {
    ...CamposNecesarios
  }
}

fragment CamposNecesarios on Curso {
  titulo
  descripcion
}
~~~


**Inline Fragments:** Nos permite pedir los campos necesarios dependiendo del *tipo* que devuelvan los **Unions** o las **Interfaces** donde no sabemos que tipos nos devuelven.<br>

*Ejemplo:* <br>
~~~
{
	buscar(query: 'GraphQL') {
		... on Curso {
			titulo
		}
		... on Profesor {
			nombre
		}
	}
}
~~~


**Directives:** Nos permiten pedir ciertos valores de una consulta dependiendo de si una variable es **true** o **false**.

Tenemos dos directivas:

 - **@include:** Incluye un campo si el argumento es **true.**
 - **@skip:** Excluye un campo si el argumento es **true.**

*Ejemplo:* <br>

Sección de variables: <br>
~~~
{
	"conDescripcion": true
}
~~~

Petición:
~~~
query Cursos($conDescripcion: Boolean!){
	cursos {
		titulo
		profesor @include(if: $conDescripcion){
			nombre
		}
	}
}
~~~



<br><br>

### Enlaces de Interes ###
[GraphQL](http://facebook.github.io/graphql/) <br>
[GraphQL.js](https://github.com/graphql/graphql-js) <br>
[Scalar Types](http://graphql.org/learn/schema/#scalar-types) <br>
[Custom Scalars](https://www.apollographql.com/docs/graphql-tools/scalars.html) <br>
[Graphql Starwars - Example GraphQL server using node.js](https://github.com/jahewson/graphql-starwars)