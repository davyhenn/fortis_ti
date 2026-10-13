# Persistencia: decisiones y reglas mínimas

**Estado:** condicional. No hay motor, conexión, esquema ni tablas elegidos. No conectarse a servidores mencionados por las referencias originales.

## 1. Elegir cuando exista una necesidad

Antes de guardar datos, definir entidades del primer flujo, volumen esperado, relaciones, concurrencia y recuperación. Evaluar un motor adecuado; PostgreSQL puede ser una opción y SQLite otra para usos compatibles, pero ninguna está adoptada.

El navegador consume una API. Las credenciales y conexiones de persistencia permanecen en el backend. No iniciar un esquema central compartido, FDW, réplicas o múltiples motores por imitación.

## 2. Modelado

- Usar claves primarias estables y relaciones explícitas. Los nombres visibles pueden cambiar sin cambiar identidad.
- Establecer restricciones reales: `NOT NULL`, `UNIQUE`, FK y checks según el motor y dominio.
- Elegir tipos adecuados para fechas, cantidades y dinero; no usar float para exactitud financiera por defecto.
- Distinguir dato desconocido (`null`) de cadena vacía o valor cero. Documentar defaults; no inventar registros para esconder ausencia.
- Fechas de instantes en UTC o tipo equivalente con zona; convertir al mostrar.
- Añadir campos de auditoría cuando el flujo los requiera, con actor validado por el servidor. No crear soft delete universal sin definir su efecto en unicidad y consultas.
- Nombres técnicos en inglés, de acuerdo con [IDIOMAS.md](IDIOMAS.md).

## 3. Consultas y transacciones

- Parametrizar valores siempre. No interpolar entrada del usuario en SQL.
- Para tabla/columna/orden dinámicos, usar una allowlist y las APIs seguras del driver; los placeholders de valores no sirven para identificadores.
- Acotar listados y seleccionar las columnas necesarias. Medir antes de prohibir un patrón SQL o crear índices por costumbre.
- Usar transacciones para cambios que deban ser atómicos; cerrar y devolver recursos al pool elegido.
- Manejar unicidad, conflictos y concurrencia explícitamente. Los checks de aplicación no sustituyen restricciones del motor.
- No cambiar esquema desde un request, importador o inicialización ordinaria; usar [MIGRATIONS.md](MIGRATIONS.md).

## 4. Fuente de verdad

Al existir persistencia, el historial de migraciones y los modelos versionados documentan el esquema. Leerlos antes de modificar queries; no inventar tablas ni columnas a partir de los documentos de origen.

Un snapshot de esquema puede ser complementario más adelante. No necesita acceso a producción ni sustituye el historial de migraciones. No copiar dumps con datos reales al repositorio.

## 5. Catálogos e aislamiento

- Una configuración de negocio editable puede necesitar tabla o catálogo; un enum estable de protocolo puede vivir en código y contrato. No obligar a crear una tabla para cada constante.
- No duplicar un mismo catálogo entre módulos. Definir dueño y reglas de actualización cuando exista.
- Multitenancy no está confirmado. Si aparece, diseñar pertenencia e aislamiento antes de aceptar datos de varias organizaciones; filtrar y autorizar desde la identidad validada, con tests de acceso cruzado.
- Una fuente externa de solo lectura debe respetarse cuando se integre, pero aquí no hay ERP ni réplica configurados.

## 6. Operación y pruebas

Credenciales con privilegio mínimo y configuración segura; entornos de prueba separados de producción. Antes de operar datos persistentes reales, definir backups, retención y probar restauración. Validar migraciones y queries en una base aislada.

**Checklist:** [ ] motor elegido por necesidad; [ ] IDs y constraints; [ ] SQL seguro; [ ] transacciones; [ ] esquema versionado; [ ] tests aislados; [ ] recuperación antes de producción.

**Diferido:** esquemas fiscales, catálogo de ecommerce, terminal offline, tablas de personas del otro sistema, servidores compartidos y dumps automatizados de infraestructura ajena.
