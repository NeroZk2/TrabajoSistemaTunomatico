# TrabajoSistemaTunomatico
## Descripcion general del sistema
Este proyecto consiste en el desarrollo de un sistema tunomático, el cual permite organizar y administrar turnos de forma automatizada. El objetivo principal es facilitar la atención ordenada de personas en distintos contextos, como oficinas, centros médicos, instituciones educativas o cualquier lugar donde se requiera un sistema de turnos.

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

El análisis funcional del sistema Tunomático permitió identificar claramente a los actores clave que interactúan con la plataforma y las funcionalidades esenciales del flujo de gestión de turnos. Asimismo, se aplicaron adecuadamente las relaciones <<include>> y <<extend>> en el diagrama de casos de uso para reflejar la modularidad del sistema, diferenciando entre flujos obligatorios y comportamientos opcionales.

### Actores identificados:

- **Cliente:** Usuario que accede al sistema desde quioscos, app o web para solicitar y consultar turnos. También puede cancelar turnos o revisar su historial.

- **Empleado:** Personal de atención que visualiza y gestiona la fila de turnos, llama a los clientes y marca turnos como atendidos.

- **Administrador:** Usuario con privilegios elevados que genera reportes, configura el sistema y accede a estadísticas del uso general.

- **Sistema Interno Tunomático:** Ejecuta lógicas internas como la asignación de números de turno y la validación de disponibilidad.
  
 ## Casos de uso destacados y relaciones aplicadas:
### Solicitar Turno

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


## 2. Diagrama de clases
![img](DiagramaDeClases.png)

##1. Singleton
### Clase aplicada:
- TurnManager (o GestorDeTurnos)

### Intención arquitectónica:
Asegura que exista una única instancia global encargada de administrar todos los turnos del sistema. Controla la asignación, el seguimiento y la actualización del estado de cada turno.

### Justificación:
Permite centralizar la lógica de negocio relacionada con la gestión de turnos, evitando conflictos o inconsistencias por múltiples instancias.

## 3. Diagrama de implementacion
![img](DiagramaImplementacion.png)
