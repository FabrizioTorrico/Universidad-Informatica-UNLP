# 📘 Guía de Estudio Definitiva: Visualización de Datos (Teoría 9)

Esta guía abarca los conceptos de **Visualización de Datos**, sus principios fundamentales, tipos de gráficos y herramientas clave como Tableau.

---

## 1. Fundamentos de la Visualización

### 1. Conceptos Clave y Definiciones (Alta Fidelidad)

- **Visualización de Datos:** "Representación gráfica de información y datos. Al utilizar elementos visuales como cuadros, gráficos, infografías o mapas, las herramientas proporcionan una manera accesible de ver y fácil de comprender tendencias, valores atípicos y patrones".
- **Beneficios:** Compartir información fácilmente, exploración interactiva, reconocimiento rápido de patrones.
- **Riesgos:** Información sesgada o inexacta. Correlación no implica causalidad. Pérdida del mensaje en la traducción.

### 2. Desarrollo Estructurado

#### Dimensiones vs Medidas (Tableau)

Esta distinción es fundamental para usar herramientas de BI.

- **Dimensiones (Azul):** Datos cualitativos o categóricos (Nombres, Fechas, Regiones). Definen el nivel de detalle (granularidad).
- **Medidas (Verde):** Datos numéricos cuantitativos (Ventas, Ganancias, Cantidad). Se pueden agregar (Sumar, Promediar).

> **💡 Analogía (Dimensiones vs Medidas):**
>
> - **Dimensión:** Etiqueta del frasco ("Mermelada de Frutilla").
> - **Medida:** Cantidad que hay adentro ("500 gramos").

---

## 2. Tipos de Gráficos y Mejores Prácticas

### 1. Gráficos Clásicos

| Gráfico            | Uso Ideal                                                   | Cuándo Evitar                                                          |
| :----------------- | :---------------------------------------------------------- | :--------------------------------------------------------------------- |
| **Tabla**          | Mostrar valores exactos, detalle granular.                  | Análisis de tendencias o patrones.                                     |
| **Barras**         | Comparar valores entre categorías (discreto).               | Demasiadas categorías (se vuelve ilegible).                            |
| **Línea**          | Tendencias a lo largo del **tiempo** (continuo).            | Comparar cosas que no tienen orden temporal.                           |
| **Circular (Pie)** | Parte de un todo (Porcentaje). Pocas categorías (2-5).      | Muchas categorías, valores similares, suma $\neq$ 100%.                |
| **Dispersión**     | Correlación entre 2 medidas numéricas.                      | Datos categóricos sin valores numéricos.                               |
| **Burbujas**       | Comparación de alto nivel con 3 dimensiones (X, Y, Tamaño). | Comparar valores precisos (el ojo falla al comparar áreas circulares). |
| **Treemap**        | Jerarquías y proporciones anidadas.                         | Demasiados niveles de profundidad, valores negativos.                  |
| **Mapas**          | Datos geoespaciales.                                        | Cuando la ubicación no es relevante.                                   |

#### Infografías

- Combinan íconos, texto y gráficos para contar una historia.
- Objetivo: **Comunicación** y Engagement. No análisis profundo.

### 2. Errores Comunes de Visualización

- **Escala engañosa:** Cortar el eje Y para exagerar diferencias.
- **Demasiados colores:** Distraen en lugar de informar.
- **Falta de contexto:** Un número sin referencia no dice nada.
- **Correlación Automática (Mapas):** Creer que puntos cercanos están relacionados cuando es coincidencia.

### 3. Visualización (Dimensiones vs Medidas)

```mermaid
graph TD
    A[Datos] --> B{Tipo}
    B -->|Cualitativo| C[Dimensión (Azules)]
    B -->|Cuantitativo| D[Medida (Verdes)]
    C -->|Ejemplos| E[País, Fecha, Categoría]
    D -->|Ejemplos| F[Ventas, Ganancia, Cantidad]
    D -->|Operación| G[Agregación (SUM, AVG)]
    C -->|Operación| H[Agrupación (GROUP BY)]

    style C fill:#9cf,stroke:#333
    style D fill:#9f9,stroke:#333
```

---

## 3. Tableau: Conceptos Específicos

### 1. Conceptos Clave

- **Campos Discretos:** Tienen valores finitos (ej: Provincias). Se muestran en **AZUL**.
- **Campos Continuos:** Tienen valores infinitos en un rango (ej: Ventas, Tiempo). Se muestran en **VERDE**.
- **Roles:** Tableau asigna automáticamente roles (Dimensión o Medida) al cargar los datos, pero el usuario puede cambiarlos.

> **💡 Tip de Examen:**
> Si te preguntan el color de un campo en Tableau:
>
> - **Azul** = Discreto (Cabeceras).
> - **Verde** = Continuo (Ejes)
