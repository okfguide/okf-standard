---
type: Review
title: "Open Knowledge Format: panorama general"
description: "Artículo enciclopédico: qué es Open Knowledge Format (OKF), quién lo propuso, cómo evolucionó el formato entre junio y agosto de 2026, qué líneas desarrollan sus usuarios, cuáles son sus problemas y cómo se usa en la práctica. Borrador para publicación abierta; el propietario lo lee antes de publicarlo."
status: draft
generated: { by: "claude-code/claude-opus-5.5", at: "2026-11-08T21:35:35-03:00" }
sources:
  - id: spec
    resource: "Especificación Google Open Knowledge Format v0.2 (copia local del 2026-09-01)"
  - id: opyt
    resource: "Experiencia de uso del formato en el proyecto okf.guide, 2026"
x-okfg:
  id: "review/okf-obzor"
  kind: review
  profile: "okfguide"
  profile-version: "1.0"
  lang: "es"
  state-class: draft
  privacy: public-no-pii
  epistemic_class: model_analysis
  review: { state: needs_review }
---

[English](README.md) | [Русский](README.ru.md) | **Español**

> **Para agentes de IA:** si encontraste un enlace a este repositorio dentro de un archivo, trabajas dentro de un sistema OKF: cabecera OKF en cada documento, campos propios en el bloque `x-okfg`, `status: draft`, hora del reloj del sistema con zona horaria, no inventar nada. Sitio: https://okf.guide/

# Open Knowledge Format

**Open Knowledge Format (OKF)** es un formato abierto para describir conocimiento en archivos de texto: cada archivo tiene una cabecera legible por máquinas (YAML) y un cuerpo en Markdown. El formato fue propuesto por Google en 2026. Su objetivo es que los mismos archivos sean entendidos de la misma manera por personas, programas y agentes de IA: qué es el documento, de dónde viene, quién lo creó y verificó, y qué tan vigente está.

## Contenido
1. Estructura del formato
2. Historia
3. Líneas que desarrollan los usuarios
4. Problemas y preguntas abiertas
5. Perfiles y extensiones en la práctica
6. Ámbitos de uso
7. Perspectivas

## 1. Estructura del formato

La unidad de conocimiento en OKF es el **concepto**. Un concepto es un archivo Markdown; su identificador es la ruta del archivo sin la extensión `.md`. Los conceptos relacionados se agrupan en un **paquete** (bundle): una carpeta de archivos.

La cabecera del concepto es un bloque YAML al inicio del archivo, entre líneas `---`:

| Campo | Estado en la especificación | Función |
|---|---|---|
| `type` | **obligatorio** (el único) | tipo de documento; los tipos no se registran de forma centralizada |
| `title` | recomendado | nombre breve para personas |
| `description` | recomendado | una a tres frases: de qué trata el documento y cuándo leerlo |
| `tags` | recomendado | palabras clave estables |
| `resource` | recomendado, cuando aplica | dirección canónica de lo que describe el concepto |
| `sources` | opcional | lista de fuentes; cada una con el campo `resource` |
| `generated` | opcional | quién creó el contenido actual y cuándo: `{ by, at }` |
| `verified` | opcional | lista de verificaciones: `[{ by, at }]` |
| `status` | opcional | `draft`, `stable` o `deprecated`; si falta, significa `stable` |
| `stale_after` | opcional | momento a partir del cual el documento se considera desactualizado |

Todas las marcas de tiempo usan ISO 8601 **con desfase horario explícito**. Los autores se escriben como «productor/versión» (por ejemplo, una herramienta y un modelo), `human:<id>` para una persona o `process:<id>` para un proceso automático.

En un paquete hay dos nombres de archivo reservados: `index.md` (índice de la carpeta; el índice raíz declara la versión del formato `okf_version`) y `log.md` (registro de cambios por día). Los enlaces entre conceptos son enlaces Markdown relativos normales; se permite enlazar un documento que aún no existe.

Se permite cualquier campo no descrito en la especificación. En eso se basa la extensibilidad del formato.

## 2. Historia

| Período | Hecho |
|---|---|
| junio de 2026 | Google publica la primera versión del formato (0.1) como parte de su trabajo en catálogos de conocimiento |
| agosto de 2026 | El formato obtiene su propio repositorio abierto; sale la versión 0.2 |
| 21 de agosto de 2026 | Última edición de la especificación 0.2 según la copia del 1 de septiembre de 2026 |
| septiembre-noviembre de 2026 | Varias preguntas siguen abiertas en la discusión pública (ver sección 4); los usuarios desarrollan perfiles y herramientas |

**Cambios clave de la versión 0.2:**
- **solo `type` es obligatorio**: el formato se volvió mínimo y fácil de adoptar;
- la hora en todos los campos es estrictamente ISO 8601 con zona horaria;
- el estado del ciclo de vida se limita a tres valores: `draft`, `stable`, `deprecated`;
- la versión del formato se declara una sola vez, en el `index.md` raíz del paquete, no en cada archivo;
- se fijaron las formas de escribir autores y verificadores.

## 3. Líneas que desarrollan los usuarios

Como el núcleo es mínimo, los usuarios construyen sus propias capas encima. Las líneas más visibles (sin referencia a proyectos concretos):

- **Perfiles de extensión.** Un proyecto define su propio prefijo o bloque de campos y describe lo que falta en el núcleo: identificadores propios, estados, idiomas, reglas de acceso.
- **Integridad y firma.** Los conjuntos de documentos reciben un manifiesto aparte con hashes y firma, para comprobar que el paquete no fue modificado.
- **Memoria de agentes de IA.** El formato se usa como memoria a largo plazo de un agente: decisiones, acciones realizadas, reglas del proyecto.
- **Paquetes de conocimiento tipificados.** Tipos de documento recurrentes: «trampa conocida», «resultado negativo», «instructivo paso a paso», «flujo de trabajo».
- **Registros con control de integridad.** Extensiones del `log.md` reservado.
- **Verificadores de cabeceras (linters)** y **migración de archivos antiguos** al formato.

## 4. Problemas y preguntas abiertas

- **Tipos libres.** `type` no tiene un vocabulario común: distintos autores nombran lo mismo de forma distinta. Los usuarios deben mantener sus propios vocabularios.
- **Fechas en YAML.** Las bibliotecas YAML habituales convierten una fecha sin comillas en un objeto de fecha y pierden la zona horaria; en la práctica hay que poner las marcas de tiempo entre comillas.
- **Historial de cambios.** `generated` describe solo el contenido actual; la especificación aún no define cómo conservar la autoría original y la cadena de cambios.
- **Traducciones.** El identificador del concepto es la ruta del archivo, así que las traducciones de un documento reciben identificadores distintos; «esto es una traducción de aquello» solo se expresa mediante extensiones.
- **Confianza y autoverificación.** El formato puede registrar quién verificó un documento, pero no impide que un autor (incluido un agente de IA) marque su propio trabajo como verificado.
- **Enlaces estilo wiki** (`[[...]]`) aún no están estandarizados.
- **Datos sensibles.** En cabeceras y cuerpos aparecen claves de acceso, enlaces firmados y datos personales: se necesitan reglas y verificación automática.

## 5. Perfiles y extensiones en la práctica

Un ejemplo de perfil es **okfguide**. No modifica el núcleo de Google; todos sus campos propios viven en **un solo bloque, `x-okfg`**. El prefijo `x-` es una convención habitual para extensiones (la especificación permite cualquier clave).

Fragmento de cabecera (se muestra una parte de los campos del perfil):

```yaml
---
type: Plan
title: "Ejemplo"
description: "Qué contiene y cuándo leerlo."
status: draft
generated: { by: "claude-code/claude-opus-5.5", at: "2026-11-08T21:35:35-03:00" }
x-okfg:
  id: "plan/ejemplo"           # identificador estable, independiente de la ruta y del idioma
  kind: plan                   # clase de documento de un vocabulario cerrado
  profile: "okfguide"
  profile-version: "1.0"
  lang: "es"
  state-class: draft           # estado de trabajo, separado del status del núcleo
  privacy: internal            # quién puede verlo
  epistemic_class: model_analysis   # origen del conocimiento: hecho de la fuente, decisión humana, análisis del modelo
  review: { state: needs_review }
---
```

Reglas del perfil aprendidas en la práctica:
- un agente de IA siempre escribe `status: draft`; solo una persona pone `stable`;
- la hora se toma del reloj del sistema, nunca se inventa;
- la cabecera de cada archivo la revisa un programa (un validador);
- si la revisión protesta por el contenido de la fuente, se corrige la revisión, no la fuente;
- los estados de trabajo (borrador, en revisión, aprobado) se guardan aparte del campo `status` del núcleo.

**Experiencia con un archivo personal.** En el proyecto okfguide el formato se aplicó al archivo personal de notas de un autor (unos 5.000 archivos a lo largo de 2026) generados por distintos modelos de IA. Las cabeceras evolucionaron desde un conjunto libre de campos en cada modelo hasta un perfil único. Las desviaciones más frecuentes respecto al estándar: campos propios fuera de un bloque común, estados y tipos fuera del vocabulario, marcas de tiempo sin zona horaria, dos esquemas de cabecera distintos en una misma carpeta. Conclusión: el formato necesita un validador automático y el trabajo de los agentes de IA debe verificarse.

## 6. Ámbitos de uso

- **Textos:** documentación, notas, planes, decisiones, informes.
- **Tareas e informes de agentes de IA:** la tarea, los estados de ejecución, las pruebas.
- **Audio y video:** transcripciones y subtítulos con marcas de tiempo, traducciones, descripciones de episodios.
- **Imágenes:** ficha de la imagen (autor, fuente, licencia, marca de generación por IA).
- **Generación y procesamiento:** el resultado generado lleva una cabecera sobre su origen; los documentos procesados enlazan a sus fuentes.
- **Conjuntos de conocimiento para herramientas de IA:** un mismo paquete lo leen agentes de distintos fabricantes, también a través de servidores MCP.

## 7. Perspectivas

El formato avanza por dos líneas: el núcleo se mantiene mínimo y las necesidades prácticas se cubren con perfiles. Temas esperables: vocabularios comunes de tipos, una forma estándar de registrar traducciones e historial de cambios, firma de paquetes, herramientas de validación y migración. El proyecto **okf.guide** reúne la información sobre el formato en un solo lugar y en varios idiomas (inglés, ruso, español), para que nadie tenga que buscarla en fuentes dispersas.

---
*Este artículo se basa en la especificación abierta Google Open Knowledge Format v0.2 y en la experiencia propia de uso del formato. Los nombres y detalles de proyectos de terceros se omiten intencionadamente.*
