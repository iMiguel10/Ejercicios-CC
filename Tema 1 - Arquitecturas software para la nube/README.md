# Ejercicios Tema 1 - Arquitecturas software para la nube
---

### 1. Buscar una aplicación de ejemplo, preferiblemente propia, y deducir qué patrón es el que usa. ¿Qué habría que hacer para evolucionar a un patrón tipo microservicios?

Por ejemplo, una aplicación que genere contraseñas y las almacene, todo esto con una arquitectura monolítica de manera que todo sea una unidad.

Para evolucionar esta aplicación a microservicios tendríamos que dividir en servicios independientes cada una de las funcionalidades en las que no exista una dependencia entre si. Por ejemplo, en el caso anterior, sería divirlo en microservicios tales que: Gestión de usuarios, gestión de contraseñas y generador de contraseñas. De esta manera nos quedan 3 servicios distintos.

---

### 2. En la aplicación que se ha usado como ejemplo en el ejercicio anterior, ¿podría usar diferentes lenguajes? ¿Qué almacenes de datos serían los más convenientes?

Si se podrían utilizar diferentes lenguajes. Y en cuanto a los almacenes de datos que se pueden usar, los más convenientes serían almacenes SQL para los usuarios, y almacenes NoSQL para la gestión de las contraseñas de los usuarios.

---
