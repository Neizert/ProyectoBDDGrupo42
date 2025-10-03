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
- Distribuir carga de trabajo.  
- Aumentar la tolerancia a fallos.  
- Facilitar la recuperación ante desastres.  

Este proyecto aborda de manera sistemática los fundamentos teóricos y su futura implementación práctica.  
Como caso de estudio, se analiza la implementación de un sistema de información orientado a la gestión de reservas de productos.  
El sistema debe permitir tanto operaciones de lectura (consultar productos, precios, disponibilidad) como de escritura (reservas, registros de usuarios, pedidos).  

**Problema:** dado que la mayoría de las operaciones son lecturas concurrentes pero también existen transacciones críticas de escritura, la base de datos puede convertirse en un cuello de botella.  
Se propone implementar **replicación maestro-esclavo** para distribuir consultas, mantener escrituras en un nodo central y garantizar disponibilidad ante fallos.  

---

# Capítulo II: Marco Conceptual

## 2.1 Conceptos básicos de replicación  
La replicación es el proceso de propagar cambios de una instancia principal (Maestro) hacia una o más instancias secundarias (Réplicas), para mantener copias alineadas de los datos.  
Los cambios pueden transmitirse como eventos lógicos (INSERT, UPDATE, DELETE) o como bloques físicos de log.  

## 2.2 Tipos de replicación  
- **Transaccional:** cambios confirmados se propagan garantizando propiedades ACID.  
- **Asíncrona:** el Maestro confirma sin esperar a réplicas (bajo impacto en latencia, riesgo de datos obsoletos).  
- **Semi-síncrona:** el Maestro espera al menos una confirmación antes del commit.  
- **Síncrona:** requiere confirmación de múltiples réplicas, priorizando consistencia fuerte a costa de mayor latencia.  
- **Física vs. lógica:** propagación de bloques frente a propagación de instrucciones SQL.  

## 2.3 Topologías comunes  
- Maestro con réplicas de lectura.  
- Replicación en cascada.  
- Clúster síncrono con consenso (ej: InnoDB Cluster).  

## 2.4 Casos de uso representativos  
- Escenarios de lectura intensiva (e-commerce, reservas, catálogos).  
- Escenarios de escritura intensiva (telemetría, IoT).  
- Analítica y backups.  
- Multi-región para reducción de latencia percibida.  

---

# Capítulo III: Metodología
Este proyecto se desarrolla en fases.  
En la etapa actual se abordan los **aspectos teóricos** mediante el análisis de bibliografía y documentación técnica.  
La siguiente etapa contempla la implementación práctica de un entorno de laboratorio.  

**Acciones previstas:**  
1. Diseño del laboratorio con contenedores locales.  
2. Configuración de Maestro y Réplicas.  
3. Ejecución de consultas de prueba y scripts controlados.  
4. Evaluación de métricas como latencia de replicación y tolerancia a fallos.  

> **Nota:** hasta el momento no se han realizado pruebas prácticas, por lo que este capítulo se limita al planteo metodológico.  

---

# Capítulo IV: Desarrollo del tema / Resultados
En esta primera entrega no se presentan resultados prácticos, dado que aún no se implementaron pruebas en entorno de laboratorio.  
El desarrollo de resultados quedará pendiente para la segunda fase del proyecto, donde se aplicará la metodología planteada.  

---

# Capítulo V: Conclusiones
Las conclusiones se elaborarán una vez realizada la implementación práctica y el análisis de resultados.  
En esta fase inicial solo se destaca la importancia de la replicación como estrategia de alta disponibilidad y escalabilidad de bases de datos.  

---

# Capítulo VI: Bibliografía
- Material de la cátedra: *Bases de Datos I - Licenciatura en Sistemas de Información, UNNE*.  
- Oracle. (2024). *MySQL 8.4 Reference Manual: Replication*. Recuperado de: https://dev.mysql.com/doc/refman/8.4/en/replication.html  
