# Propiedad intelectual y publicación responsable

**Estado:** licencia y política de distribución pendientes de decisión del titular. Al preparar las guías, el repositorio de destino era público.

## 1. No asumir licencia o privacidad

- Público no significa open source ni autoriza cualquier reutilización; una licencia aplicable debe decidirse y registrarse expresamente.
- No añadir `LICENSE`, declarar «privado/propietario» o colocar SPDX propietario en cada archivo sin autorización y coherencia con la licencia real.
- Verificar licencias de paquetes, fuentes, iconos, imágenes y contenido externo antes de distribuirlos.
- Los documentos originales se usan como referencia autorizada para esta adaptación. No se copian íntegros al repo público ni se publican sus detalles operativos.
- Si se quieren incorporar fragmentos externos con obligaciones de atribución/licencia, resolverlas antes de publicarlos; una reescritura no elimina por sí sola obligaciones sobre material protegido.

## 2. Límites de una aplicación web

Todo JavaScript, WebAssembly, asset o configuración entregado al navegador puede inspeccionarse. Minificar no cifra ni vuelve incopiable el código; el build web no admite `--obfuscate`.

Mantener en servidor secretos, autorizaciones y reglas que deban ser autoritativas. Algunas validaciones de UX y cálculos de presentación pueden estar en cliente sin fingir que constituyen un secreto.

Revisar mapas de fuentes y diagnósticos antes de servirlos públicamente; si se necesitan para observabilidad, elegir distribución privada/controlada. No asumir su presencia o ausencia sin inspeccionar el build real.

## 3. Contenido y herramientas externas

- No publicar ni enviar a servicios adicionales datos personales, credenciales, dumps o documentos confidenciales sin autorización adecuada.
- La autorización del usuario para trabajar y publicar en este GitHub no implica autorización universal para otras plataformas.
- Usar ejemplos sintéticos en guías y previews. Un identificador de build público nunca es una credencial.
- El historial Git ayuda a seguir cambios y autoría; no demuestra por sí solo titularidad legal. Commits firmados se evalúan si hay una necesidad concreta.

## 4. Medidas que no incorporamos

No crear rutas camufladas, honeytokens, instrucciones para agentes embebidas en bundles ni rastreo encubierto para «anticopia». No son un control de acceso y añaden operación, falsos positivos y consideraciones de privacidad.

Si más adelante se necesita licenciamiento o detección de abuso, diseñarlo como una función explícita, proporcional y aprobada, con alcance y política de datos. No insertar trampas ni prometer protección absoluta.

## 5. Si se decide una licencia

Actualizar este documento, `ALCANCE.md` y los archivos legales pertinentes. Si se exige SPDX, usar la sintaxis del lenguaje (`//` en Dart, `#` en Python, `--` en SQL) y el identificador correcto, no una cabecera copiada indiscriminadamente.

**Checklist:** [ ] visibilidad comprendida; [ ] licencia decidida antes de distribuir bajo ella; [ ] dependencias/recursos revisados; [ ] sin contenido confidencial; [ ] límites web honestos; [ ] sin rastreo encubierto.

Referencias: [SECURITY.md](SECURITY.md), [DEPENDENCIAS.md](DEPENDENCIAS.md), [DEPLOY.md](DEPLOY.md).
