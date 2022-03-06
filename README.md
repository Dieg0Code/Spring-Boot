# Apuntes

## ¿Qué es Spring Boot?

**Spring Boot** es un sub-proyecto de Spring Framework que busca facilitarnos la creación de proyectos con **Spring Framework** eliminando la necesidad de crear largos archivos de configuración XML.

- Estas configuraciones tediosas y propensas a errores ya no son necesarias debido a que **Spring Boot** provee **configuraciones por defecto** para la mayoría de las tecnologías usadas (Spring MVC, Spring Data JPA & Hibernate, Spring Security, Spring REST, etc).

- **Spring Boot** nos ayuda a administrar todas las dependencias (archivos JAR y versiones compatibles).

- **Spring Boot** provee un modelo de programación parecido a las aplicaciones java tradicionales que se inician en el método main.

## ¿Como funciona Spring Boot?

Proceso típico para desarrollar una aplicación de Spring.

1. Seleccionar Dependencias necesarias con Maven (deben ser compatibles).
2. Crear nuestra aplicación.
3. Realizar el Deployment en el servidor.

**Spring Boot** nace con la intención de simplificar los pasos 1 y 3, y que nos podamos centrar en el desarrollo de nuestra aplicación.

### ¿Cómo se simplifican el paso 1 y 3?

- Permite crear aplicaciones **Stand-Alone** con Spring.
    - **Stand-Alone**: Aplicación independiente (no requiere un servidor web).
    - Aplicación que corre desde la línea de comandos (cmd, shell) y necesariamente tiene que contener un método main. *$ java -jar mywebapp.jar*
- Incluye un servidor web Apache Tomcat Embebido (se puede cambiar por Jetty o Undertow).
- Se requiere mínima configuración debido a:
    - No es necesario más archivos XML.
    - Las configuraciones para la mayoría de las tecnologías ya se incluyen con valores por defecto (Spring MVC, Spring Data JPA & Hibernate, Spring Security, Spring REST, etc).
        - La configuración por defecto es en base a los parámetros y valores más usados por la mayoría de los usuarios que usan Spring.
- Incluye características listas para entornos de producción:
    - Revisión de funcionalidad.
    - Métricas de la aplicación.

## Crear un proyecto con Spring Boot

Para crear un proyecto con Spring Boot, debemos ir a la página [Spring Initializer](https://start.spring.io/) y configurar el proyecto que se desea crear, ahí mismo podemos configurar las dependencias que necesitamos.

Los proyectos creados con Spring Boot usan los decoradores para agregar funcionalidades.

Por ejemplo el archivo main se ve así:

```java
package dig0code.holaMundo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class HolaMundoApplication {

	public static void main(String[] args) {
		SpringApplication.run(HolaMundoApplication.class, args);
	}

}
```

Un controlador se ve así:

```java
package dig0code.holaMundo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HomeController {

    @GetMapping("/")
    public String inicio() {
        return "Hola Mundo";
    }

}
```

Con el decorador ``@SpringBootApplication``, indicamos que la clase es una aplicación de Spring Boot.

Con el decorador ``@RestController``, indicamos que la clase es un controlador REST.

Y con el método ``@GetMapping``, indicamos que la ruta ``"/"`` es la ruta de acceso al controlador mediante una petición GET.

### ¿Que pasa si queremos cambiar el puerto por defecto?

Por defecto el servidor web Apache Tomcat embebido utiliza el puerto 8080, puede que queramos cambiar esto, para eso debemos configurar el archivo ``application.properties``.

```java
// application.properties
server.port=8888
```

Este archivo se usa para diferentes configuraciones del proyecto.

## ¿Qué es Spring MVC?

- Spring MVC utiliza una arquitectura de aplicaciones siguiendo el patrón de diseño MVC (**M**odel-**V**iew-**C**ontroller).
- Spring MVC es un framework web basado en Servlets que viene incluido en Spring Framework (spring-webmvc).
- Spring MVC está diseñado siguiendo el patrón de diseño de **Front Controller**.
- En Spring MVC el Front Controller es mejor conocido como **DispatcherServlet**.

Otras funciones del Front Controller son:

- Enviar las peticiones (request) a los manejadores (handlers) para que sean procesadas.
- El default handler son los controladores (@Controller, @RequestMapping).
- Encargado de resolver las vistas (views).

A partir de Spring 3.0 se pueden crear RESTFul Web Services utilizando la notación **@RestController** y **@PathVariable**.

- Basado en Spring IOC container (Inyección de Dependencias).
- Spring MVC se integra muy fácil con otros proyectos de Spring:
    - Nosotros integraremos **Spring Boot**, **Spring Data JPA**, **Spring Security**, **Spring REST**, etc.


## ¿Qué es un Controlador en Spring MVC?

Un controlador (Controller) en Spring MVC es una clase normal a la cual se le agrega la anotación **@Controller** a nivel de la clase.

Es una aplicación web, estos métodos principalmente están marcados con las notaciones **@GetMapping**, **@PostMapping**, y **@RequestMapping** (Action Controller).

Los métodos pueden tener cualquier nombre y deben regresar un String (nombre de la vista).

Los métodos son ejecutados al ser invocados por medio de la URL especificada como parámetro en las anotaciones **@GetMapping**, **@PostMapping**, etc.

```java
@Controller
public class HomeController {

    @GetMapping("/miUrl") // localhost:8080/miUrl
    public String inicio() {
        // Mi lógica de negocio
        return "home";
    }

}
```

Cuando se utiliza el motor de plantillas **Thymeleaft** se buscará un archivo (vista) llamado **home.html** en el directorio: **src/main/resources/templates**.

## ¿Qué es Thymeleaft?

Thymeleaft es un motor de plantillas para aplicaciones web desarrolladas con Java. Es algo similar a los JSP's, con algunas diferencias.

- Página Oficial: www.thymeleaf.org

Comúnmente utilizado con Spring Boot para generar vistas con código HTML para aplicaciones web.

En un proyecto Spring Boot, ya viene configurado Thymeleaf con valores por defecto al momento de agregar la dependencia.

### Configuración

Agregar la siguiente dependencia al archivo ``pom.xml`` a un proyecto Spring Boot ya creado:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

Otra forma es:

Crear el proyecto con **Spring Initializr**:

Dependecies: Thymeleaf

Para utilizar **Thymeleaf** en un archivo HTML se debe agregar el namespace.

```html
// home.html
<!DOCTYPE html>
<html xmlns:th="https://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Titulo</title>
</head>
<body>
    <h1 th:text="${mensaje}"></h1>
</body>
</html>
```

```java
// HomeController.java
@Controller
public class HomeController {
    
    @GetMapping("/")
    public String mostrarHome(Model model) {
        model.addAttribute("mensaje", "Hola Mundo");
        return "home";
    }

}
```

La variable ``mensaje`` es agregada al modelo desde un controlador, en este caso tendría el texto ``"Hola Mundo"``.

## Iteraciones en Thymeleaf

En Thymeleaf las iteraciones se pueden realizar con la expresión ``th:each``. Similar a un ``for`` en Java.

Esta expresión puede iterar sobre diferentes tipos de datos como:

- List
- Map
- Iterable

```html
<!-- Vista (detalle.html) -->
<tr th:each="tmpEmp: ${empleos}">

    <td th:text="${tmpEmp}" />
</tr>
```

Con la expresión ``th:each`` se puede iterar sobre una lista de objetos, luego declaramos la variable temporal ``tmpEmp`` la cual va a representar cada elemento durante la iteración de la lista, luego con ``${empleos}`` se indica el nombre del atributo que se va a iterar. De esta forma en donde este declarado el ``th:each``, se va a renderizar ``n`` veces dependiendo del numero de elementos que contenga la lista.

```java
// Controlador
@GetMapping("/detalle")
public String mostrarDetalle(Model model) {
    List<String> lista = new LinkedList<>();
    lista.add("Ingeniero de Sistemas");
    lista.add("Auxiliar de Contabilidad");
    model.addAttribute("empleos", lista);
    return "detalle";
}
```

## Condicionales en Thymeleaf

### Operador Elvis (?:)

El operador Elvis permite renderizar texto **dentro** de un elemento HTML, dependiendo de una expresión Booleana. Es muy similar al operador ternario en otros lenguajes de programación.

Ejemplo:

```html
<td th:text="${usuarios.estatus == 1} ? 'ACTIVO' : 'BLOQUEADO'"/>
```

Donde ``ACTIVO`` es el valor renderizado si la expresión es verdadera o ``BLOQUEADO`` si es falsa.

### IF - Unless

La expresión ``if - unless`` permite renderizar un elemento HTML, dependiendo de una expresión Booleana. Es muy similar a un ``if - else`` en otros lenguajes de programación.

Ejemplo:

```html
<td>
    <span th:if="${alumno.genero == 'F'}"> Femenino </span>
    <span th:unless="${alumno.genero == 'F'}"> Masculino </span>
</td>
```

## Urls relativas al ContextPath en Thymeleaf

Las URLs relativas al ContextPath son las que son relativas al directorio raíz (ROOT) de una aplicación web, una vez están publicadas en el servidor.

Las URLs relativas al ContextPath deben iniciar con "/" cuando vayamos a formar una URL para referenciar un recurso (imágenes, CSS, JS, PDF, etc) en nuestra aplicación.

En un proyecto web cuando se utiliza ``Thymeleaf`` como motor de plantillas, los recursos estáticos deben guardarse en el directorio **src/main/resources/static**.

Ejemplos:

- Para incluir el archivo CSS myStyles.css en una vista se utilizaría la siguiente expresión:

```html
<link th:href"@{/css/myStyles.css}" rel="stylesheet">
```

- Para incluir el archivo JavaScript funciones.js en una vista se utilizaría la siguiente expresión:

```html
<script th:src="@{/js/funciones.js}"></script>
```

- Para incluir la imagen foto.png en una vista, se utilizaría la siguiente expresión:

```html
<img th:src="@{/images/foto.png}" width="136" height="136">
```

Para incluir archivos JavaScript y CSS vía CDN (content delivery network) se utiliza la sintaxis estándar (sin expresiones Thymeleaf).

## Arquitectura de Spring MVC - Ciclo de vida de una petición HTTP

![arquitectura-spring-mvc](/assets/arq-spring-mvc.png)

El ciclo de vida de una petición HTTP comienza cuando un usuario hace una solicitud a una aplicación desarrollada en este caso con **Spring MVC** que está alojada en un servidor web, este servidor web por lo general tiene integrado un motor para procesar Servlets y JSP, en La gráfica se muestra a manera de ejemplo que este motor de Servlets es **apache tomcat**.

La petición HTTP puede ser solicitar una página web por medio de una URL a través de un navegador, después de que la petición es enviada por el usuario, esta es recibid por el ``Front Controller`` que básicamente es un Servlet llamado ``DispatcherServlet`` el cual está configurado para recibir todas las URLs que sean procesadas por Spring MVC. En términos sencillos este Servlet recibe todas las peticiones HTTP, en la gráfica está representado por el numero ``2``.

Después de que el ``Front Controller`` recibe la petición analiza la URL a la cual fué hecha la petición, en este punto según la configuración del ``DispatcherServlet`` el ``From Controller`` va a buscar las clases que tienen la notación ``@Controller`` que es propia de Spring MVC, es decir, va a revisar todos los controladores que hay registrados para ver cual está mapeado a la URL, en caso de encontrar un controlador mapeado a la URL a la cual fué enviada la petición, el ``Front Controller`` delega la petición (numero ``3``), en caso de no encontrar un ``Controller`` mapeado a esta URL, el ``Front Controller`` enviá el error ``404 Not Found``.

Si existe un controlador encargado de procesar la URL, el controlador recibe la petición y se encarga de procesarla, aquí es donde la verdadera lógica de la aplicación se aplica, por lo general esta lógica consiste en recibir los datos enviados por el usuario a través de su petición, por ejemplo los datos de un formulario HTML, posteriormente estos datos son procesados, por ejemplo pueden ser almacenados en una base de datos, hacer algunos cálculos o generar reportes, en la gráfica es el numero ``4``.

Para realizar el procesamiento de los datos y en general la lógica del negocio, los controladores casi siempre hacen uso de componentes de la capa de servicio de la aplicación, estos componentes de servicio a su vez utilizan componentes de la capa de datos para interactuar con la base de datos. Esto no siempre es así, puede ser que un controlador solo tenga asignada la tarea de regresar una vista con un formulario HTML para ser completado por el usuario y por lo tanto puede ser que no necesite ningún otro tipo de componente.

Después de que el controlador ha terminado de procesar la solicitud, el controlador puede generar el modelo que será renderizado en la vista posteriormente. El modelo básicamente son objetos que representan los datos de nuestra aplicación, en la gráfica es el numero ``5``. Una vez que esta generado el modelo el controlador debe indicar cual será la vista que posteriormente será la encargada de renderizar el modelo que previamente ha sido generado, en este paso el controlador envía el modelo junto con el nombre de la vista al ``Front Controller`` en la gráfica es el numero ``6``. En el ``Front Controller`` está la configuración del ``view resolver``, este es un componente de Spring-MVC que básicamente es el encargado de buscar las vistas de nuestra aplicación, con está configuración el ``Front Controller`` busca el nombre de la vista que previamente fue enviada por el controlador y la renderiza, en la gráfica es el numero ``7``.

Después de que la respuesta del usuario es generada, que por lo general es HTML, el motor de plantillas regresa el control de flujo de la petición al ``Front Controller``, en la gráfica es el paso numero ``8``.

Después de que el ``Front Controller`` recibe la respuesta final del usuario, se prepara y en la mayoría de los casos esta respuesta es enviada al navegador en formato HTML, en la gráfica es el paso numero ``9`` y ``10``.