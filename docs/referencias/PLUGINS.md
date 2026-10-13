# Integraciones externas y plugins

**Estado:** framework de plugins diferido. Un paquete Flutter de pub.dev se evalúa con [DEPENDENCIAS.md](DEPENDENCIAS.md); esta guía trata conectores de negocio.

## 1. Qué sí sirve desde la primera integración

- Aislar cada servicio externo detrás de un adaptador con contrato explícito.
- Definir datos que lee, datos que puede escribir, credenciales, límites, errores y responsabilidad de mantenimiento.
- Mantener timeouts, validación y mapeo fuera de UI y del dominio puro.
- Probar con respuestas falsas/sintéticas, sin usar producción como entorno de test.
- No compartir estado mutable oculto entre conectores ni acoplar módulos al formato de un proveedor.

Un único conector puede ser un servicio pequeño. No necesita registry, launcher, scheduler ni tablas de ejecución genéricas.

## 2. Escrituras y propiedad de datos

- El adaptador no crea ni altera tablas durante `run()` o un request; cambios de esquema pasan por [MIGRATIONS.md](MIGRATIONS.md).
- Puede escribir datos solo mediante contratos/casos de uso autorizados y con reglas de consistencia. La posibilidad de DML no significa permiso para modificar cualquier tabla.
- Respetar fuentes de solo lectura y los términos de API. No adivinar que un ERP o una réplica permite escritura.
- Definir identificadores externos, deduplicación y política de actualización antes de importar; no sobrescribir cambios del usuario silenciosamente.
- Eliminar el adaptador no autoriza borrar datos importados ni tablas. Una eliminación de datos necesita decisión y recuperación explícitas.

## 3. Ejecución segura, si se necesitan importaciones

- Decidir si la operación cabe en una request o requiere un job duradero.
- No usar un thread daemon como garantía de ejecución: un reinicio puede perder el trabajo. Elegir un mecanismo adecuado al riesgo y duración.
- Evitar ejecuciones incompatibles concurrentes con un mecanismo que funcione en el despliegue real; un lock local no cubre múltiples procesos.
- Reintentar solo operaciones seguras/idempotentes, con límites y registro de resultado. Un import parcial debe ser diagnosticable y recuperable.
- No loguear credenciales ni datos externos completos. Autorizar inicio y consulta de ejecuciones server-side si el flujo es restringido.

## 4. Cuándo reconsiderar un framework de plugins

Solo si hay varias integraciones reales con ciclos de ejecución, configuración y estado suficientemente comunes. Antes de crearlo, comparar un framework genérico con servicios explícitos y documentar el coste.

Entonces definir registro, interfaces, lifecycle, versionado, propiedad del esquema, permisos y recuperación. La dependencia entre conectores, si existe, debe ser explícita; no imponer independencia absoluta cuando un pipeline necesita orden.

**Checklist para una integración real:** [ ] proveedor y contrato; [ ] acceso aprobado; [ ] secretos protegidos; [ ] límites; [ ] propiedad de datos; [ ] deduplicación; [ ] errores y recuperación; [ ] tests sin producción.

**Fuera del corto plazo:** importadores ERP específicos, decoradores de discovery, launcher genérico, scheduler, polling universal y locks PostgreSQL sin base elegida.
