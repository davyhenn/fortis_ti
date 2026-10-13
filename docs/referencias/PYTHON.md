# Python: backend pequeño, validado y comprobable

**Estado:** guía activa para el futuro backend. Python está disponible; no se ha elegido framework ni se han instalado dependencias del proyecto.

## 1. Arquitectura mínima

- Un punto de entrada ligero configura la aplicación y registra sus componentes.
- Los handlers HTTP validan transporte y delegan casos de uso; no acumulan UI, queries y reglas de negocio en una única función.
- Separar servicios por responsabilidad. Crear una capa de repositorio si existe persistencia que lo justifique; no fabricar abstracciones vacías.
- Dividir un módulo cuando haya responsabilidades distintas, no al alcanzar exactamente tres endpoints.
- Mantener configuración y dependencias inyectables para probar sin servicios externos.

Si se elige FastAPI, usar routers y modelos Pydantic. Si se elige otro framework, conservar el mismo contrato y validación equivalente; la documentación no obliga a instalar FastAPI ahora.

## 2. Tipos, errores y límites

- Anotar tipos en interfaces y funciones de negocio; usar modelos de entrada/salida explícitos.
- Validar tipo, longitud, rango y reglas del dominio en el servidor. Rechazar datos inválidos con el error del contrato, sin truncarlos silenciosamente.
- Conservar la diferencia entre `None`, valor vacío y error. Un dato ausente no autoriza inventar una empresa, usuario o resultado.
- Capturar excepciones concretas donde se puedan manejar. No usar `except:`, `except Exception: pass` ni devolver éxito ante un fallo.
- Un handler general puede sanitizar errores inesperados y registrarlos sin secretos; no filtrar SQL, paths internos o stack traces al cliente.

## 3. I/O, conexiones y concurrencia

- Usar timeouts y cierres garantizados en HTTP, archivos y conexiones.
- El patrón de adquirir/devolver conexiones depende del driver y pool elegidos; un context manager genérico no garantiza que una conexión vuelva al pool.
- No bloquear un event loop con librerías síncronas. Elegir handlers síncronos o async de acuerdo con el framework y sus dependencias.
- Transacciones cortas y explícitas para escrituras relacionadas; no mantenerlas abiertas durante llamadas lentas a terceros.
- SQL parametrizado y nombres dinámicos controlados según [DATABASE.md](DATABASE.md).
- No crear DDL ni ejecutar cambios de esquema desde requests; seguir [MIGRATIONS.md](MIGRATIONS.md).

## 4. Seguridad y logging

- Aplicar [SECURITY.md](SECURITY.md) y [SEGURANCA_WEB.md](SEGURANCA_WEB.md) antes de exponer funciones restringidas.
- Determinar actor y permisos desde la sesión validada, nunca desde `X-Role` o un usuario del body.
- Hashear contraseñas con una librería mantenida si se implementan cuentas propias; no escribir un algoritmo de hashing casero ni copiar una migración de usuarios que no existen aquí.
- Usar logging estructurado suficiente para diagnosticar: severidad, operación y correlación; no guardar tokens, contraseñas ni payloads personales completos.
- El servicio devuelve datos y errores de dominio; el handler los convierte al contrato HTTP.

## 5. Entorno y calidad

- Crear `backend/.venv/` cuando se inicie el backend; no versionarlo. Revisar la compatibilidad real de los paquetes con la versión de Python elegida.
- Definir dependencias reproducibles con [DEPENDENCIAS.md](DEPENDENCIAS.md).
- Ruff y pytest son herramientas recomendadas, pendientes de incorporación, no ya instaladas por estas guías.
- Cuando estén configuradas, desde `backend/` usar `python -m ruff check .`, `python -m ruff format --check .` y `python -m pytest`.
- Tests unitarios sin red; de API para payloads, auth y errores; de persistencia contra una base aislada cuando exista.
- `py_compile` puede detectar sintaxis, pero no reemplaza lint, tests ni validación de contrato.

## 6. Mantenibilidad

Preferir funciones breves y archivos por responsabilidad; 200-300 líneas es una señal de revisión, no un gate existente. Comentarios explican decisiones y riesgos, no repiten código. Registrar deuda concreta con criterio de resolución, sin copiar baselines de un sistema legado.

**Checklist:** [ ] framework decidido; [ ] entradas/salidas tipadas; [ ] errores explícitos; [ ] recursos cerrados; [ ] auth del servidor; [ ] tests pertinentes; [ ] dependencias reproducibles.
