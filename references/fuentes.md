# Fuentes oficiales citadas por la skill

Regla de la casa: cada regla legal citada en el SKILL.md sale de una fuente oficial con fecha
de consulta. Si una regla no está aquí, la skill no la usa para bloquear.

**Fecha de consulta de todas las fuentes de este archivo: 2026-10-29.**

## F1 · Ley Aduanera (texto vigente)

- **Qué es:** ley federal que rige el despacho aduanero en México. Artículos usados por la
  skill:
  - **Art. 36-A** — documentos que acompañan al pedimento, entre ellos la factura, el documento
    de transporte y la prueba de origen.
  - **Art. 47** — consulta a la autoridad sobre la clasificación arancelaria.
  - **Art. 64** — base gravable: el valor en aduana de las mercancías.
  - **Art. 65** — incrementables, cuando no estén incluidos en el precio.
  - **Art. 83** — las contribuciones se pagan al presentar el pedimento.
  - **Art. 89** — rectificación de los datos del pedimento.
- **URL:** https://www.diputados.gob.mx/LeyesBiblio/pdf/LAdua.pdf (Cámara de Diputados, texto
  vigente con la última reforma publicada en el DOF el 19-11-2025; consultado el 2026-10-29).
- **Regla de uso:** citar siempre «Ley Aduanera art. X (texto vigente en diputados.gob.mx,
  consultado el 2026-10-29)», solo de los artículos de la lista de arriba.
- **Límite de esta fuente:** aquí no hay plazos ni sanciones. Esta edición no transcribe del PDF
  ningún párrafo con plazo o multa, así que la skill **no** menciona «60 días», «5 días» ni
  artículos de sanción. Lo que la autoridad pueda revisar después del despacho se nombra como
  riesgo, `SIN-FUENTE`, y no sirve para bloquear.

## F2 · Reglas Generales de Comercio Exterior (RGCE)

- **Qué es:** reglas administrativas de la SHCP/ANAM que operativizan la Ley Aduanera: cómo se
  presentan los pedimentos y los documentos electrónicos de comercio exterior. Se publican en el
  DOF por anualidad, cada una con su propia fecha.
- **Qué usa la skill:** solo que los documentos electrónicos de comercio exterior —incluido el
  COVE— se generan en VUCEM (F3) y que el importador los obtiene de su propia cuenta. **No se
  cita regla ni capítulo concreto**: en la consulta de esta corrida (2026-10-29) no se localizó
  la regla exacta con su anualidad y fecha de publicación en el DOF, y sin ese dato no se afirma.
- **URL:** https://www.dof.gob.mx/ (Diario Oficial de la Federación; buscar «Reglas Generales
  de Comercio Exterior» para la edición vigente, que es la que hay que nombrar con año y fecha si
  algún día se cita una regla concreta).

## F3 · VUCEM — Ventanilla Única / COVE

- **Qué es:** plataforma oficial donde se generan los documentos electrónicos de comercio
  exterior, incluido el Documento Electrónico de Valor Comercial (COVE).
- **URL:** https://www.ventanillaunica.gob.mx
- **Uso en la skill:** el usuario obtiene el folio y el contenido del COVE de su propia cuenta
  VUCEM; la skill lo cruza, no lo genera.

## F4 · TIGIE — Tarifa de la Ley de los Impuestos Generales de Importación y de Exportación

- **Qué es:** el catálogo oficial de fracciones arancelarias y sus gravámenes.
- **URL:** https://www.dof.gob.mx/ (buscar «LIGIE» / «Tarifa» para el texto vigente).
- **Uso en la skill:** la skill **no** consulta la TIGIE para dictar la fracción correcta
  (eso es del agente aduanal, art. 47 de la Ley Aduanera, F1); solo marca cuando dos documentos
  del usuario se contradicen entre sí sobre una misma línea.

## F5 · ANAM — Administración General de Aduanas

- **Qué es:** la autoridad aduanera; publica los lineamientos de operación y los servicios
  oficiales de automatización y prevalidación que corren a través de las agencias aduanales
  autorizadas.
- **URL:** https://anam.gob.mx
- **Uso en la skill:** sustento del límite declarado en §1: la prevalidación es un servicio
  autorizado que cobra la agencia aduanal por pedimento, con conectividad a la autoridad; esta
  skill no la reemplaza.

## Nota de límites

- Ninguna fuente de este archivo valida datos de un caso concreto: validan **reglas
  generales**. El caso de ejemplo usa datos 100 % ficticios de Andén Norte (empresa ficticia
  del parque) y un proveedor ficticio de Estados Unidos.
- Las fechas de vigencia de tratados, esquemas de origen y porcentajes de gravamen cambian: la
  skill no los guarda; cada corrida cita la fuente vigente con su fecha de consulta.
