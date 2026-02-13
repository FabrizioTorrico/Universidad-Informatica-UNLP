# 📘 Guía de Estudio Definitiva: Normalización Avanzada (BCNF)

Esta guía procesa el contenido de **teoria_5.txt**, centrada en el proceso de llevar un esquema a la Forma Normal de Boyce-Codd (BCNF) y verificar la pérdida de dependencias.

---

## 1. Forma Normal de Boyce-Codd (BCNF)

### 1. Conceptos Clave y Definiciones (Alta Fidelidad)

- **Definición de BCNF:** Un esquema $R$ está en BCNF si, siempre que una dependencia funcional $X \rightarrow A$ es válida en $R$, entonces se cumple que:
  1.  $X$ es **superclave** de $R$.
  2.  O bien, $X \rightarrow A$ es una dependencia funcional **trivial** ($A \subseteq X$).
  - _Explicación simplificada:_ Todo determinante (el lado izquierdo de la flecha) debe ser una clave candidata. Si algo que no es clave determina a otro atributo, hay una anomalía.

### 2. Desarrollo Estructurado

#### El Algoritmo de Normalización a BCNF

1.  **Identificar DFs violadoras:** Buscar una DF $X \rightarrow Y$ donde $X$ **NO** sea superclave.
2.  **Descomponer:** Si existe tal DF, partir $R$ en dos:
    - $R_1(X, Y)$ (La parte "problema").
    - $R_2(R - Y)$ (El resto, manteniendo $X$ como nexo).
3.  **Recursividad:** Verificar si $R_1$ y $R_2$ están en BCNF. Si no, repetir el paso 1 con ellas.
4.  **Verificación Final:**
    - ¿Se perdió información? (Lossless Join).
    - ¿Se perdieron dependencias? (Dependency Preservation).

> **💡 Analogía (BCNF):**
> Imagina una "Regla de Oro": **"Solo el Jefe (Clave) manda"**.
>
> - En BCNF, si alguien da una orden ($X \rightarrow Y$), _tiene_ que ser el Jefe ($X$ es Superclave).
> - Si un empleado raso ($X$ no clave) empieza a dar órdenes ($X \rightarrow Y$), hay que echarlo (sacarlo a otra tabla) para que mande en su propio quiosco.

---

## 2. Verificación de Pérdida de Información

### 1. Conceptos Clave

- **Intersección ($R_1 \cap R_2$):** Los atributos comunes entre las tablas descompuestas.
- **Condición de No Pérdida:** La intersección debe ser **clave** en al menos una de las dos tablas resultantes.

### 2. Desarrollo Estructurado

- Si cortas una tabla y el atributo nexo no identifica unívocamente a ninguna fila en los pedazos, no podrás reconstruir la tabla original sin duplicados falsos.
- **Ejemplo FIESTAS:**
  - Original: `FIESTAS(#salon, direccion, fecha...)`.
  - DF problemática: `#salon -> direccion`. (#salon no era clave completa, la clave era `#salon, fecha`).
  - Descomposición:
    - $F_1(\#salon, direccion)$ -> Aquí `#salon` ES CLAVE. **Correcto**.
    - $F_2(\#salon, fecha...)$ -> Resto.

---

## 3. Algoritmo de Preservación de Dependencias

### 1. Conceptos Clave

- **Problema:** Al partir tablas, una DF original ($A \rightarrow B$) puede quedar "rota" si $A$ y $B$ terminan en tablas distintas.
- **Algoritmo de Verificación:** Permite saber si la DF rota aún se puede "deducir" lógicamente cruzando las tablas resultantes.

### 2. Desarrollo Estructurado (Paso a Paso)

Para verificar si una DF $X \rightarrow Y$ se perdió:

1.  Calcular la clausura de $X$ ($X^+$) **usando solo las DFs locales de cada tabla** ($R_i$).
2.  Empezar con `Res = X`.
3.  Iterar sobre las tablas $R_i$:
    - Interceptar `Res` con atributos de $R_i$.
    - Calcular la clausura local en $R_i$.
    - Unir lo nuevo a `Res`.
4.  Repetir hasta que `Res` no cambie.
5.  Si al final `Y` está en `Res`, la dependencia **NO se perdió** (se preserva transitivamente). Si no está, **se perdió**.

> **💡 Ejemplo Real (Algoritmo):**
>
> - $A \rightarrow B$ (en $R_1$).
> - $B \rightarrow C$ (en $R_2$).
> - ¿Se perdió $A \rightarrow C$?
> - Calculamos $A^+$:
>   - En $R_1$, $A$ determina $B$. Tengo $\{A, B\}$.
>   - En $R_2$, con $B$ determino $C$. Tengo $\{A, B, C\}$.
>   - ¡Exito! $C$ está en la clausura. La cadena no se rompió.

### 3. Visualización (Diagrama de Flujo de Normalización)

```mermaid
graph TD
    A[Inicio: Esquema R con DFs] --> B{¿Hay DF X->A donde X no es SK?}
    B -- No --> C[R está en BCNF]
    B -- Si --> D[Descomponer en R1(X,A) y R2(Resto)]
    D --> E[Verificar Pérdida Información]
    E -->|OK| F[Verificar Pérdida DFs]
    F -->|OK| B
    F -->|Perdida| G[¡ALERTA! Posiblemente BCNF no sea ideal]
```

### 4. Ayudas de Memoria

- **Trampa común:** Creer que si una DF se rompe (sus atributos se separan), _automáticamente_ se perdió. No siempre. A veces se salva por transitividad (como en el ejemplo arriba). El algoritmo es la única forma segura de saberlo.

---
