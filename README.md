# Supertools MCP

Conecta tu asistente de IA a [Supertools Hosting](https://supertools.cl). Le pides
un sitio en lenguaje natural y queda publicado en `tu-nombre.supertools.cl`, sin
salir del editor.

No hay nada que instalar ni que compilar: el servidor MCP corre en Supertools y
se habla por HTTP. Esto es solo la configuración para cada cliente.

## Antes de empezar

1. Necesitas un **plan de Hosting activo** ([precios](https://supertools.cl/precios)).
2. Copia tu API key desde [supertools.cl/dashboard/hosting/mcp](https://supertools.cl/dashboard/hosting/mcp).
3. Déjala en el entorno, así no queda escrita en ningún archivo de configuración:

```bash
echo 'export SUPERTOOLS_API_KEY="tu-key-aqui"' >> ~/.zshrc
source ~/.zshrc
```

## Claude Code

Como plugin:

```
/plugin marketplace add Drozast/supertools-mcp
/plugin install supertools@supertools
```

O en una línea, sin plugin:

```bash
claude mcp add --transport http supertools https://api.supertools.cl/api/v1/mcp/hosting \
  --header "x-api-key: $SUPERTOOLS_API_KEY"
```

## opencode

Copia el bloque `mcp` de [`opencode.json`](opencode.json) a tu `opencode.json`
(del proyecto o `~/.config/opencode/opencode.json`).

## Codex

Copia [`codex-config.toml`](codex-config.toml) a `~/.codex/config.toml`. Usa
`mcp-remote` como puente porque Codex habla MCP por stdio, así que necesitas Node
instalado.

## Claude Desktop / Cursor

Mismo puente que Codex, en `claude_desktop_config.json` o `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "supertools": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://api.supertools.cl/api/v1/mcp/hosting",
               "--header", "x-api-key:${SUPERTOOLS_API_KEY}"]
    }
  }
}
```

## Qué puede hacer

| Herramienta | Qué hace |
|---|---|
| `list_sites` | Lista tus sitios publicados |
| `check_slug` | Avisa si `nombre.supertools.cl` está libre |
| `create_site` | Publica un sitio desde código HTML |
| `deploy_github` | Clona un repo de GitHub y lo publica |
| `get_site` | Estado y URL de un sitio |
| `delete_site` | Borra un sitio (pide `confirm: true`) |

Ejemplo: *"Hazme una landing para mi panadería con los horarios y un mapa, y
publícala en panaderia-lopez.supertools.cl"*.

## Si algo falla

**"API key inválida"** — la variable no llegó al cliente. Comprueba con
`echo $SUPERTOOLS_API_KEY` en la misma terminal desde la que lo abres. En apps de
escritorio (Claude Desktop, Cursor) el entorno del shell no siempre se hereda:
ahí pega la key literal en vez de `${SUPERTOOLS_API_KEY}`.

**"Necesitas un plan de Hosting activo"** — la key es correcta pero la cuenta no
tiene plan vigente.

**El servidor no aparece** — verifica que el endpoint responde:

```bash
curl -s -X POST https://api.supertools.cl/api/v1/mcp/hosting \
  -H 'content-type: application/json' -H "x-api-key: $SUPERTOOLS_API_KEY" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

Debe devolver las 6 herramientas. Si da 401, la key está mala; si no responde,
el problema es de red o del servicio.
