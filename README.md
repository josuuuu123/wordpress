Informe de Práctica: Implementación de Infraestructura Web con Docker
1. Título

Despliegue de un CMS WordPress mediante Microservicios Orquestados: Integración de Servidor Web, Base de Datos y Gestor de DB en Entornos de Contenedores

2. Tiempo de duración

120 minutos

3. Fundamentos

La tecnología de contenedores ha transformado significativamente el ciclo de vida del desarrollo y despliegue de software moderno. Los contenedores permiten empaquetar aplicaciones junto con todas sus dependencias, librerías y configuraciones necesarias, garantizando que el software funcione de manera consistente en diferentes entornos. Esta característica reduce problemas relacionados con incompatibilidades de versiones y diferencias entre sistemas operativos.

Docker es una de las plataformas más utilizadas para la administración de contenedores debido a su facilidad de uso, eficiencia y capacidad de automatización. A diferencia de las máquinas virtuales tradicionales, Docker comparte el kernel del sistema operativo anfitrión, permitiendo un menor consumo de recursos y una mayor velocidad de ejecución. Gracias a ello, múltiples servicios pueden ejecutarse de manera aislada dentro de un mismo equipo físico sin afectar el rendimiento general del sistema.

En esta práctica se implementó una arquitectura basada en microservicios utilizando varios contenedores independientes. Cada contenedor cumplió una función específica dentro del ecosistema de la aplicación web:

Docker Engine: Encargado de administrar la creación, ejecución y monitoreo de los contenedores.
MySQL: Servicio de base de datos encargado de almacenar la información del CMS WordPress.
WordPress: Aplicación web utilizada para la administración y creación del sitio web.
phpMyAdmin: Herramienta gráfica para gestionar bases de datos MySQL desde el navegador.

Otro aspecto importante fue el uso de redes virtuales personalizadas mediante Docker Networking. Esto permitió la comunicación interna entre contenedores utilizando nombres de host, facilitando la conexión entre WordPress y MySQL sin necesidad de utilizar direcciones IP manuales.

También se implementaron volúmenes persistentes para almacenar información importante de manera permanente. Gracias a los volúmenes, los datos de WordPress y MySQL permanecen disponibles incluso si los contenedores son eliminados o reiniciados.

Finalmente, se utilizó el mecanismo de mapeo de puertos para permitir el acceso externo a los servicios web desde el navegador. WordPress se publicó mediante el puerto 80 y phpMyAdmin mediante el puerto 8081.

4. Conocimientos previos

Para desarrollar correctamente esta práctica fue necesario contar con conocimientos básicos sobre los siguientes temas:

Manejo de comandos Linux mediante terminal.
Uso de Docker y contenedores.
Gestión de redes y puertos.
Conceptos básicos de bases de datos relacionales.
Uso de navegadores web y protocolos HTTP.
Administración básica de servicios web.
5. Objetivos a alcanzar
Objetivo General

Implementar una infraestructura web basada en contenedores Docker para desplegar un entorno funcional de WordPress conectado a una base de datos MySQL.

Objetivos Específicos
Implementar contenedores interconectados mediante una red privada virtual.
Configurar volúmenes persistentes para almacenar información de manera segura.
Desplegar WordPress y phpMyAdmin mediante contenedores Docker.
Validar el acceso externo a los servicios utilizando mapeo de puertos.
Verificar la comunicación correcta entre los contenedores.
6. Equipo necesario
Computador con acceso a internet.
Cuenta activa en la plataforma Killercoda.
Docker Engine v24.0 o superior.
Navegador web actualizado.
Terminal Linux o consola Bash.
7. Material de apoyo
Manual oficial de Docker.
Documentación técnica de Docker Hub.
Guía de comandos Linux básicos.
Tutoriales de redes y volúmenes en Docker.
8. Procedimiento
Paso 1: Creación de la Red Virtual

Se creó una red personalizada llamada red-wp para permitir la comunicación entre los contenedores.

docker network create red-wp
Paso 2: Creación de Volúmenes Persistentes

Se crearon dos volúmenes para almacenar información persistente de MySQL y WordPress.

docker volume create db-data
docker volume create wp-data
Paso 3: Despliegue del Contenedor MySQL

Se ejecutó un contenedor MySQL utilizando variables de entorno para definir usuarios, contraseñas y base de datos inicial.

docker run -d \
--name mysql-db \
--network red-wp \
-v db-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=741951 \
-e MYSQL_DATABASE=wordpress_db \
-e MYSQL_USER=josue \
-e MYSQL_PASSWORD=741951 \
mysql:8.0
Parámetros utilizados
Parámetro	Función
--name	Define el nombre del contenedor
--network	Conecta el contenedor a la red privada
-v	Asocia un volumen persistente
-e	Define variables de entorno
mysql:8.0	Imagen oficial de MySQL
Paso 4: Despliegue de phpMyAdmin

Se desplegó phpMyAdmin para administrar la base de datos desde el navegador.

docker run -d \
--name phpmyadmin \
--network red-wp \
-e PMA_HOST=mysql-db \
-p 8081:80 \
phpmyadmin/phpmyadmin
Paso 5: Despliegue de WordPress

Se implementó WordPress conectado al contenedor MySQL mediante la red creada previamente.

docker run -d \
--name wordpress \
--network red-wp \
-v wp-data:/var/www/html \
-e WORDPRESS_DB_HOST=mysql-db \
-e WORDPRESS_DB_USER=josue \
-e WORDPRESS_DB_PASSWORD=741951 \
-e WORDPRESS_DB_NAME=wordpress_db \
-p 80:80 \
wordpress
Paso 6: Verificación de Contenedores

Finalmente, se verificó el estado de ejecución de todos los contenedores mediante:

docker ps
9. Diagrama de la Solución

La arquitectura implementada estuvo conformada por tres contenedores principales conectados mediante una red privada Docker.

                ┌──────────────────────┐
                │      Navegador       │
                └─────────┬────────────┘
                          │
          ┌───────────────┴────────────────┐
          │                                │
     Puerto 80                        Puerto 8081
          │                                │
┌──────────────────┐          ┌────────────────────┐
│    WordPress     │          │    phpMyAdmin      │
│   Contenedor     │          │    Contenedor      │
└────────┬─────────┘          └─────────┬──────────┘
         │                               │
         └────────── Red Docker ─────────┘
                         │
               ┌──────────────────┐
               │     MySQL DB     │
               │    Contenedor    │
               └──────────────────┘
                         │
                  Volumen db-data
10. Resultados Esperados (Capturas de Pantalla)
A. Creación y Estado de Contenedores

En esta sección se debe evidenciar:

Ejecución correcta de comandos docker run.
Verificación de servicios mediante docker ps.
Estado Up en todos los contenedores.
Evidencia esperada
Captura de terminal mostrando:
mysql-db
wordpress
phpmyadmin

Figura 1-1. Contenedores Docker ejecutándose correctamente.

B. Validación de Servicios Web

Se verificó el acceso exitoso a los servicios web desde el navegador.

Servicios comprobados
Servicio	URL
WordPress	http://localhost
phpMyAdmin	http://localhost:8081
Evidencias esperadas
Pantalla inicial de configuración de WordPress.
Acceso exitoso a phpMyAdmin.
Conexión correcta con la base de datos MySQL.

Figura 1-2. Interfaz de WordPress lista para configuración.

Figura 1-3. phpMyAdmin conectado correctamente al servidor MySQL.

11. Conclusiones
Docker facilita el despliegue rápido de aplicaciones mediante contenedores aislados.
El uso de redes personalizadas permitió la comunicación eficiente entre servicios.
Los volúmenes persistentes garantizaron la conservación de la información.
WordPress y phpMyAdmin funcionaron correctamente gracias al mapeo de puertos.
La arquitectura basada en microservicios mejora la organización y escalabilidad de las aplicaciones.
12. Bibliografía
Docker Documentation. (2024). Volumes and Networking in Containers. Recuperado de:
Docker Documentation
Docker Documentation. (2024). Docker Networking Overview. Recuperado de:
Docker Networking Overview
Méndez, A. R. (2021). Contenedores de software: una alternativa para el despliegue de aplicaciones web. Revista Cubana de Ciencias Informáticas.
Containerization and the PaaS Cloud — Pahl, C. (2015). Containerization and the PaaS Cloud. IEEE Cloud Computing.
