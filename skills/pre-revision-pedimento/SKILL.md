---
name: pre-revision-pedimento
description: >
  Pre-revisa un pedimento de importación BORRADOR contra sus documentos de soporte
  (factura comercial, lista de empaque, documento de transporte, COVE y prueba de origen)
  antes de que la agencia aduanal lo presente o prevalide. Úsala SIEMPRE que haya que
  cruzar a mano un pedimento contra su papeleta documental antes de la validación:
  discrepancias de cantidad, valor, fracción arancelaria, origen, bultos/pesos, COVE e
  incrementables, y para armar la lista de preguntas precisas para el agente aduanal.
  NO la uses para prevalidar (eso es un servicio oficial que corre la agencia aduanal con
  conectividad a la autoridad), para clasificar aranceles, para calcular la glosa final
  ni para decidir el tratamiento fiscal de la operación.
license: LicenseRef-Heroldo-1.0
---

# Pre-revisión de pedimento contra sus documentos

Lo que hoy hace a mano el importador o el auxiliar de comercio exterior la tarde anterior a la
presentación. Convierte los documentos del usuario en un resultado revisable: **una matriz de
discrepancias con su evidencia y las preguntas exactas para el agente aduanal**, antes de que un
error se vuelva rectificación del pedimento o requerimiento de la autoridad.

## 1 · Lo que hace y lo que NO hace

**Hace:** cruza el pedimento borrador contra los cinco soportes típicos de una importación
(factura, lista de empaque, documento de transporte, COVE y prueba de origen), línea por línea y
totales contra totales (§3–§8), y entrega la matriz de discrepancias clasificadas (§9) con las
preguntas para el agente aduanal (§10).

**NO hace — y lo dice siempre:**

- **No es la prevalidación oficial.** La prevalidación es un servicio que corre la agencia
  aduanal con conectividad ante la autoridad; esta skill revisa coherencia documental, no
  sintaxis ante el sistema de la aduana. Si el usuario cree que esto reemplaza la
  prevalidación, se le corrige una vez y se sigue.
- **No sustituye al agente aduanal, al contador ni al SAT.** Cruza, marca y arma evidencia.
  La clasificación arancelaria definitiva, la valoración y el destino del pedimento los
  resuelve el agente aduanal; el tratamiento fiscal lo resuelve el contador.
- **No decide si una diferencia es una glosa.** Señala el riesgo potencial y cita la regla
  general con su fuente oficial (§11), pero el cálculo y la contestación son del agente
  aduanal y el contador.
- **No inventa reglas ni catálogos.** Toda regla citada viene de una fuente oficial con fecha
  (`references/fuentes.md`). Lo que no tiene fuente se marca como `SIN-FUENTE` y no se usa
  para bloquear.
- **No corrige el pedimento por el usuario.** Marca; quien corrige es el importador con su
  agente aduanal (la Ley prevé rectificar el pedimento ya presentado —art. 89, ver §11—, pero es
  más barato arreglar el borrador).

## 2 · Entradas (6 documentos)

El usuario adjunta los documentos **como los tenga**: PDF, XML, Excel, foto o CSV. La skill
extrae de ellos los campos de la tabla; si falta uno, lo pregunta **una sola vez** y sigue. No
se rechaza un expediente por venir en otro formato, y no se le pide que lo reescriba.

| # | Documento | Qué hay que poder leerle |
|---|---|---|
| 1 | Pedimento borrador | clave/número, aduana, patente, fecha, régimen; por línea: fracción TIGIE, descripción, cantidad + UMT, valor unitario y valor comercial de línea, país de origen, tipo de cambio; al total: incrementables y valor en aduana |
| 2 | Factura comercial | folio, fecha, vendedor, moneda, incoterm y plaza; por línea: referencia/SKU, descripción, cantidad, precio unitario, importe |
| 3 | Lista de empaque | folio; por línea: bultos, tipo de bulto, cantidad, peso neto/bruto por línea o totales |
| 4 | Documento de transporte | folio, tipo, fecha, puerto/aeropuerto, aduana o cruce de ingreso, contenedor o número económico, peso bruto total, bultos, consignatario, quién paga el flete |
| 5 | COVE | folio VUCEM; por línea: descripción, valor unitario, valor total, cantidad, fracción arancelaria, país de origen |
| 6 | Prueba de origen | tipo (certificado de origen, e-CRT, declaración en factura), tratado o esquema, folio, fecha, firmante, líneas amparadas |

Los nombres de archivo que ves en `ejemplos/caso-01-importacion-anden-norte/entrada/` (`.csv`,
`.txt`) son **la forma del ejemplo**, no un requisito de entrada.

Un documento faltante no detiene el cruce: la línea afectada se marca `DOCUMENTO-FALTANTE` y
sale en la matriz como **BLOQUEO** si su ausencia impide verificar un dato del pedimento
(p. ej. origen declarado con preferencia arancelaria y sin prueba de origen).

## 3 · Paso 1 — Identidad y estructura del pedimento

Antes de cruzar nada, verifica que el borrador sea usable:

- **Campos de cabecera presentes:** clave o número de pedimento, aduana de despacho, patente
  de la agencia, fecha de elaboración, régimen aduanero, tipo de cambio. Falta uno →
  `ESTRUCTURA / BLOQUEO` (no hay pedimento que revisar).
- **Fechas coherentes entre documentos:** la factura no puede ser posterior al pedimento
  borrador ni al documento de transporte. Fecha imposible → `ESTRUCTURA / RIESGO`.
- **Consistencia de terceros:** el vendedor de la factura debe coincidir con el exportador del
  COVE y con el shipper del documento de transporte. El consignatario debe ser el importador
  del pedimento. Diferencia → `ESTRUCTURA / RIESGO` (posible compra a intermediario o error de
  consignatario: pregunta al agente aduanal).

## 4 · Paso 2 — Cruce comercial línea por línea: factura ↔ pedimento

Empareja líneas por SKU/referencia o por descripción; documenta el criterio de emparejamiento
en la matriz. Por línea emparejada compara:

| Campo | ¿Qué choca? | Clase |
|---|---|---|
| Cantidad | factura vs cantidad del pedimento | `CANTIDAD` |
| Valor unitario | precio de factura (convertido a la moneda del pedimento con el tipo de cambio declarado) vs valor unitario del pedimento | `VALOR` |
| Valor de línea | importe de factura vs valor comercial de la línea del pedimento | `VALOR` |
| Unidad de medida | unidad comercial de factura vs UMT del pedimento | `CANTIDAD` |
| Fracción arancelaria | fracción declarada vs la que corresponde a la descripción **según la factura** (verificación real: agente aduanal) | `PARTIDA` solo si factura y pedimento se contradicen internamente o contra COVE |
| País de origen | factura/COVE vs pedimento | `ORIGEN` |

Líneas sin pareja: línea de factura sin renglón del pedimento → `CANTIDAD / BLOQUEO` (mercancía
facturada no declarada); línea del pedimento sin factura → `DOCUMENTO-FALTANTE / BLOQUEO`.

Totales: la suma de valores de línea del pedimento debe cuadrar con su valor total; la suma de
importes de factura + incrementables (§7) con el valor aduana del pedimento. Diferencia de
totales sin línea identificable → `VALOR / RIESGO` con la diferencia exacta en cifras.

## 5 · Paso 3 — Cruce físico: factura ↔ lista de empaque ↔ transporte

- **Bultos:** total de bultos de la lista de empaque vs el declarado en el pedimento y vs el
  documento de transporte. Diferencia → `PESO-BULTO / BLOQUEO` (la mercancía que falta o sobra
  físicamente no se arregla en aduana).
- **Pesos:** peso bruto total de la lista de empaque vs peso bruto del documento de transporte.
  Diferencia → `PESO-BULTO / RIESGO` (posible peso de tarima no reportado o bulto omitido).
- **Identidad logística:** contenedor o número económico, tipo de transporte y punto de
  ingreso del documento de transporte vs lo declarado en el pedimento. Diferencia →
  `TRANSPORTE / RIESGO`.

## 6 · Paso 4 — COVE

El COVE (documento electrónico de valor comercial generado en VUCEM) es la declaración de valor
que la autoridad cruza contra la factura. Verifica:

- **Existencia y folio** para la operación; sin COVE → `COVE / BLOQUEO`.
- **Valores:** valor unitario y total por línea del COVE vs factura. Diferencia →
  `COVE / BLOQUEO` (el COVE es un documento electrónico generado en VUCEM, fuente F3; lo que ahí
  se declare es lo que la autoridad cruza contra la factura, así que el arreglo se hace en el
  documento, no a la hora del despacho).
- **Fracción y origen por línea del COVE** vs pedimento. Diferencia → `PARTIDA` / `ORIGEN` con
  nota de que el documento electrónico manda sobre el borrador.

## 7 · Paso 5 — Prueba de origen

Solo aplica si el pedimento reclama **trato arancelario preferencial** por tratado o esquema:

- **Prueba presente:** sin certificado de origen, e-CRT o declaración en factura válida →
  `ORIGEN / BLOQUEO` (la preferencia se pierde y puede haber contribuciones omitidas).
- **Cobertura de líneas:** el certificado ampara las líneas que la usan en el pedimento.
  Línea declarada con preferencia y no amparada → `ORIGEN / BLOQUEO`.
- **Coherencia de datos:** tratado/esquema del certificado vs país de origen declarado, y
  fracción de origen vs fracción del pedimento. Diferencia → `ORIGEN / RIESGO`.
- **Vigencia y formalidades:** folio, fecha y firmante presentes; declaraciones en factura con
  los elementos mínimos exigidos por el esquema. Incompleto → `ORIGEN / RIESGO` con pregunta
  al agente aduanal (los esquemas difieren por tratado: no se dicta la plantilla aquí sin
  fuente, §11).

## 8 · Paso 6 — Incrementables y decrementables (síntesis, no cálculo fiscal)

Compara los incrementables declarados en el pedimento contra lo documentado: flete
internacional y seguros del documento de transporte o póliza; comisiones, regalías o cánones si
aparecen en factura. Regla de trabajo:

- Todo incrementable declarado **sin soporte documental** se marca `INCREMENTABLE / RIESGO` con
  la pregunta al agente aduanal de dónde sale esa cifra.
- Toda cifra documentada (flete del BL, prima del seguro) **no reflejada** en el pedimento se
  marca `INCREMENTABLE / RIESGO`.
- La skill **no decide** si un concepto es o no incrementable: eso es valoración aduanera
  (materia del agente aduanal y, en su caso, del especialista; ver Ley Aduanera arts. 64–65,
  fuente F1). Lo que la skill exige es que **cada peso declarado tenga un documento que lo
  respalde o una pregunta que lo explique**, y que marque el caso contrario: un concepto que la
  factura ya incluye por incoterm (CIP, CIF, DDP) y que además suma el pedimento, porque ahí el
  riesgo es contar dos veces el mismo flete o seguro (art. 65 — F1).

## 9 · Salida 1 — Matriz de discrepancias

Una tabla, un renglón por discrepancia, en este orden (bloqueos primero):

| # | Clase | Severidad | Renglón / línea | Documento A dice | Documento B dice | Diferencia exacta | Impacto probable | Pregunta al agente aduanal |
|---|---|---|---|---|---|---|---|---|

- **Severidades:** `BLOQUEO` (no se puede presentar así; arreglar el documento o el pedimento
  antes), `RIESGO` (se puede presentar pero probablemente genera requerimiento, corrección o
  pérdida de preferencia), `INFO` (diferencia explicable, confirmar y listo).
- **Impacto probable** se redacta con las reglas generales de §11 citando fuente y fecha —
  nunca como veredicto («posible rectificación del pedimento (Ley Aduanera art. 89, F1,
  consultada el 2026-10-29)»).
- Toda discrepancia incluye **la cifra exacta** de cada lado y la diferencia. «No cuadra» sin
  números no es una discrepancia, es una queja.
- Termina con un renglón **«Verificado sin nota»**: lo que sí cuadró y por tanto no se
  pregunta (ahorra las 10 preguntas de rutina al agente).

## 10 · Salida 2 — Preguntas para el agente aduanal

De cada discrepancia que no se resuelve sola (todas las `BLOQUEO` y las `RIESGO` no
explicadas) sale UNA pregunta, en este formato:

> **Q{n} · {clave}:** {pregunta concreta con las dos cifras que chocan}. ¿Se corrige el
> {documento} o el pedimento? ¿Falta soporte de {concepto}?

Reglas: una pregunta por discrepancia; la pregunta lleva sus números (el agente aduanal contesta
mejor con cifras que con adjetivos); nunca se pregunta lo que la documentación ya resuelve;
si la respuesta cambia contribuciones o el tratamiento fiscal, la pregunta se marca
`(fiscal-contador)` para que también lo vea el contador.

## 11 · Reglas generales citadas (con fuente y fecha)

Toda mención de impacto legal usa solo estas reglas, citadas con fuente oficial y fecha de
consulta (detalle en `references/fuentes.md`; Ley Aduanera, texto vigente con la reforma
publicada en el DOF el 19-11-2025, consultado el 2026-10-29 — F1):

- **Corrección del pedimento:** rectificación del pedimento (art. 89 — F1), **sin plazo**: esta
  edición no transcribe del PDF ningún plazo con su párrafo, así que la skill no afirma «60
  días». Lo que sí sostiene es que arreglar el borrador cuesta menos que rectificar el pedimento
  ya presentado.
- **Pago de las contribuciones:** se pagan **al presentar el pedimento** (art. 83 — F1). Si la
  pregunta fiscal toca liquidez, va marcada `(fiscal-contador)`.
- **Riesgo posterior:** **revisión de la autoridad después del despacho**, sin número de
  artículo. Queda `SIN-FUENTE` y **no se usa para bloquear**: se redacta como «posible
  requerimiento», nunca como glosa calculada.
- **Clasificación arancelaria:** la declara el importador con su agente aduanal; puede
  consultarse a la autoridad (art. 47 — F1). La skill solo marca contradicciones documentales.
- **Base gravable e incrementables:** la base gravable es el valor en aduana (art. 64 — F1) y se
  suman incrementables **cuando no estén incluidos en el precio** (art. 65 — F1). De ahí los dos
  marcar: el incrementable sin soporte (§8) y el que la factura ya trae dentro del precio por su
  incoterm y el pedimento vuelve a sumar. Un faltante de valor es diferencia en la **base
  gravable**; a cuánto ascienden las contribuciones omitidas lo calcula el agente o el contador.
- **Documentos que acompañan al pedimento:** factura, documento de transporte y prueba de origen
  (art. 36-A — F1). Por eso son entradas de esta skill (§2), y por eso una línea con preferencia
  y sin prueba que la ampare se marca `ORIGEN / BLOQUEO`: es una regla documental, no un dictamen
  del tratado.

## 12 · Ejecución mínima y orden

1. Recoger los 6 documentos (§2); listar los faltantes.
2. Paso 1 (estructura). Si hay `ESTRUCTURA / BLOQUEO`, parar y entregar solo eso: no tiene
   caso cruzar un pedimento incompleto.
3. Pasos 2 a 6 en orden; dentro de cada paso, bloqueos primero.
4. Armar matriz (§9) y preguntas (§10).
5. Entregar también el renglón «Verificado sin nota» y, si hubo documentos faltantes, la lista
   de qué falta para correr el cruce completo.

Caso completo de entrada y salida esperada, seguible a mano:
`../../ejemplos/caso-01-importacion-anden-norte/` (datos 100 % ficticios de Andén Norte).
