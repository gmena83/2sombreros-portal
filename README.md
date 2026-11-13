# Portal de Auspicios B2B - "2 Sombreros para Impactar B2B" (humanB)

Sitio estatico de la propuesta de auspicio corporativo para la primera edicion del manual de campo
**"2 Sombreros para Impactar B2B: la Compra y la Venta"** de Luis Felipe Meneses Faundez / humanB.cl.

Dominio destino: **2sombreros.com** (registrado en NameBright).

## Estructura del repositorio

| Ruta | Que es |
| :--- | :--- |
| `dist/index.html` | **Pagina en produccion** (version final del portal). Es lo que se publica. |
| `netlify.toml` | Configuracion de Netlify: sitio estatico, carpeta publicada `dist/`, sin build. |
| `portal_de_auspicios_b2b_libro_2_sombreros_edici_n_y_lanzamiento.html` | Variante de diseno anterior (respaldo). |
| `gemini-code-1794506701829.html` | Variante de diseno anterior (respaldo). |
| `WhatsApp Image 2026-11-12 ...jpeg` | Imagen de referencia del libro (respaldo). |

No requiere compilacion ni dependencias: es HTML con Tailwind via CDN.

## Despliegue en Netlify (recomendado)

1. Ingresar a https://app.netlify.com e iniciar sesion (o crear cuenta gratuita).
2. **Add new site -> Import an existing project -> Deploy with GitHub** y autorizar el acceso a este repositorio.
3. Ajustes de build (Netlify los toma del `netlify.toml`, verificar que queden asi):
   - **Build command:** vacio
   - **Publish directory:** `dist`
4. **Deploy site.** En segundos queda disponible una URL temporal tipo `https://nombre-aleatorio.netlify.app`.
5. **Site configuration -> Domain management -> Add a domain** e ingresar `2sombreros.com`.
6. Netlify mostrara las instrucciones de DNS exactas. Seguir una de las dos opciones de la seccion siguiente.
7. El certificado SSL (HTTPS) se emite automatico (Let's Encrypt) una vez que el DNS apunta a Netlify.

Alternativa sin Git: arrastrar la carpeta `dist/` a https://app.netlify.com/drop.

## Configuracion DNS en NameBright

El dominio esta en **NameBright** con sus nameservers por defecto
(`ns1.namebrightdns.com`, `ns2.namebrightdns.com`). Elegir UNA de estas dos opciones.

### Opcion A - Delegar DNS a Netlify (mas simple, resuelve apex + www + SSL)

1. En Netlify: **Domain management -> Add domain -> 2sombreros.com -> usar Netlify DNS.**
2. Netlify entrega 4 nameservers (ejemplo: `dns1.p01.nsone.net` ... `dns4.p01.nsone.net`).
3. En el panel de NameBright del dominio, en **Nameservers / DNS**, reemplazar
   `ns1.namebrightdns.com` y `ns2.namebrightdns.com` por los 4 nameservers que entrego Netlify.
4. Guardar. La propagacion suele tardar entre 1 y 24 horas.

### Opcion B - Mantener el DNS de NameBright (crear registros manuales)

En el editor de DNS de NameBright, crear estos registros (reemplazar `NOMBRE-DEL-SITIO` por el
subdominio real que entrego Netlify, p.ej. `2sombreros-portal.netlify.app`):

| Tipo | Host / Nombre | Valor | TTL |
| :--- | :--- | :--- | :--- |
| A | `@` (o `2sombreros.com`) | `75.2.60.5` | 3600 |
| CNAME | `www` | `NOMBRE-DEL-SITIO.netlify.app` | 3600 |

Notas:
- El registro apex (`@`) debe ser **A** apuntando a la IP del balanceador de Netlify `75.2.60.5`
  (un dominio raiz no admite CNAME).
- `www` va como **CNAME** al subdominio `.netlify.app` del sitio.
- Luego, en Netlify, agregar tanto `2sombreros.com` como `www.2sombreros.com` como dominios del sitio
  y definir cual es el principal.

## Verificacion

- DNS: `dig 2sombreros.com +short` debe devolver `75.2.60.5` (Opcion B) o resolver via Netlify (Opcion A).
- HTTPS: al cargar `https://2sombreros.com` el candado debe aparecer sin advertencias.

---
Mantenido por Menatech (Gonzalo Mena) para humanB. Contacto del autor: luisfelipe@humanb.cl
