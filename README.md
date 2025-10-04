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

Los sistemas de información modernos requieren alta disponibilidad, baja latencia y escalabilidad bajo alta demanda.  
La base de datos suele ser un recurso compartido crítico y un posible cuello de botella.  

La **replicación de datos**, entendida como el mantenimiento de copias sincronizadas en uno o más nodos, es una técnica esencial para:  

1. Distribuir la carga de trabajo (principalmente lecturas).  
2. Aumentar la tolerancia a fallos y reducir el tiempo de inactividad.  
3. Implementar estrategias de recuperación ante desastres.  

Sin embargo, la replicación conlleva decisiones arquitectónicas que implican compromisos:  
consistencia vs. disponibilidad, latencia adicional en modos síncronos, riesgo de lecturas obsoletas en esquemas asíncronos y complejidad operativa (configuración, monitoreo y manejo de fallas).  

Este proyecto aborda de manera sistemática los fundamentos teóricos y su futura implementación práctica.  

Como caso de estudio, se analiza la implementación de un **sistema de información orientado a la gestión de reservas de productos**.  
El sistema debe permitir operaciones de lectura (consultar productos, precios, disponibilidad, reseñas) y escritura (realizar reservas, registrar usuarios, generar pedidos).  

Dado que la mayoría de las operaciones son lecturas concurrentes, pero también existen transacciones críticas de escritura, la base de datos puede convertirse en un cuello de botella.  
Por ello, se propone implementar una **replicación maestro–esclavo** para:  

- Distribuir la carga de consultas de lectura hacia las réplicas.  
- Mantener las escrituras en un único nodo maestro.  
- Garantizar disponibilidad del sistema en caso de fallo del nodo principal.  

El entorno permitirá evaluar el comportamiento de la réplica tanto en consistencia de datos (sincronización) como en disponibilidad (respuesta ante fallas o desconexiones).  
El caso de estudio se desarrollará en un **laboratorio virtual**, simulando un entorno real de comercio electrónico.  
El enfoque estará puesto en la **replicación asíncrona y semi-síncrona**, midiendo latencia y capacidad de recuperación.  

---

# Capítulo II: Marco Conceptual  

## 2.1 Conceptos básicos de replicación  

La replicación es el proceso de **propagar cambios** desde una instancia principal (Maestro) hacia una o más instancias secundarias (Réplicas) para mantener copias alineadas de los datos.  
Los cambios pueden transmitirse como eventos lógicos (por ejemplo: `INSERT`, `UPDATE`, `DELETE`) o como bloques físicos de log.  

**Objetivos típicos:**  
- Escalar lecturas distribuyendo consultas `SELECT` en réplicas.  
- Asegurar alta disponibilidad: continuidad del servicio ante fallas del Maestro.  
- Facilitar recuperación ante desastres y realizar backups sin sobrecargar el Maestro.  

---

## 2.2 Tipos de replicación  

### Replicación transaccional  
Modelo clásico en el que los cambios confirmados se propagan garantizando propiedades transaccionales en el destino.  
Es un caso particular de **replicación lógica**, centrada en la consistencia y el control sobre qué objetos se replican.  

### Según el modo temporal  

- **Asíncrona:**  
  El Maestro confirma transacciones sin esperar a las réplicas.  
  Riesgo de pérdida temporal de datos (`RPO > 0`) y lecturas obsoletas.  

- **Semi-síncrona:**  
  El Maestro espera al menos una confirmación antes de finalizar el commit.  
  Reduce el `RPO` con ligera penalización de latencia.  

- **Síncrona:**  
  Requiere confirmación de múltiples réplicas antes del commit.  
  Favorece consistencia fuerte (`RPO ≈ 0`) a costa de mayor latencia.  

---

## 2.3 Topologías comunes  

- **Maestro con réplicas de lectura:** la más usada para distribuir lecturas.  
- **Replicación en cascada:** las réplicas a su vez actúan como fuentes para otras réplicas.  
- **Clúster síncrono con consenso:** varios nodos participan de la toma de decisiones (ejemplo: *InnoDB Cluster*).  

---

## 2.4 Casos de uso representativos  

- **Lectura intensiva:** e-commerce, reservas, catálogos.  
- **Escritura intensiva:** telemetría, IoT, logística.  
- **Analítica y backups:** réplicas dedicadas a ETL o copias en caliente.  
- **Multi-región:** acercar lecturas al usuario y reducir latencia percibida.  

---

# Capítulo III: Metodología
La metodología aplicada en este trabajo se divide en dos fases: **teórica** y **práctica**.  

- En la fase teórica se revisaron conceptos fundamentales de replicación mediante bibliografía académica y documentación oficial de motores de bases de datos como MySQL.  
- En la fase práctica se diseñara un laboratorio controlado que permitira implementar y probar distintos modos de replicación.

**Actividades a realizar:**  
1. Instalación de instancias Maestro y Réplica en contenedores Docker.  
2. Configuración de parámetros de replicación (binlogs, usuarios replicadores, permisos).  
3. Ejecución de scripts de carga con operaciones de lectura y escritura.  
4. Monitoreo de métricas como *lag de réplica*, tiempo de confirmación de transacciones y recuperación tras caídas.  
5. Análisis comparativo entre replicación asíncrona y semi-síncrona.

**Herramientas a utilizar:**  
- MySQL 8.4  
- Docker  
- Sysbench para benchmarking  
- Comandos internos (`SHOW SLAVE STATUS`) para supervisar la replicación  

Se documentaran también los errores encontrados y los pasos de resolución.  

---

# Capítulo IV: Desarrollo del tema / Resultados esperados
El caso de estudio seleccionado corresponde a un **sistema de reservas de productos**, similar a un e-commerce.  
Este sistema combina un alto volumen de consultas de lectura con operaciones críticas de escritura, como la generación de pedidos.  
Se probaran escenarios en los que el Maestro gestionara las escrituras mientras las réplicas responderan a las consultas de lectura concurrentes.

**Resultados esperados minimamente la primera:**  
- **Replicación asíncrona:** permitir una rápida confirmación de transacciones, con un lag de réplica que oscile entre 1 y 3 segundos bajo carga.
- **Replicación semi-síncrona:** reducir el riesgo de pérdida de datos al requerir confirmación de al menos una réplica antes de validar el commit. Sin que suba demasiado la latencia.
- **Recuperación ante fallos:** simular la caída del nodo maestro y probar las réplicas para consultas de lectura, garantizando disponibilidad parcial del sistema. 

Estos experimentos demostraran que la replicación puede mejorar significativamente la tolerancia a fallos y la capacidad de respuesta en sistemas con alta concurrencia, aunque requiere un diseño cuidadoso para equilibrar consistencia y rendimiento.  

---

# Capítulo V: Conclusiones
El análisis realizado confirma que la replicación es una técnica indispensable para los sistemas modernos que requieren disponibilidad continua y baja latencia.  
En conclusión, el proyecto permite comprender los fundamentos y los desafíos prácticos de la replicación de bases de datos,demuestra que esta técnica, correctamente implementada, mejora la robustez y confiabilidad de los sistemas de información.  

---

# Capítulo VI: Bibliografía
- Oracle. (2024). *MySQL 8.4 Reference Manual: Replication*. https://dev.mysql.com/doc/refman/8.4/en/replication.html  
- Material de cátedra: *Bases de Datos I*, Licenciatura en Sistemas de Información, UNNE.  
- Elmasri, R. & Navathe, S. (2017). *Sistemas de Bases de Datos*. Pearson.  
- Silberschatz, A., Korth, H., & Sudarshan, S. (2019). *Database System Concepts*. McGraw-Hill.  
Proyecto_investigacion_BD_GITHUB.md
9 KB
