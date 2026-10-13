# Diseño: propuesta, validación y artefactos

**Estado:** proceso para las futuras pantallas. No existe identidad visual aprobada ni galería de componentes.

## 1. Proporción y aprobación

- Antes de la primera UI, acordar dirección visual, densidad, navegación y necesidades de los usuarios.
- Para un cambio visual importante o un patrón nuevo compartido, presentar una propuesta y pedir aprobación antes de consolidarlo como estándar.
- Ofrecer alternativas cuando haya una decisión real; no exigir tres variantes para corregir un overflow, aplicar un token aprobado o mejorar accesibilidad sin cambiar la identidad.
- Reutilizar un patrón aprobado no requiere otra votación por cada instancia. Un cambio de comportamiento relevante sí debe explicarse y validarse.
- No copiar la paleta, tamaños rígidos o el inventario de widgets de otro producto como si el usuario los hubiera elegido aquí.

## 2. Previews

- Guardar artefactos de comparación/validación en `scratch/`, no en la raíz ni en código runtime.
- Elegir el medio más fiel y barato: mockup HTML para composición, imagen para dirección visual o prototipo Flutter para comportamiento y fidelidad del widget.
- HTML no sustituye una prueba del rendering, foco y accesibilidad de Flutter. No construir obligatoriamente dos galerías para la misma decisión.
- Usar datos sintéticos sin marcas, personas o datos operativos del origen.
- Comparar estados relevantes y tamaños de pantalla; mostrar antes/después cuando exista diseño actual.
- Evaluar light/dark si ambos están dentro del alcance aprobado. No simular una paleta «real» que aún no existe.
- Iterar la misma propuesta y dejar registrada la elección y sus límites.

Los previews se versionan solo si ayudan a mantener una decisión; no son assets productivos. No eliminarlos por una limpieza automática sin revisar su valor o propiedad.

## 3. Componentes

Revisar el tema y componentes existentes. Un componente compartido se crea cuando hay consumidores reales o un patrón fundacional aprobado; documentar propósito, variantes y estados en [DESIGN_SYSTEM.md](DESIGN_SYSTEM.md).

No es obligatorio crear una galería interactiva inicialmente. Si crece el catálogo, evaluar una galería sencilla sin convertirla en requisito para cada entrega.

## 4. Pruebas y organización

| Artefacto | Ubicación prevista | Qué valida |
|---|---|---|
| Unitarias Dart | `frontend/test/` | Modelos, transformaciones y lógica cliente. |
| Widgets | `frontend/test/` | Estados, validación, interacción y semántica de componentes. |
| Integración/E2E | Ubicación definida con el runner web elegido | Flujo real, red, navegación y compatibilidad de navegador. |
| Backend | `backend/tests/` | Casos de uso, API y persistencia aislada. |
| Preview | `scratch/` | Decisión visual; no sustituye tests. |

No asumir que `flutter test integration_test -d chrome` es un flujo válido sin revisar el soporte del SDK y runner elegidos. Para la primera entrega, añadir al menos una comprobación manual en navegador; documentar después el comando automatizado que se haya probado.

Tests de screenshots/golden pueden incorporarse para patrones estables; controlar fuentes y entorno y no confundir coincidencia de píxeles con accesibilidad.

**Checklist:** [ ] dirección/propuesta aprobada cuando corresponde; [ ] datos sintéticos; [ ] artefactos organizados; [ ] responsive/estados; [ ] tokens consistentes; [ ] validación Flutter y navegador.
