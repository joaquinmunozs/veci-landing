# Veci — Landing "Vota por tu edificio" 🏢💚

Landing pública de captación de residentes, en vivo en **https://veci.condorai.cl**.

- **Diseño**: creado por Max (la landing `/vota` del monorepo Veci), portado aquí a
  HTML/CSS/JS estático 1:1 (frame celular 9:16, tarjeta verde expandible,
  contadores live, pantalla de éxito con link para compartir).
- **Backend**: el formulario guarda en la BD real de Veci vía
  `POST https://veci-api-e0px.onrender.com/api/v1/community-requests`
  (validación estricta + rate limit 5/min + honeypot + geocoding del edificio).
  El staff ve las solicitudes con mapa en `veci-web.vercel.app/staff/solicitudes`.
- **Conteo social**: `GET /community-requests/count` (público, solo un número).
- **Hosting**: GitHub Pages, rama `main`, dominio custom `veci.condorai.cl`
  (CNAME en este repo + registro CNAME `veci → joaquinmunozs.github.io` en el
  Cloudflare de condorai.cl). HTTPS forzado. Cada push a `main` publica solo.

## Estructura

```
index.html        → toda la landing (autocontenida: HTML+CSS+JS inline)
assets/max/       → assets del diseño de Max (optimizados: 7.2MB → ~475KB)
assets/           → logos oficiales de Veci (versión anterior de la landing)
CNAME             → dominio custom (no borrar: GitHub Pages lo necesita)
```

## Seguridad del formulario

- SQL injection: imposible por diseño — el backend usa Prisma (queries
  parametrizadas), nunca SQL crudo.
- Anti-bot: honeypot oculto (`website`) + rechazo de envíos <1.5s + throttle
  5/min por IP en la API.
- El endpoint público SOLO escribe; las lecturas requieren login de staff.
- Plan B: si el POST falla, el voto se guarda en localStorage del dispositivo.

## Para editar

Editar `index.html`, verificar con Chrome headless (screenshots), commit y push:

```bash
git add -A && git commit -m "..." && git push origin main
```
