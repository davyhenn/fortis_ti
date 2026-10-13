# Módulos: entregar una funcionalidad completa y pequeña

**Estado:** usar cuando el usuario defina la primera funcionalidad. No existe un catálogo de módulos del producto.

## 1. Definir el flujo antes de la estructura

Un módulo agrupa una responsabilidad del producto; no implica automáticamente pestañas, permisos granulares, tablas y múltiples endpoints.

Antes de crearlo, acordar:

- Usuario, problema y criterio de aceptación.
- Entrada, resultado y datos necesarios.
- Navegación y estados de carga/vacío/error.
- Necesidad real de backend, persistencia o acceso restringido.
- Qué queda explícitamente fuera de esta entrega.

No suponer funcionalidades por el nombre Fortis TI ni copiar áreas del otro sistema.

## 2. Integración vertical mínima

1. Acordar una propuesta visual si hay una pantalla o identidad nueva: [DESIGN.md](DESIGN.md).
2. Definir modelos y contrato si hay comunicación servidor: [API_CONTRACT.md](API_CONTRACT.md).
3. Crear una entrada de navegación útil y una pantalla integrada, no un archivo sin consumidor.
4. Implementar el caso mínimo con servicio/repositorio apropiado y backend real cuando corresponda.
5. Completar estados, validaciones, errores y pruebas; validar el flujo en navegador.

Una estructura navegable puede servir para aprobar UX, pero no equivale a una función terminada. No crear endpoints «dev» por cada pestaña ni exponer placeholders como capacidades disponibles en producción.

## 3. Estructura orientativa

```text
frontend/lib/features/<feature>/
  presentation/   # pantalla, widgets y estado de UI
  domain/         # modelos/reglas cliente si son necesarios
  data/           # cliente/repositorio si consume datos
backend/          # handlers y servicios de la misma capacidad
```

Crear solo las capas necesarias. No pasar un `Map<String, dynamic> userData` gigante a toda pantalla; preferir modelos y dependencias tipados con el mínimo contexto. Las pestañas se agregan por necesidad de navegación, no por convención universal.

## 4. Acceso y datos

- Si hay restricciones, definir acciones/recursos concretos y aplicarlas server-side; la UI refleja los permisos, no los concede.
- Identidad y actor de auditoría se obtienen del contexto validado.
- No inventar organizaciones, nombres de tablas o relaciones. Si un dato necesario falta, manejar el estado explícitamente.
- Añadir persistencia y migración solo si el flujo la requiere y la elección del motor está aprobada.
- Una pantalla de solo cliente no necesita un endpoint artificial para cumplir una checklist.

## 5. Definición de terminado

- [ ] Objetivo y alcance confirmados.
- [ ] Navegación y enlaces directos coherentes.
- [ ] UI acordada y accesible; estados completos.
- [ ] Contrato/modelos alineados cuando hay API.
- [ ] Validación y acceso probados cuando aplican.
- [ ] Tests pertinentes y comprobación de navegador.
- [ ] Guías actualizadas si se aprobó una decisión nueva.
- [ ] Commit y push a `master` según [AGENTS.md](../../AGENTS.md).

**Diferido:** launcher universal, jerarquía de permisos por cada pestaña, widgets del producto anterior y scaffold con código vacío para módulos aún no pedidos.
