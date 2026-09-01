---
name: idea-a-proyecto
description: >
  Úsame cuando el usuario diga "convierte esta nota en proyecto", "conviérteme
  las ideas de esta nota", "arranca proyectos desde esta nota" o pida convertir
  ideas ya escritas en una nota (de inbox/ o cualquier otra carpeta) en
  proyectos dentro de proyectos/. Complementa a crear-brief: en vez de
  preguntar todo desde cero, reusa el contenido que ya está escrito.
---

# Idea a proyecto

## Qué hace esta skill
Toma una nota existente (indicada por el usuario) y, por cada idea con
desarrollo propio que encuentra, crea o actualiza un proyecto en proyectos/
usando la plantilla de brief.md de crear-brief. Al terminar, marca la nota
origen como convertida sin moverla ni borrarla.

## Cómo identifica una idea
Solo cuentan como idea los headers de nivel `##` que tienen párrafos de
desarrollo debajo (contexto, ejemplos, explicación). Listas de una sola línea
o menciones sueltas (por ejemplo dentro de una sección "ideas propuestas")
NO cuentan como idea propia — ver regla de contenido insuficiente abajo.

## Proceso
1. Leer la nota completa que el usuario señale.
2. Identificar cada `##` con desarrollo propio como una idea candidata.
3. Para cada idea candidata:
   a. Evaluar si tiene contenido suficiente para llenar el brief (al menos
      un "por qué" o justificación, y algo de contexto de qué se necesita
      para considerarla lista). Si NO alcanza: avisar al usuario cuál idea es
      y qué le falta, y preguntar si quiere completarla ahora, dejarla
      pendiente, o saltarla. No inventar contenido para rellenar huecos.
   b. Si alcanza: buscar en proyectos/ si ya existe una carpeta de proyecto
      alineada con esa idea (mismo tema, nombre parecido, contenido
      relacionado).
      - Si existe: agregar el contenido nuevo al brief.md existente (sección
        "Notas" o la que corresponda), sin duplicar la carpeta.
      - Si no existe: crear proyectos/[nombre_en_guion_bajo]/brief.md nuevo
        con la plantilla de abajo, usando el contenido ya escrito en la nota.
4. Al terminar de procesar todas las ideas de la nota, agregar al final de la
   nota origen la sección "Convertida a proyecto(s)" (ver formato abajo) con
   fecha y wikilinks a cada brief.md creado o actualizado.
5. La nota origen nunca se mueve ni se borra — queda en su carpeta original.

## Regla de contenido insuficiente
Si una idea no tiene desarrollo suficiente, no se crea el proyecto en
silencio ni se descarta sin avisar. Siempre preguntar antes de actuar sobre
esa idea en particular.

## Convención de nombres
Carpetas de proyecto en guion_bajo, coherente con CLAUDE.md
(ej. proyectos/marketplace_insumos_b2b/).

## Plantilla del brief (igual a crear-brief)
```markdown
---
proyecto: [nombre]
inicio: YYYY-MM-DD
límite: YYYY-MM-DD (o "abierto")
estado: activo
tags: []
---

# [Nombre del proyecto]

## Por qué existe
[Tomado del contenido ya escrito en la nota origen.]

## Qué tiene que pasar para que esté listo
[Derivado de la nota; si no está explícito, dejarlo como lista corta a
completar y avisarlo al usuario.]

## Próximos pasos
- [ ] ...
- [ ] ...

## Lo que ya existe en el vault
[Contenido de investigación/validación ya presente en la nota origen, más
cualquier otra nota relacionada encontrada en el vault.]

## Notas
[Contexto adicional de la nota origen que no encaja en las secciones de arriba.]
```

## Formato de la marca de conversión en la nota origen
```markdown
## Convertida a proyecto(s)
- YYYY-MM-DD: [[nombre_proyecto/brief]] (Idea N)
```

## Manejo de errores
Si algo falla al escribir (ruta inválida, colisión de nombre, error de
escritura): explicar exactamente qué pasó y pedir confirmación antes de
reintentar o de tomar cualquier acción correctiva. Nunca reintentar en
silencio.

## Reglas heredadas del vault
- No borrar archivos sin preguntar primero
- No mover la nota origen de su carpeta
- Confirmar antes de editar notas que tengan más de 7 días (CLAUDE.md regla 3)
- Las conexiones entre notas van en ambas direcciones

## Al terminar
Reporta: cuántas ideas se procesaron, cuáles se convirtieron en proyecto
nuevo, cuáles actualizaron un proyecto existente, y cuáles quedaron
pendientes por falta de contenido — una línea por idea.
