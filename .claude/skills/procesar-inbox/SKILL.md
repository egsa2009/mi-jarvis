---
name: procesar-inbox
description: >
  Úsame cuando el usuario diga "procesa el inbox", "revisa lo que entró",
  "clasifica mis notas nuevas" o pida limpiar/ordenar la carpeta inbox/.
  Lee cada archivo de inbox/, identifica su tipo y muévelo a la carpeta
  correcta con frontmatter completo.
---

# Procesar inbox

## Qué hace esta skill
Lee cada archivo en inbox/ y lo procesa uno por uno:
1. Identifica el tipo: idea suelta, recurso externo, tarea, referencia de persona
2. Agrega frontmatter YAML con fecha, tags y tipo
3. Lo mueve a su carpeta correcta (notas/, recursos/, proyectos/, personas/)
4. Crea un [[wikilink]] si la nota conecta con algo que ya existe

## Reglas de procesamiento
- Nunca borrar el archivo original, siempre mover
- Si no estás seguro de la carpeta destino, pregunta
- Un archivo de inbox puede dar origen a más de una nota
- Archivos con más de 3 días sin procesar, mencionarlo al usuario

## Formato de frontmatter a agregar
```yaml
---
fecha: YYYY-MM-DD
tipo: [idea | recurso | proyecto | persona | tarea]
tags: []
origen: inbox
conexiones: []
---
```

## Al terminar
Reporta: cuántos archivos procesaste, a qué carpetas fueron, y si encontraste
conexiones con notas existentes. Una línea por archivo.
