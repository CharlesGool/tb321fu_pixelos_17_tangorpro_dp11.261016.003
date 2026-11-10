---
name: project-log-es
description: Errores, limitaciones, decisiones y traspaso del proyecto
metadata:
  version: "1.0.0"
  lang: "es"
---

# Registro

## Multilingüe

[简体中文](../LOG.md) | [English](../en/LOG.md) | **Español**

## Documentación

- Descripción general del proyecto: [README](README.md)

- Justificación del diseño: [DESIGN](DESIGN.md)

- Estado del proyecto: [LOG](LOG.md)
- Registros históricos: [HISTORY](HISTORY.md)
- Historial de cambios: [CHANGELOG](CHANGELOG.md)

- Avisos de terceros: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Registros

- [Registros históricos](HISTORY.md)
- [Historial de cambios](CHANGELOG.md)

## Errores

No hay errores confirmados.

## Limitaciones

- El paquete base es la imagen Android 17 QPR3 Beta 3 de Google, no un sistema de versión oficial.
- La imagen del sistema incluye una semilla de keybox de respaldo; caduca o queda invalidada si se revoca, y a partir de ese momento la autenticación de claves requiere que el usuario importe un keybox válido. La validez de la semilla de esta versión no se ha revisado.
- El flasheo conservando datos solo se aplica si el dispositivo ya tenía el sistema Android 17 de este proyecto.
- Con la suplantación global activada, el "Número de versión" de Ajustes muestra el valor del modelo suplantado; para saber qué versión se ha flasheado, consulte la propiedad `ro.tb321fu.mod_version`.
- La marca de agua de la cámara de Lenovo no funciona: requiere que el HAL de cámara reconozca la cámara de Lenovo, y esta versión no lo implementa.
- La cámara de Lenovo necesita leer el modelo real; si el modelo se cambia por un medio distinto de la suplantación global, puede que no funcione.
- Los auriculares de modo dual emparejados antes necesitan reiniciar el Bluetooth una vez tras activar "Audio LE" en los detalles del auricular.
- El desbloqueo facial es biometría WEAK: sirve para desbloquear y para verificaciones débiles, pero no autoriza pagos ni abre credenciales de StrongBox.
- La animación de arranque se orienta según la dirección en que se sujeta la tableta en horizontal; sujetándola en vertical, la imagen se ve de lado.
- Al ocultar la barra de navegación queda un espacio de unos 3 píxeles en la parte inferior.
- Con el modo escritorio sin límite de aplicaciones, si hay muchas ventanas abiertas a la vez la presión sobre la memoria es alta y el sistema recupera las aplicaciones en segundo plano.

## Decisiones

| Fecha | Decisión | Estado | Motivo |
| --- | --- | --- | --- |
| 2026-11-06 | Una versión base por repositorio | Aceptada | Coincide con la línea de Android 15 |
| 2026-11-06 | Los Release solo incluyen el paquete 9008 completo, no variantes con root ni parches incrementales | Aceptada | Coincide con la línea de Android 15 |
| 2026-11-06 | Volúmenes 7z de 2000 MiB cada uno | Aceptada | Por debajo del límite de 2 GiB por archivo de GitHub |
| 2026-11-06 | La numeración de versiones es por línea de versión base; la primera versión de este repositorio es user-v1.0 | Aceptada | Cada línea de versión base se numera desde v1.0 |
| 2026-11-06 | Paquete base `DP11.261016.003`, no `CP3A.260905.009` | Aceptada | El paquete base CP3A no arranca en este equipo; el paquete DP11 arranca con normalidad |
| 2026-11-06 | La cámara integrada es la cámara original de Lenovo, no la de Pixel | Aceptada | La cámara de Pixel funciona de forma intermitente en Android 17 |
| 2026-11-06 | Añadir una marca propia al último campo de `ro.system_ext.build.fingerprint` en cada versión | Aceptada | Con huellas idénticas, flashear conservando datos no cuenta como actualización del sistema y se reutilizan los registros anteriores; por eso el WebView de user-v1.1 no podía cargar su biblioteca nativa. Solo cambia la huella de system_ext; `ro.build.fingerprint`, que usa la certificación, no cambia |
| 2026-11-06 | Licencias de terceros sin verificar al hacer público el repositorio | Aceptada | Sigue lo hecho en el repositorio de Android 15; la verificación queda como paso pendiente |

## Traspaso

Consulte el traspaso vigente en el [LOG.md en chino simplificado](../LOG.md#交接).
