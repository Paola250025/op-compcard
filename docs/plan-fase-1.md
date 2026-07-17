# 🏠 O&P CompCard — Fase 1 — Plan de Implementación

## 1. Objective

Construir la Fase 1 de O&P CompCard: una app web privada, mobile-first, para
Olaf y Paola, donde meten una dirección y ven estimado + comps + flags de costos
+ calculadora de pago, guardan casas por mínimo 2 días, y comparten una imagen
con marca Olaf & Paola. Datos vía RentCast (tier gratis), sin exponer la llave
del API.

## 2. Problem Context

Cuando un comprador pregunta "¿en cuánto se vende?" o "¿me conviene?" frente a
la casa, hoy Olaf y Paola abren el MLS y hacen cuentas mentales — lento y poco
presentable, y se les pueden pasar los costos que mueven la decisión (HOA alta,
assessment, non-warrantable). La app les pone esa respuesta, con números y
presentable, en el celular en segundos.

## 3. Reference Spec

Ver `O&P CompCard/doc/2026-07-16-op-compcard-fase-1.md` (aprobado por Paola,
2026-07-16). Alcance Fase 1: búsqueda por dirección, hoja de comps, flags de
dinero (HOA/impuestos/assessment/non-warrantable) con fallback manual, HOA vs.
comps, calculadora, guardar casas ≥2 días, compartir como imagen, contraseña,
marca de homesbuyolaf.com en subdominio. Fuera: sincronización entre equipos,
HOA por ciudad, tasa automática, CRM, comps del MLS.

## 4. Tasks

### Fundación (primero, todo depende de esto)

**Decisión de hosting (tomada): GitHub Pages.** Paola prefiere un solo lugar y
sin cuentas nuevas. GitHub Pages no puede esconder la llave del API (solo sirve
archivos públicos), así que la llave va en el código del cliente, expuesta.
Se acepta el riesgo con estas condiciones:
- **Solo plan gratis de RentCast, nunca el pagado.** Así una llave expuesta no
  puede generar cobros; lo peor es que alguien gaste las 50 consultas del mes.
- **Restringir la llave a los endpoints que usa la app** en el panel de RentCast
  (limita el mal uso; no lo elimina).
- Si algún día se sube a plan pagado, migrar a un hosting que esconda la llave
  (ej. Cloudflare). Documentado como límite conocido.

**T1 — Repo, GitHub Pages y subdominio**
- Crear el repo `op-compcard`.
- Activar GitHub Pages en el repo.
- Configurar el subdominio en GoDaddy (`card.homesbuyolaf.com` → CNAME al usuario
  de GitHub, ej. `paola250025.github.io`). No se toca la raíz (Showit intacto).
- **Hecho cuando:** una página carga en `card.homesbuyolaf.com` por HTTPS.

**T2 — Integración con RentCast (lado cliente)**
- La app llama a RentCast directo, con la llave en un archivo de config marcado
  claramente. (En GitHub la llave queda expuesta — ver decisión de hosting.)
- Verificar la forma real de la respuesta de RentCast: qué trae de verdad
  (estimado, beds/baths/sqft, comps, y si trae HOA/impuestos/assessment). Ajustar
  cuáles flags son "auto" vs "solo manual" según lo que el API realmente da.
- Contar consultas usadas del mes (límite 50 free) y mostrarlo en la app.
- **Hecho cuando:** una dirección real muestra datos reales de RentCast en la app.
- **Depende de:** T1, y de que Paola active el plan gratis Developer de RentCast.

**T3 — Contraseña de acceso**
- Puerta con contraseña sencilla compartida (Olaf y Paola), se recuerda en el
  teléfono. Nota honesta: en GitHub esto oculta la interfaz, no el código; sirve
  para mantener fuera a curiosos, no es seguridad fuerte. Suficiente para este uso.
- **Hecho cuando:** sin contraseña no se ve la app; con ella entra y se recuerda.
- **Depende de:** T1.

### Funciones core (en el orden en que se construyen)

**T4 — Búsqueda por dirección + datos de la propiedad**
- Campo de dirección → llama al proxy → muestra estimado de valor, beds/baths/
  sqft. Manejar bien: dirección no encontrada ("No encontramos datos de esta
  dirección") y datos parciales.
- **Hecho cuando:** una dirección real muestra datos reales; una mala muestra un
  mensaje claro y deja seguir a la calculadora manual.
- **Depende de:** T2, T3.

**T5 — Mini hoja de comps + rango**
- Casa sujeto + hasta 3-4 comps con precio, sqft, $/sqft; calcular y mostrar un
  rango de precio defendible.
- **Hecho cuando:** se arma la hoja desde los datos del proxy y el rango sale de
  los comps.
- **Depende de:** T4.

**T6 — Flags de dinero + fallback manual**
- HOA, impuestos, assessment, non-warrantable. Auto cuando el dato existe; campo
  opcional para teclear cuando no. Resaltado verde/rojo. Nunca inventar un dato.
- **Hecho cuando:** muestra los valores automáticos; los campos vacíos se pueden
  teclear; los colores reflejan el estado.
- **Depende de:** T4.

**T7 — HOA vs. comps**
- Comparar la HOA del sujeto contra la HOA de los comps; rojo si está más alta,
  con cuánto. Solo cuando el dato de HOA existe en los comps; si no, no se muestra.
- **Hecho cuando:** aparece la comparación cuando hay datos de HOA; se oculta
  limpio cuando no.
- **Depende de:** T5, T6.

**T8 — Calculadora de pago**
- Inputs: pago mensual y/o enganche + tasa de interés ajustable (default fijo en
  Fase 1; la tasa automática es Fase 2). Mostrar desglose (préstamo + impuestos
  + HOA + seguro) y el pago mensual. Sin veredicto "sí/no".
- **Hecho cuando:** los números calculan bien y la tasa se puede editar.
- **Depende de:** T4 (usa precio/impuestos/HOA como inputs cuando existen).

**T9 — Guardar casas (≥2 días, auto-borrado) + lista reciente**
- Guardar cada casa vista (con estimado y flags) en el teléfono; lista de
  recientes en la pantalla de inicio; auto-borrado después de mínimo 2 días.
- **Hecho cuando:** una casa buscada persiste ≥2 días, se auto-borra después, y
  la lista de recientes la vuelve a abrir sin teclear.
- **Depende de:** T4.

**T10 — Compartir como imagen**
- Botón que genera una imagen con marca Olaf & Paola: casa (comps + costos) +
  números del cliente + aviso "aproximado" visible. Se guarda/comparte por el
  teléfono. No es link, no da acceso a la app.
- **Hecho cuando:** tocar "Compartir" produce una imagen con toda la info y el
  aviso, lista para mandar por texto/WhatsApp.
- **Depende de:** T5, T6, T8.

### Acabado

**T11 — Marca visual Olaf & Paola + responsive**
- Aplicar la identidad de homesbuyolaf.com: serif elegante, blanco/crema, taupe/
  gris pizarra. Mobile-first (se usa en el celular). Conseguir el logo real de
  Olaf & Paola de Paola, o aproximar con el wordmark por ahora.
- **Hecho cuando:** en un teléfono se ve como parte de la marca Olaf & Paola, no
  como default genérico.
- **Depende de:** puede ir en paralelo, se cierra al final sobre T4-T10.

## Orden de construcción
T1 → T2 → T3 → T4 → (T5, T6, T8 en paralelo) → T7 → T9 → T10 → T11.
El objetivo es tener algo usable en cuanto T4-T8 estén; T10 (compartir) al final
de la Fase 1, como acordamos.

## Decisiones que necesitan a Paola antes de construir
- Confirmar hosting (Cloudflare Pages recomendado vs. GitHub Pages + Worker).
- Crear una cuenta gratis de RentCast y darme la llave del API (va como secreto,
  nunca en el código público).
- Definir la contraseña compartida.
- Pasar el logo de Olaf & Paola si existe (si no, arranco con el wordmark).
