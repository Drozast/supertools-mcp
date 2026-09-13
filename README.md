# Supertools MCP

Conecta [Supertools Hosting](https://supertools.cl) a tu asistente de IA. Le pides
una página con tus palabras y queda publicada en `tu-nombre.supertools.cl`, sin
subir archivos ni configurar nada más.

> **El panel arma todo por ti.** Entra a
> [supertools.cl/dashboard/hosting/mcp](https://supertools.cl/dashboard/hosting/mcp),
> elige tu asistente y copia lo que aparece: ya viene con tu clave adentro. Este
> README es lo mismo, por si prefieres leerlo acá.

Necesitas un plan de Hosting activo ([precios](https://supertools.cl/precios)) y tu
clave personal, que está en esa misma página del panel.

---

## Claude Code

Un comando en la terminal, reemplazando `TU-CLAVE`:

```bash
claude mcp add --transport http supertools \
  "https://api.supertools.cl/api/v1/mcp/hosting?api_key=TU-CLAVE"
```

Cierra Claude Code y vuelve a abrirlo. Listo.

## Claude Desktop

En Claude: **Configuración → Desarrollador → Editar configuración**. Se abre un
archivo; pega esto dentro y guarda:

```json
{
  "mcpServers": {
    "supertools": {
      "command": "npx",
      "args": ["-y", "mcp-remote",
               "https://api.supertools.cl/api/v1/mcp/hosting?api_key=TU-CLAVE"]
    }
  }
}
```

Necesita [Node](https://nodejs.org) instalado. Cierra Claude y vuelve a abrirlo.

## Cursor

**Settings → MCP → Add new MCP server**, y pega el mismo bloque de Claude Desktop.

## opencode

En tu `opencode.json`:

```json
{
  "mcp": {
    "supertools": {
      "type": "remote",
      "url": "https://api.supertools.cl/api/v1/mcp/hosting?api_key=TU-CLAVE",
      "enabled": true
    }
  }
}
```

## Codex

En `~/.codex/config.toml`:

```toml
[mcp_servers.supertools]
command = "npx"
args = ["-y", "mcp-remote",
        "https://api.supertools.cl/api/v1/mcp/hosting?api_key=TU-CLAVE"]
```

---

## Cuida tu clave

La clave va dentro de la dirección que pegaste, así que **ese texto es tu llave**:
quien lo tenga puede crear y borrar sitios en tu cuenta. No lo subas a GitHub ni lo
pegues en un chat público.

Si se te escapó, entra al panel y genera una nueva — la anterior deja de funcionar
al instante y solo tienes que volver a pegar la configuración.

## Qué le puedes pedir

> "Hazme una página para mi panadería con los horarios y el teléfono, y publícala en
> panaderia-lopez.supertools.cl"

> "¿Qué sitios tengo publicados?"

> "Cambia el teléfono de mi página y vuelve a publicarla"

Por dentro son seis herramientas: ver tus sitios, revisar si un nombre está libre,
publicar una página, traer un proyecto desde GitHub, consultar un sitio y borrarlo.
Borrar siempre pide confirmación.

## Si algo no funciona

**"API key inválida"** — la clave quedó mal pegada. Fíjate que no hayas dejado
`TU-CLAVE` ni un espacio de más, y que esté completa.

**"Necesitas un plan de Hosting activo"** — la clave está bien, pero la cuenta no
tiene plan vigente.

**No aparece el asistente o dice que no puede conectar** — casi siempre es que
faltó cerrar y volver a abrir la app. Si sigue, comprueba que el servicio responde:

```bash
curl -s -X POST "https://api.supertools.cl/api/v1/mcp/hosting?api_key=TU-CLAVE" \
  -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

Tiene que devolver una lista con seis herramientas. Si da 401, la clave está mala.

---

## Plugin de Claude Code

Si prefieres no copiar comandos a mano, instala el plugin y él te conecta:

```
/plugin marketplace add Drozast/supertools-mcp
/plugin install supertools@supertools
```

Después, dentro de Claude Code:

```
/supertools:conectar TU-CLAVE
```

Comprueba que la clave sirva antes de guardarla y te avisa en castellano si está
mala o si te falta plan. Cierra Claude Code, ábrelo de nuevo y listo.

El plugin además le enseña a Claude a publicar bien: que el sitio se vea en
teléfono, que no invente tu teléfono ni tus horarios, y que te pida confirmación
antes de borrar algo.
