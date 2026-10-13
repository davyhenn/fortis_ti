# Seguridad: reglas transversales

**Estado:** activa desde el primer archivo. Las medidas ligadas a cuentas, datos o producción se implementan antes de exponer ese riesgo, no después de un incidente.

## 1. Secretos y repositorio público

- No poner contraseñas, tokens, claves privadas, cookies de sesión, dumps o datos personales reales en código, Markdown, previews, tests, logs o commits.
- La configuración sensible vive en variables del backend o un gestor de secretos. Un `.env` local es un mecanismo de configuración, no una caja fuerte; limitar acceso.
- Antes de crear archivos sensibles o generar artefactos, añadir las exclusiones pertinentes al `.gitignore`. Hoy estas guías no crean `.env` ni entornos virtuales.
- Un `.env.example` contiene únicamente nombres y valores ficticios seguros. No copiar el catálogo de credenciales del otro producto.
- Todo dato enviado al cliente web, incluidos `--dart-define` y URLs de API, es público. Ninguna ofuscación cambia esto.
- Si se expone una credencial, revocarla/rotarla primero; quitarla de un archivo no la borra del historial. Coordinar cualquier limpieza de historia, sin force push unilateral.

## 2. Identidad y autorización

> [!IMPORTANT]
> El servidor determina quién actúa y qué puede hacer. Ocultar un botón no es autorización.

- Validar la sesión o token con un mecanismo mantenido; no implementar criptografía casera.
- Derivar actor, roles y pertenencia del contexto autenticado y de datos autorizados del servidor.
- No confiar en `X-Role`, permisos en localStorage, un usuario de auditoría enviado en el body o el tenant seleccionado en la UI.
- Verificar permiso para la operación y para el recurso concreto en cada lectura/escritura restringida. Denegar por defecto lo que requiera acceso no concedido.
- Probar ausencia de sesión, falta de permiso y acceso a recursos de otro usuario/organización cuando aplique.
- No inventar una identidad, organización o privilegio como fallback. Devolver el error del [contrato](API_CONTRACT.md).

## 3. Contraseñas y credenciales recuperables

- Si se implementan cuentas propias, usar hashing adaptativo con una librería mantenida; preferir Argon2id y parámetros evaluados para el entorno. Bcrypt puede requerir una decisión de compatibilidad y límites explícitos.
- Nunca comparar o almacenar contraseñas de usuario en texto plano ni usar hash rápido generalista como sustituto.
- No registrar contraseñas ni incluirlas en respuestas. Diseñar recuperación y expiración de tokens antes de habilitar ese flujo.
- Credenciales para servicios externos, que deben recuperarse, se protegen con un gestor de secretos o cifrado gestionado y control de acceso. Necesitar recuperarlas no justifica almacenamiento sin protección.
- No añadir una conversión silenciosa de credenciales legadas: aquí no hay usuarios legados que migrar.

## 4. Datos, errores y auditoría

- SQL parametrizado y privilegio mínimo; controles HTTP en [SEGURANCA_WEB.md](SEGURANCA_WEB.md).
- Rechazar datos inválidos sin truncar ni simular éxito. Un vacío válido se comunica como tal.
- Registrar eventos relevantes con actor validado y correlación, evitando payloads sensibles completos. Sanear entradas para no permitir inyección de logs.
- Recoger solo los datos personales necesarios; definir propósito, acceso, retención y eliminación cuando se concrete el producto.
- No afirmar cumplimiento legal por tener un checklist. Las obligaciones dependen de usuarios, jurisdicción y tratamiento reales.

## 5. Antes de publicar cambios

Revisar diff y archivos nuevos; asegurar que las exclusiones necesarias existen antes de stage. No subir builds, entornos o datasets reales por accidente. Se pueden incorporar herramientas de secret scanning cuando se configure CI; aún no hay un gate automático.

**Checklist:** [ ] sin secretos/datos reales; [ ] configuración sensible solo servidor; [ ] actor validado; [ ] autorización por recurso; [ ] entradas y errores seguros; [ ] logs mínimos; [ ] pruebas negativas pertinentes.

**Descartado del origen:** nombres de sistemas y credenciales, roles privilegiados universales, endpoints congelados inexistentes y reglas de negocio de otras empresas.
