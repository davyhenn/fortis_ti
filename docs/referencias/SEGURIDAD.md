# Seguridad aplicada al riesgo real

Reglas de acceso y exposición en un único lugar. Que el producto sea un prototipo no permite publicar datos o acciones restringidas sin protección; no hace falta implementar controles para funciones que aún no existen.

## 1. Desde el primer archivo

- No versionar secretos, datos personales reales, dumps o documentos confidenciales; ejemplos y tests con datos sintéticos. No copiarlos a previews, logs o herramientas externas sin autorización apropiada.
- Configuración sensible solo en backend/gestor de secretos. Un `.env` necesita acceso limitado y exclusión de Git antes de crearlo; `.env.example` solo contiene placeholders seguros.
- Todo lo entregado al navegador es inspeccionable, incluidos `--dart-define`, assets y mapas de fuentes que se sirvan. Minificación no crea confidencialidad.
- Si se expone una credencial, revocarla/rotarla; borrarla de un archivo no elimina el historial. Coordinar cualquier limpieza sin reescribir Git unilateralmente.
- No asumir que repo público significa licencia open source ni que es privado por carecer de licencia. Acordar distribución/licencia y respetar licencias de contenido/dependencias.
- No importar rastreo encubierto, señuelos o promesas de anticopia: no son autorización. La autorización para este GitHub no se extiende automáticamente a otras plataformas.

## 2. Antes de exponer acceso restringido

- Validar identidad con una solución mantenida; derivar actor, roles y pertenencia desde la sesión y datos autorizados del servidor.
- Autorizar cada operación y recurso; ocultar UI, usar IDs opacos o recibir `X-Role` no demuestra permiso.
- No confiar en el usuario de auditoría del body o en el tenant elegido por el cliente. Denegar acceso no concedido, sin defaults de identidad o privilegios.
- Si hay cuentas propias, usar hashing adaptativo mantenido (preferentemente Argon2id con parámetros evaluados), nunca texto plano ni un hash rápido generalista. No implementar criptografía casera.
- Credenciales recuperables de terceros necesitan gestor de secretos o cifrado gestionado y acceso controlado; no una excepción sin protección.
- Limitar intentos en login/recuperación y operaciones expuestas a abuso; evitar revelar si una cuenta existe. Límites locales no cubren automáticamente múltiples instancias.
- Recoger datos personales solo con propósito y retención definidos; no afirmar cumplimiento legal a partir de este documento.

## 3. Sesión, cuando se elija

No imponer JWT ni cookies antes de conocer topología y riesgo; definir expiración, logout y recuperación de acceso si esa función se incorpora.

- Cookie de sesión: `HttpOnly`, `Secure` en HTTPS y `SameSite` apropiado. `SameSite=None` exige `Secure`; operaciones mutantes autenticadas automáticamente por cookies necesitan protección CSRF adecuada.
- Token: validar firma, algoritmos permitidos, expiración y claims pertinentes si se usa JWT; definir almacenamiento y revocación según riesgo. No guardar tokens persistentes en localStorage por comodidad sin evaluar XSS.
- `SameSite` no sustituye toda protección CSRF; `HttpOnly` no elimina todo impacto de XSS. No mezclar mecanismos sin definir su interacción.

## 4. Al desplegar frontend/API

- HTTPS en producción, tipos MIME correctos y `X-Content-Type-Options: nosniff`; no filtrar detalles internos en errores.
- CORS solo cuando se necesite entre orígenes; allowlists separadas por entorno y sin comodín con credenciales. CORS no es autenticación ni protege contra clientes externos al navegador.
- Probar CSP con el build/renderer real de Flutter y sus recursos; no habilitar `unsafe-eval`, comodines o permisos extra indiscriminadamente. Report-only sirve para evaluar antes de aplicar una política que bloquee recursos.
- Restringir framing, referrer y APIs de hardware según uso. HSTS exige HTTPS fiable; `includeSubDomains`/preload requieren comprobar todos los dominios afectados.
- Acotar bodies, consultas y tiempos de ejecución. Confiar en headers de proxy solo tras configurar proxies de confianza.
- Logs mínimos útiles, sin tokens, contraseñas o payloads personales completos. SQL seguro y recuperación están en [DATOS.md](DATOS.md).

## 5. Uploads, si se solicitan

Definir tipos, tamaños y cuotas; validar contenido real, no solo extensión/MIME declarado. Generar nombres seguros, impedir traversal y limitar recursos de procesamiento. Almacenar fuera de rutas ejecutables y autorizar descargas privadas; valorar análisis adicional según riesgo.

Antes de exponer un flujo restringido, comprobar identidad ausente/inválida, permiso insuficiente y acceso a recursos ajenos. Para despliegue, revisar respuestas HTTP reales; tener una regla escrita no prueba que esté implementada. Validación y publicación: [ENTREGA.md](ENTREGA.md).
