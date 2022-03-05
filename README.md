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