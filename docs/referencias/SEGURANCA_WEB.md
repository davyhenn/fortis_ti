# Seguridad web: navegador y superficie HTTP

**Estado:** aplicar según aparezcan frontend desplegado, API, sesiones o uploads. Complementa [SECURITY.md](SECURITY.md); no define una implementación existente.

## 1. HTTPS y headers

- HTTPS en producción para frontend y API; no enviar sesiones por HTTP. El entorno local puede tener una política distinta y explícita.
- `X-Content-Type-Options: nosniff` y tipos MIME correctos.
- Elegir `Referrer-Policy` que minimice filtraciones; evitar datos sensibles en URLs.
- Impedir framing si no hay necesidad de embedding mediante `frame-ancestors` en CSP y, cuando corresponda, `X-Frame-Options` compatible.
- `Permissions-Policy` restringe cámara, micrófono y geolocalización si no se usan; habilitar solo lo requerido por una función aprobada.
- Activar HSTS solo después de comprobar HTTPS y renovación de certificados. `includeSubDomains` o preload requieren verificar todos los dominios afectados; no copiarlos sin análisis.

## 2. CSP compatible con Flutter web

Inventariar scripts, fuentes, conexiones, recursos del renderer y workers del build real. Empezar con una política probada, usar report-only al endurecerla y retirar permisos innecesarios.

No añadir globalmente `unsafe-eval`, `unsafe-inline`, comodines o `blob:` para «hacer funcionar» la app. Si el renderer o WebAssembly necesitan una directiva concreta, justificarla, restringirla y probarla en los navegadores soportados. Una CSP estricta que rompe la app no es un resultado correcto, ni eliminar CSP es la solución por defecto.

## 3. CORS

- Preferir una topología simple. Si frontend/API comparten origen, no añadir CORS amplio sin necesidad.
- Cuando haya orígenes distintos, permitir solo los concretos y necesarios, con métodos/headers adecuados al contrato.
- No combinar credenciales con `Access-Control-Allow-Origin: *`. Separar allowlists de desarrollo y producción.
- CORS es una restricción del navegador, no autenticación ni autorización. No protege contra clientes HTTP externos.
- No confiar en un header de rol o usuario aunque esté permitido por CORS.

## 4. Sesiones y CSRF

El mecanismo está pendiente de decisión. No mezclar cookies y Bearer tokens sin definir su interacción.

- Si hay sesión en cookie: `HttpOnly`, `Secure` en HTTPS y `SameSite` acorde a la topología. No usar `SameSite=None` sin `Secure`.
- Para operaciones mutantes autenticadas automáticamente por cookies, usar protección CSRF adecuada: token y/o comprobaciones de origen según el framework y diseño. `SameSite` es defensa adicional, no una garantía universal.
- No almacenar tokens persistentes en localStorage por comodidad sin evaluar XSS, expiración y logout. `HttpOnly` tampoco elimina todo impacto de XSS.
- Si se elige JWT: validar firma con algoritmos permitidos y claims pertinentes (`exp`, emisor, audiencia); definir rotación, expiración y revocación según riesgo. No heredar HS256, TTL o renovación deslizante del origen.
- Logout y expiración deben tener comportamiento comprobado, no solo borrar un estado visual.

## 5. Límites y abuso

Acotar bodies, paginación, tiempo de ejecución y solicitudes costosas. Aplicar rate limiting antes de publicar login, recuperación o endpoints vulnerables a abuso; respuestas consistentes y sin revelar cuentas existentes.

Un contador in-memory sirve solo con limitaciones documentadas. Si hay varios procesos o instancias, elegir una solución coherente; Redis no es obligatorio sin esa necesidad. No confiar ciegamente en `X-Forwarded-For`: configurar proxies de confianza.

## 6. Uploads, si se solicitan

- Acordar tipos, tamaño y número de archivos con el requisito; no heredar cuotas de fotos de productos.
- Validar contenido real con herramientas adecuadas, no solo extensión o MIME declarado. Para imágenes, evaluar decodificación/reencode seguro y límites de dimensiones/descompresión.
- Generar un nombre seguro en servidor; impedir traversal y no usar la ruta enviada por el cliente.
- Guardar fuera de directorios ejecutables o estáticos públicos por defecto; autorizar descarga de archivos privados.
- Aplicar límites antes de agotar memoria/disco; evaluar antimalware según tipo y riesgo.

**Checklist antes de exposición:** [ ] HTTPS; [ ] headers/CSP probados; [ ] CORS mínimo; [ ] sesión/CSRF definidos; [ ] límites; [ ] uploads seguros si existen; [ ] auth por recurso.

No existen rutas móviles compartidas ni middleware heredado que debamos preservar. Cada control debe ubicarse en el hosting, proxy o backend real y verificarse con respuestas HTTP reales.
