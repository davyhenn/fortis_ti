# Dependencias y reproducibilidad

**Estado:** política activa; no se incorporan paquetes por crear estas guías.

## 1. Antes de añadir un paquete

1. Explicar qué necesidad concreta resuelve y por qué no basta el SDK o una solución existente.
2. Revisar mantenimiento, licencia, soporte web/Python, versiones compatibles, dependencias transitivas y superficie de riesgo.
3. Pedir aprobación si no formaba parte de una elección ya autorizada por el usuario.
4. Incorporar configuración, lock y prueba de uso en el mismo cambio. No instalar globalmente herramientas del proyecto por costumbre.

No elegir un paquete solo porque aparecía en las referencias originales. Tampoco actualizar un lock sin revisar qué cambia.

## 2. Flutter/Dart

- Al existir una aplicación, versionar `pubspec.yaml` y `pubspec.lock` para reproducir su resolución de paquetes.
- Declarar constraints de SDK y dependencias compatibles; rangos en el manifiesto y lock resuelto no son contradictorios.
- Registrar la versión de Flutter elegida para el proyecto. Evaluar FVM o el pin de CI cuando aporte reproducibilidad; no hay `.fvmrc` actualmente.
- `flutter pub outdated` informa versiones disponibles, **no** demuestra ausencia de vulnerabilidades.
- Revisar avisos de seguridad del ecosistema y una herramienta que soporte el lock/formato real si se incorpora auditoría.
- Validar actualizaciones con analyzer, tests y build web; probar los paquetes que usan navegador.

## 3. Python

- Usar un entorno virtual por proyecto y comprobar soporte real de la versión de Python.
- Elegir un único flujo reproducible: por ejemplo manifiesto más lock con una herramienta aprobada, o requirements compilados con versiones exactas de directas y transitivas. No imponer a la vez varios gestores.
- Separar dependencias runtime y de desarrollo según el mecanismo elegido.
- No tratar `pip freeze` de un entorno global como definición del proyecto.
- Evaluar `pip-audit` u otra herramienta mantenida antes de producción; no está instalada/configurada por esta guía. Un escáner no sustituye revisión de riesgo y licencia.
- Probar instalación desde cero usando los archivos versionados, no solo el entorno ya caliente de una máquina.

## 4. Actualizaciones

- Mantener actualizaciones pequeñas y con motivo: seguridad, compatibilidad o necesidad funcional.
- Revisar changelog y cambios de comportamiento; documentar breaking changes y plan de recuperación.
- No ejecutar `flutter upgrade` o upgrades amplios que cambien la base del proyecto sin decisión.
- Cuando haya CI, ejecutar validaciones con versiones conocidas y permisos mínimos. Revisar supply chain de acciones externas y fijar revisiones verificables cuando corresponda.
- Dependabot u otra automatización se evalúan al existir manifiestos y CI. No generar PRs y configuración para ecosistemas todavía vacíos, ni asumir una política de PR que contradiga trabajo directo en `master`.

**Checklist:** [ ] necesidad aprobada; [ ] compatibilidad/licencia; [ ] resolución reproducible; [ ] cambios revisados; [ ] instalación limpia; [ ] tests/build; [ ] vulnerabilidades evaluadas.

Referencias: [DEPLOY.md](DEPLOY.md), [PROPRIEDADE_INTELECTUAL.md](PROPRIEDADE_INTELECTUAL.md), [SECURITY.md](SECURITY.md).
