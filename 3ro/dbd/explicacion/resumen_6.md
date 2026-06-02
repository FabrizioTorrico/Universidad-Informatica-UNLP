# 📘 Guía de Estudio Definitiva: Álgebra Relacional (Teoría 6)

Esta guía condensa los fundamentos del **Álgebra Relacional**: el lenguaje formal que sustenta las operaciones de bases de datos (SQL).

---

## 1. Operaciones Fundamentales (Unarias y Binarias)

### 1. Conceptos Clave y Definiciones (Alta Fidelidad)

- **Selección ($\sigma$):** "Operación unaria ($\sigma_{\text{condición}} R$). El resultado es una relación con un subconjunto 'horizontal' de la relación dada".
  - _SQL:_ `WHERE`. Elimina filas que no cumplen la condición.
- **Proyección ($\Pi$):** "Operación unaria ($\Pi_{\text{lista\_atributos}} R$). Dada una lista de atributos produce un corte 'vertical' de la relación".
  - _SQL:_ `SELECT columna1, columna2`. Elimina columnas no deseadas.
- **Producto Cartesiano ($X$):** "Operación binaria ($A \times B$). El resultado incluye todas las tuplas posibles concatenando cada tupla de A con cada una de B".
  - _SQL:_ `FROM tabla1, tabla2` (Sin `WHERE`). Genera una "explosión" de filas ($N \times M$).

### 2. Desarrollo Estructurado

#### Operaciones de Conjuntos

Para que funcionen, las relaciones deben ser **Unión-Compatibles** (mismo número y tipo de columnas):

1.  **Unión ($\cup$):** $A \cup B$. Suma las filas de A y B (sin repetidos).
2.  **Diferencia ($-$):** $A - B$. Filas que están en A pero **NO** en B.
3.  **Renombre ($\rho$):** Cambia el nombre de la tabla o sus columnas para evitar ambigüedades en operaciones complejas.

> **💡 Analogía (Proyección vs Selección):**
> Imagina una hoja de cálculo gigante.
>
> - **Selección ($\sigma$):** Usas un resaltador para marcar solo las filas de "Empleados de Ventas". (Filtrado horizontal).
> - **Proyección ($\Pi$):** Recortas con tijeras solo las columnas de "Nombre" y "Sueldo", tirando el resto a la basura. (Recorte vertical).

---

## 2. Operaciones Derivadas (Joins y División)

### 1. Conceptos Clave

- **Producto Natural/Natural Join ($|X|$):** Combina dos tablas basándose en columnas con el **mismo nombre**.
  - _Explicación:_ Es un Producto Cartesiano + Selección de igualdad + Proyección para eliminar columnas duplicadas.
- **Producto Theta ($|X|_\theta$):** Un Join donde la condición no es necesariamente igualdad de nombres, sino cualquier condición lógica $\theta$ (ej: `A.sueldo > B.sueldo`).
- **División ($\%$):** La operación inversa al producto cartesiano. Responde preguntas de "todos".
  - _Ejemplo:_ Buscar estudiantes que cursaron **todas** las materias de la carrera.

### 2. Desarrollo Estructurado

#### La División Relacional: El "Jefe Final" del Álgebra

Es la operación más difícil de entender.

- **Formato:** $R \div S$.
- **Resultado:** Las tuplas de $R$ que están asociadas con **todas** las tuplas de $S$.

> **💡 Ejemplo Real (División):**
>
> - $R$ (Reservas): Lista de [Piloto, Avión] que han volado.
> - $S$ (Aviones): Lista de [Avión] (Boeing 747, Airbus A320).
> - $R \div S$: Devuelve los Pilotos que han volado **TODOS** los aviones de la lista $S$. Si a un piloto le falta volar el Airbus, no sale en el resultado.

### 3. Visualización (Operaciones)

| Símbolo  | Operación        | Equivalente SQL | Efecto Visual    |
| :------: | :--------------- | :-------------- | :--------------- | -------------- | ---------------------- |
| $\sigma$ | Selección        | `WHERE`         | Cortar Filas     |
|  $\Pi$   | Proyección       | `SELECT`        | Cortar Columnas  |
| $\times$ | Prod. Cartesiano | `CROSS JOIN`    | Multiplicar Todo |
|    $     | X                | $               | Natural Join     | `NATURAL JOIN` | Pegar por Coincidencia |
|  $\cup$  | Unión            | `UNION`         | Sumar Listas     |
|   $-$    | Diferencia       | `EXCEPT`        | Restar Listas    |
|  $\div$  | División         | `NOT EXISTS...` | "Para todo..."   |

---

## 3. Modificación de la Base de Datos (DML)

### 1. Conceptos Clave

El álgebra relacional también define cómo cambiar los datos.

- **Inserción ($R \leftarrow R \cup E$):** Agregar filas nuevas. Unión de la tabla original con las nuevas tuplas.
- **Eliminación ($R \leftarrow R - E$):** Borrar filas. Resta de la tabla original menos las tuplas a borrar.
- **Actualización ($\delta$):** Modificar valores. Se denota como una proyección generalizada que calcula nuevos valores
