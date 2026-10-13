# Datos, migraciones y recuperación

**Condicional:** consultar cuando un flujo necesite persistencia. Todavía no se ha elegido motor, ORM ni migrador. No conectarse a infraestructura del proyecto de origen.

## 1. Modelar lo necesario

- Elegir persistencia según relaciones, concurrencia y operación del primer flujo, no por imitación. El navegador accede por API; las conexiones privadas pertenecen al backend.
- Usar claves estables y restricciones reales de unicidad, relaciones y nulabilidad según el motor. Los checks de aplicación no sustituyen restricciones de datos.
- Distinguir `null`, vacío y cero; documentar defaults. No crear identidades o registros ficticios para esconder ausencia.
- Elegir tipos apropiados para instantes, cantidades y dinero; datos de auditoría solo cuando aporten valor, con actor verificado por el servidor.
- Un catálogo editable puede necesitar tabla; un enum estable puede vivir en código/contrato. No guardar cada constante en una tabla ni añadir soft delete universal.
- No crear multitenancy por anticipación. Si hay datos de varias organizaciones, diseñar y probar aislamiento antes de operar con ellos.

## 2. Consultas y transacciones

- SQL con valores parametrizados, nunca entrada del usuario interpolada. Para nombres dinámicos de tabla/columna/orden, allowlist y APIs seguras del driver; placeholders de valores no sirven para identificadores.
- Acotar consultas/listas y medir antes de añadir índices o prohibir patrones SQL.
- Usar transacciones para cambios que deban ser atómicos; mantenerlas cortas y devolver conexiones al pool. Evitar llamadas lentas a terceros dentro de una transacción.
- Resolver conflictos y concurrencia explícitamente. Un error no debe convertirse en éxito o dato inventado.
- Leer el esquema/modelos/migraciones versionados antes de modificar queries. Un snapshot puede ayudar, pero no sustituye el historial ni requiere importar datos reales.

## 3. Cambios de esquema

- Elegir un único mecanismo con versiones registradas. No alterar esquema desde requests, conectores o arranque ordinario.
- Una migración con propósito claro; no editar silenciosamente versiones ya aplicadas ni borrar historia porque exista un snapshot.
- `IF NOT EXISTS` no valida que el esquema existente sea el esperado. Idempotencia de seeds y corrección del esquema son comprobaciones distintas.
- Seeds con claves y política de actualización claras; no sobrescribir datos del usuario ni conceder privilegios universales.
- Considerar locks, transacciones soportadas y compatibilidad: cambios aditivos, backfill acotado y retirada posterior cuando haya consumidores anteriores.
- Probar creación desde vacío y actualización desde la versión soportada con datos sintéticos, incluidas restricciones y queries posteriores.

## 4. Datos reales y recuperación

Antes de operar datos persistentes reales, definir backup, retención y comprobar restauración. Separar pruebas de producción y usar privilegio mínimo según [SEGURIDAD.md](SEGURIDAD.md).

Una operación destructiva requiere autorización explícita y recuperación evaluada. Publicar una migración no autoriza ejecutarla contra producción. Definir downgrade si es seguro o corrección hacia adelante/restauración; no prometer rollback automático de un cambio irreversible.

Las pruebas pertinentes se ejecutan en una base aislada según [ENTREGA.md](ENTREGA.md).
