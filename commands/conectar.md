---
description: Conecta tu cuenta de Supertools para poder publicar sitios web desde acá
argument-hint: "[tu clave de supertools.cl]"
---

El usuario quiere conectar su cuenta de Supertools Hosting.

Clave entregada: `$ARGUMENTS`

Sigue estos pasos:

1. **Si la clave viene vacía**, pídesela y detente hasta que la entregue. Dile
   exactamente dónde está: en https://supertools.cl/dashboard/hosting/mcp, con un
   botón para copiarla. No sigas sin ella y no te la inventes.

2. **Revisa que tenga forma de clave** antes de usarla: son ~56 caracteres sin
   espacios. Si pegó una frase, una URL completa o algo con espacios, avísale y
   pídela de nuevo en vez de intentar conectar.

3. **Comprueba que la clave sirve** antes de guardar nada:

   ```bash
   curl -s -m 30 -X POST "https://api.supertools.cl/api/v1/mcp/hosting?api_key=LA_CLAVE" \
     -H 'content-type: application/json' \
     -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
   ```

   - Si devuelve una lista de herramientas, la clave es buena: sigue.
   - Si devuelve `API key inválida`, dile que la clave está mala o incompleta y que
     la copie de nuevo del panel. No sigas.
   - Si devuelve que necesita un plan de Hosting activo, dile que su cuenta no tiene
     plan vigente y mándalo a https://supertools.cl/precios. No sigas.

4. **Recién ahí, conéctala:**

   ```bash
   claude mcp add --transport http supertools "https://api.supertools.cl/api/v1/mcp/hosting?api_key=LA_CLAVE"
   ```

5. **Cuéntale cómo quedó**, en lenguaje simple y sin jerga técnica:
   - Que tiene que cerrar Claude Code y volver a abrirlo para que tome efecto.
   - Que después puede pedirle cosas con sus palabras, por ejemplo: *"hazme una
     página para mi negocio con los horarios y publícala en mi-negocio.supertools.cl"*.
   - Que su clave quedó guardada en la configuración de Claude Code, así que no
     comparta ese archivo con nadie, y que si se le escapa puede generar una nueva
     desde el panel.

Nunca muestres la clave completa en tu respuesta final: si la mencionas, deja solo
los primeros caracteres.
