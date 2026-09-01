---
proyecto: marketplace_insumos_b2b
inicio: 2026-09-01
límite: abierto
estado: activo
tags: [negocio, marketplace, b2b]
---

# Marketplace B2B de insumos

## Por qué existe
Conectar personas/negocios que venden insumos con quienes los necesitan (ej.
un vendedor de café al por mayor con una cafetería que lo busca, que a su vez
necesita proveedor de panadería). Hoy ese match se hace de forma manual e
informal. En Colombia/LatAm no existe un marketplace de matching oferta-demanda
para pequeños proveedores — solo hay software de punto de venta (Loggro,
Vendty, Dixeb), no de conexión. Posible hueco real de mercado.

## Qué tiene que pasar para que esté listo
- [x] Definir si arranca nicho o multi-categoría → multi-categoría desde el inicio
- [x] Definir modelo de monetización → comisión por transacción
- [x] Validar la capa de delivery con repartidores independientes → tarifa fija
      por entrega (ajustada por zona/distancia)
- [x] Definir cómo validar demanda real → MVP funcional mínimo (formulario +
      matching simple) probado con un grupo piloto

## Próximos pasos
- [x] Definir el alcance exacto del MVP (ver "Alcance del MVP" abajo)
- [ ] Diseñar el flujo de pago de comisión (cómo se cobra al cerrarse una venta)
- [ ] Diseñar el flujo de asignación y pago fijo a repartidores independientes
- [ ] Reclutar un grupo piloto de proveedores y compradores en una zona/ciudad
      para probar el MVP
- [ ] Construir el MVP

## Lo que ya existe en el vault
- [[../../inbox/2026-08-30_ideas_negocio_marketplace|2026-08-30_ideas_negocio_marketplace]]
  — nota origen con la idea completa, investigación de competencia
  (FoodByUs, Notch, Ankorstore, Nutrada, Ingredients Online a nivel global;
  Loggro, Vendty, Dixeb, App La Compra en Colombia/LatAm) y el refinamiento
  de la capa de delivery con repartidores independientes.

## Notas
Modelo de 3 lados: proveedor de insumo / negocio que necesita / repartidor
independiente — similar al patrón que usan Rappi o Uber para resolver
logística sin flota propia.

### Decisiones (2026-09-01)
- **Alcance:** multi-categoría desde el inicio, no solo un nicho.
- **Monetización:** comisión por transacción (% de cada venta conectada en
  la plataforma). Implica que el pago debe pasar por la plataforma para
  poder cobrar la comisión.
- **Pago a repartidores:** tarifa fija por entrega, ajustada por zona/
  distancia (no tarifa dinámica tipo Uber, al menos no en el MVP).
- **Validación de demanda:** MVP funcional mínimo (formulario + matching
  simple) probado con un grupo piloto, en vez de un piloto 100% manual sin
  construir nada.

Pendiente definir: si el repartidor ve solo el pedido o también puede
negociar directo con quien compra/vende.

### Alcance del MVP (2026-09-01)
- **Categorías del piloto (3):** café/bebidas al por mayor, panadería/
  repostería, insumos de cocina/restaurante en general. El resto de
  categorías queda para después de validar el piloto, aunque el modelo
  de datos ya es multi-categoría desde el diseño.
- **Datos al publicar una oferta o necesidad:** categoría, descripción libre,
  precio o rango de precio, ubicación y zona de reparto. (Cantidad mínima de
  pedido queda fuera del MVP — se puede negociar directo entre las partes por
  ahora.)
- **Lógica de matching del MVP:** match automático por categoría + zona de
  reparto (sin notificaciones activas todavía — eso queda para una iteración
  posterior).
