# Aprendiendo-SpringBoot

Este repositorio sirve como guía, tutorial y bitácora de aprendizaje para el desarrollo de aplicaciones backend utilizando **Spring Boot 3.x** y **Java**.

---

## IDE
Para el desarrollo de este proyecto se utiliza **IntelliJ IDEA**, un entorno de desarrollo integrado (IDE) nativo para la máquina virtual de Java (JVM) que optimiza la navegación de código, la gestión de dependencias con Maven y la refactorización.

### Configuración del Entorno en IntelliJ
* **Autoguardado:** Configurado para guardar cambios automáticamente cuando el IDE detecta inactividad o al cambiar de ventana.
* **Corrección Ortográfica:** Desactivada (`Editor > Inspections > Proofreading > Typo`) para evitar advertencias innecesarias en el código fuente y en los archivos de documentación.

---

## Inicialización del Proyecto
Para generar la estructura base del proyecto se utiliza **[Spring Initializr](https://start.spring.io/)**, una herramienta oficial que permite configurar y descargar un esqueleto de proyecto Spring Boot con las dependencias necesarias.

### Configuración utilizada
* **Project:** Maven
* **Language:** Java
* **Spring Boot:** 4.0.7
* **Java:** 17
* **Packaging:** Jar
* **Configuration:** Properties
* **Group:** `com.astrod`
* **Artifact:** `api`
* **Package name:** `com.astrod.api`

### Dependencias
* **Spring Web:** para construir aplicaciones RESTful con Spring MVC (Tomcat embebido).
* **Lombok:** librería de anotaciones para reducir el código repetitivo (boilerplate).
* **MySQL Driver:** driver JDBC para conectar con bases de datos MySQL.
* **Spring Data JPA:** para persistencia de datos en bases de datos SQL usando JPA y Hibernate.

## Ejecutar en IntelliJ IDEA

Para ejecutar el proyecto en IntelliJ IDEA, sigue estos pasos:
1. **Importar como proyecto Maven:** Haz clic derecho en el archivo `pom.xml` y selecciona **"Add as Maven Project"**. Esto permitirá que IntelliJ IDEA reconozca la estructura del proyecto y gestione las dependencias correctamente.
2. **Esperar la sincronización:** Deja que IntelliJ IDEA descargue las dependencias y sincronice el proyecto con Maven.
3. **Ejecutar la aplicación:** Busca la clase principal `ApiApplication.java` (ubicada en `src/main/java/com/astrod/api/`), haz clic derecho sobre ella y selecciona **"Run 'ApiApplication.main()'"**. 

Ahora se ejecuta, pero me da un error porque no está conectado a una base de datos

```
Description:

Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.

Reason: Failed to determine a suitable driver class

```

Vamos a crear la base de datos.

---

## Host de base de datos

El host será **[Clever Cloud](https://www.clever.cloud/)**. Es gratuito hasta 10MB de almacenamiento.

## 1. Crear Add-on
1. En Clever Cloud: **Create... > an add-on > MySQL**.
2. Configura: Nombre, región **Paris (Europe)** y plan **DEV (0€ / Gratis)**.

## 2. Credenciales a copiar
Del panel de control

## 3. Configurar application.properties
click en export environment variables
```
MYSQL_ADDON_HOST=begey73bcijbj9q7igtc-mysql.services.clever-cloud.com
MYSQL_ADDON_DB=begey73bcijbj9q7igtc
MYSQL_ADDON_USER=unv5oyj4qiiv2ujc
MYSQL_ADDON_PORT=3306
MYSQL_ADDON_PASSWORD=<<CONTRASEÑA>>
```

y escribir esto en application.properties
```
spring.datasource.url=jdbc:mysql://begey73bcijbj9q7igtc-mysql.services.clever-cloud.com:3306/begey73bcijbj9q7igtc?serverTimezone=UTC&useSSL=false
spring.datasource.username=unv5oyj4qiiv2ujc
spring.datasource.password=<<CONTRASEÑA>>

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.open-in-view=false
```

Ahora ejecuta sin errores

---

## Creación de una tabla

He decidido que mi base de datos va a ser de sitios web, y que el frontend va a ser un dashboard configurable.
Mi clase website va a tener atributos id, name, url, section, category.

por ejemplo, youtube sería algo así:

```
YouTube  https://www.youtube.com/  Social    RRSS
```

voy a crear un paquete model con una clase Website

como tenemos Lombok (biblioteca) podemos ahorrarnos todos los getter, setter, constructor, tostring
usando anotaciones, como se puede ver aqui

Tambien usamos las anotaciones de @table, @entity, @id para que se genere el codigo sql
```
package com.astrod.api.model;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "websites")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@ToString
@EqualsAndHashCode(of = "id")
public class Website {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;

    private String name;
    private String url;
    private String section;
    private String category;
}

```

al ejecutar me aparece esto:

```
Hibernate: create table websites (id bigint not null auto_increment, category varchar(255), name varchar(255), section varchar(255), url varchar(255), primary key (id)) engine=InnoDB
```
Se ha generado la tabla websites!

---

## Creación de repositorio
voy a crear un paquete repository dentro de model

---

## Host de frontend
El backend se encuentra desplegado en **[Render](https://render.com/)**.

Render es una plataforma en la nube (PaaS) que automatiza el despliegue de aplicaciones conectándose directamente a este repositorio de GitHub mediante un **Web Service**. Se ha seleccionado esta opción debido a que ofrece un plan gratuito que permite ejecutar la máquina virtual de Java (JVM) sin necesidad de introducir métodos de pago de entrada.

---