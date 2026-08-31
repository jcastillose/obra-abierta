# obra-abierta

Tablero diario de ofertas de empleo en construcción en Chile, publicado en
**https://jcastillose.github.io/obra-abierta/**

Una tarea programada busca ofertas una vez al día, verifica cada aviso abriendo su
enlace y reescribe `data.json`. La página lee ese archivo cada vez que alguien la
abre, así que la dirección nunca cambia y siempre muestra la última búsqueda.

## Archivos

| Archivo | Qué contiene |
| --- | --- |
| `index.html` | El tablero. Estático, sin dependencias salvo las tipografías de Google Fonts. |
| `data.json` | Ofertas vigentes, el perfil buscado y la fecha de la última actualización. Es lo único que cambia a diario. |
| `runs.json` | Bitácora de cada ejecución: fuentes revisadas, cuántas ofertas nuevas y cerradas, qué se descartó y por qué. No lo descarga la página. |
| `robots.txt` | Pide a los buscadores que no indexen el sitio. La página es accesible por enlace, pero no aparece en resultados de búsqueda. |

## Perfil buscado

Ingeniero Civil en Obras Civiles e Ingeniero Constructor, más de 10 años en
construcción, unos 6 como administrador de obra en edificación en altura. Diplomado
en Coordinación BIM. Base en Santiago, disponible para todo Chile.

Cargos objetivo, por prioridad: administrador de obra o de contrato; jefe de oficina
técnica y jefe de terreno senior; coordinador BIM; y cargos de ascenso (jefe de
proyecto, gerente de obra o de construcción).

## Criterios

- **Se incluye**: edificación (habitacional, oficinas, retail), industrial liviano
  (plantas, bodegas, logística), obras civiles sin exigencia de experiencia vial o
  hidráulica previa, y coordinación BIM. Entre 3 y 12 años de experiencia exigidos.
  Cualquier región de Chile, advirtiendo relocalización y sistema de turnos.
- **Se excluye**: minería en todas sus formas, prácticas y cargos junior,
  "administrativo de obra" (cargo contable), inspección técnica de obra (ITO),
  avisos de más de 30 días, empresas no identificables y avisos con texto plantilla
  repetido entre empresas distintas.

## Estructura de `data.json`

```jsonc
{
  "updated": "YYYY-MM-DD HH:MM",   // hora de Santiago
  "profile": "…",                  // texto bajo el título; no lo modifica la tarea
  "jobs": [
    {
      "id": "empresa-cargo-ciudad",  // estable, evita duplicados
      "title": "…", "company": "…", "location": "…", "region": "…",
      "sector": "edificacion | industrial | obras-civiles | otro",
      "salary": "texto del aviso o null",   // nunca estimada
      "source": "…", "url": "…",
      "first_seen": "YYYY-MM-DD", "last_seen": "YYYY-MM-DD",
      "status": "new | active | closed",
      "note": "por qué encaja y qué advertir"
    }
  ]
}
```

Una oferta es `new` solo el día en que se detecta; luego pasa a `active`. Si deja de
aparecer y su enlace ya no abre, se marca `closed`. A los 30 días se retira.
