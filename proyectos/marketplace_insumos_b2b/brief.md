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
- [x] Diseñar el flujo de pago de comisión (ver "Flujo de pago de comisión" abajo)
- [x] Diseñar el flujo de pago a repartidores (ver "Flujo de pago a
      repartidores" abajo) — falta definir la asignación de pedidos
- [ ] Reclutar un grupo piloto de proveedores y compradores en una zona/ciudad
      para probar el MVP
- [x] Backend del MVP (Supabase) — ver "Infraestructura del MVP" abajo
- [x] Frontend del MVP (web app en Vercel) — https://marketplace-insumos-b2b.vercel.app
- [ ] Integrar Mercado Pago (fase posterior al primer piloto funcional)

## Lo que ya existe en el vault
- [[../../inbox/2026-08-30_ideas_negocio_marketplace|2026-08-30_ideas_negocio_marketplace]]
  — nota origen con la idea completa, investigación de competencia
  (FoodByUs, Notch, Ankorstore, Nutrada, Ingredients Online a nivel global;
  Loggro, Vendty, Dixeb, App La Compra en Colombia/LatAm) y el refinamiento
  de la capa de delivery con repartidores independientes.

## Búsqueda por producto y lista de compra comparativa (2026-09-02)
Funcionalidad nueva agregada tras el MVP inicial: el comprador indica qué
producto necesita, la app busca proveedores en su zona que lo ofrecen y los
ordena del precio más bajo al más alto. También puede armar una lista de
varios productos (con cantidad) y ver el costo total estimado por cada
proveedor que cubra parte o todo de la lista, ordenado de más barato a más
caro.

**Decisiones:**
- **Modelo de producto:** catálogo fijo por categoría (tabla `productos`,
  15 productos sembrados en las 3 categorías del MVP) en vez de texto libre
  — el proveedor elige el producto de una lista al publicar, garantiza
  match y orden por precio confiables.
- **Precio para ordenar/comparar:** se mantiene el rango min-max (para que
  ambos lados negocien), pero se agregó `precio_promedio` como columna
  generada `(min+max)/2` — ese valor es el que se usa para ordenar
  publicaciones y sumar totales en la lista de compra.
- **Validación de rango contra el mercado:** al publicar, la app compara el
  precio promedio ingresado contra el promedio de todas las ofertas activas
  del mismo producto y avisa (sin bloquear) si se desvía más de 40% del
  promedio — probado y funciona (detectó correctamente un caso 111% por
  encima).
- **Lista de compra:** cada comprador tiene una lista persistente
  (`listas_compra` + `lista_items`) donde agrega producto + cantidad. La
  comparación busca ofertas activas en la zona del comprador para cada
  producto de la lista, toma el precio más barato por producto y proveedor,
  suma cantidad×precio_promedio, y muestra cuántos de los N productos cubre
  cada proveedor — ordenado del total más barato al más caro. Probado de
  punta a punta y funciona correctamente.
- **Esquema:** `productos` (catálogo, RLS solo lectura), `publicaciones`
  ahora referencia `producto_id` (antes solo categoría + texto libre;
  `descripcion` pasó a ser notas opcionales), `listas_compra` y
  `lista_items` (RLS: cada comprador solo ve/edita las suyas).

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

### Flujo de pago de comisión (2026-09-01)
- **Quién paga la comisión:** el proveedor/vendedor — se descuenta de lo que
  recibe, igual que Mercado Libre o Amazon. El comprador paga el precio
  pactado sin recargos.
- **Pasarela del MVP:** Mercado Pago, usando su funcionalidad de split
  payment para marketplace (`marketplace_fee` en Checkout Pro /
  `application_fee` en Checkout API). El comprador paga a través de la
  plataforma, Mercado Pago retiene la comisión del marketplace
  automáticamente y transfiere el resto al proveedor — sin cálculo manual.
  Comisión de Mercado Pago en Colombia: ~3.49% + IVA por pago con tarjeta
  (se suma al costo, hay que tenerlo en cuenta al definir el % propio).
- **Por qué no Nequi/Daviplata/Llave en el MVP:** tienen APIs de cobro pero
  sin split payment nativo — no reparten automáticamente entre comprador y
  proveedor descontando la comisión. Llave/Bre-B es solo transferencia
  persona a persona. Quedan como opción futura (posiblemente vía un
  intermediario tipo Wava) si se busca reducir el costo de Mercado Pago o
  dar una opción de pago más familiar en Colombia — no se descartan, solo
  no arrancan en el MVP.

Pendiente definir: el % exacto de comisión, y en qué momento del flujo se
considera "venta cerrada" para poder liquidar al proveedor.

### Flujo de pago a repartidores (2026-09-01)
- **Quién paga el delivery:** el comprador — se suma a lo que paga, como en
  Rappi/Uber Eats. El proveedor solo asume la comisión del marketplace, no
  el costo de delivery.
- **Cuándo se le paga al repartidor:** liquidación periódica (semanal), no
  pago inmediato por entrega. Se acumulan las entregas de la semana y se
  liquidan de una vez — evita tener que construir un flujo de pago
  transaccional por cada entrega desde el MVP.
- **Medio de la liquidación:** el repartidor elige entre Nequi, Daviplata o
  cuenta bancaria. Más flexible para ellos, pero implica soportar varios
  métodos de pago de salida desde el inicio (no requiere split payment
  automático porque es un lote semanal, no por transacción — se puede hacer
  como transferencias manuales o por lote en el MVP).

Pendiente definir: cómo se asignan los pedidos entre repartidores
disponibles (por cercanía, orden de llegada, u otro criterio), y si el
repartidor ve solo el pedido asignado o también puede negociar directo con
quien compra/vende.

### Infraestructura del MVP (2026-09-02)
- **Formato:** web app responsive (no apps nativas) — desplegable en Vercel,
  igual que jarvis-capture.
- **Backend:** Supabase (proyecto `marketplace-insumos-b2b`, org
  `egsa2009's Org`, región us-east-1, tier gratuito $0/mes).
  URL: `https://mrykqbtnxdqldtiwzeex.supabase.co`
- **Esquema aplicado:** tablas `perfiles` (rol: proveedor/comprador/
  repartidor), `publicaciones` (oferta/necesidad por categoría + zona),
  `pedidos` (conecta publicación + comprador + proveedor + repartidor +
  montos), `liquidaciones_repartidor` (registro semanal). Row Level
  Security activo en las 4 tablas.
- **Pagos:** Mercado Pago se integra en una fase posterior al primer
  piloto funcional — no bloquea probar el matching. El pedido puede
  marcarse "pagado manualmente" (campo `pagado_manual`) mientras tanto.
- **Frontend:** desplegado en https://marketplace-insumos-b2b.vercel.app
  (nombre de producto: "Enlace"). Login/registro con Supabase Auth
  (email+contraseña), onboarding de perfil (nombre/rol/teléfono/zona),
  listado de publicaciones con filtros por categoría/tipo/zona, modal de
  nueva publicación, modal de cerrar pedido (calcula comisión 5% en el
  momento), vista de "Mis pedidos" (comprador y proveedor), vista de
  "Repartos" para repartidores (toma pedidos abiertos con delivery en su
  zona). Código en `C:\proyectos\JARVIS_EGSA\marketplace-insumos-b2b`
  (no versionado en git todavía).
- **Probado manualmente de punta a punta (2026-09-02):** registro y login,
  onboarding de perfil, publicar una oferta, listado con filtros, cerrar un
  pedido como comprador (con cálculo de comisión y tarifa de delivery),
  repartidor tomando un reparto disponible, modo oscuro. Los 3 roles
  (proveedor, comprador, repartidor) validados end-to-end.
- **Confirmación por correo desactivada** en Supabase Auth (Authentication →
  Providers → Email → "Confirm email" apagado) — el plan gratuito tiene un
  rate limit muy bajo de envío de emails que bloqueaba el registro incluso
  con una sola cuenta nueva por hora. Para un piloto controlado, registro
  instantáneo sin confirmación es aceptable; revisar si conviene reactivarlo
  (con SMTP propio) al escalar más allá del piloto.
- **2 bugs de RLS encontrados y corregidos durante las pruebas** (mismo
  patrón en ambos: una policy exigía que el usuario ya fuera parte del
  pedido, pero un repartidor "tomando" un pedido nuevo nunca lo es todavía):
  1. SELECT de `pedidos` no dejaba a los repartidores *ver* pedidos abiertos
     sin asignar — se agregó policy para pedidos con `repartidor_id` nulo y
     `tarifa_delivery > 0`.
  2. UPDATE de `pedidos` no dejaba a los repartidores *tomar* un pedido
     (el update se ejecutaba sin error pero sin efecto) — se agregó policy
     de UPDATE con la misma condición más `with check (repartidor_id =
     auth.uid())`.
- **% de comisión usado en el MVP:** 5% (hardcodeado en el frontend por
  ahora, pendiente hacerlo configurable).
