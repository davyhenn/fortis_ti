# Alcance y análisis de las referencias

Adaptación inicial: 2026-10-13. Estas guías parten de 17 documentos aportados por el usuario y se han reescrito para Fortis TI. Se conserva su organización práctica: reglas, responsabilidades, referencias cruzadas y checklists; no se importa su infraestructura ni su estado de producción.

## 1. Base confirmada

- Flutter/Dart para una aplicación web y Python para el backend.
- Desarrollo local en Windows; el navegador es el destino inicial, también en pantallas pequeñas.
- Cambios publicados en `davyhenn/fortis_ti`, directamente en `master`.
- Al revisar el entorno funcionaban Flutter 3.47.7, Dart 3.13.5 y Python 3.13.14. Son una observación local, no un pin de proyecto ni una garantía de compatibilidad de paquetes futuros.
- El repositorio remoto era público al preparar estas guías. No cambiar su visibilidad sin decisión del usuario.
- Todavía no existe código de aplicación. Ninguna referencia a un componente, servicio o script del origen significa que exista aquí.

## 2. Decisiones pendientes

| Decisión | Cuándo resolverla | Criterio |
|---|---|---|
| Primer flujo y usuarios del producto | Antes de crear módulos | Necesidad real, datos y criterios de aceptación. |
| Framework Python | Al iniciar la API | Complejidad, validación, documentación y experiencia del equipo; FastAPI es una opción, no una elección. |
| Persistencia y motor | Cuando un flujo necesite guardar datos | Relaciones, concurrencia, operación y recuperación; no copiar PostgreSQL por costumbre. |
| Identidad y sesiones | Antes de publicar funciones restringidas | Riesgo, topología de dominios, proveedor de identidad y experiencia de usuario. |
| Hosting, dominio y CI | Antes del primer despliegue | HTTPS, configuración, logs, coste y rollback. |
| Identidad visual e idiomas adicionales | Antes de la primera UI y al definir usuarios | Diseño aprobado; español es la base provisional, no portugués heredado. |
| Licencia y visibilidad | Antes de distribuir código o incorporar contenido con restricciones | Decisión del titular; un repositorio público no equivale a una licencia open source. |

## 3. Análisis documento por documento

Los nombres originales se conservan para facilitar la trazabilidad; el contenido de las versiones locales está en español.

| Referencia adaptada | Qué aprovechamos | Qué descartamos o posponemos |
|---|---|---|
| [FLUTTER.md](FLUTTER.md) | Separación de UI, reutilización, tema, accesibilidad, web y pruebas. | Inventario de widgets ajenos, excepciones de módulos y gates inexistentes. |
| [DART.md](DART.md) | Modelos tipados, lógica pura y clientes HTTP aislados. | Allowlists de legado; no degradar tipos a `dynamic` para eludir null safety. |
| [PYTHON.md](PYTHON.md) | API fina, servicios, errores explícitos y pruebas. | FastAPI obligatorio, nombres de tablas heredadas y límites de líneas como bloqueos automáticos. |
| [API_CONTRACT.md](API_CONTRACT.md) | Contratos estables, errores estructurados y paginación consistente. | Endpoints del ecosistema ajeno y actor de auditoría tomado del body. |
| [SECURITY.md](SECURITY.md) | Secretos fuera del código, SQL seguro y autorización del servidor. | Confianza en `X-Role`, datos operativos y migración silenciosa de contraseñas de otro sistema. |
| [SEGURANCA_WEB.md](SEGURANCA_WEB.md) | CORS preciso, CSRF según sesión, CSP, límites y uploads seguros. | Congelación de rutas móviles inexistentes, HS256/TTL obligatorios y cuotas de negocio sin requisito. |
| [DATABASE.md](DATABASE.md) | Claves estables, restricciones, transacciones y fuente de esquema versionada. | Servidores, FDW, ERP, catálogos fiscales, PDV y obligación de guardar todo enum en tablas. |
| [MIGRATIONS.md](MIGRATIONS.md) | Cambios versionados, un propósito y pruebas de actualización. | Borrar historia consolidada y considerar `IF NOT EXISTS` una validación del esquema. |
| [DEPENDENCIAS.md](DEPENDENCIAS.md) | Justificación, reproducibilidad y auditoría de paquetes. | FVM y automatizaciones antes de existir proyecto; `pub outdated` no es un escáner de vulnerabilidades. |
| [MODULO.md](MODULO.md) | Navegación integrada y entrega vertical pequeña. | Una pestaña/endpoint de prueba por obligación y contratos basados en mapas sin tipar. |
| [PLUGINS.md](PLUGINS.md) | Aislar adaptadores y definir propiedad de datos. | Registry, launcher, scheduler y ejecución por threads daemon como arquitectura inicial. |
| [DESIGN.md](DESIGN.md) | Aprobar cambios visuales importantes, ordenar previews y tests. | Tres variantes y galería obligatorias para cada cambio menor. |
| [DESIGN_SYSTEM.md](DESIGN_SYSTEM.md) | Tokens, estados, jerarquía y componentes consistentes. | Paleta y medidas rígidas heredadas; prohibir Material aunque esté correctamente tematizado. |
| [IDIOMAS.md](IDIOMAS.md) | Inglés técnico y textos de UI centralizados. | Portugués por defecto y traducción a tres idiomas sin usuarios que lo necesiten. |
| [DEPLOY.md](DEPLOY.md) | Separar build, publicación y despliegue; prever caché y rollback. | Servidores, Coolify y script CCCP ajenos; subir bundles compilados como política inicial. |
| [PROPRIEDADE_INTELECTUAL.md](PROPRIEDADE_INTELECTUAL.md) | Lógica sensible en servidor, licencias y publicación consciente. | Licencia propietaria asumida, señuelos para IA, rutas camufladas y promesas de anticopia web. |
| [IOS_MOBILE.md](IOS_MOBILE.md) | Responsive y tipado sirven también para web. | Todo build nativo, Xcode, SPM, CocoaPods y publicación en tiendas. |

## 4. Correcciones importantes tras la segunda revisión

1. **Identidad y privilegios:** un header de rol o un nombre de usuario del body no demuestra quién actúa. El backend deriva ambos de una sesión validada y aplica autorización por recurso.
2. **Credenciales:** las contraseñas de usuarios se hashean; las credenciales recuperables de terceros necesitan un gestor de secretos o cifrado gestionado, no una excepción de texto plano sin protección.
3. **Ausencia de datos:** conservar `null`, vacío o error según el contrato. No reemplazar indiscriminadamente por cadenas vacías, entidades ficticias o truncamientos silenciosos.
4. **Migraciones:** el registro de versiones y las pruebas de esquema son la base. La idempotencia de un seed no demuestra la corrección de una migración.
5. **Dart:** conservar tipos nullable reales. Un cast a `dynamic` oculta un problema en lugar de arreglarlo.
6. **Web:** no usar `--obfuscate` en `flutter build web`; minificación y ocultar mapas de fuentes no hacen secreto el cliente.
7. **Seguridad HTTP:** HSTS, CSP, cookies y CORS se ajustan a dominios y build reales; no copiar configuraciones globales sin probarlas.
8. **Calidad:** no declarar gates, tests o CI activos cuando solo hay documentación. Diferenciar intención, implementación y evidencia.

## 5. Prioridades y disparadores

### Ahora

Mantener estas guías; definir el primer flujo; acordar diseño inicial; al crear código, aplicar separación de responsabilidades, contratos tipados, pruebas proporcionadas y protección de secretos. No instalar dependencias por completar una lista.

### Antes de una primera entrega web funcional

Elegir framework, transporte HTTP y estado mínimos; integrar un flujo con datos sintéticos representativos; validar en navegador; decidir persistencia y auth solo si el flujo las necesita. No publicar un prototipo abierto con datos o acciones restringidas.

### Antes de producción

Resolver HTTPS, sesiones, autorización, límites, privacidad, backups cuando haya persistencia, observabilidad y rollback. Estos controles no se descartan por ser un MVP: se activan antes de exponer el riesgo correspondiente.

### Fuera del corto plazo

iOS/Android nativos, ERP y FDW, fiscalidad, checkout offline, multitenancy genérico, marketplace de plugins, Redis, colas distribuidas, microservicios, galería completa, automatizaciones de anticopia y traducciones adicionales. Reconsiderarlos solo con un requisito concreto, dueño y criterio de aceptación.

## 6. Resultado de la revisión

- Cada original tiene una versión propia y un enlace en [AGENTS.md](../../AGENTS.md).
- Las guías técnicas comparten una estructura prevista `frontend/` + `backend/`, sin afirmar que ya existe.
- No se copian IP, dominios privados, credenciales, nombres de empresas ni rutas operativas del origen.
- No se añade una licencia, framework, motor de datos o plataforma de despliegue por inferencia.
- Las guías diferidas conservan criterios de activación, no tareas que deban implementarse ahora.

Actualizar este alcance cuando el usuario apruebe una decisión; no usar la antigüedad de una referencia como evidencia del estado actual.
