# Validación, build, GitHub y despliegue web

**Estado:** publicación de documentación en GitHub activa. Build y despliegue de aplicación pendientes porque no hay código ni hosting elegido.

## 1. Separar cuatro operaciones

1. **Validar:** formato, análisis, tests y comprobaciones relevantes.
2. **Compilar:** producir artefactos del frontend web.
3. **Publicar código:** commit y push a `master` en GitHub.
4. **Desplegar:** servir frontend y ejecutar backend en un entorno configurado.

Un push no confirma que haya un servidor actualizado. No hay autodeploy, CCCP, Coolify, Docker o CI establecidos por estas guías.

## 2. Desarrollo y build futuros

Al existir `frontend/`, desde esa carpeta:

```powershell
flutter pub get
flutter analyze
flutter test
flutter run -d chrome
```

`flutter run` es una sesión de desarrollo, no un gate que finaliza por sí solo. Para generar un artefacto de publicación:

```powershell
flutter build web --release
```

El resultado está en `frontend/build/web/`. No usar `--obfuscate`: no es una opción del build web. No versionar ese bundle por defecto; definir una vía de distribución/CI cuando haya hosting. La configuración compilada del cliente es pública.

El backend Python corre como servicio separado aunque comparta dominio o un reverse proxy. No se compila «dentro» de Flutter ni basta abrir un HTML local para probar integración.

## 3. Primera elección de hosting

- Comparar hosting estático + API o mismo origen detrás de proxy; no asumir que Python debe servir todos los assets.
- Elegir dominio, HTTPS, proceso backend, configuración de secretos y orígenes permitidos.
- Si se usan rutas path, configurar fallback de SPA solo para navegación de frontend; una ruta API inexistente debe seguir devolviendo un error API, no `index.html`.
- Probar URLs directas, refresco de páginas, requests de API y compatibilidad de navegador.
- Definir health/readiness adecuados al servicio, logs sanitizados y forma de detectar fallos sin exponer datos internos.
- Si hay base de datos, coordinar migraciones, compatibilidad y recuperación antes de desplegar.

## 4. Caché y releases

- Definir headers según assets reales: HTML/arranque deben poder detectar una versión nueva; cachear largo solo recursos con versionado o fingerprint adecuado.
- No suponer que todos los archivos generados están hasheados ni que existe un service worker. Verificar el output del SDK y el hosting.
- No agregar soporte offline o un service worker por defecto; requieren estrategia de actualización y datos.
- Hard reload es una herramienta de diagnóstico, no una política de actualización para usuarios.
- Usar artefactos y una revisión identificables; distribuir de forma coherente y probar que no se mezclen archivos de versiones distintas.
- Conservar una versión anterior desplegable y definir rollback. Un rollback de código puede ser incompatible con una migración destructiva.

## 5. GitHub ahora

El usuario autorizó publicar directamente en `master`. Seguir [AGENTS.md](../../AGENTS.md): revisar estado/diff, stage explícito de archivos de la tarea, commit descriptivo y `git push origin master`; nunca force push.

No incluir credenciales, `.venv`, caches, dumps, builds ni instalaciones locales. Si aparecen cambios remotos inesperados, inspeccionarlos antes de integrar. Si el push falla, informarlo sin afirmar publicación.

Cuando exista CI, elegir SDK/runtime reproducibles y validar antes de cualquier deploy. No fijar FVM o un proveedor por copiar un script ajeno.

**Checklist:** [ ] validación real; [ ] artefacto web si aplica; [ ] diff seguro; [ ] push confirmado; [ ] hosting explícito si existe; [ ] HTTPS/cache/rutas; [ ] smoke test y rollback antes de producción.
