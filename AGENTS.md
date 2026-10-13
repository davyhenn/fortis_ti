# Guía de trabajo de Fortis TI

## 1. Alcance y estado real

- Stack confirmado por el usuario: Flutter y Dart para el frontend web; Python para el backend.
- Repositorio: `https://github.com/davyhenn/fortis_ti.git`. Rama de trabajo y publicación: `master`.
- Este repositorio comienza con documentación. Todavía no hay aplicación, API, base de datos ni despliegue configurados.
- Framework Python, base de datos, autenticación, hosting y funcionalidades del producto siguen pendientes de definición. No convertir recomendaciones en decisiones ya tomadas.
- La documentación y la comunicación se mantienen en español. Los identificadores técnicos nuevos se escriben en inglés.
- Las guías son adaptaciones propias, no instrucciones para operar la infraestructura del proyecto de origen.

## 2. Cómo consultar las referencias

Leer primero [ALCANCE.md](docs/referencias/ALCANCE.md). Después consultar las guías relacionadas con la tarea; no es necesario cargar todas para un cambio pequeño.

| Archivo | Cuándo consultarlo |
|---|---|
| [FLUTTER.md](docs/referencias/FLUTTER.md) | Pantallas, widgets, navegación, estado y compatibilidad web. |
| [DART.md](docs/referencias/DART.md) | Modelos, lógica pura, tipado, asincronía y clientes HTTP. |
| [PYTHON.md](docs/referencias/PYTHON.md) | Estructura del backend, validaciones, errores y pruebas. |
| [API_CONTRACT.md](docs/referencias/API_CONTRACT.md) | Endpoints, payloads, respuestas y cambios de contrato. |
| [SECURITY.md](docs/referencias/SECURITY.md) | Secretos, identidad, autorización, privacidad y publicación. |
| [SEGURANCA_WEB.md](docs/referencias/SEGURANCA_WEB.md) | HTTP, CORS, cookies, CSRF, CSP, límites y uploads. |
| [DATABASE.md](docs/referencias/DATABASE.md) | Elección de persistencia, modelado, consultas y transacciones. |
| [MIGRATIONS.md](docs/referencias/MIGRATIONS.md) | Cambios de esquema, seeds, compatibilidad y recuperación. |
| [DEPENDENCIAS.md](docs/referencias/DEPENDENCIAS.md) | Incorporación de paquetes, versiones, locks y auditorías. |
| [MODULO.md](docs/referencias/MODULO.md) | Incorporación de una funcionalidad de extremo a extremo. |
| [PLUGINS.md](docs/referencias/PLUGINS.md) | Integraciones externas; el framework de plugins está diferido. |
| [DESIGN.md](docs/referencias/DESIGN.md) | Propuestas visuales, aprobación, previews y pruebas de UI. |
| [DESIGN_SYSTEM.md](docs/referencias/DESIGN_SYSTEM.md) | Tokens, tema, componentes y coherencia visual. |
| [IDIOMAS.md](docs/referencias/IDIOMAS.md) | Textos, localización y formatos de fechas/números. |
| [DEPLOY.md](docs/referencias/DEPLOY.md) | Validación, build web, publicación en GitHub y futuro despliegue. |
| [PROPRIEDADE_INTELECTUAL.md](docs/referencias/PROPRIEDADE_INTELECTUAL.md) | Licencias, contenido público y límites de protección del cliente web. |
| [IOS_MOBILE.md](docs/referencias/IOS_MOBILE.md) | Límites del alcance web y condiciones para evaluar una app nativa futura. |

## 3. Reglas transversales

1. Inspeccionar los archivos existentes antes de editar. No borrar, sobrescribir ni incluir en un commit trabajo ajeno sin autorización.
2. Implementar solo el alcance solicitado. No instalar frameworks, crear infraestructura o añadir paquetes por anticipación.
3. Separar UI, lógica de cliente, acceso HTTP y reglas autoritativas del servidor. El navegador no conecta directamente a una base de datos.
4. El backend valida los datos y determina identidad y permisos. No confiar en roles, usuarios de auditoría o permisos declarados por el cliente.
5. No inventar datos, entidades, conexiones o autorizaciones para esconder un error. Los estados vacíos válidos no son errores.
6. No incluir secretos ni datos personales reales en código, documentación, tests, previews, bundles, commits o logs.
7. Reutilizar lo que exista; dividir por responsabilidad. Los tamaños recomendados no son gates automáticos implementados.
8. Mantener soporte web, teclado, texto escalado y estados de carga, vacío y error desde la primera pantalla.
9. Usar el tema para estilos compartidos. La identidad visual inicial debe acordarse con el usuario; no heredar una paleta o galería inexistentes.
10. Cambiar el contrato y sus consumidores juntos; documentar la compatibilidad cuando haya consumidores desplegados.

La autoridad transversal de seguridad es `SECURITY.md`; el contrato HTTP se define en `API_CONTRACT.md`; la aprobación visual en `DESIGN.md`; los tokens en `DESIGN_SYSTEM.md`. Si dos guías se contradicen, resolverlo y actualizarlas antes de continuar, sin ignorar las instrucciones del usuario ni las de la herramienta.

## 4. Estructura prevista, no scaffold ya creado

Cuando se solicite iniciar la aplicación, la propuesta mínima es:

```text
frontend/          # proyecto Flutter: lib/, test/, web/
backend/           # proyecto Python y sus tests
docs/referencias/  # guías mantenidas
scratch/           # previews aprobados como parte de una tarea
```

Las carpetas de runtime se crean solo cuando sean necesarias. Los ejemplos de las guías no prueban que una clase, comando, endpoint o herramienta exista.

## 5. Validación proporcional al cambio

- Solo documentación: revisar enlaces locales, coherencia, alcance y ausencia de información sensible. No fingir que se ejecutaron tests de una aplicación inexistente.
- Flutter, cuando exista `frontend/pubspec.yaml`: desde `frontend/`, ejecutar formato, `flutter analyze`, tests pertinentes y `flutter build web --release` para cambios que afecten al cliente web.
- Python, cuando exista backend y estén aprobadas sus herramientas: revisar formato/lint y ejecutar las pruebas pertinentes desde `backend/`. Usar el entorno virtual del proyecto.
- Cambios de API o datos: añadir pruebas de contrato, autorización y migraciones según corresponda.
- Registrar qué se comprobó y qué no pudo comprobarse. No afirmar que una regla documental está automatizada si no existe su implementación.

No existen `cccp.sh`, gates numerados, baselines heredadas, CI, FVM ni scripts de despliegue en este estado inicial. No invocarlos ni crearlos para imitar las referencias originales.

## 6. GitHub: publicación directa en master

El usuario ha autorizado publicar los cambios de este proyecto directamente en `master`.

1. Revisar `git status`, rama y diff antes de preparar el commit.
2. Incluir explícitamente solo los archivos de la tarea. Comprobar secretos, datos reales y artefactos generados.
3. Validar lo que corresponda; no ocultar fallos con un mensaje de éxito.
4. Crear un commit descriptivo y ejecutar `git push origin master` al completar una unidad de trabajo, salvo que el usuario pida no publicarla.
5. No usar force push ni reescribir historia remota. Si existe trabajo remoto divergente, inspeccionarlo antes de integrar.
6. Informar resultado y commit. Si el push falla, decirlo; no confundir un commit local con una publicación.

GitHub contiene código y documentación, no las instalaciones locales ni los entornos virtuales. Un push no equivale a desplegar la aplicación.

## 7. Mantenimiento de estas guías

Al aprobar una decisión, actualizar `ALCANCE.md` y las referencias afectadas en el mismo cambio. Mantener cada tema en su documento, usar enlaces relativos y retirar instrucciones obsoletas. Conservar el índice de esta raíz sincronizado con los archivos existentes.
