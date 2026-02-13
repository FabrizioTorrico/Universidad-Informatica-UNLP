# 📘 Guía de Estudio Definitiva: Datos Abiertos (Teoría 8)

Esta guía abarca los conceptos de **Datos Abiertos (Open Data)**, sus principios fundamentales, metadatos, licencias y la legislación vigente en Argentina.

---

## 1. Datos vs Información vs Datos Abiertos

### 1. Conceptos Clave y Definiciones (Alta Fidelidad)

- **Dato:** "Elemento descontextualizado que puede dar origen a la generación de información. Símbolos que describen hechos, condiciones, valores o situaciones".
- **Información:** "Dato procesado con valor agregado, dotado de relevancia, utilidad e interpretación".
- **Dato Público:** Cualquier dato generado en el ámbito gubernamental.
- **Datos Abiertos (Open Data):** "Aquellos de origen público o no a los que cualquier persona puede acceder, usar y compartir libremente. Sólo deben atribuirse y compartirse con la misma licencia".

> **NO son Datos Abiertos:**
>
> - PDFs o imágenes de texto (formatos cerrados).
> - Datos estadísticos agregados o resumidos (son información, no datos primarios).
> - Informes de gestión anuales (son documentos).

---

## 2. Los 8 Principios de los Datos Abiertos

Para que un dato sea realmente "abierto", la iniciativa Open Government Data define 8 características esenciales:

1.  **Completos:** Todo el dato disponible.
2.  **Primarios:** Recolectados en la fuente de origen (sin modificar/agregar).
3.  **Oportunos:** Disponibles rápido para preservar su valor.
4.  **Accesibles:** Para el rango más amplio de usuarios.
5.  **Procesables:** Estructurados para procesamiento automático (CSV, JSON, XML).
6.  **No discriminatorios:** Disponible para cualquier ciudadano, sin registro previo.
7.  **No propietarios:** Formato sobre el cual nadie tiene control exclusivo. (Evitar XLS propietario, usar CSV/ODS).
8.  **Licencia de uso libre:** Legalmente abiertos.

### 3. Visualización (Niveles de Apertura - Estrellas de Tim Berners-Lee)

| Nivel | Descripción                             | Ejemplo             |  Reutilizable?  |
| :---: | :-------------------------------------- | :------------------ | :-------------: |
|   ★   | Publicado en la web (cualquier formato) | PDF, JPG            |    No mucho     |
|  ★★   | Formato estructurado propietario        | Excel (.xls)        |   Sí, pero...   |
|  ★★★  | Formato estructurado NO propietario     | CSV, XML, JSON, ODS | **SÍ (Mínimo)** |
| ★★★★  | Usa URIs para identificar cosas         | RDF                 | Sí (Semántico)  |
| ★★★★★ | Enlazado a otros datos (LOD)            | RDF Linked Data     | Sí (Conectado)  |

---

## 3. Metadatos y Estándares

### 1. Conceptos Clave

- **Metadatos:** "Datos sobre los datos". Describen qué contiene el dataset, quién lo creó, cuándo se actualizó, qué licencia tiene.
- **Estándares Clave:**
  - **Dublin Core (ISO 15836):** El más conocido y simple. (Título, Creador, Materia, Descripción, etc).
  - **DCAT (W3C):** Vocabulario RDF para catálogos de datos web.
  - **INSPIRE (UE):** Infraestructura de información espacial europea.

### 2. Desarrollo Estructurado

Sin metadatos, el dato es inútil porque no se puede encontrar ni entender su contexto.

- _Ejemplo:_ Un archivo llamado `datos.csv` con columnas `Col1, Col2` no sirve.
- Con Metadatos: El archivo `presupuesto_2023.csv` contiene `Monto` (pesos argentinos) y `Area` (Ministerio). Autor: Min. Economía. Licencia: CC-BY.

---

## 4. Licencias y Marco Legal

### 1. Conceptos Clave

Los datos deben estar abiertos **Técnica** y **Legalmente**.

#### Licencias Comunes

1.  **PDDL (Public Domain):** Dominio público. Sin restricciones.
2.  **ODbL (Open Database License):** Atribución + Share-Alike (Compartir Igual).
3.  **CC-BY 4.0 (Creative Commons Atribución):** La más usada. Permite uso comercial, modificar y distribuir, **siempre que cites la fuente**.

#### Leyes en Argentina

- **Ley 27.275:** Derecho de Acceso a la Información Pública. (Obliga al estado a responder pedidos de informe).
- **Ley 25.326:** Protección de Datos Personales (Habeas Data). (Los datos abiertos NO deben contener datos personales sensibles).
- **Decreto 117/2016:** Plan Nacional de Apertura de Datos.

> **💡 Trampa Común (Privacidad):**
> Abrir datos NO significa violar la privacidad. Los datos personales (DNI, Nombre, Dirección exacta de personas físicas) deben ser anonimizados antes de publicar. La Ley 25.326 protege esto.

### 5. Wikidata

- Base de conocimiento secundaria, libre y colaborativa.
- Almacena datos estructurados para Wikipedia y otros proyectos.
- Licencia **CC0 (Dominio Público)**. Cualquiera puede usar los datos sin pedir permiso.

---
