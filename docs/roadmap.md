# Roadmap: 🏠 O&P CompCard (tool de bienes raíces)
Fecha: 2026-07-16
Repo/técnico: op-compcard

## La idea en una frase
Un tool privado (link en el celular) para Paola y Olaf, que al mostrar una
casa a un comprador les da en segundos el panorama de precio y los costos que
mueven la decisión del cliente — para responder "¿en cuánto se vende?" y
"¿me conviene?" ahí mismo, con números, no con adivinanzas.

## La acción core
Meter una dirección y ver, frente al cliente, un panorama de precio defendible
(estimado de valor + comps + rango de $/sqft) junto con los flags de dinero
(HOA, impuestos, assessments). Todo lo demás sirve a eso.

## Restricciones y decisiones de datos (contexto)
- **Usuarios:** solo Paola y Olaf. Privado. En el celular, en el campo.
- **Datos automáticos:** RentCast tier gratis (50 consultas/mes). Trae estimado
  de valor, recámaras/baños/sqft, comps de venta, e impuestos/HOA *cuando el
  dato existe*. Costo: $0.
- **Cero tecleo para datos de propiedad** como default; campo manual opcional
  solo para lo que el API y los datos públicos no encuentren (ej. assessment,
  non-warrantable).
- **Lo que NINGÚN API barato tiene** (non-warrantable, "HOA vs área"): no se
  promete como automático. Ver Backlog.

## Nombre, marca, hosting y privacidad
- **Nombre en la app:** 🏠 O&P CompCard (O&P = Olaf & Paola). Repo: `op-compcard`.
- **Marca visual:** la de **homesbuyolaf.com** (sitio "Olaf & Paola — Southern
  California Real Estate"): serif elegante, blanco/crema, taupe/gris pizarra,
  fotografía cálida. NO es la marca de Paola Adventurer (esa es verde bosque).
- **Hosting:** subdominio de homesbuyolaf.com (ej. `card.homesbuyolaf.com`),
  repo propio en GitHub, un registro en GoDaddy. El sitio de Showit no se toca.
- **Privacidad:** app privada con contraseña sencilla (solo Olaf y Paola). Nadie
  más entra a la app.
- **Compartir:** genera una **imagen** (no link, no acceso a la app) que se manda
  al chat/WhatsApp. Muestra casa + números del cliente, marca Olaf & Paola, y un
  aviso de que precios y pagos son **aproximados**.

## Fase 1 — Lanzamiento
El camino más corto a que Paola/Olaf lo usen con un cliente y les sirva.

| # | Feature | Por qué va primero | Depende de |
|---|---------|--------------------|------------|
| 1 | Búsqueda por dirección → auto-jala lo disponible (estimado de valor, beds/baths/sqft, impuestos/HOA cuando existen) vía RentCast free | Es el "no quiero teclear nada". Sin esto no hay datos. | — |
| 2 | Mini hoja de comps: casa sujeto + 3-4 comps con precio, sqft, $/sqft y un rango defendible | Es la acción core (opción B que elegiste) | #1 |
| 3 | Flags de dinero: HOA, impuestos, assessment, non-warrantable — auto cuando el dato existe, con campo manual opcional si no; resaltados verde/rojo | Es lo que mueve la decisión del comprador en el momento | #1 |
| 3b | HOA vs. comps: compara la HOA del sujeto contra la HOA de los comps que ya se jalaron y la resalta en rojo si está más alta (cuando el dato de HOA existe en los comps) | Responde "¿la HOA está cara para el área?" sin datos extra — reusa los comps de #2 | #2 |
| 4 | Calculadora: metes pago mensual y/o enganche + tasa ajustable (default = tasa del día). Muestra desglose completo (préstamo + impuestos + HOA + seguro) y el pago mensual. Sin veredicto sí/no, solo números | Alto valor y es independiente (pura matemática) — el early win más fácil | — (usa el precio de #1/#2 como referencia) |
| 5 | Compartir como imagen: botón que genera una imagen bonita con marca Olaf & Paola (casa + números del cliente + aviso "aproximado"). Se manda al chat/WhatsApp; no es link, no da acceso a la app | Paola lo pidió como must-have para el lanzamiento | #2, #4 |

## Fase 2 — Mejora
Con lo aprendido de usarlo en el campo:
- **Guardar casas ~1 semana con auto-borrado** + guardar estimados por poco
  tiempo. Vive en el teléfono (localStorage), gratis. Cada quien ve las suyas.
- **Tasa del día automática** en la calculadora (en vez de que la teclees),
  si encontramos una fuente gratis confiable.
- **HOA por ciudad (tabla propia que crece con el uso)** — cada búsqueda guarda
  la HOA por ciudad; con las semanas te da tu promedio real de cada ciudad
  donde trabajas, gratis. Es el "HOA vs. la ciudad" de verdad, construido de
  tus propias búsquedas en vez de comprar datos.

## Backlog
Nada se pierde, pero nada de aquí bloquea el lanzamiento:
- **"HOA más alto que las áreas de alrededor" (automático)** — no hay fuente de
  datos barata con promedios de HOA por zona. Feature impresionante pero sin
  datos reales que la sostengan. Esperar a encontrar una fuente, o hacerla con
  benchmarks que tú misma mantengas.
- **CRM completo** (guardar clientes, qué buscan, presupuesto, seguimiento) —
  es otro producto. Lo mencionaste al inicio pero aleja del precio-en-el-momento
  que es lo urgente. Se decide después.
- **Sincronizar entre Olaf y tú** (ver las mismas casas guardadas) — necesita
  un servidor, no solo el teléfono. Cuesta y complica. Después.
- **Subir a RentCast pagado ($74/mes, 1,000 consultas)** — solo cuando las 50
  gratis no alcancen y el tool ya te esté ahorrando tiempo. Decisión con datos
  de uso, no ahora.

## Siguiente paso
Convertir la Fase 1 en spec con design-spec, usando este roadmap como contexto.
