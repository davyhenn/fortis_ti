# Backend Python y contrato con el cliente

Aplicar al iniciar la API. No se prescribe framework ni esquema HTTP antes de definir el primer flujo; consultar [ALCANCE.md](ALCANCE.md).

## 1. Entregar un flujo, no un framework

- Acordar usuario, tarea, entradas, resultado y criterio de aceptación. Crear solo la navegación, API y persistencia necesarias.
- Mantener el arranque ligero; handlers HTTP validan transporte y delegan lógica reutilizable. Separar por responsabilidad cuando ayude a entender o probar.
- No crear capas vacías, routers por un contador de endpoints, pestañas obligatorias o endpoints de prueba sin consumidor.
- Tipar interfaces y definir modelos de entrada/salida. Si se elige FastAPI, aprovechar sus modelos y routers; otras opciones deben ofrecer validación equivalente.
- Inyectar o sustituir dependencias externas para probar sin producción.

## 2. Contrato HTTP antes de integrar

Para cada operación, registrar método/ruta, acceso, campos/tipos/límites, respuesta, errores y efectos de una repetición. Mantener ejemplos sintéticos y cliente/servidor compatibles; no adoptar un envelope o prefijo solo porque estaba en una referencia.

- Distinguir campo omitido, `null` y vacío, sobre todo en actualizaciones parciales. Rechazar datos inválidos sin truncarlos silenciosamente.
- Usar identificadores estables; un ID opaco no sustituye autorización. Determinar actor y permisos según [SEGURIDAD.md](SEGURIDAD.md).
- Fechas de calendario como `YYYY-MM-DD`; instantes en ISO-8601 con zona. Cantidades exactas requieren representación acordada, por ejemplo decimal como string o unidades menores; no asumir float financiero.
- Respetar semántica HTTP: 401 para identidad no válida, 403 para acceso denegado, conflictos explícitos y 204 sin body JSON. Acordar el tratamiento de validación con el framework.
- Errores con código estable y mensaje saneado; la UI puede traducirlos. No devolver stack traces, SQL ni paths internos.
- Acotar listas y bodies por necesidad real; paginación y orden deterministas. Validar campos de filtro/orden, sin tamaños globales inventados.
- Para escrituras sensibles, resolver dobles envíos, concurrencia e idempotencia. Versionar o migrar contratos cuando cambie un consumidor desplegado, no cada endpoint por costumbre.

## 3. Recursos, errores y observabilidad

- Capturar errores donde se puedan tratar; no usar `except:` ni ignorarlos para devolver éxito. Un handler general puede registrar y sanear fallos inesperados.
- Usar timeouts y cierre garantizado de HTTP, archivos y conexiones. El mecanismo de devolver conexiones depende del driver/pool elegido.
- No bloquear un event loop con I/O síncrono; elegir ejecución sync/async coherente con el framework y librerías, sin imponer async a todo el backend.
- Logging con severidad y contexto suficiente para diagnosticar, sin secretos ni payloads personales completos.
- Persistencia y transacciones se rigen por [DATOS.md](DATOS.md); las pruebas y dependencias por [ENTREGA.md](ENTREGA.md).

## 4. Integraciones, solo cuando existan

Un servicio externo necesita un adaptador pequeño con contrato, timeouts y mapeo explícitos, no un framework de plugins. Definir qué datos puede modificar, cómo deduplica y cómo se recupera de un fallo parcial; respetar fuentes de solo lectura.

Si el trabajo debe sobrevivir a reinicios, elegir ejecución duradera. Un thread daemon no la garantiza; un lock local no cubre varias instancias. No introducir una cola distribuida hasta que el requisito lo justifique, ni borrar datos al retirar un conector.

Un flujo terminado valida sus entradas, devuelve errores útiles, respeta acceso y tiene pruebas de sus resultados y límites; un scaffold navegable no cumple por sí solo ese criterio.
