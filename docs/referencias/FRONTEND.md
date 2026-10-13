# Frontend web: Flutter, Dart y experiencia de uso

Consultar al crear o modificar la aplicación. Las comprobaciones se concentran en [ENTREGA.md](ENTREGA.md); las decisiones pendientes en [ALCANCE.md](ALCANCE.md).

## 1. Estructura y estado

- La UI compone widgets y eventos; el acceso HTTP y las transformaciones reutilizables viven fuera de `build()`.
- Mantener los modelos y cálculos independientes de Flutter cuando sea útil para probarlos. El estado de presentación sí puede usar `setState`, `ChangeNotifier` u otra solución justificada.
- Empezar con archivos por responsabilidad; agrupar por funcionalidad cuando crezca el código. No crear capas o componentes compartidos sin consumidores reales.
- No realizar requests repetidas durante `build()`. Iniciarlas desde el ciclo de vida o la acción adecuada.
- Liberar controllers/listeners/subscriptions propios. Tras un `await`, comprobar que la pantalla o contexto siguen vigentes y evitar que respuestas antiguas sobrescriban búsquedas nuevas.
- Tratar carga, éxito, vacío y error; impedir dobles envíos y ofrecer reintentos seguros según el contrato.

## 2. Tipado y frontera HTTP

- Usar modelos, colecciones y resultados tipados. Validar JSON al entrar; no propagar `dynamic` por toda la UI.
- Conservar null safety: una ausencia válida se modela como nullable, no se oculta con casts a `dynamic` o `!` sin justificar.
- Diferenciar ausencia de datos, respuesta inválida y fallo de red. Los defaults documentados son válidos; fabricar datos o convertir todo error en vacío no lo es.
- Configurar timeouts; un timeout de escritura no prueba que el servidor no haya ejecutado la operación. No repetir efectos sensibles sin resolver idempotencia.
- La UI puede validar y calcular para UX; las decisiones autoritativas de acceso y negocio siguen en el servidor.
- Compartir formatos y límites con el contrato definido en [BACKEND.md](BACKEND.md), sin truncar entradas silenciosamente.

## 3. Navegador

- No usar `dart:io`/`dart:ffi` en código que deba compilar para web. Evitar código nuevo basado en `dart:html`; si hace falta una API de navegador, evaluar `package:web`/JS interop detrás de un adaptador.
- Comprobar soporte web de cada paquete. Que se instale no garantiza que funcione en navegador.
- Probar URLs directas, recarga y atrás/adelante. Elegir rutas hash o path junto al hosting, no por una convención heredada.
- Todo lo enviado al navegador, incluida configuración compilada, es inspeccionable: aplicar [SEGURIDAD.md](SEGURIDAD.md).

## 4. Diseño y accesibilidad

- Acordar la dirección visual inicial y validar cambios importantes antes de convertirlos en estándar. Reutilizar un patrón aprobado o corregir accesibilidad no exige una votación nueva.
- Usar `ThemeData`, `ColorScheme` y `TextTheme`; centralizar estilos repetidos. Material tematizado es válido; no duplicar colores o crear wrappers para cada widget por obligación.
- Añadir tokens/componentes propios cuando resuelvan consistencia real. No hay una paleta, galería o inventario heredados que debamos implementar.
- Diseñar con constraints y probar pantallas pequeñas. Respetar escala de texto y zoom; verificar foco visible, teclado, semántica/labels, contraste y controles táctiles.
- Comunicar estados con texto/ícono además de color. Labels breves deben seguir siendo claros, especialmente en acciones destructivas.
- Formularios: reglas de tipo, longitud y dominio según contrato, mensajes asociados al campo y caracteres válidos para texto libre.
- Si ayuda un preview, guardarlo en `scratch/` con datos sintéticos. Elegir HTML, imagen o Flutter según lo que se deba evaluar; HTML no demuestra la accesibilidad del widget real.

## 5. Textos y formatos

- Español es la base provisional de UI; confirmar usuarios y región antes de fijar locales adicionales. Documentación en español e identificadores técnicos nuevos en inglés.
- Centralizar textos compartidos; usar catálogos/ARB cuando aparezca una necesidad real de localización. No es obligatorio un selector de idiomas para el primer prototipo.
- No concatenar frases que dificulten traducción o plurales; incluir labels accesibles y errores en cualquier catálogo que se adopte.
- Formatear fechas, cantidades y moneda al mostrar según locale y dominio. El idioma no determina la moneda ni una precisión universal.
- Preservar nombres y texto del usuario; no restringirlo a ASCII ni «traducirlo» por comodidad.

Una pantalla útil integra navegación y estados, no solo un placeholder. Sus pruebas deben cubrir la lógica y el comportamiento relevantes, además de una comprobación en navegador.
