# Guía de trabajo de Fortis TI

## 1. Contexto

Flutter/Dart para web y Python para backend. Publicar en `https://github.com/davyhenn/fortis_ti.git`, directamente en `master`, según autorización del usuario.

El alcance, las decisiones pendientes y el estado del proyecto se mantienen únicamente en [ALCANCE.md](docs/referencias/ALCANCE.md). No confundir una recomendación con una tecnología elegida o una herramienta ya instalada.

## 2. Referencias por tarea

Leer `ALCANCE.md` al iniciar trabajo y después solo las guías pertinentes. Para una tarea pequeña no es necesario cargar toda la documentación.

| Guía | Consultar para |
|---|---|
| [ALCANCE.md](docs/referencias/ALCANCE.md) | Decisiones, prioridades y temas fuera del corto plazo. |
| [FRONTEND.md](docs/referencias/FRONTEND.md) | Flutter/Dart, estado, diseño, accesibilidad y textos. |
| [BACKEND.md](docs/referencias/BACKEND.md) | Python, contratos HTTP y entrega de funcionalidades. |
| [DATOS.md](docs/referencias/DATOS.md) | Persistencia, SQL, migraciones y recuperación, cuando haya datos que guardar. |
| [SEGURIDAD.md](docs/referencias/SEGURIDAD.md) | Secretos, acceso, sesiones y exposición web. |
| [ENTREGA.md](docs/referencias/ENTREGA.md) | Dependencias, validación, build y despliegue. |

Cada tema tiene una guía responsable. Enlazarla en lugar de duplicar sus reglas. Si hay contradicciones, corregirlas respetando las instrucciones del usuario y de la herramienta.

## 3. Reglas de trabajo

- Inspeccionar antes de editar y respetar cambios ajenos. No borrar ni sobrescribir trabajo no propio sin autorización.
- Implementar el alcance solicitado con la estructura mínima útil. No crear capas, paquetes o infraestructura para satisfacer un documento.
- Mantener documentación/comunicación en español e identificadores técnicos nuevos en inglés, salvo un contrato existente que deba conservarse.
- No incluir secretos o datos personales reales en archivos versionados; usar ejemplos sintéticos. Aplicar la guía de seguridad antes de exponer el riesgo correspondiente.
- Dividir por responsabilidad, no por un número de líneas. Reutilizar código existente sin inventariar cada widget trivial.
- Validar de manera proporcional y decir qué se comprobó y qué quedó pendiente; no presentar un test, CI o despliegue inexistente como realizado.

## 4. Publicación en GitHub

Al completar una unidad de trabajo, salvo que el usuario indique lo contrario:

1. Revisar rama, estado y diff; validar lo pertinente según `ENTREGA.md`.
2. Preparar explícitamente solo los archivos de la tarea, incluidas eliminaciones intencionales. No publicar secretos ni artefactos locales.
3. Crear un commit descriptivo y ejecutar `git push origin master`.
4. No usar force push ni descartar historia o cambios ajenos. Inspeccionar cualquier divergencia remota antes de integrarla.
5. Confirmar el resultado y el commit; informar si el push falla. Publicar código no significa desplegarlo ni autoriza operaciones destructivas sobre datos.

## 5. Mantener las referencias

Actualizar `ALCANCE.md` al aprobar decisiones y la guía responsable cuando cambie una convención. Mantener enlaces e índice coherentes. Añadir un documento solo si un tema crece y necesita consultarse de forma independiente; no recuperar la división del proyecto de origen por costumbre.
