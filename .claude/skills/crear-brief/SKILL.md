---
name: crear-brief
description: >
  Úsame cuando el usuario diga "crea un brief", "arranca este proyecto",
  "quiero empezar a trabajar en X", "necesito un plan para Y" o cualquier
  pedido de estructura para un proyecto nuevo.
---

# Crear brief de proyecto

## Qué hace esta skill
Genera el archivo brief.md para un proyecto nuevo dentro de proyectos/.
Busca primero en el vault si ya hay notas, ideas o recursos relacionados.

## Proceso
1. Preguntar: nombre del proyecto, fecha límite (si existe), para quién es
2. Buscar en el vault todo lo que ya existe sobre ese tema
3. Crear la carpeta proyectos/[nombre-proyecto]/
4. Escribir el brief.md con la plantilla de abajo

## Plantilla del brief
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
[Una oración. El problema o la oportunidad que justifica este proyecto.]

## Qué tiene que pasar para que esté listo
[Lista corta. Criterios de éxito concretos y verificables.]

## Próximos pasos
- [ ] ...
- [ ] ...

## Lo que ya existe en el vault
[Links a notas, recursos o ideas relevantes encontradas.]

## Notas
[Espacio libre para contexto adicional.]
```

## Después de crearlo
Confirma la ruta del archivo y pregunta si quieres crear una nota de inbox
para el siguiente paso inmediato.
