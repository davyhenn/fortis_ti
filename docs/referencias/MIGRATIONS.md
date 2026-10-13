# Migraciones: cambios de esquema verificables

**Estado:** condicional a elegir persistencia. No existe herramienta de migraciones ni carpeta de SQL operativo.

## 1. Elegir un mecanismo único

Elegir migrador junto al motor/ORM. Alembic es una posibilidad si la stack lo justifica, no una dependencia aprobada. No mezclar migraciones del ORM y scripts manuales sin definir su autoridad y orden.

- Un cambio con propósito claro por migración; nombres descriptivos e identificadores ordenados según la herramienta.
- Registrar versiones aplicadas y verificar la cadena de dependencias.
- No modificar silenciosamente una migración ya aplicada o distribuida. Añadir una nueva que corrija el estado.
- Mantener el historial necesario para reconstruir y actualizar bases; no borrar migraciones porque exista un snapshot.

## 2. Idempotencia sin ocultar deriva

Un migrador normalmente aplica cada versión una vez. `IF NOT EXISTS` no comprueba que una tabla existente tenga las columnas o restricciones correctas; puede ocultar un esquema divergente.

Usarlo solo cuando sea semánticamente correcto y acompañado de comprobaciones. Para seeds repetibles, usar claves estables y una política clara de insert/update: no sobrescribir datos del usuario por repetir la carga.

## 3. Compatibilidad y seguridad

- Crear o cambiar esquema fuera de handlers y jobs de negocio, con permisos específicos del proceso de migración.
- Usar transacciones donde el motor lo soporte; evaluar operaciones que requieran otro tratamiento, locks y tiempo de ejecución.
- Para datos existentes: añadir estructura compatible, migrar/backfill de forma acotada, comprobar y después imponer restricciones o retirar campos.
- No conceder roles privilegiados universalmente mediante seeds de ejemplo. Las asignaciones de acceso necesitan una política aprobada.
- No crear DDL en tablas de una integración ajena sin autorización y propiedad explícita.
- Un `DROP`, borrado masivo o cambio irreversible sobre datos reales requiere autorización explícita y estrategia de recuperación; publicar código no autoriza ejecutarlo en producción.

## 4. Pruebas y recuperación

Cuando existan migraciones, verificar:

1. Creación desde una base vacía.
2. Actualización desde la versión anterior soportada con datos sintéticos representativos.
3. Restricciones, índices y queries de la aplicación tras el cambio.
4. Repetición según el mecanismo: una versión ya aplicada no vuelve a ejecutarse; seeds repetibles mantienen el estado esperado.
5. Rollback probado si es viable o recuperación documentada por corrección hacia adelante/restauración. No prometer que todo cambio destructivo tiene un downgrade seguro.

Antes de cambios de datos reales, disponer de backup y comprobación de restauración. Un backup sin prueba no garantiza recuperación.

**Checklist:** [ ] mecanismo y orden; [ ] historial intacto; [ ] propósito único; [ ] deriva detectada; [ ] compatibilidad; [ ] tests de creación/upgrade; [ ] recuperación; [ ] autorización si hay destrucción.

Referencias: [DATABASE.md](DATABASE.md), [SECURITY.md](SECURITY.md), [PLUGINS.md](PLUGINS.md).
