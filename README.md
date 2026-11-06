# trabajo-integrador-modelos-y-sistemas
# TP Integrador: Modelos y Sistemas

**Sistema elegido:** Opción B, Sistema de Turnos y Asignación de Laboratorios de Informática.

| | |
|---|---|
| **Materia** | Modelos y Sistemas, 7mo año, Escuela Técnica |
| **Profesor** | Gian Franco Denaro |
| **Alumno** | Santiago Fernandez |
| **Fecha** | 06/11/2026 |

## Descripción

Sistema para que docentes y alumnos reserven puestos en los laboratorios de informática de la escuela, eligiendo laboratorio, fecha, horario, equipo y, si hace falta, el software a usar. El sistema valida sanciones y disponibilidad, registra la reserva con un código único y envía una confirmación por correo.

Este trabajo documenta el sistema desde cinco puntos de vista: marco conceptual, instructivo de usuario, diagrama de flujo, caso de uso formal e interfaz.

## Contenido

1. [Marco conceptual y clasificación](#1-marco-conceptual-y-clasificación)
2. [Instructivo paso a paso](#2-instructivo-paso-a-paso)
3. [Diagrama de flujo](#3-diagrama-de-flujo)
4. [Caso de uso formal (CU-01)](#4-caso-de-uso-formal-cu-01)
5. [Captura anotada e interfaz](#5-captura-anotada-e-interfaz)

---

## 1. Marco conceptual y clasificación

- **Documentación de usuario:** enseña a usar el sistema.
- **Documentación técnica:** explica cómo está hecho el sistema para poder mantenerlo.

| Aspecto | Documentación de Usuario | Documentación Técnica (de Sistema) |
|---|---|---|
| Audiencia | Docentes y alumnos que reservan laboratorios | Programadores, administradores de servidores, equipo de mantenimiento |
| Lenguaje | Simple, sin tecnicismos, con ejemplos y pasos | Preciso, con términos técnicos, esquemas y código |
| Objetivo | Que la persona logre una tarea sin ayuda (ej.: reservar un puesto) | Que otro técnico pueda instalar, modificar, reparar y ampliar el sistema |

### Clasificación de elementos

- **Manual de configuración de cadenas de conexión a la base de datos y despliegue del servidor → Documentación Técnica.** Lo usa quien instala y mantiene el sistema. Trata servidores, credenciales y bases de datos, que un usuario final nunca toca.
- **Guía rápida en PDF para solicitar el préstamo de una herramienta desde la app → Documentación de Usuario.** Explica una tarea cotidiana paso a paso y no requiere conocimientos técnicos.

## 2. Instructivo paso a paso

**Tarea:** reservar un puesto en un laboratorio de informática.

**Objetivo:** dejar confirmado un puesto en el laboratorio elegido, para la fecha y el horario deseados.

### Prerrequisitos

- Tener cuenta activa en el sistema (docente o alumno).
- Haber iniciado sesión con usuario y contraseña.
- No tener sanciones activas por reservas no utilizadas.

### Pasos

1. Ingresá al menú principal y tocá **Nueva reserva**.
2. Elegí el laboratorio (por ejemplo, Laboratorio 1) en la lista desplegable.
3. Seleccioná la fecha en el calendario. Solo se habilitan días futuros.
4. Elegí el horario disponible. Los horarios ocupados aparecen en gris.
5. Seleccioná el puesto o equipo libre que querés usar.
6. Si lo necesitás, indicá el software que vas a usar (por ejemplo, un editor de código con licencia).
7. Revisá el resumen de la reserva y tocá **Confirmar reserva**.

### Resultado esperado

El sistema muestra el mensaje "Reserva confirmada" con un código único. La reserva aparece en **Mis reservas** y llega un aviso por correo.

### Errores posibles y soluciones

| Error | Causa | Solución |
|---|---|---|
| "Puesto no disponible" | Otra persona lo reservó primero en ese horario | Volver al paso 4 y elegir otro puesto u otro horario |
| "Reserva bloqueada por sanción activa" | Hubo reservas sin usar y hay una sanción vigente | Esperar a que venza la sanción o consultar con el administrador del laboratorio |
| "Sesión expirada" | Mucho tiempo sin actividad | Volver a iniciar sesión y repetir la reserva desde el paso 1 |

## 3. Diagrama de flujo

*Flujo de validación de una reserva: 3 decisiones, 3 caminos de error.*

El proceso valida los datos, la sanción del docente y la disponibilidad del puesto. Cualquier respuesta "No" corta o desvía el flujo hacia un camino de error.

![Diagrama de flujo de validación de una reserva](docs/diagrama-flujo.png)

## 4. Caso de uso formal (CU-01)

**Caso de uso:** CU-01 Reservar puesto en laboratorio.

**Actor principal:** Docente (el alumno usa el mismo caso de uso con permisos de reserva limitados).

**Actores secundarios:** Sistema de correo (envía la confirmación).

**Precondiciones:**

- El docente está registrado y con sesión iniciada.
- Existe al menos un laboratorio con puestos cargados en el sistema.
- El docente no tiene sanciones activas.

**Postcondición:** la reserva queda registrada y el puesto figura como ocupado en ese horario.

### Flujo principal

1. El docente selecciona "Nueva reserva".
2. El sistema muestra los laboratorios disponibles.
3. El docente elige laboratorio, fecha y horario.
4. El sistema muestra los puestos libres para esa franja.
5. El docente elige un puesto y, si corresponde, el software requerido.
6. El sistema verifica que el puesto siga libre y que el docente no tenga sanciones.
7. El docente confirma la reserva.
8. El sistema registra la reserva, genera un código único y envía un correo de confirmación.
9. El sistema muestra "Reserva confirmada". Fin del caso de uso.

### Flujos alternativos y excepciones

**A1. Puesto ocupado (paso 6).** Otra persona reservó el puesto mientras el docente decidía.
1. El sistema informa "Puesto no disponible".
2. El sistema vuelve al paso 4 con la lista de puestos actualizada.
3. El docente elige otro puesto y el flujo continúa en el paso 5.

**A2. Docente con sanción activa (paso 6).**
1. El sistema informa "Reserva bloqueada por sanción activa" y muestra la fecha de vencimiento.
2. No se registra la reserva. El caso de uso termina sin éxito.

**A3. Software sin licencia disponible (paso 5).**
1. El sistema avisa que no quedan licencias del software pedido en esa franja.
2. El docente elige otro horario u otro laboratorio y vuelve al paso 3, o continúa sin ese software.

**A4. Docente cancela (cualquier paso).** El sistema descarta los datos ingresados y vuelve al menú principal sin registrar nada.

## 5. Captura anotada e interfaz

Boceto recortado de la pantalla **Nueva reserva**, que es la zona donde el usuario hace todo el trabajo.

![Boceto de la pantalla Nueva reserva](docs/boceto-nueva-reserva.png)

### Referencias

1. **Laboratorio:** lista desplegable para elegir en qué laboratorio se quiere reservar.
2. **Fecha:** campo de fecha; solo acepta días futuros.
3. **Horario:** botones de franjas horarias. La franja elegida se resalta y las ocupadas aparecen en gris.
4. **Puesto:** grilla de equipos del laboratorio. Los libres se pueden tocar, el elegido se resalta y los ocupados quedan en gris.
5. **Confirmar reserva:** registra la reserva y envía el correo de confirmación. Antes valida sanción y disponibilidad.
6. **Cancelar:** descarta lo cargado y vuelve al menú principal sin guardar nada.
