---
name: project-third-party-notices-es
description: Atribuciones y avisos de cumplimiento de terceros
metadata:
  version: "1.0.0"
  lang: "es"
---

# Avisos de terceros

## Multilingüe

[简体中文](../THIRD_PARTY_NOTICES.md) | [English](../en/THIRD_PARTY_NOTICES.md) | **Español**

## Documentación

- Descripción general del proyecto: [README](README.md)

- Justificación del diseño: [DESIGN](DESIGN.md)

- Estado del proyecto: [LOG](LOG.md)
- Registros históricos: [HISTORY](HISTORY.md)
- Historial de cambios: [CHANGELOG](CHANGELOG.md)

- Avisos de terceros: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Avisos de terceros

### Componentes

| Componente | Versión o hash | URL de origen | Licencia | Uso | Titular de los derechos | Obligaciones de distribución | Verificado el |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Imagen oficial de Google para Pixel Tablet (convertida a GSI con la herramienta MysticGSI) | `DP11.261016.003`, incremental 16708287 | No verificado | No verificado | Paquete base, repaquetado tras modificarlo | Google | No verificado | No verificado |
| Desbloqueo facial (portado de ParanoidSense de PixelOS, incluye el SDK de Megvii) | No verificado | https://github.com/CharlesGool/paranoidsense-gsi-port | No verificado | Compilado como la aplicación de sistema FaceUnlock, incluida en la imagen del sistema | PixelOS / Megvii | No verificado | No verificado |
| Firmware original de ZUI de Lenovo y herramienta 9008 | ZUI 17.0.12.044 | No verificado | No verificado | Se distribuye dentro del paquete 9008 con los firmwares de vendor, boot, etc. y las herramientas de flasheo | Lenovo | No verificado | No verificado |
| Cámara de Lenovo (ZuiCamera) | 17.0.2.250609115111 | No verificado | No verificado | Tomada del firmware original de ZUI, parcheada y refirmada; instalada como `system/priv-app` | Lenovo | No verificado | No verificado |
| OhMyKeymint (bifurcación del motor de certificación) | Commit base bafe5ba | https://github.com/ITxiao6666/OhMyKeymint | No verificado (el upstream incluye el texto de AGPL-3.0 y un acuerdo adicional de qwq233) | Compilado como `keymint` e `inject`, incluido en la imagen del sistema | James Clef / qwq233 | No verificado | No verificado |
| Tango (traducción binaria de aplicaciones de 32 bits) | v2.1.0 | No verificado | No verificado | Traductor y otros componentes incluidos en la imagen del sistema | No verificado | No verificado | No verificado |

### Textos de licencia

Los componentes no verificados todavía no tienen texto de licencia. Hasta completarlo, este archivo no constituye una licencia para esos componentes.

### Oferta de código fuente

El código fuente del motor de certificación está en el repositorio [herald-core](https://github.com/CharlesGool/herald-core), en `engine/`. Los demás componentes no están verificados.

### Revisión de cumplimiento

No realizada. OhMyKeymint está sujeto a copyleft; todavía no hay registro de análisis de distribución y separación.
