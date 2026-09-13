---
name: publicar-sitio
description: Publica y administra sitios web en Supertools Hosting. Úsala cuando el usuario quiera crear, publicar, subir, actualizar, listar o borrar una página o sitio web en Supertools, o cuando pida "publícalo en supertools" o mencione un subdominio .supertools.cl.
---

# Publicar sitios en Supertools

Las herramientas `supertools` publican sitios reales en internet, en vivo y al
instante. Hay gente pagando por esto: trátalas con ese cuidado.

## Antes de publicar

**Elige bien el nombre.** El sitio queda en `<slug>.supertools.cl`. El slug lleva
solo minúsculas, números y guiones. Propón uno a partir del negocio del usuario
(`panaderia-lopez`, no `sitio1`) y **siempre pasa antes por `check_slug`**: si está
ocupado, ofrece dos o tres alternativas en vez de elegir tú por él.

**Escribe el HTML completo.** `create_site` recibe un documento entero, no un
fragmento: `<!doctype html>`, `<head>` con `<meta charset="utf-8">`, `<title>` y
`<meta name="viewport" content="width=device-width, initial-scale=1">`. Sin eso se
ve mal en teléfono, que es donde lo van a mirar casi todos.

Va todo en un archivo: CSS dentro de `<style>`, nada de recursos externos que
puedan no cargar. Si necesitas imágenes, usa colores, degradados o SVG en línea
antes que enlazar fotos de otro sitio.

**Escribe en el idioma del usuario** y con los datos que te dio. Si te faltan el
teléfono, la dirección o los horarios, pregúntale en vez de inventarlos: son datos
que van a quedar publicados y que sus clientes van a usar para llamarlo.

## Publicar

- `create_site` para una página escrita por ti.
- `deploy_github` para traer un repositorio ya hecho. Necesita un `index.html` en
  la raíz; si el repo no lo tiene, el sitio va a responder 404 aunque el despliegue
  diga que salió bien. Avísale antes.

Después de publicar, dale la URL completa (`https://...`) y dile que ya está en
vivo, que puede abrirla y compartirla.

## Cambios

No hay "editar": para cambiar una página se vuelve a publicar. Si el usuario pide
un ajuste, parte del HTML que ya escribiste, cámbialo y publica de nuevo sobre el
mismo slug. Si no tienes el HTML a mano en la conversación, pídeselo o reescríbelo
confirmando antes los datos, para no perder contenido que él haya puesto.

## Borrar

`delete_site` es definitivo y no se recupera. Pide confirmación explícita mostrando
qué sitio se va a borrar, y solo entonces manda `confirm: true`. Nunca borres algo
que el usuario no nombró: si pidió borrar "la prueba" y hay varias, muéstrale
`list_sites` y que elija.

## Cuando algo falla

- **"Necesitas un plan de Hosting activo"** — la cuenta no tiene plan vigente.
  Mándalo a https://supertools.cl/precios, no reintentes.
- **"API key inválida"** — la conexión quedó mal configurada. Que revise su clave
  en https://supertools.cl/dashboard/hosting/mcp.
- **El slug ya existe** — ofrece alternativas, no le agregues números al azar.

Traduce siempre el error a algo que se entienda. El usuario no sabe qué es una API
key ni un slug: háblale de "tu clave" y "el nombre de tu página".
