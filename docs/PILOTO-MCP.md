# Piloto MCP autorizado — guía rápida

Guía para usuarios del piloto del conector MCP de Elicita. Si el piloto te lo
ofreció Elicita, esto es todo lo que necesitas.

## Qué obtienes

Acceso programático y desde Claude a la contratación pública española
verificable: **snapshots de mercado bit-a-bit reproducibles** y búsqueda de
licitaciones sobre 13,4M de registros PLACSP.

- **4 datasets de mercado:** `technology`, `construction`, `salud`, `energia`
- **Fuentes:** 16 fuentes públicas, ventana 2022 → 2026

## Conexión

Servidor: `https://elicita.es/mcp` (streamable HTTP, MCP `2026-07-28` y
compatibilidad con clientes previos).

### Desde Claude (recomendado)

```bash
/plugin marketplace add elCanosail/elicita-marketplace
/plugin install elicita@elicita-marketplace
```

La primera llamada abrirá OAuth en el navegador: entra con tu cuenta de
Elicita (plan Pro) y la clave MCP se crea automáticamente.

### Cliente MCP directo

```json
{
  "mcpServers": {
    "elicita": {
      "url": "https://elicita.es/mcp",
      "transport": "streamable-http"
    }
  }
}
```

### HTTP crudo (programático)

```bash
curl -X POST https://elicita.es/mcp \
  -H "Authorization: Bearer elicita_live_TU_CLAVE" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"market_snapshot","arguments":{"dataset":"energia"}}}'
```

## Herramientas

| Herramienta | Auth | Qué hace |
|---|---|---|
| `search_licitaciones` | Pro | Búsqueda de licitaciones por texto/CPV/órgano |
| `market_snapshot` | pública | Snapshot aprobado del mercado por dataset |
| `rank_companies` | pública | Ranking de empresas con cuotas y HHI |
| `analyze_buyer` | pública | Perfil de un comprador público |
| `trace_metric` | pública | Evolución de una métrica por periodo |
| `get_stats` · `get_cobertura` · `search_cpv` | pública | Corpus, fuentes y taxonomía CPV |

Cada snapshot incluye checksums sha256 y metodología versionada: el mismo
resultado es reproducible bit a bit y auditable ante terceros.

## Límites del piloto

- **Pro:** 1.000 consultas/mes, MCP incluido (rate limit 100 tokens, recarga 100/min)
- **Free:** 3 búsquedas/día por IP, sin acceso MCP
- Los snapshots se regeneran tras cada aprobación editorial; los ID antiguos
  siguen resolviéndose (no hay breaking changes de datos)

## Feedback del piloto

Anota en tus pruebas: qué herramienta usaste, si el dato cuadraba con lo que
esperabas y qué te faltó. Envíalo al canal acordado con Elicita — el piloto
decide si abrimos nuevos sectores o subimos límites.
