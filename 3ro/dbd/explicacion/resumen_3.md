# 📘 Guía de Estudio Definitiva: Práctica de Diseño (Caso Taller Mecánico)

Esta guía analiza el **Ejemplo Práctico** de `teoria_3.txt`. A diferencia de la teoría pura, aquí nos enfocamos en **cómo aplicar** los conceptos para resolver un problema real de negocio.

---

## 1. Análisis de Requerimientos (El Problema)

### 1. Conceptos Clave

- **Abstracción:** El proceso de leer un texto narrativo (el problema del cliente) y extraer solo los sustantivos (Entidades) y verbos (Relaciones) relevantes para el sistema.
- **Entidad:** "Vehículos, Clientes, Empleados, Servicios".
- **Atributo Discriminador:** Un atributo que permite distinguir entre tipos de una entidad genérica (ej: `tipo_cliente` para diferenciar Física de Jurídica).

### 2. Desarrollo Estructurado

El primer paso es **desglosar el enunciado**.

#### Identificación de Entidades y Datos

- **Cliente:** Puede ser Física (CUIL, Nombre) o Jurídica (CUIT, Razón Social). _Esto sugiere una Jerarquía/Herencia_.
- **Vehículo:** Patente (PK), marca, modelo.
- **Motor:** Cilindrada, marca. _Restricción:_ Un motor está en un único vehículo (1:1).
- **Servicio:** Mantenimiento que se realiza. Tiene Fecha Ingreso/Egreso, motivo, etc.
- **Empleado:** Realiza tareas. Los Administrativos tienen turno (otra Jerarquía).
- **Tarea/Mantenimiento:** Definición del trabajo a realizar (nombre, costo, tiempo).

> **💡 Estrategia de Resolución:**
> Subraya los **sustantivos** (posibles Entidades) en azul y los **verbos** (posibles Relaciones) en rojo.
>
> - "Un cliente **posee** varios vehículos" -> Relación 1:N entre Cliente y Vehículo.

---

## 2. Modelo Entidad-Relación (Solución Conceptual)

### 1. Decisiones de Diseño Importantes

#### Jerarquías (Generalización/Especialización)

El problema presenta dos casos de herencia con soluciones distintas en el modelo lógico posterior:

1.  **Cliente (Física/Jurídica):** Todos son clientes, pero con datos ligeramente distintos.
2.  **Empleado (Administrativo/Mecánico):** Todos son empleados, pero el administrativo tiene atributos extra (turno).

#### Relaciones Complejas

- **Servicio - Tarea - Mantenimiento:** Un servicio incluye tareas de varios mantenimientos. Esto implica una relación **Muchos a Muchos (N:M)** que seguramente generará una tabla intermedia con atributos propios (como `valor_actual` o `km_realizado`).

### 3. Visualización (Diagrama ER Simplificado)

Hemos reconstruido el diagrama Mermaid en base al texto para visualizar las relaciones clave.

erDiagram
%% JERARQUIA CLIENTE (Tabla Única)
CLIENTE ||--o{ VEHICULO : "posee"
CLIENTE {
string tipo_cliente "Discriminador"
string identificador "CUIL/CUIT"
}

    %% RELACION 1:1
    VEHICULO ||--|| MOTOR : "tiene"

    %% RELACION TRANSACCIONAL
    VEHICULO ||--o{ SERVICIO : "recibe"

    %% RELACION N:M (Desglosada en entidad asociativa)
    SERVICIO ||--|{ DETALLE_SERVICIO : "contiene"
    TAREA ||--|{ DETALLE_SERVICIO : "se_aplica_en"
    DETALLE_SERVICIO {
        float valor_actual
        int km_realizado
    }

    %% JERARQUIA EMPLEADO (Tabla por Clase)
    EMPLEADO ||--o{ DETALLE_SERVICIO : "realiza"
    EMPLEADO ||--o| ADMINISTRATIVO : "es"
    ADMINISTRATIVO {
        string turno
    }

---

## 3. Transformación al Modelo Relacional (Lógico)

### 1. Reglas de Transformación Aplicadas (Fidelidad Técnica)

El texto aplica reglas específicas para transformar el dibujo en tablas.

- **Regla 1 (Entidades Fuertes):** Cada entidad se convierte en una tabla.
- **Regla 2 (Relaciones 1:N):** La clave primaria (PK) del lado "1" pasa como clave foránea (FK) al lado "N".
  - _Ejemplo:_ `id_cliente` pasa a la tabla `VEHICULO`.
- **Regla 3 (Relaciones N:M):** Se crea una **tabla nueva** (intermedia) con las PKs de ambas entidades + atributos propios.
  - _Ejemplo:_ La relación entre `MANTENIMIENTO` y `TAREA` genera la tabla `compuesto_por(#mantenimiento, #tarea, valor)`.

### 2. Desarrollo Estructurado: Estrategias de Herencia

Aquí es donde el experto decide cómo optimizar. El texto usa dos estrategias diferentes:

#### Estrategia A: "Tabla Única" (Single Table) -> Usada en CLIENTE

- **Decisión:** "Dejar solo al padre agregando un atributo discriminador".
- **Resultado:** Una sola tabla `CLIENTE` con todos los campos (`CUIT`, `CUIL`, `Razón Social`, `Nombre`).
- _Ventaja:_ Consultas rápidas, sin JOINs.
- _Desventaja:_ Muchos valores NULL (si es empresa, no tiene CUIL; si es persona, no tiene CUIT).

#### Estrategia B: "Tabla por Clase" (Class Table) -> Usada en ADMINISTRATIVO

- **Decisión:** "Dejar padre e hija (raíz/hoja)".
- **Resultado:** Dos tablas. `EMPLEADO` (datos comunes) y `ADMINISTRATIVO` (solo `turno` y la FK al empleado).
- _Ventaja:_ Estructura limpia, sin NULLs innecesarios.
- _Desventaja:_ Requiere JOIN para obtener los datos completos del administrativo.

> **💡 Analogía (Estrategias de Herencia):**
>
> - **Tabla Única:** Una sola planilla de Excel gigante para "Mascotas". Si es perro, llenas la columna "Ladra"; si es gato, la dejas vacía. Rápido pero sucio.
> - **Tabla por Clase:** Carpetas separadas. Una carpeta general "Mascota" con el nombre, y carpetas específicas "Perro" y "Gato" con sus fichas médicas. Más ordenado, pero requiere buscar en dos lugares.

### 3. Esquema Resultante (Síntesis)

Listado final de tablas clave generadas:

| Tabla              | Columnas Clave                            | Origen                              |
| :----------------- | :---------------------------------------- | :---------------------------------- |
| **Cliente**        | `id`, `tipo`, `cuit_cuil`, `razon_nombre` | Entidad Padre (Estrategia A)        |
| **Vehículo**       | `patente`, `id_cliente(FK)`               | Entidad Fuerte + FK de Relación 1:N |
| **Realiza**        | `patente(FK)`, `id_servicio(FK)`, `km`    | Relación N:M convertida a Tabla     |
| **Administrativo** | `cuil(FK)`, `turno`                       | Entidad Hija (Estrategia B)         |

---

## 4. Casos Especiales (Bonus)

### 1. Recursividad (Supervisión)

- **Problema:** "Para cada empleado se desea conocer quién es su supervisor".
- **Solución:** Una **Relación Recursiva** (Auto-relación).
- **En Tabla:** Se agrega una columna `id_supervisor` en la tabla `EMPLEADO` que apunta a... la misma tabla `EMPLEADO`.
  - _SQL:_ `FOREIGN KEY (supervisor_id) REFERENCES EMPLEADO(id)`

### 2. Historicidad (Precios)

- **Problema:** "Mantener el historial del valor de cada tarea".
- **Solución:** No basta con guardar el precio _actual_ en la tabla Tarea. Se necesita una tabla histórica nueva:
  - `HISTORIAL_PRECIO(#tarea, fecha_inicio, fecha_fin, valor)`
- _Concepto:_ **Slowly Changing Dimensions** (Dimensiones lentamente cambiantes).

### 4. Ayudas de Memoria (Trampas)

- **Trampa 1:** Olvidar los atributos de la relación. En el diagrama, `km_actual` no pertenece ni al auto ni al servicio solamente, pertenece al **hecho** de realizar el servicio (la relación). Al pasar a tablas, esto va en la tabla intermedia o en la tabla de hechos.
- **Trampa 2:** Tratar 1:1 como 1:N. En Motores-Vehículos (1:1), la FK puede ir en _cualquiera_ de las dos tablas (o unificarlas), pero lo estándar es ponerla en la que tiene dependencia de existencia total
