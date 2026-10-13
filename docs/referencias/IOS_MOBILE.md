# iOS y móvil: alcance diferido

> [!IMPORTANT]
> El destino confirmado es web. Una web responsive en un teléfono no requiere una app iOS o Android nativa.

## 1. Qué aplicamos ahora

- Layout adaptable, controles utilizables con touch y scroll con constraints correctas.
- Probar pantallas pequeñas, teclado y texto aumentado; no fijar el diseño a un monitor de escritorio.
- Tipado y null safety de [DART.md](DART.md), sin casts a `dynamic` para ocultar errores.
- Aislar APIs de plataforma para no romper el build web.
- Usar HTTPS y un contrato que no presuponga un cliente nativo.

Estas reglas están en [FLUTTER.md](FLUTTER.md) y [DESIGN_SYSTEM.md](DESIGN_SYSTEM.md); no necesitan dos árboles de navegación.

## 2. Qué no se implementa ni instala

- Proyecto iOS/Android, pantallas `IosHomeScreen` o navegación nativa separada.
- Xcode, CocoaPods, Swift Package Manager, Android SDK o herramientas de tiendas para cumplir una guía.
- IPA/APK, firma, certificados, permisos de `Info.plist` o pipelines de publicación nativa.
- Una lista de módulos móviles o dominio de API heredados de otro producto.

No elegir SPM frente a CocoaPods por una prohibición del origen; sería una decisión técnica futura según SDK y plugins compatibles.

## 3. Disparador para reconsiderar

El usuario pide expresamente una aplicación nativa y existe una necesidad que la web no cubre adecuadamente: distribución en tiendas, una integración de hardware concreta o capacidad offline definida, por ejemplo.

Entonces realizar primero una evaluación de alcance, usuarios, APIs de plataforma, dependencias, autenticación, operación y coste. Compilar iOS requiere un entorno macOS/Xcode compatible, no solo el Windows actual.

La evaluación no autoriza migrar todo el producto ni instalar herramientas automáticamente. Acordar el primer flujo nativo, su compatibilidad con web y sus criterios de aceptación antes de crear scaffold.

**Checklist actual:** [ ] web responsive; [ ] tipos correctos; [ ] APIs web aisladas; [ ] sin dependencias nativas innecesarias.

**Resumen:** conservar calidad multiplataforma donde beneficia a web; diferir toda infraestructura y publicación nativa.
