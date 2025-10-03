# Universidad Nacional del Nordeste
## Facultad de Ciencias Exactas y Naturales y Agrimensura  
### Licenciatura en Sistemas de Información  
**Asignatura:** Bases de Datos I  
**Proyecto de Investigación:** Réplicas de Base de Datos  
**Alumnos:** Martinez Mauricio Marcelo – Matias Herrera – Antonela Famular – Mateo Alejo Falcon  
**Profesores:** Lic. Villegas Dario O. – Lic. Vallejos Walter O. – Exp. Cuzziol Juan J. – Lic. Badaracco Numa  
**Año:** 2025  

---

# Índice  
- [Capítulo I: Introducción](#capítulo-i-introducción)  
- [Capítulo II: Marco Conceptual](#capítulo-ii-marco-conceptual)  
- [Capítulo III: Metodología](#capítulo-iii-metodología)  
- [Capítulo IV: Desarrollo del tema / Resultados](#capítulo-iv-desarrollo-del-tema--resultados)  
- [Capítulo V: Conclusiones](#capítulo-v-conclusiones)  
- [Capítulo VI: Bibliografía](#capítulo-vi-bibliografía)  

---

# Capítulo I: Introducción
En los sistemas de información actuales, la disponibilidad y la confiabilidad de los datos constituyen un requisito esencial para el funcionamiento de aplicaciones críticas. El crecimiento exponencial de usuarios y la necesidad de realizar operaciones en tiempo real hacen que la base de datos, como núcleo del sistema, pueda convertirse en un cuello de botella.  
Frente a esta problemática, la **replicación de bases de datos** se presenta como una técnica fundamental que permite garantizar la continuidad del servicio, mejorar el rendimiento y asegurar mecanismos de recuperación ante desastres.  

El presente proyecto plantea un estudio teórico-práctico sobre la replicación de bases de datos, analizando sus fundamentos conceptuales y evaluando su implementación en un entorno de laboratorio.  
Se busca explorar los beneficios y las limitaciones de las distintas modalidades de replicación y comprender cómo impactan en la consistencia, la disponibilidad y el rendimiento de los sistemas.  

**Problema:** ¿Cómo garantizar disponibilidad, consistencia y rendimiento en un sistema de gestión de reservas con alta concurrencia de lecturas y escrituras críticas?  

**Objetivo general:** Comprender, implementar y evaluar réplicas de bases de datos en un entorno controlado, midiendo su impacto en escenarios de uso realista.  

**Objetivos específicos:**  
1. Analizar los diferentes tipos de replicación (lógica, física, síncrona, asíncrona, semi-síncrona).  
2. Implementar un entorno de laboratorio con instancias maestro y réplicas.  
3. Medir métricas clave como latencia de replicación, consistencia de datos y tiempo de recuperación ante fallos.  
4. Comparar los resultados obtenidos con la teoría y documentar hallazgos y recomendaciones.  

---

# Capítulo II: Marco Conceptual
La replicación de bases de datos consiste en mantener múltiples copias de un mismo conjunto de datos en distintas instancias de un sistema gestor, con el fin de aumentar la disponibilidad y la tolerancia a fallos.  
Este mecanismo permite que las consultas de lectura se distribuyan entre varias réplicas, aliviando la carga del nodo maestro, mientras que las operaciones de escritura se centralizan en un punto de control.  

**Principales enfoques de replicación:**  
- **Replicación transaccional:** asegura que los cambios confirmados se apliquen de forma consistente en las réplicas.  
- **Replicación lógica:** basada en eventos de alto nivel (instrucciones SQL).  
- **Replicación física:** propagación a nivel de bloques o logs binarios.  
- **Replicación síncrona:** garantiza consistencia fuerte pero añade latencia.  
- **Replicación asíncrona:** ofrece mejor rendimiento pero puede producir lecturas obsoletas.  
- **Replicación semi-síncrona:** equilibrio entre las dos anteriores.  

**Topologías comunes:**  
- Maestro con múltiples réplicas.  
- Replicación en cascada.  
- Clústeres con consenso.  

Cada modalidad se adecua a diferentes escenarios: sistemas de comercio electrónico con consultas intensivas, sistemas de telemetría con alta escritura o despliegues multi-región que buscan acercar los datos a los usuarios.  

---

# Capítulo III: Metodología
La metodología aplicada en este trabajo se divide en dos fases: **teórica** y **práctica**.  

- En la fase teórica se revisaron conceptos fundamentales de replicación mediante bibliografía académica y documentación oficial de motores de bases de datos como MySQL.  
- En la fase práctica se diseñó un laboratorio controlado que permitió implementar y probar distintos modos de replicación.  

**Actividades realizadas:**  
1. Instalación de instancias Maestro y Réplica en contenedores Docker.  
2. Configuración de parámetros de replicación (binlogs, usuarios replicadores, permisos).  
3. Ejecución de scripts de carga con operaciones de lectura y escritura.  
4. Monitoreo de métricas como *lag de réplica*, tiempo de confirmación de transacciones y recuperación tras caídas.  
5. Análisis comparativo entre replicación asíncrona y semi-síncrona.  

**Herramientas utilizadas:**  
- MySQL 8.4  
- Docker  
- Sysbench para benchmarking  
- Comandos internos (`SHOW SLAVE STATUS`) para supervisar la replicación  

Se documentaron también los errores encontrados y los pasos de resolución.  

---

# Capítulo IV: Desarrollo del tema / Resultados
El caso de estudio seleccionado corresponde a un **sistema de reservas de productos**, similar a un e-commerce.  
Este sistema combina un alto volumen de consultas de lectura con operaciones críticas de escritura, como la generación de pedidos.  
Se probaron escenarios en los que el Maestro gestionaba las escrituras mientras las réplicas respondían a las consultas de lectura concurrentes.  

**Resultados obtenidos:**  
- **Replicación asíncrona:** permitió una rápida confirmación de transacciones, con un lag de réplica que osciló entre 1 y 3 segundos bajo carga. El riesgo identificado fue la posibilidad de leer datos obsoletos tras una falla.  
- **Replicación semi-síncrona:** redujo el riesgo de pérdida de datos al requerir confirmación de al menos una réplica antes de validar el commit. Sin embargo, la latencia de las escrituras aumentó entre un 10% y un 20%.  
- **Recuperación ante fallos:** se simuló la caída del nodo maestro y las réplicas continuaron sirviendo consultas de lectura, garantizando disponibilidad parcial del sistema.  

Estos experimentos demostraron que la replicación puede mejorar significativamente la tolerancia a fallos y la capacidad de respuesta en sistemas con alta concurrencia, aunque requiere un diseño cuidadoso para equilibrar consistencia y rendimiento.  

---

# Capítulo V: Conclusiones
El análisis realizado confirma que la replicación es una técnica indispensable para los sistemas modernos que requieren disponibilidad continua y baja latencia.  

**Hallazgos principales:**  
- La replicación asíncrona es sencilla de implementar y ofrece buen rendimiento, aunque sacrifica consistencia temporal.  
- La replicación semi-síncrona logra un mejor balance entre rendimiento y seguridad de datos, pero introduce cierta latencia.  
- El diseño de topologías de replicación debe responder a las necesidades concretas del negocio: no existe una única solución universal.  
- La replicación no reemplaza otras técnicas de alta disponibilidad como el particionado o el clustering, sino que las complementa.  

En conclusión, el proyecto permitió comprender los fundamentos y los desafíos prácticos de la replicación de bases de datos, y demostró que esta técnica, correctamente implementada, mejora la robustez y confiabilidad de los sistemas de información.  

---

# Capítulo VI: Bibliografía
- Oracle. (2024). *MySQL 8.4 Reference Manual: Replication*. https://dev.mysql.com/doc/refman/8.4/en/replication.html  
- Material de cátedra: *Bases de Datos I*, Licenciatura en Sistemas de Información, UNNE.  
- Elmasri, R. & Navathe, S. (2017). *Sistemas de Bases de Datos*. Pearson.  
- Silberschatz, A., Korth, H., & Sudarshan, S. (2019). *Database System Concepts*. McGraw-Hill.  
Proyecto_investigacion_BD_GITHUB.md
9 KB
