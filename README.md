# Elicita para Claude — marketplace de plugins

[![Verified MCP handshake](https://img.shields.io/badge/MCP-v2%20streamable--http-brightgreen)](https://elicita.es/mcp-v2)

Plugin de [Elicita](https://elicita.es) para Claude: contratación pública española
verificable para agentes — búsqueda de licitaciones, estadísticas del corpus,
cobertura de fuentes y **snapshots de mercado bit-a-bit reproducibles**
(tecnología, construcción, salud).

Servidor MCP en producción: `https://elicita.es/mcp-v2` (streamable HTTP,
`elicita-mcp-sidecar 2.0.0`).

## Instalación

### Claude Code

```bash
/plugin marketplace add elCanosail/elicita-marketplace
/plugin install elicita@elicita-marketplace
```

### Claude Desktop / Cowork

Añade el marketplace desde este repositorio de GitHub (Ajustes → Extensiones →
Marketplaces → añadir desde repo) e instala el plugin `elicita`.

## Herramientas

| Herramienta | Auth | Qué da |
| --- | --- | --- |
| `search_licitaciones` | API key Pro | Búsqueda semántica de licitaciones vivas con re-ranking |
| `search_cpv` | libre | Búsqueda por CPV/texto en el corpus histórico |
| `get_stats` | libre | Estado del corpus (fuentes, registros, frescura) |
| `get_cobertura` | libre | Cobertura y frescura por fuente |
| `market_snapshot` | libre | Snapshot sectorial aprobado (`dataset`: technology, construccion, salud) |
| `rank_companies` | libre | Ranking de empresas por contratación |
| `analyze_buyer` | libre | Análisis de un organismo comprador |
| `trace_metric` | libre | Trazabilidad de una métrica a sus expedientes |

## Clave Pro (opcional)

El uso libre tiene cuota por IP. Para quitar límites, hazte Pro en
https://elicita.es y añade tu propio servidor con la clave:

```bash
claude mcp add elicita-pro --transport http https://elicita.es/mcp-v2 \
  --header "Authorization: Bearer TU_API_KEY"
```

## Verificación (2026-09-23)

- `initialize` → 200, `elicita-mcp-sidecar 2.0.0`, protocolo `2025-06-18`.
- `tools/call get_cobertura` sin clave → datos live (16 fuentes, PLACSP
  13.419.448 registros, última ejecución del pipeline 2026-09-23T20:12Z).

## Nota operativa

El edge de Cloudflare de elicita.es bloquea User-Agents de herramientas
(curl, wget, python-requests…) en todo el sitio **salvo `/mcp-v2`**, que
tiene una excepción explícita en la regla de firewall desde el 2026-09-23.
Cualquier cliente MCP funciona con independencia de su User-Agent. Si
recibes 403 HTML, comprueba que apuntas a `/mcp-v2` — no es una caída
del servicio.
