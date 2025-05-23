# TrabajoSistemaTurnomatico
#### Por Dilan Abarca

## Descripcion general del sistema
Este proyecto consiste en el desarrollo de un sistema turnomático, el cual permite organizar y administrar turnos de forma automatizada. El objetivo principal es facilitar la atención ordenada de personas en distintos contextos, como oficinas, centros médicos, instituciones educativas o cualquier lugar donde se requiera un sistema de turnos.

El sistema sistema cuenta con funciones como:

- Generación automática de números de turno.

- Visualización del turno actual.

- Llamado de turnos en pantalla.

- Solicitar un turno desde un "Quiosco" o una "App Movil"

Este proyecto fue desarrollado como parte de un trabajo académico para aplicar conocimientos adquiridos en la asignatura de **Patrones de Diseño**.

---
## Objetivos del modelado

- Definir la estructura lógica y funcional del sistema mediante diagramas que permitan visualizar cómo se organiza la información.

- Aplicar y ejercitar el uso de **Patrones de Diseño** aprendidos en clases.

---
## 1. Diagrama de caso de uso UML
![img](DiagramaCasoDeUso.png)
## Descripcion General:

El análisis funcional del sistema Turnomático permitió identificar claramente a los actores clave que interactúan con la plataforma y las funcionalidades esenciales del flujo de gestión de turnos. Asimismo, se aplicaron adecuadamente las relaciones <<include>> y <<extend>> en el diagrama de casos de uso para reflejar la modularidad del sistema, diferenciando entre flujos obligatorios y comportamientos opcionales.

### Actores identificados:

- **Cliente:** Usuario que accede al sistema desde quioscos, app o web para solicitar y consultar turnos. También puede cancelar turnos o revisar su historial.

- **Empleado:** Personal de atención que visualiza y gestiona la fila de turnos, llama a los clientes y marca turnos como atendidos.

- **Administrador:** Usuario con privilegios elevados que genera reportes, configura el sistema y accede a estadísticas del uso general.

- **Sistema Interno Turnomático:** Ejecuta lógicas internas como la asignación de números de turno y la validación de disponibilidad.
  
 ## Casos de uso destacados y relaciones aplicadas:
### Solicitar turno

- `<<include>>` Seleccionar tipo de turno: es obligatorio que el cliente indique el tipo de atención.

- `<<include>>` Validar disponibilidad: siempre se verifica si hay capacidad antes de asignar el turno.

- `<<include>>` Asignar número de turno: acción obligatoria que genera el identificador único del turno.

### Cancelar turno

- `<<extend>>` Consultar turno: el cliente puede consultar su turno actual antes de cancelarlo, como paso opcional.

### Llamar siguiente turno

- `<<include>>` Actualizar estado del turno: es parte inherente del proceso de llamado, ya que el turno cambia de “esperando” a “en atención”.

### Generar Reportes

- `<<include>>` Filtrar por fecha o tipo de atención: el administrador puede aplicar filtros opcionales para acotar el reporte.

## Justificación de las relaciones aplicadas:
  
Se utilizó `<<include>>` en procesos que dependen obligatoriamente de otros subprocesos, asegurando una ejecución coherente y secuencial. Por ejemplo, solicitar turno siempre conlleva seleccionar un tipo, validar disponibilidad y recibir la asignacion de un número.

Se empleó `<<extend>>` para acciones opcionales o condicionales,por ejemplo al cancelar un turno, se puede volver a consultar por un turno. Ya que dependen del contexto o de decisiones del usuario.

Estas relaciones permiten mantener flujos funcionales limpios y modulares, respetando principios de reutilización y claridad.

---

## 2. Diagrama de clases
![img](DiagramaDeClases.png)

## 1. Singleton
### Clase aplicada:

- `GestorDeTurnos`

### Intención arquitectónica:
Asegura que exista una única instancia global encargada de administrar todos los turnos del sistema. Controla la asignación, el seguimiento y la actualización del estado de cada turno.

### Justificación:
Permite centralizar la lógica de negocio relacionada con la gestión de turnos, evitando conflictos o inconsistencias por múltiples instancias.

---

## 2. Prototype
### Clase aplicada:

- `Turno`(Turno base que puede ser clonado)

- Subtipos como `TurnoGeneral` y `TurnoPreferencial`.

### Intención arquitectónica:
Permite crear nuevos turnos clonando prototipos existentes, lo que optimiza la generación de objetos con configuraciones similares y reduce la dependencia de constructores complejos.

### Justificación:
El sistema necesita generar muchos turnos similares (con diferencias menores como prioridad o tipo). Usar Prototype permite crear nuevas instancias de forma eficiente sin recrear lógicas repetitivas.

---

## 3. Bridge

### Clases aplicadas:

- `Abstracción: PantallaTurnos`

- `Implementación concreta: PantallaMovil`

### Intención arquitectónica:
Desacoplar la lógica de presentación de turnos de la plataforma específica donde se visualizan. Aunque actualmente solo se implementa una versión para dispositivos móviles, la estructura permite agregar fácilmente nuevas formas de presentación (como pantallas LED o paneles web) sin modificar la lógica general del sistema.

### Justificación:
Si bien la única implementación activa es para una app móvil, el uso de Bridge garantiza que el sistema pueda escalar a otros entornos de presentación de forma modular, sin alterar las clases principales de control o lógica de turnos.

---

## 3. Diagrama de implementacion
![img](DiagramaImplementacion.png)

### Despliegue físico

El sistema Turnomático se despliega en una arquitectura de tres capas distribuidas en distintos nodos físicos o virtuales, permitiendo escalabilidad, mantenimiento independiente y separación de responsabilidades. Los componentes se alojan en los siguientes nodos:

---

### Cliente Móvil

- Dispositivo Android/iOS con la aplicación instalada.

- Comunicación vía red con el backend (REST API).

- Presenta los turnos mediante el patrón Bridge (PantallaTurnos → PantallaMovil).

---

### Servidor de Aplicaciones (Backend)

Implementa la lógica de negocio del sistema.

Contiene componentes como:

- GestorDeTurnos (Singleton): gestiona todos los turnos del sistema.

- Clases de Turno, clonadas usando el patrón Prototype.

- Lógica de asignación, cancelación y consulta de turnos.

Expone una API RESTful que es consumida por el cliente móvil.

---

### Base de Datos

Nodo físico o instancia en la nube donde se almacenan:

- Datos de usuarios.

- Historial y estado de turnos.

- Configuraciones del sistema.

---

## Decisiones técnicas

API RESTful: Se eligió este tipo de API por su simplicidad, compatibilidad con múltiples plataformas (como apps móviles) y facilidad de integración futura.

Patrones usados en backend:

- Singleton: GestorDeTurnos centraliza la lógica de turnos, asegurando consistencia global.

- Prototype: facilita la creación de turnos personalizados clonando plantillas.

- Bridge: usado en el cliente móvil para desacoplar la vista de la lógica de presentación.

---

## Reflexion final sobre modelado

El proceso de modelado arquitectónico del sistema Turnomático permitió plasmar de forma estructurada tanto los aspectos funcionales como técnicos de un sistema realista y escalable. A través de este ejercicio, se demostró la importancia de transitar desde una visión de alto nivel (casos de uso) hacia una arquitectura concreta y bien fundamentada (clases e implementación física), aplicando patrones de diseño reconocidos como herramientas clave de calidad y sostenibilidad del software.

El uso de patrones como Singleton, Prototype y Bridge no solo permitió organizar el sistema de forma más coherente, sino que también fomentó la reutilización, modularidad y bajo acoplamiento, principios esenciales para el mantenimiento y evolución del sistema a futuro. Además, se logró un equilibrio entre flexibilidad y control, por ejemplo al permitir clonar fácilmente tipos de turnos o integrar nuevas formas de visualización sin alterar el núcleo del sistema.
