# Alcance y decisiones de Fortis TI

Revisión de contenido y consolidación: 2026-10-13. Este archivo distingue lo decidido de lo posible; no es una lista de infraestructura que debamos implementar.

## 1. Confirmado y estado actual

- Flutter/Dart para web y Python para backend, desarrollados inicialmente en Windows.
- GitHub: `davyhenn/fortis_ti`, publicación directa en `master` según [AGENTS.md](../../AGENTS.md).
- Actualmente el repositorio contiene documentación; aún no hay aplicación, API, persistencia ni despliegue configurados.
- El entorno local ya fue comprobado, pero sus versiones instaladas no equivalen a un SDK/runtime fijado para el proyecto.
- El repo era público al revisarlo. No asumir confidencialidad ni cambiar visibilidad o licencia sin decisión del titular.

## 2. Decidir cuando la necesidad aparezca

| Decisión | Momento y criterio |
|---|---|
| Primer flujo | Antes de crear aplicación: usuario, tarea, datos y criterio de aceptación. No inferir funcionalidades por el nombre del repo. |
| Diseño inicial | Al crear la primera pantalla: dirección visual y navegación, sin requerir un sistema de diseño completo. |
| Framework Python y dependencias base | Al iniciar la API: complejidad del flujo y soporte del runtime. Ningún framework está elegido. |
| Persistencia | Cuando haya datos que guardar: relaciones, concurrencia y recuperación. Ningún motor está elegido. |
| Identidad y acceso | Antes de exponer datos o acciones restringidas: usuarios y riesgo, sin imponer JWT ni cuentas propias. |
| Hosting y operación | Antes del primer despliegue: HTTPS, configuración, actualización y recuperación. CI puede ayudar, pero no existe hoy. |
| Distribución/licencia | Antes de declarar una licencia o incorporar contenido con restricciones; publicar en GitHub no resuelve esta decisión. |

La propuesta organizativa es `frontend/` y `backend/` cuando se creen proyectos. La estructura interna dependerá del código real; no hay obligación de crear `domain/data/presentation`, repositories o múltiples capas vacías.

## 3. Qué cambió al revisar nuestras propias guías

| Problema encontrado | Corrección y motivo |
|---|---|
| 17 guías heredaban la división del origen, con reglas repetidas. | Seis guías por responsabilidad; un único lugar para seguridad, datos y validación. Menos mantenimiento y menos contradicciones. |
| Propuestas concretas de API, paginación y escalas visuales sin requisitos. | Se retiran valores y formatos arbitrarios. El primer flujo fijará un contrato comprobable. |
| Modularidad recomendada junto a árboles de carpetas demasiado anticipados. | Separar responsabilidades en código; introducir capas solo cuando ayuden. No medir calidad por número de líneas. |
| Catálogos de textos, inventarios de componentes y aprobaciones podían frenar un prototipo. | Español provisional y tema coherente; catálogos/galerías solo cuando aporten valor. Aprobar dirección o cambios importantes, no cada ajuste. |
| Prohibiciones amplias sobre fallbacks podían impedir defaults válidos. | Permitir defaults documentados; impedir datos inventados, permisos asumidos y fallos convertidos silenciosamente en éxito. |
| Reglas de producción y temas futuros aparecían como trabajo inmediato. | Controles activados por riesgo real y una sola lista de temas diferidos. No posponer acceso seguro o recuperación cuando ya son necesarios. |

Se conserva lo técnicamente útil: tipado y null safety, separación de UI/HTTP, validación server-side, identidad verificada, SQL seguro, migraciones comprobadas, accesibilidad y validación proporcional. No se conservaron secretos, infraestructura o automatismos del origen.

## 4. Fuera del corto plazo

Apps iOS/Android nativas, ERP/FDW y fiscalidad, checkout offline, multitenancy genérico, framework de plugins, microservicios/colas distribuidas, galería completa, varios idiomas y mecanismos anticopia encubiertos.

Reabrir un tema solo con un requisito concreto y criterios de aceptación. Responsive, límites HTTP y permisos no se aplazan por compartir vocabulario con esos temas. Si un flujo exige aislamiento, trabajos duraderos o traducciones, resolver esa necesidad antes de entregarlo, sin instalar todo un framework por anticipación.

## 5. Trazabilidad sin mantener duplicados

Los siguientes son nombres históricos, no archivos activos ni instrucciones adicionales:

| Guía actual | Referencias de origen aprovechadas |
|---|---|
| [FRONTEND.md](FRONTEND.md) | `FLUTTER.md`, `DART.md`, `DESIGN.md`, `DESIGN_SYSTEM.md`, `IDIOMAS.md`. |
| [BACKEND.md](BACKEND.md) | `PYTHON.md`, `API_CONTRACT.md`, `MODULO.md` y aislamiento útil de `PLUGINS.md`. |
| [DATOS.md](DATOS.md) | `DATABASE.md`, `MIGRATIONS.md`. |
| [SEGURIDAD.md](SEGURIDAD.md) | `SECURITY.md`, `SEGURANCA_WEB.md`, `PROPRIEDADE_INTELECTUAL.md`. |
| [ENTREGA.md](ENTREGA.md) | `DEPENDENCIAS.md`, `DEPLOY.md`; reúne además las comprobaciones que se repetían. |
| Este alcance | `IOS_MOBILE.md`: se difiere la app nativa; lo responsive útil está en frontend. |

Las primeras versiones propias permanecen en el historial Git, no como documentación paralela. Los originales externos no se modifican ni se copian al repo. Actualizar este archivo al aprobar una decisión o cambiar el estado real del proyecto.
