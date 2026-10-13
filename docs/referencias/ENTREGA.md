# Dependencias, validación y entrega web

Esta guía distingue validar, compilar, publicar código y desplegar. Ninguna de esas operaciones demuestra por sí sola las demás. GitHub se rige por [AGENTS.md](../../AGENTS.md).

## 1. Dependencias justificadas y reproducibles

- Preferir SDK/código existente. Para una dependencia nueva, revisar necesidad, mantenimiento, licencia, seguridad y compatibilidad web/runtime; acordar elecciones tecnológicas nuevas con el usuario antes de incorporarlas.
- Versionar manifiestos y resolución: `pubspec.lock` para la aplicación Flutter; en Python, un único flujo de lock o requirements compilados que incluya transitivas.
- Registrar versiones de SDK/runtime del proyecto al crearlo. La versión global de una máquina no es un pin; no añadir FVM u otro gestor si todavía no aporta valor.
- Usar entorno virtual Python y excluirlo de Git. No construir dependencias a partir de `pip freeze` global ni instalar herramientas automáticamente por esta guía.
- Actualizar de forma revisable, comprobar changelog y validar compatibilidad. `pub outdated` informa versiones, no vulnerabilidades; revisar avisos o usar una auditoría apropiada cuando se configure.
- Probar instalación desde los archivos versionados al establecer o cambiar la base del proyecto, no solo en un entorno ya preparado.

## 2. Comprobar según el cambio

| Cambio | Evidencia necesaria |
|---|---|
| Documentación | Enlaces locales, coherencia, alcance y ausencia de información sensible; no ejecutar tests de una app inexistente. |
| Lógica/UI cliente | Formato/análisis y tests del comportamiento afectado; navegador para layout, rutas y APIs web. |
| Backend/API | Lint/formato configurados y tests de casos de uso, payloads y errores; controles de acceso cuando aplique. |
| Persistencia | Tests contra base aislada y migraciones de creación/actualización según [DATOS.md](DATOS.md). |
| Dependencias/build o una entrega web | Resolución/instalación pertinente y build release, además de las pruebas afectadas. |

Mantener tests Dart bajo `frontend/test/` y Python bajo `backend/tests/` cuando esos proyectos existan. Pruebas unitarias sin servicios externos reales; integración con entornos aislados. Elegir un runner compatible antes de afirmar que hay E2E web automatizado.

Al existir Flutter, desde `frontend/`, comandos base de validación:

```powershell
dart format --output=none --set-exit-if-changed lib test
flutter analyze
flutter test
```

Ajustar rutas a directorios existentes y alcance de tests al cambio. Una build release es necesaria para entregar o comprobar cambios de compilación/dependencias; no aporta evidencia a un cambio solo documental.

Para Python, Ruff y pytest son candidatos, no herramientas ya instaladas. Si se adoptan y configuran, desde el backend usar `python -m ruff check .`, `python -m ruff format --check .` y `python -m pytest`. Compilar sintaxis no sustituye pruebas.

No hay CI, gates heredados ni scripts de publicación implementados. Declarar qué pasó, qué falló y qué quedó sin comprobar; no cambiar un resultado fallido por una checklist marcada.

## 3. Compilar y distribuir

Con la aplicación creada, `flutter run -d chrome` abre desarrollo local y no es un gate que termina solo. Desde `frontend/`, `flutter build web --release` produce `build/web/`; no admite `--obfuscate`.

No versionar builds, caches, entornos o dumps por defecto; establecer `.gitignore` antes de generarlos. El backend Python se ejecuta aparte aunque comparta origen/proxy: no se compila dentro de Flutter. Publicar una versión de código no confirma que esté desplegada.

## 4. Antes de producción

- Elegir hosting/dominio y configuración pública/sensible; aplicar [SEGURIDAD.md](SEGURIDAD.md). No presumir un autodeploy por hacer push.
- Si se usan rutas path, el fallback de SPA no debe devolver HTML para una ruta API inexistente. Probar enlaces directos, recarga, requests de API y el flujo principal.
- Definir caché según los assets realmente generados: HTML/arranque actualizables; caché larga solo para recursos correctamente versionados. No asumir que existe service worker o que todo archivo está hasheado.
- No depender de hard reload como actualización de usuarios; comprobar coherencia entre archivos de la misma versión. Soporte offline requiere un requisito y estrategia propios.
- Identificar releases y disponer de versión anterior desplegable; coordinar compatibilidad/recuperación de datos antes de migrar. Rollback de código no revierte necesariamente un esquema.
- Comprobar salud del servicio, logs útiles y un smoke test del flujo real. No afirmar producción validada porque una build terminó bien.
