# Pre-revisión de pedimento — borrador contra factura, empaque, transporte, COVE y origen

Skill que recibe el **borrador del pedimento** de una importación y sus cinco soportes típicos
(factura comercial, lista de empaque, documento de transporte, COVE y prueba de origen) y
produce dos archivos:

1. **Matriz de discrepancias** — un renglón por cada choque documental, con la clase
   (`CANTIDAD`, `VALOR`, `PARTIDA`, `ORIGEN`, `PESO-BULTO`, `TRANSPORTE`, `COVE`,
   `INCREMENTABLE`, `ESTRUCTURA`, `DOCUMENTO-FALTANTE`), la severidad (`BLOQUEO` / `RIESGO` /
   `INFO`), la cifra exacta de cada lado y su impacto probable citando fuente oficial con fecha.
2. **Preguntas para el agente aduanal** — una pregunta concreta con sus números por cada
   discrepancia que no se resuelve sola, más las que tocan contribuciones marcadas
   `(fiscal-contador)`.

**La regla central: no es la prevalidación oficial ni sustituye al agente aduanal, al
contador ni al SAT.** La prevalidación la corre la agencia aduanal con conectividad a la
autoridad; esta skill revisa coherencia documental antes de la presentación y arma la
evidencia con la que el importador habla con su agente. No dicta la fracción arancelaria, no
calcula la glosa y no decide el tratamiento fiscal.

## Estructura del paquete

```
pre-revision-pedimento/
├── LICENSE                              Licencia de uso de Heroldo 1.0 (copia exacta)
├── README.md                            este archivo
├── skills/pre-revision-pedimento/
│   └── SKILL.md                         el cerebro (Agent Skill)
├── references/fuentes.md                fuentes oficiales con URL y fecha de consulta
└── ejemplos/
    └── caso-01-importacion-anden-norte/
        ├── entrada/                     los 6 documentos del caso (ficticios)
        └── salida-esperada.md           la matriz y las preguntas, calculadas a mano
```

## Cómo se usa (3 pasos)

1. Carga la skill en tu Claude (o en el agente que uses): copia `skills/pre-revision-pedimento/`
   y `references/` a la carpeta de skills de tu agente.
2. Adjunta el borrador del pedimento y sus soportes **tal como los tienes**: PDF, XML, Excel,
   foto o CSV. No hay que reescribir nada en CSV; la skill extrae los campos y, si le falta
   uno, te lo pregunta una sola vez.
3. Escribe «pre-revisa este pedimento». Recibes la matriz de discrepancias y las preguntas
   listas para copiar al correo del agente aduanal (las marcadas `(fiscal-contador)`, al
   contador).

## Cómo probarla a mano (10 minutos)

1. Lee los 6 archivos de `ejemplos/caso-01-importacion-anden-norte/entrada/`: un borrador de
   pedimento de 3 líneas y sus soportes, con datos 100 % ficticios de Andén Norte.
2. Sigue el pipeline del `SKILL.md` (§3–§8) paso a paso, cruzando con papel y calculadora.
3. Compara contra `salida-esperada.md`: deben salir **7 discrepancias** (5 `BLOQUEO`,
   2 `RIESGO`), **7 preguntas** para el agente aduanal (3 marcadas `fiscal-contador`) y el
   renglón «Verificado sin nota». Toda cifra de la salida sale de los documentos de entrada.

## Origen

Primera ola del catálogo de skills: «pre-revisión de un pedimento borrador contra factura, lista
de empaque, documento de transporte, COVE y prueba de origen: matriz de discrepancias y
preguntas para el agente aduanal». Es el trabajo manual que hoy hace el importador la tarde
anterior a la presentación, antes de que un error se vuelva rectificación del pedimento ya
presentado (Ley Aduanera art. 89, texto vigente consultado el 2026-10-29) o requerimiento de la
autoridad en su revisión posterior al despacho. La prevalidación no es esto: es un servicio
autorizado que cobra la agencia aduanal por pedimento.

---

## Licencia

**Propiedad intelectual de Heroldo Escobedo**, con el código a la vista ([Licencia de uso de Heroldo 1.0](LICENSE)):

- **Uso personal: gratis.** Léelo, pruébalo y adáptalo para aprender o en tus propios datos.
- **Uso en una empresa o con fines de lucro: solo con compra.** Cómpralo en [Agensi](https://agensi.io/skills/pre-revision-pedimento)
  o escribe a heroldo@lolo-enterprises.com. La compra cubre a tu empresa.
- **Siempre se cita la fuente:** «Basado en una skill de Heroldo Escobedo — heroldo.org».
- No se revende ni se vuelve a publicar.

> **In English:** Source-available, not open source. Free for personal use. Business use requires purchase on
> [Agensi](https://agensi.io/skills/pre-revision-pedimento). Attribution to Heroldo Escobedo — heroldo.org is always required.
