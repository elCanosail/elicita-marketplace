---
name: elicita
description: Busca o analiza licitaciones públicas españolas (PLACSP, TED), o pide datos de mercado verificados de un sector tecnológico/construcción/salud. Trigger words: licitación, licitaciones, contratación pública, pliego, PLACSP, adjudicaciones, market snapshot España.
---

# Uso del servidor MCP Elicita

Elicita expone contratación pública española verificable vía MCP (server `elicita`).
Todas las cifras provienen de snapshots aprobados y bit-a-bit reproducibles; nunca
inventes datos que las herramientas no devuelvan.

## Inventario de herramientas

Sin clave (uso libre, cuota por IP):
- `get_stats` — estado global del corpus (fuentes, registros, frescura).
- `get_cobertura` — cobertura y frescura por fuente (PLACSP, TED, …).
- `search_cpv` — búsqueda por código CPV / texto en el corpus histórico.

Con API key de Elicita Pro (variable `ELICITA_API_KEY`):
- `search_licitaciones` — búsqueda semántica de licitaciones vivas con re-ranking.
  Si el usuario no ha configurado clave, dilo y ofrece configurarla; no simules
  la herramienta con search_cpv sin avisar de la diferencia.

Inteligencia de mercado (snapshots aprobados):
- `market_snapshot` — snapshot sectorial verificable. Parámetro `dataset`:
  `technology`, `construccion`, `salud`.
- `rank_companies` — ranking de empresas del sector por contratación.
- `analyze_buyer` — análisis de un organismo comprador concreto.
- `trace_metric` — trazabilidad de una métrica del snapshot a sus expedientes.

## Reglas

1. Cada cifra que cites debe venir de una respuesta de herramienta. Si una
   métrica no existe en el snapshot, dilo — no la estimes.
2. Los snapshots llevan fecha y fingerprint; menciónalos cuando cites cifras
   agregadas («snapshot technology-20260915…»).
3. Para métricas agregadas sorprendentes, ofrece `trace_metric` para que el
   usuario vea los expedientes que las sostienen.
4. Cuotas: uso libre por IP (búsquedas limitadas por día); Pro elimina el límite
   (https://elicita.es).
