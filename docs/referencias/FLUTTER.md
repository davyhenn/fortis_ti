# Flutter: arquitectura y experiencia web

**Estado:** guía activa para código futuro. El proyecto Flutter aún no se ha creado. La raíz propuesta es `frontend/`.

## 1. Separación de responsabilidades

- Widgets y pantallas componen UI, eventos y accesibilidad. No contienen SQL ni reglas autoritativas de permisos o negocio.
- Modelos y transformaciones puras siguen [DART.md](DART.md). El acceso HTTP se concentra en clientes o repositorios, nunca en `build()`.
- Un estado local sencillo puede usar `setState`; elegir una solución global solo cuando los flujos compartidos la justifiquen. No incorporar varias librerías de estado por anticipación.
- Separar módulos por funcionalidad. Compartir componentes cuando exista reutilización real, no por una futura necesidad imaginada.

Estructura orientativa, no carpetas obligatorias vacías:

```text
frontend/lib/
  app/                   # arranque, rutas y composición
  core/                  # HTTP y utilidades realmente transversales
  theme/                 # tema y tokens
  features/<feature>/    # presentación, modelos y acceso a datos del flujo
  shared/widgets/        # componentes con varios consumidores
```

## 2. Tamaño y reutilización

- Preferir archivos centrados en una responsabilidad. Un archivo de unas 300-500 líneas o un `build()` difícil de leer es señal de revisar la composición, no prueba automática de mala calidad.
- Extraer widgets con significado, no helpers triviales para cumplir un contador.
- Inspeccionar componentes y tema antes de crear uno nuevo. Un componente local pertenece a su feature; uno compartido tiene un contrato pequeño y documentado.
- No existe un inventario de widgets aprobados del proyecto de origen. Registrar aquí los componentes reales cuando se creen.

## 3. Ciclo de vida y estado

- No disparar requests repetidas durante `build()`; iniciarlas desde un flujo o ciclo de vida apropiado.
- Liberar controllers, focus nodes, listeners y subscriptions que se hayan creado.
- Tras un `await`, comprobar `mounted` o `context.mounted` antes de tocar estado o contexto de UI; gestionar también respuestas tardías de búsquedas para que no sobrescriban resultados nuevos.
- Tratar carga, éxito, vacío y error; conservar datos útiles durante una recarga cuando tenga sentido.
- Evitar dobles envíos; mostrar progreso y permitir reintento seguro según el contrato.

## 4. Compatibilidad web

- No importar `dart:io` o `dart:ffi` en código del navegador ni introducir `dart:html` como nueva dependencia.
- Para APIs del navegador, evaluar `package:web` y JS interop detrás de un adaptador; aprobar primero la dependencia.
- Verificar soporte web efectivo de cada paquete, no solo que se instale.
- Diseñar rutas que permitan enlaces directos, recarga y navegación atrás/adelante. La estrategia hash o path se elige junto al servidor, no se presupone.
- La URL de API es configuración pública. Nunca transportar una credencial con `--dart-define` creyendo que queda oculta.

## 5. Diseño, accesibilidad y entradas

- Aplicar [DESIGN_SYSTEM.md](DESIGN_SYSTEM.md) y el flujo de [DESIGN.md](DESIGN.md). Material tematizado es una base válida; evitar estilos arbitrarios o copias del tema anterior.
- Usar constraints y `LayoutBuilder` cuando corresponda; no diseñar solo para un monitor específico.
- Respetar la escala de texto del sistema; no reemplazarla ni desactivarla globalmente. Si se ofrece zoom propio, integrarlo sin anular accesibilidad.
- Probar teclado, foco visible, labels/semántica, contraste, mensajes de error y tamaños táctiles.
- Formularios: límites y validaciones alineados con [API_CONTRACT.md](API_CONTRACT.md). No filtrar caracteres válidos de nombres o texto libre por reglas fiscales que no aplican.
- El servidor valida de nuevo. No truncar silenciosamente datos para que entren en una columna.
- Textos y formatos siguen [IDIOMAS.md](IDIOMAS.md).

## 6. Validación

Cuando exista la aplicación, desde `frontend/`:

```powershell
dart format --output=none --set-exit-if-changed lib test
flutter analyze
flutter test
flutter build web --release
```

Adaptar las rutas de formato a directorios existentes. Añadir pruebas de widgets para estados y formularios y una comprobación en navegador para navegación, layout y APIs web. Las `Key` estables ayudan a probar controles críticos, pero no sustituyen semántica accesible.

**Checklist:** [ ] separación UI/HTTP; [ ] estados completos; [ ] recursos liberados; [ ] tema; [ ] teclado y texto escalado; [ ] validación coherente; [ ] analyzer/tests/build pertinentes.

## 7. No incorporar ahora

Galería obligatoria, widgets del otro producto, excepción de colores por módulo, restricciones artificiales por número de parámetros, app nativa o un framework complejo de estado. Evaluarlos únicamente ante una necesidad real.
