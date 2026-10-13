# Dart: lógica tipada y aislada

**Estado:** guía activa. Complementa [FLUTTER.md](FLUTTER.md); no define reglas autoritativas del backend.

## 1. Capas

- **Dominio y modelos:** Dart puro, sin Flutter, `BuildContext`, widgets ni dependencias de navegador.
- **Acceso a datos:** clientes HTTP y conversión de DTO; consumen el contrato de [API_CONTRACT.md](API_CONTRACT.md), no una base de datos.
- **Adaptadores de plataforma:** navegador o almacenamiento, fuera del dominio puro.
- **Presentación:** puede usar primitives de Flutter como `ChangeNotifier` cuando sea adecuado. No llamar a esa capa «Dart puro» ni prohibirle Flutter por una regla de otro proyecto.

## 2. Tipado y null safety

- Usar modelos explícitos, enums cerrados cuando representen un protocolo y colecciones tipadas.
- Mantener `dynamic` o `Map<String, dynamic>` en la frontera de JSON; validar y convertir antes de exponerlo al resto de la aplicación.
- No usar casts a `dynamic` ni `!` para esconder un dato ausente. Distinguir ausencia válida de respuesta inválida.
- `firstWhere` debe devolver el tipo de la colección también en `orElse`. Para una búsqueda nullable, usar una API nullable disponible o un helper tipado, por ejemplo:

```dart
T? findOrNull<T>(Iterable<T> values, bool Function(T) matches) {
  for (final value in values) {
    if (matches(value)) return value;
  }
  return null;
}
```

No añadir una librería solo por ese helper si ya existe una solución simple.

## 3. Asincronía y errores

- Declarar `Future<T>`/`Stream<T>` con tipos; esperar las operaciones cuyo resultado importa.
- Diferenciar errores de transporte, contrato y dominio. No convertir cualquier excepción en lista vacía o éxito.
- Configurar timeouts en el cliente HTTP. Reintentar solo errores transitorios y operaciones seguras; un timeout de una escritura no demuestra que el servidor no la ejecutó.
- Mantener la lógica de cancelación o descarte de respuestas obsoletas explícita.
- Los servicios devuelven datos o errores tipados; la UI traduce los mensajes para el usuario.

## 4. Datos y convenciones

- Archivos `snake_case.dart`; tipos `UpperCamelCase`; miembros `lowerCamelCase`.
- Fechas intercambiadas en ISO-8601 con zona cuando representan instantes; localizar solo al mostrar.
- Importes exactos según contrato: unidades menores enteras o decimal como string, sin asumir que un `double` sirve para cualquier operación monetaria.
- La validación cliente mejora UX, pero el backend sigue siendo la autoridad.
- No incluir credenciales, SQL ni reglas que permitan saltar autorización en el cliente.

## 5. Pruebas

Probar lógica pura sin `pumpWidget`: JSON válido/inválido, nulls, límites, cálculos y errores. Probar HTTP con un transporte falso o inyectable, sin tocar producción. Mantener las pruebas bajo `frontend/test/` cuando exista la aplicación.

**Checklist:** [ ] tipos explícitos; [ ] JSON validado; [ ] ausencia tratada; [ ] sin Flutter en dominio; [ ] error visible; [ ] pruebas de límites y asincronía.

**Descartado del origen:** allowlists de transición, imports de archivos inexistentes y degradar colecciones a `dynamic` para aceptar `null`.
