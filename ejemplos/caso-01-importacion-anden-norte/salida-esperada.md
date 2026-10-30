# Salida esperada — caso 01, importación Andén Norte (ficticio)

Resultado de correr el pipeline del SKILL.md sobre `entrada/`. Todo sale de los documentos de
entrada y se puede verificar con calculadora. **Moneda del caso:** en un pedimento real el valor
en aduana se expresa en pesos; el caso usa USD para simplificar los cruces (tipo de cambio
ficticio 18.50 del propio borrador; no afecta los cruces porque todos los documentos están en
USD).

Los impactos legales citan solo los artículos de `references/fuentes.md` F1, con su fecha de
consulta. Donde no hay fuente transcrita se dice el riesgo **sin número de artículo**.

## Documentos faltantes

- **Póliza o certificado de seguro:** el pedimento declara seguro de 85.00 USD como
  incrementable y no hay documento de soporte en el expediente (alimenta D5, y choca con el
  incoterm CIP de la factura: ver D7). La ausencia no detiene el cruce: la línea afectada sale
  marcada y con pregunta.

## Matriz de discrepancias (7: 5 BLOQUEO, 2 RIESGO)

| # | Clase | Severidad | Renglón | Documento A dice | Documento B dice | Diferencia exacta | Impacto probable | Pregunta al agente aduanal |
|---|---|---|---|---|---|---|---|---|
| D1 | CANTIDAD | BLOQUEO | L3 · AN-9033 manómetro | Factura: 200 pzas · Pedimento: 200 pzas | Lista de empaque: 180 pzas | 20 pzas de menos embarcadas | Si el pedimento se presenta con 200 y llegan 180, hay que rectificar el pedimento (Ley Aduanera art. 89, F1, consultada el 2026-10-29); además lo puede ver la autoridad en su revisión después del despacho (sin artículo: SIN-FUENTE, no bloquea por sí solo) | Q1 |
| D2 | VALOR | BLOQUEO | L2 · AN-2210 sello mecánico | Factura y COVE: 8.50 USD/pza | Pedimento: 8.05 USD/pza | 0.45 USD/pza × 300 = 135.00 USD | Valor en aduana declarado 12,650.00 vs 12,785.00: **135.00 USD de valor en aduana subdeclarado (base gravable, art. 64, F1)**. Las contribuciones omitidas son un porcentaje de esa cifra y las calcula el agente aduanal o el contador, no esta skill. Rectificación si ya se presenta (art. 89, F1) | Q2 (fiscal-contador) |
| D3 | ORIGEN | BLOQUEO | L3 · AN-9033 manómetro | Pedimento: trato T-MEC en la línea | Certificado e-CRT MXUS-2026-11830: ampara solo L1 y L2 | Línea completa (3,000.00 USD) sin prueba de origen | Sin prueba que ampare la línea se pierde la preferencia: la línea 3 paga **impuesto general de importación (arancel)** sin trato preferencial; el monto lo calcula el agente o el contador. La prueba de origen es documento que acompaña al pedimento (art. 36-A, F1) | Q3 (fiscal-contador) |
| D4 | PESO-BULTO | BLOQUEO | Totales | Lista de empaque: 118 cajas / 1,260 kg bruto | BOL: 120 cartons / 1,240 kg bruto · Factura: 120 cajas | **2 cajas menos pero 20 kg más en la lista de empaque que en el BOL: los pesos no se explican por el faltante de D1.** 20 manómetros pesan ≈26.7 kg brutos (240 kg ÷ 180 pzas × 20), no 20 kg | Bultos distintos entre lista de empaque (118) y BOL (120): BLOQUEO por la regla de §5 (la mercancía que falta o sobra físicamente no se arregla en aduana). El peso por sí solo sería RIESGO: diferencia documental de peso/bultos: posible requerimiento de la autoridad y retardo en el reconocimiento aduanero (sin artículo: SIN-FUENTE) | Q4 |
| D7 | VALOR | BLOQUEO | Incrementables / totales | Factura: **CIP Nuevo Laredo** → el vendedor paga flete y seguro, van dentro del precio | BOL: flete **collect** 1,150.00 USD · Pedimento: suma flete 1,150.00 + seguro 85.00 = 1,235.00 como incrementables | Los dos documentos se contradicen sobre quién pagó el flete; el pedimento toma la posición del BOL | **Contradicción documental verificable:** si el flete y el seguro ya van en el precio CIP, no se suman (art. 65, F1) y el valor en aduana queda inflado en 1,235.00 USD; si van aparte, la factura CIP está mal redactada y hay que corregirla. No es «no verificable»: chocan factura, BOL y pedimento entre sí. Ligado con D5 (el mismo seguro, sin póliza) | Q7 (fiscal-contador) |
| D5 | INCREMENTABLE | RIESGO | Incrementables | Pedimento: seguro 85.00 USD | Expediente: sin póliza ni certificado de seguro · Factura: incoterm CIP (el vendedor paga el seguro) | 85.00 USD sin soporte documental | El incrementable se suma **cuando no está incluido en el precio** (art. 65, F1); si la factura es CIP, el seguro ya va en el precio y no se suma. Y sí va aparte, hace falta el documento que lo respalde (art. 36-A, F1). Se rectifica antes de presentar (art. 89, F1). Ligado con D7 | Q5 |
| D6 | ESTRUCTURA | RIESGO | Cabecera / terceros | Pedimento: importador «Andén Norte, S. de R.L.» | BOL: consignatario «Anden Norte Logistics S.A. de C.V.» | Razón social distinta en consignatario | Posible requerimiento por consignatario distinto al importador (sin artículo: la identidad la resuelve el agente con la naviera/transportista antes del arribo) | Q6 |

Cuentas de la matriz (para verificar a mano):

- L2: 300 × 8.50 = 2,550.00 (factura/COVE) · 300 × 8.05 = 2,415.00 (pedimento) ·
  2,550.00 − 2,415.00 = **135.00** (es base gravable, no impuesto).
- Totales: 6,000.00 + 2,415.00 + 3,000.00 = **11,415.00** (mercancía en el pedimento) ·
  6,000.00 + 2,550.00 + 3,000.00 = **11,550.00** (factura y COVE) ·
  11,415.00 + 1,235.00 = **12,650.00** (valor en aduana declarado) ·
  11,550.00 + 1,235.00 = **12,785.00** (valor en aduana esperado por precio) ·
  12,785.00 − 12,650.00 = **135.00** (la D2 sola explica toda la diferencia de mercancía).
- Incrementables: 1,150.00 + 85.00 = **1,235.00** sumados por el pedimento. Con factura **CIP**,
  flete y seguro ya van en el precio → ese 1,235.00 no debió sumarse (art. 65, F1): valor en
  aduana potencialmente inflado en **1,235.00 USD** (la D7).
- Empaque: 60 + 36 + 22 = **118 cajas** · 640 + 380 + 240 = **1,260 kg** brutos ·
  BOL 120 cartones − 118 = **2 cajas menos** · 1,260 − 1,240 = **20 kg más** (la empaque pesa
  más con 2 cajas menos: la dirección es contraria a D1).
- Peso unitario L3: 240 kg ÷ 180 pzas = 1.3333 kg/pza · × 20 pzas = **26.67 kg** (lo que
  deberían pesar las 20 piezas que faltan, no 20 kg).

## Preguntas para el agente aduanal (7)

> **Q1 (D1):** En la línea 3 el pedimento y la factura declaran 200 manómetros y la lista de
> empaque trae 180 (faltan 20). ¿El proveedor facturó de más o embarcó de menos? ¿Se corrige la
> factura/COVE o el pedimento antes de presentar?
>
> **Q2 (D2) (fiscal-contador):** Línea 2: factura y COVE dicen 8.50 USD/pza y el pedimento
> declara 8.05 (diferencia de 135.00 USD en el valor en aduana, base gravable del art. 64).
> ¿Es error de captura del borrador? ¿Se rectifica el pedimento (art. 89) y a cuánto asciende el
> faltante de contribuciones sobre esa base — lo confirma el contador para el IGI/IVA/DTA?
>
> **Q3 (D3) (fiscal-contador):** El pedimento reclama T-MEC en la línea 3 pero el e-CRT
> MXUS-2026-11830 ampara solo las líneas 1 y 2. ¿El proveedor agrega la línea 3 al certificado o
> se importa esa línea sin preferencia? Si va sin preferencia, ¿cuánto impuesto general de
> importación extra paga la línea y cómo lo ve el contador?
>
> **Q4 (D4):** Lista de empaque: 118 cajas / 1,260 kg; BOL: 120 cartones / 1,240 kg; factura:
> 120 cajas. La empaque trae 2 cajas menos y 20 kg más: 20 manómetros pesarían ≈26.7 kg, así que
> el faltante de D1 no cierra los pesos. ¿Cuál documento manda para bultos y peso en el pedimento
> y cuál va en los anexos?
>
> **Q5 (D5):** El pedimento incrementa 85.00 USD de seguro, no hay póliza en el expediente y la
> factura es CIP (el seguro la paga el vendedor). ¿Nos falta anexar una póliza propia, o el
> seguro ya está en el precio y hay que quitar el incrementable?
>
> **Q6 (D6):** El BOL consigna a «Anden Norte Logistics S.A. de C.V.» y el pedimento importa
> como «Andén Norte, S. de R.L.». ¿Son la misma empresa (filial/consignataria) o hay que
> corregir el consignatario con el transportista antes del arribo?
>
> **Q7 (D7) (fiscal-contador):** La factura es **CIP Nuevo Laredo** —el vendedor paga flete y
> seguro, van en el precio— y el BOL dice flete **collect** 1,150.00 USD, que el pedimento suma
> como incrementable junto con los 85.00 de seguro. ¿Cuál de los dos es verdad: el flete ya está
> en el precio (y el valor en aduana está inflado en 1,235.00 USD) o la factura trae mal el
> incoterm? ¿Lo corrige el proveedor en la factura o lo quitamos del pedimento? ¿Lo confirma el
> contador?

## Verificado sin nota (no se pregunta)

- **Cabecera (§3):** número y clave de pedimento, aduana 240, patente 3991, fecha 2026-10-30, régimen 01 y tipo de cambio 18.50 presentes; fechas coherentes (factura 2026-10-18 ≤ BOL 2026-10-22 ≤ pedimento 2026-10-30).
- **L1 (válvula):** cantidad 500, precio 12.00 y valor 6,000.00 cuadran en factura, pedimento
  y COVE; fracción 8481.80.99 idéntica en pedimento y COVE.
- **L2:** la cantidad (300) sí cuadra entre factura, empaque, COVE y pedimento; solo el precio
  difiere (D2).
- **L3:** el valor (3,000.00) cuadra; su problema es de cantidad física (D1) y de origen (D3).
- **Bultos L1–L2:** 60 + 36 = 96 cajas consistentes entre factura y empaque.
- **Tipo de bulto y UMT:** «caja» en empaque y factura, «pza» consistente en pedimento y COVE.
- **País de origen:** USA consistente en factura, COVE y pedimento en las tres líneas.
- **Aduana y cruce:** 240 Nuevo Laredo en el pedimento coincide con el cruce del BOL (antes
  había un Matamoros suelto en el transporte; ya no se contradice).

## Qué sigue después de esta salida (fuera de la skill)

El importador resuelve Q1–Q7 con su agente aduanal (y el contador para Q2, Q3 y Q7), corrige el
borrador y los documentos que correspondan, y recién entonces la agencia aduanal corre la
**prevalidación** (servicio autorizado que corre la agencia con conectividad ante la autoridad).
Esta skill ya entregó su parte: discrepancias con evidencia y preguntas con números.
