# 🏠 O&P CompCard — Fase 1

## 1. Overview

O&P CompCard es una herramienta web privada para Olaf y Paola (Homes Buy Olaf,
bienes raíces en el sur de California). La abren en el celular mientras le
muestran una casa a un comprador, meten la dirección, y en segundos ven un
panorama de precio (estimado + comps + rango de $/sqft), los costos que mueven
la decisión del comprador (HOA, impuestos, assessment, non-warrantable), y una
calculadora de pago. Con un botón generan una imagen bonita con marca Olaf &
Paola para mandarle al cliente por texto o WhatsApp. Todo con costo de datos
$0 usando el tier gratis de RentCast.

## 2. Target User

Dos personas, mismo uso: **Olaf y Paola**, agentes de bienes raíces, parados
frente a una casa con un comprador. No es para clientes ni para otros agentes.

- **Contexto de uso:** en el celular, en el campo, mientras muestran una
  propiedad. Necesitan la respuesta rápido y que se vea presentable para
  enseñársela al comprador en el momento.
- **Nivel técnico:** no técnico. La herramienta tiene que ser de meter dirección
  y ver resultado, sin configurar nada.

El comprador es un usuario indirecto: nunca entra a la app, solo recibe la
imagen que Olaf o Paola le comparten.

## 3. Problem Context

Cuando un comprador pregunta "¿en cuánto se vende esta casa?" o "¿me conviene?"
estando frente a la propiedad, hoy Olaf y Paola tienen que abrir el MLS, buscar
comps a mano, y hacer cuentas mentales de HOA, impuestos y pago. Eso es lento y
poco presentable en el momento, y los costos que más pesan en la decisión (una
HOA alta, un assessment, un condominio non-warrantable que complica el
financiamiento) se les pueden pasar cuando van de prisa.

El disparador: quieren tener "el CRM y los comps en la punta de los dedos" para
responder con números y verse preparados justo cuando el comprador lo pregunta,
sin depender de sentarse frente a una computadora.

## 4. V1 Scope

**In scope (Fase 1):**
- Búsqueda por dirección que auto-jala lo disponible vía RentCast free:
  estimado de valor, recámaras/baños/sqft, comps de venta, e impuestos/HOA
  cuando el dato existe.
- Mini hoja de comps: casa sujeto + hasta 3-4 comps con precio, sqft, $/sqft, y
  un rango de precio defendible.
- Flags de dinero: HOA, impuestos, assessment, non-warrantable. Auto cuando el
  dato existe; campo manual opcional cuando no. Resaltados en verde/rojo.
- HOA vs. comps: compara la HOA del sujeto contra la HOA de los comps y la
  resalta en rojo si está más alta (cuando el dato existe en los comps).
- Calculadora de pago: Olaf/Paola meten pago mensual y/o enganche + tasa de
  interés ajustable (con un default). Muestra desglose (préstamo + impuestos +
  HOA + seguro) y el pago mensual. Sin veredicto "sí/no", solo números.
- Compartir como imagen: botón que genera una imagen con marca Olaf & Paola
  (casa + números del cliente + aviso "aproximado") para mandar al chat.
- Guardar casas vistas (con su estimado) por **mínimo 2 días**, con auto-borrado
  después. Se guardan en el mismo teléfono; cada quien ve las suyas.
- Acceso privado con contraseña sencilla (solo Olaf y Paola).
- Marca visual de homesbuyolaf.com (serif elegante, blanco/crema, taupe/gris
  pizarra). Vive en un subdominio (ej. card.homesbuyolaf.com).
- **Idioma de la app: inglés** (marca Olaf & Paola, clientes en el sur de
  California).

**Out of scope for v1 (después):**
- Sincronizar las casas guardadas entre los teléfonos de Olaf y Paola — Fase 2
  (necesita servidor). En Fase 1 cada quien ve solo las que guardó en su equipo.
- HOA promedio real por ciudad (tabla propia que crece con el uso) — Fase 2.
- Tasa de interés del día automática — Fase 2.
- CRM completo (guardar clientes, seguimiento) — backlog.
- Sincronizar datos entre Olaf y Paola — backlog (necesita servidor).
- Jalar comps del MLS automáticamente — backlog (caro/complejo).
- "HOA vs. promedio oficial de toda la ciudad" automático de fuentes externas —
  backlog (no hay dato barato).

## 5. Expected Behavior

**Entrar a la app**
1. Olaf o Paola abren `card.homesbuyolaf.com` en el celular.
2. Piden una contraseña. La meten una vez; la app la recuerda en ese teléfono.
3. Ven una pantalla simple con un campo de dirección.

**Buscar una casa (camino feliz)**
1. Escriben o pegan la dirección de la casa que están mostrando.
2. En 1-3 segundos ven: estimado de valor, recámaras/baños/sqft, y un rango de
   precio basado en los comps.
3. Debajo, la mini hoja de comps: la casa sujeto arriba y 3-4 comps con precio,
   sqft y $/sqft lado a lado.
4. Los flags de dinero aparecen con color: HOA, impuestos, assessment,
   non-warrantable. Si la HOA del sujeto es más alta que la de los comps, sale
   en rojo con cuánto más alta.

**Cuando falta un dato**
1. Si RentCast no trae la HOA, el assessment, o el flag non-warrantable, ese
   campo aparece vacío con la opción de teclearlo en 3 segundos (porque Olaf/
   Paola están viendo el listing del MLS ahí mismo).
2. Si no lo teclean, se queda vacío y no rompe nada; la hoja se arma con lo que
   sí hay.
3. Si la dirección no la encuentra RentCast, la app lo dice claro ("No
   encontramos datos de esta dirección") y deja meter los números a mano para
   la calculadora.

**Calcular el pago**
1. Olaf/Paola meten el pago mensual que el cliente quiere y/o el enganche.
2. Ajustan la tasa de interés si quieren (viene con un default).
3. La app muestra el desglose (préstamo + impuestos + HOA + seguro) y el pago
   mensual estimado. No dice si el cliente califica; solo da los números para
   que Olaf/Paola se los comenten.

**Casas guardadas**
1. Cada casa que buscan se guarda sola en ese teléfono, con su estimado y sus
   flags.
2. En la pantalla de inicio ven una lista corta de las casas vistas
   recientemente, para volver a abrir una sin teclear la dirección de nuevo.
3. Cada casa se guarda al menos 2 días y se borra sola después, para no
   acumular datos viejos.

**Compartir con el cliente**
1. Tocan "Compartir".
2. La app genera una imagen con marca Olaf & Paola: la casa (comps + costos) y
   los números del cliente, con un aviso visible de que precios y pagos son
   aproximados.
3. Guardan/mandan esa imagen por texto o WhatsApp. El cliente ve la imagen en
   el chat; no recibe ningún link ni entra a la app.

## 6. Risks & Mitigations

| Riesgo | Mitigación |
|---|---|
| El tier gratis de RentCast son 50 consultas/mes; en un día activo se puede acabar | Contador visible de consultas usadas; cuando se acabe, la app sigue sirviendo para meter datos a mano y calcular. Subir a plan pagado ($74/mes) solo si el uso lo justifica (decisión con datos, Fase 2) |
| La cobertura de HOA/impuestos en RentCast free es inconsistente | Campo manual opcional para teclear lo que falte; la app nunca inventa un dato, muestra vacío si no lo tiene |
| La llave del API quedaría expuesta si va en el código del navegador (alguien podría gastar las consultas o generar cobros) | Necesita resolverse en el plan: usar un proxy pequeño (ej. función serverless gratis) que esconda la llave. Decisión técnica para la fase de plan, no del spec |
| Datos financieros de clientes en una página web | Contraseña para entrar; nada de datos de cliente se guarda en Fase 1 (todo es en el momento). El compartir es imagen, no link |
| El comprador podría tomar el "estimado aproximado" como una promesa de precio | Aviso de "aproximado" visible en pantalla y en la imagen compartida; la calculadora no da veredicto "sí/no" |
| non-warrantable y assessments especiales no existen en ningún API | Se aceptan como campos manuales; no se prometen como automáticos (ya acordado con Paola) |
| Un subdominio mal configurado podría afectar el sitio de Showit | El subdominio es independiente; solo se agrega un registro DNS nuevo en GoDaddy, sin tocar la raíz. Verificar antes de publicar |
| El "compartir imagen" hace la Fase 1 más grande y retrasa el lanzamiento | Construir en orden: primero búsqueda→calculadora usable, y el compartir al final de la Fase 1 |
