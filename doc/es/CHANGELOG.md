---
name: project-changelog-es
description: Historial de cambios
metadata:
  version: "1.0.0"
  lang: "es"
---

# Historial de cambios

## Multilingüe

[简体中文](../CHANGELOG.md) | [English](../en/CHANGELOG.md) | **Español**

## Documentación

- Descripción general del proyecto: [README](README.md)

- Justificación del diseño: [DESIGN](DESIGN.md)

- Estado del proyecto: [LOG](LOG.md)
- Registros históricos: [HISTORY](HISTORY.md)
- Historial de cambios: [CHANGELOG](CHANGELOG.md)

- Avisos de terceros: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Historial de cambios

### user-v1.2(V3)- 2026-11-06

- Nota: primera versión pública de este repositorio. La Release de user-v1.0 no se terminó de subir y se retiró por no tener un WebView funcional; user-v1.1 (V2) no se publicó y sus cambios se incluyen aquí.
- Corregido: el sistema no tenía un WebView funcional, y el inicio de sesión de la cuenta de Google y pantallas similares se cerraban. El nuevo módulo obligatorio `webview-full` incluye el Android System WebView completo 156.0.8078.47 (desde user-v1.1).
- Corregido: tras flashear una versión nueva conservando datos, el sistema no volvía a analizar las aplicaciones del sistema y reutilizaba los registros de la versión anterior (tras flashear user-v1.1 conservando datos, WebView seguía sin poder cargar su biblioteca nativa). Ahora cada versión añade una marca propia a `ro.system_ext.build.fingerprint` (`identity` V1.4); `ro.build.fingerprint` no cambia.
- Cambiado: número de módulos 34 → 35 (31 obligatorios y 4 opcionales).

### user-v1.0(V1)- 2026-11-06

- Añadido: primera Release de este repositorio, con los volúmenes del paquete de flasheo 9008 completo para el paquete base Android 17 QPR3 Beta 3 (`DP11.261016.003`), `SHA256SUMS.txt` y la lista de módulos.
- Nota: la línea de versión base de Android 17 se numera de forma independiente desde v1.0, y su conjunto de parches está alineado con la línea de Android 15: todos los módulos obligatorios más 4 opcionales, 34 en total.
- Cambiado: el desbloqueo facial, la fluidez de pantalla, la corrección de 6GHz y Tango se han readaptado a Android 17.
- Cambiado: la cámara pasa a ser la cámara original de Lenovo integrada, y se corrige su falso aviso de "memoria llena" antes de tomar fotos en Android 17 (el obturador no respondía).
- Cambiado: solo se eliminan 8 aplicaciones de Google (Calculadora, Calendario, Contactos, Consejos de Pixel, Mensajes, Kids Space, Google Play Libros, YouTube Kids); se conservan el resto, incluido Bienestar digital.
- Añadido: la animación de arranque se ha convertido al tamaño del panel vertical de este equipo, corrigiendo la proporción.
- Añadido: el modo escritorio no limita el número de aplicaciones abiertas a la vez (antes, un máximo de 4).
- Añadido: la gestión de la batería se incluye en los módulos obligatorios (consulta de vida útil, protección de carga prolongada, límite de carga y carga de bypass).
- Corregido: el aviso de arranque "Se ha producido un problema interno en el dispositivo" (versiones no coincidentes).
- Corregido: el "Número de versión" de Ajustes mostraba siempre `Ported.Using.MysticGSI.Tool`.
