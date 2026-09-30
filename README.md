# Caso de Estudio: Arquitectura de Base de Datos Segura para solución Web TechStore

**Autor:** Gustavo Rojas VEra

## Descripción del Proyecto
El presente Repositorio contiene el análisis y diseño de la arquitectura de BD para la aplicación web "TechStore". El objetivo es garantizar la disponibilidad, el rendimiento y, sobre todo, la seguridad y consistencia de las transacciones financieras y de inventario.

## 1. Tipo de Base de Datos
Se ha optado por el despliegue de una Base de Datos SQL (Relacional) en lugar de NoSQL. 
* **Justificación técnica:** El sistema gestiona compras de e-commerce (usuarios, productos, transacciones). Las bases de datos relacionales cumplen con las propiedades ACID (Atomicidad, Consistencia, Aislamiento (Isolation) y Durabilidad).

## 2. Arquitectura de Red (3-Tier)
Para proteger la base de datos de accesos no autorizados, se implementó una arquitectura de tres capas:
1. **Capa Web (Subred Pública):** Recibe las peticiones del cliente mediante tráfico cifrado (HTTPS/TLS). En esta capa se aplica además "sanitización" mediante JavaScript.
2. **Capa de Aplicación (Subred Privada):** Servidor Backend que procesa la lógica de negocio y se aísla de internet.
3. **Capa de Datos (Subred Privada aislada):** Servidor SQL que solo acepta conexiones internas desde el Backend.

## 3. Medidas de Seguridad Implementadas
* **Consultas Parametrizadas:** Utilizadas en el Backend para separar el código SQL de los inputs del usuario, neutralizando por completo el riesgo de ataques por Inyección SQL (SQLi).
* **Validaciones Backend:** El servidor de aplicación no confía en los datos enviados por el cliente (navegador). Los precios y saldos se calculan estrictamente en el servidor.
* **Control de Acceso (DCL):** Se aplica el principio de mínimo privilegio, otorgando solo permisos de lectura y escritura al usuario de la aplicación, bloqueando comandos destructivos.
* **Segmentación de Red:** El modelo de 3 capas implementado garantiza que la BD permanece aislada y sin posibilidad de conexión desde redes públicas, acotando las conexiones solo a los servidores PRT.

## 4. Instrucciones y Transaccionalidad
La operación del modelo de BASE DE DATOS requiere los siguientes lenguajes de control:
* **DDL (Data Definition Language):** `CREATE TABLE` y `ALTER TABLE` para estructurar entidades (Usuarios, Productos, Transacciones).
* **DML (Data Manipulation Language):** `SELECT`, `INSERT`, `UPDATE` para la interacción diaria con el catálogo y las compras.
* **TML (Transaction Management Language):** Uso crítico de `START TRANSACTION`, `COMMIT` y `ROLLBACK`. Si el pago falla o el stock se agota durante el proceso, el `ROLLBACK` revierte cualquier cambio, protegiendo la integridad financiera de la tienda.

## Etc
* El diagrama de arquitectura detallado se encuentra adjunto en este repositorio en formato PDF.
