# Contrato entre Flutter web y Python

**Estado:** convenciones iniciales. No hay endpoints implementados; los ejemplos no constituyen una API existente.

## 1. Definir antes de integrar

Para cada endpoint acordar método y ruta, consumidor, autenticación/autorización, request, respuesta, errores, paginación y efectos de una repetición. Mantener ejemplos sintéticos y pruebas junto al contrato; actualizar cliente y servidor en el mismo cambio.

- Rutas y campos técnicos en inglés. El prefijo inicial `/api/` es una propuesta a resolver con el primer flujo, no una obligación de crear múltiples versiones.
- JSON para datos normales; documentar explícitamente archivos o streams cuando aparezcan.
- No crear `/movil`, `/shop`, `/pdv` o un contrato de un ecosistema externo sin requisito.

## 2. Payloads y tipos

- Bodies estructurados y validados para creación/actualización. No mezclar una misma propiedad entre query y body sin razón documentada.
- IDs estables; no resolver identidad por un nombre de pantalla mutable. Un ID opaco no sustituye autorización.
- Fechas de calendario: `YYYY-MM-DD`; instantes: ISO-8601 con zona, preferiblemente UTC.
- Dinero: código de moneda y representación exacta acordada, por ejemplo unidades menores enteras o string decimal; no convertir indistintamente a float.
- Documentar límites, nullability y diferencia entre campo omitido y `null`, especialmente en PATCH.
- Usuario/rol de auditoría se derivan del servidor. Un `user_id` puede identificar el recurso objetivo, pero no demostrar quién hace la operación.

## 3. Respuestas y errores

No añadir un envelope de éxito a todo endpoint por costumbre. Una entidad puede devolverse como objeto; las listas paginadas usan un único formato acordado. Propuesta inicial:

```json
{"items": [], "total": 0, "limit": 50, "offset": 0}
```

Error común propuesto:

```json
{"error": {"code": "VALIDATION_ERROR", "message": "Datos inválidos", "fields": {"name": "required"}}}
```

`code` y códigos de campos son estables y técnicos; la UI puede traducirlos. `message` es legible y saneado, no un stack trace. Si se añade correlación, no incluir datos sensibles.

| HTTP | Uso |
|---|---|
| 200 / 201 / 204 | Éxito, creación y éxito sin body; el cliente no intenta parsear JSON en 204. |
| 400 / 422 | Request inválido; acordar distinción con el framework y normalizar sus errores. |
| 401 | Identidad ausente o sesión no válida. |
| 403 | Identidad válida sin acceso a la operación o recurso. |
| 404 | Recurso inexistente; puede ocultar existencia según la política de acceso. |
| 409 | Conflicto de versión, duplicado o estado incompatible. |
| 429 | Límite excedido; indicar cuándo reintentar si corresponde. |
| 500 | Error inesperado sanitizado, no exposición de internals. |

## 4. Listas, filtros y escrituras

- Propuesta para las primeras listas: `limit` por defecto 50 y máximo 100, `offset >= 0`. Confirmar por endpoint; no convertirlo en carga sin límite ni asumir que siempre se necesita `total` costoso.
- Orden determinista con desempate por ID. Validar y permitir solo filtros y campos de orden definidos.
- Usar cursor si el volumen o las actualizaciones frecuentes lo requieren; no añadirlo antes de necesitarlo.
- Para escrituras repetibles, documentar idempotencia. Añadir una clave de idempotencia cuando un doble envío pueda duplicar un efecto irreversible.
- Si aparece aislamiento por organización, validar pertenencia server-side. No confiar en un filtro de tenant elegido por el navegador.

## 5. Evolución y pruebas

Añadir campos opcionales puede ser compatible; renombrar, eliminar o cambiar significado exige revisar consumidores desplegados. No versionar cada endpoint de forma independiente sin una política.

Probar payload correcto e inválido, 401/403, acceso a recursos ajenos, lista vacía, límites, errores y repetición de escrituras sensibles. Documentación generada como OpenAPI ayuda si el framework la ofrece, pero no reemplaza los tests de contrato.

**Checklist:** [ ] contrato explícito; [ ] modelos de ambas capas alineados; [ ] actor del servidor; [ ] errores estables; [ ] listas acotadas; [ ] compatibilidad y tests.
