# Sistema de diseño: una base coherente y accesible

**Estado:** principios activos, identidad y componentes pendientes de aprobación. No se adopta la paleta del sistema de origen.

## 1. Tema y tokens

- Centralizar decisiones compartidas bajo `frontend/lib/theme/` al existir el proyecto.
- Usar `ThemeData`, `ColorScheme` y `TextTheme`; añadir `ThemeExtension` si faltan tokens semánticos reales.
- Colores de marca, superficie, texto, borde y estados deben tener pares de contraste adecuados. Los widgets consumen tema/tokens, no colores hex repetidos.
- Centralizar spacing, radios, tipografía y motion que se repitan. Constantes locales de composición pueden existir; evitar números arbitrarios repetidos, no prohibir cada valor numérico.
- No imponer hexadecimal frente a otra representación equivalente por una regla cosmética. Importan fuente única, exactitud y contraste comprobado.

Base orientativa para discutir, todavía no implementación ni identidad aprobada:

| Grupo | Propuesta mínima |
|---|---|
| Espaciado | Escala coherente de 4, 8, 12, 16, 24 y 32. |
| Tipografía | Título, subtítulo, cuerpo, etiqueta y ayuda desde `TextTheme`. |
| Radios | Pocos niveles, consistentes con densidad y tipo de elemento. |
| Estados | Success, warning, error e info, cada uno con texto/ícono además de color. |
| Motion | Transiciones breves con propósito y respeto a preferencias de reducción. |

## 2. Composición

- Una acción principal clara por contexto; secundarias con menor peso visual.
- Alinear campos, cards y toolbars con jerarquía legible; no fijar un header de 70 px para todas las pantallas.
- Labels breves pero suficientemente claros. No recortar una acción peligrosa a una palabra ambigua por cumplir un contador.
- Ayuda contextual visible cuando sea importante; un tooltip no debe ser la única explicación accesible.
- Estados loading, vacío, error y disabled coherentes; el estado no se comunica solo mediante color.
- Responsive y escala de texto son requisitos, no variantes estéticas opcionales.

## 3. Material y componentes propios

Material correctamente tematizado es una base válida. No crear wrappers para todos sus widgets solo para evitar un «default» ni entregar cada pantalla con estilos arbitrarios.

Crear un componente propio cuando aporte un contrato reusable, una decisión visual aprobada o comportamiento común. Un componente de formulario debe conservar validación, foco, labels, errores y compatibilidad con teclado; uno propio que los pierde no es una mejora.

**Inventario actual:** vacío. Al crear un componente compartido, registrar aquí nombre, ruta, propósito, variantes, consumidores y estado real (propuesto/aprobado/deprecado). No marcar nada aprobado por herencia del origen.

## 4. Accesibilidad

- Contraste conforme al objetivo acordado, usando WCAG como referencia y verificando controles reales; no declarar cumplimiento global sin evaluación.
- Foco visible y orden razonable; no bloquear teclado o copiar solo interacciones de ratón.
- Respetar escala de texto, zoom de navegador y preferencias de motion.
- Semántica/labels correctos; errores asociados al campo y acciones táctiles utilizables.
- Gráficos y badges necesitan alternativas legibles, no solo una paleta atractiva.

**Checklist:** [ ] tokens/tema; [ ] jerarquía; [ ] componentes sin duplicación; [ ] estados; [ ] contraste y foco; [ ] texto escalado; [ ] identidad aprobada.

Referencias: [DESIGN.md](DESIGN.md), [FLUTTER.md](FLUTTER.md), [IDIOMAS.md](IDIOMAS.md).
