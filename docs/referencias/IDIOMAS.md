# Idiomas y formatos

**Estado:** español para documentación y como base provisional de UI. Los idiomas de usuarios reales se confirmarán al definir el producto.

## 1. Separar lenguaje técnico y visible

| Capa | Convención |
|---|---|
| Código nuevo, rutas, JSON y nombres de esquema | Inglés; nombres coherentes y sin traducciones por consumidor. |
| Documentación y comunicación con el usuario | Español. |
| Textos de UI | Español inicialmente, centralizados y listos para ampliar si existe necesidad. |
| Códigos de error/status de protocolo | Identificadores técnicos estables; la UI muestra su traducción. |

Los nombres de los Markdown aportados se conservan por trazabilidad aunque algunos estén en portugués. Eso no impone el idioma de operación del producto.

## 2. Textos de UI

- No dispersar textos repetidos en widgets o recibir mensajes traducidos como modelo de dominio.
- Al crear UI, elegir un mecanismo ligero de catálogo. Evaluar la generación estándar de Flutter (`gen-l10n`/ARB) si aporta valor; no añadir una librería externa de traducción sin justificarla.
- Mantener el catálogo inicial en español; los locales regionales y fallback se definen según usuarios, no por fiscalidad de otro proyecto.
- Si el primer prototipo usa un catálogo simple, centralizar claves y registrar su evolución; no afirmar que ya existe i18n completo.
- Usar placeholders para valores; no concatenar frases que luego sean difíciles de traducir. Considerar plurales cuando aparezcan contadores.
- Incluir labels accesibles, errores y estados, no solo títulos.

## 3. Datos y presentación

- API: fechas ISO-8601 y números sin separadores visuales; moneda con código explícito. Un locale no determina la moneda del dato.
- UI: formatear fecha, hora, decimal y moneda según locale/contexto elegidos.
- No imponer una cifra decimal, formato brasileño o abreviaturas K/M a todas las cantidades. Precisión y redondeo dependen del dominio.
- Mostrar timezone cuando sea relevante; no convertir una fecha de calendario en instante por accidente.
- Respetar nombres propios y texto libre del usuario; no traducirlo ni restringirlo a ASCII.

## 4. Evolución

Añadir portugués, inglés u otro idioma cuando haya usuarios o un requisito. Confirmar traducciones y probar expansión de textos, plurales y controles. No renombrar identificadores técnicos de un contrato desplegado para traducir la UI.

La documentación no obliga a implementar un selector de idioma hoy ni a mantener tres catálogos vacíos.

**Checklist:** [ ] inglés técnico coherente; [ ] español documental; [ ] textos centralizados; [ ] códigos traducibles; [ ] formatos acordes al dato; [ ] prueba de textos largos.
