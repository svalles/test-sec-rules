# test-sec-rules

Sitio web mínimo para desplegar en **Cloudflare Pages** y probar reglas de seguridad de Cloudflare desde el exterior.

## Contenido

- `public/index.html`: página estática simple para laboratorio de pruebas.

## Cómo hostearlo en Cloudflare Pages

1. Entra en Cloudflare Dashboard → **Workers & Pages** → **Create application** → **Pages**.
2. Elige **Connect to Git** (este repositorio) o **Direct Upload**.
3. Configuración de build:
   - **Framework preset**: `None`
   - **Build command**: *(vacío)*
   - **Build output directory**: `public`
4. Haz deploy.
5. Obtendrás una URL tipo `https://<project>.pages.dev`.

## Pruebas de security rules

Una vez desplegado, crea reglas en Cloudflare (WAF / Custom Rules / Rate Limiting) y prueba con peticiones externas:

```bash
# SQLi ejemplo
curl "https://<project>.pages.dev/api/search?q=' OR 1=1 --"

# XSS ejemplo
curl "https://<project>.pages.dev/login?user=%3Cscript%3Ealert(1)%3C/script%3E"

# fuerza bruta/rate limiting (ejemplo)
for i in {1..50}; do curl -s -o /dev/null "https://<project>.pages.dev/login"; done
```

## Nota de seguridad

Úsalo solo en un entorno de pruebas que controles. No ejecutes pruebas de ataque sobre sistemas sin autorización.
