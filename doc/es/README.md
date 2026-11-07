# tb321fu_pixelos_17_tangorpro_dp11.261016.003

[简体中文](../../README.md) | [English](../en/README.md) | **Español**

Paquete de flasheo 9008 para la Lenovo Legion Y700 de tercera generación (TB321FU) con el GSI de Android 17 de Pixel Tablet (`DP11.261016.003`).

[![Release](https://img.shields.io/github/v/release/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003?logo=github)](https://github.com/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003/releases/latest)

## Documentación

- Descripción general del proyecto: [README](README.md)

- Justificación del diseño: [DESIGN](DESIGN.md)

- Estado del proyecto: [LOG](LOG.md)
- Registros históricos: [HISTORY](HISTORY.md)
- Historial de cambios: [CHANGELOG](CHANGELOG.md)

- Avisos de terceros: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Introducción

Paquete 9008 del GSI de Android 17 de Pixel Tablet para la Lenovo Legion Y700 de tercera generación (TB321FU). El paquete base es la imagen oficial de Google Android 17 QPR3 Beta 3 (`DP11.261016.003`), un sistema en fase Beta. Versión oficial actual: user-v1.5 (versión interna V6), que sube `now-playing` a V1.1 sobre v1.4 (eliminando también los cuatro modelos acústicos de ASI, de modo que Now Playing ya no se inicia automáticamente). **Nota: user-v1.5 se ofrece como parche incremental desde user-v1.4 en Releases; el paquete completo se añadirá más adelante. Para actualizar desde una versión anterior, aplique los parches en orden (v1.2→v1.3→v1.4→v1.5); para una instalación limpia, use por ahora el paquete completo de user-v1.2.** Los problemas conocidos están en [LOG](LOG.md#limitaciones).

### Lo principal, antes de empezar

1. Este paquete 9008 se puede flashear sin desbloquear el BL. Está firmado y no requiere desactivar la verificación de vbmeta. Incluye el firmware 9008 completo de la ZUI 17.0.12.044 original de Lenovo, por lo que no hace falta flashear antes un firmware oficial.
2. Todas las funciones siguientes están implementadas sin root.
3. El paquete base es una versión Beta de Google, por lo que el propio sistema puede tener problemas de esa fase.
4. Este paquete se puede compilar automáticamente. Con un PC Linux, puede cambiar el GSI y la versión de la base y compilarlo usted mismo (la base y el firmware original deben aportarse); véase [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods).

Cada módulo está descrito en [modules-user-v1.5](../modules-user-v1.5.md). Esta versión tiene 36 módulos: 32 obligatorios y 4 opcionales.

### Identidad y autenticación

- Restaura el modelo como TB321FU (`ro.product.*`); el campo de versión del sistema conserva el valor original de Android 17, sin hacerse pasar por el Android 15 original de fábrica.
- Motor de autenticación de claves sin root: permite importar keybox y emitir claves RKP localmente (tomando como referencia las funciones del módulo Oh My Keymint).
- Suplantación global de modelo (módulo opcional `global-pif`): con un clic se hace pasar por un modelo como Pixel Tablet. Al activarla, el "Número de versión" de Ajustes también muestra el valor del modelo suplantado, y se detecta un montaje anómalo del sistema. Si no la quiere, no la active.
- `herald-core`: punto de entrada de comandos y complementos sin root (`heraldctl`); las aplicaciones de frontend lo usan para controlar funciones del sistema.

### Cámara

- Incluye la cámara original de Lenovo (ZuiCamera), tomada del firmware original de ZUI: cámara principal, frontal y macro funcionan. Se corrigieron en el GSI el cierre inesperado, el aviso de permisos y el falso aviso de "memoria llena" (en Android 17 la comprobación original siempre daba lleno y el obturador no respondía).
- Corrección de la cámara macro: la cámara macro n.º 2 está disponible para cámaras de terceros.

### Desbloqueo facial

- Desbloqueo facial, portado de ParanoidSense de PixelOS (SDK de Megvii), con la cámara frontal; sirve para el bloqueo de pantalla y para aplicaciones en escenarios que no son de pago. Es biometría WEAK (Class 2) y no autoriza pagos ni abre credenciales de StrongBox. Los hooks del framework se han readaptado al `services.jar` de Android 17.

### Pantalla y visualización

- Animación de arranque adaptada: la animación de arranque de Pixel Tablet se hizo para pantalla horizontal y se veía desproporcionada en el panel vertical de este equipo; se ha convertido al tamaño de este equipo. Con la tableta en horizontal, la imagen queda derecha.
- Corrección del brillo automático: restaura el interruptor de brillo adaptativo en Ajustes, usa la curva y la velocidad de respuesta del brillo original de Lenovo, y el nivel mínimo manual vuelve al original.
- Corrección de la orientación de pantalla: el sistema de Pixel Tablet y este equipo tienen orientaciones por defecto distintas; las aplicaciones que solo funcionan en horizontal (como la pantalla completa de B 站) ya no aparecen invertidas.
- Corrección de la tasa de refresco adaptativa: restaura los temporizadores de inactividad y de tacto originales y la detección de contenido; baja con la pantalla estática, sube al tocar, y en vídeo sigue la tasa de fotogramas.
- Niveles de fluidez de pantalla: en Ajustes, con "Fluidez de pantalla" activada no hay límite (el panel llega a 165Hz); desactivada, 120Hz.
- Rotación libre: 90, 180 y 270 grados rotan automáticamente, incluida la vertical invertida.
- Sin límite de aplicaciones en modo escritorio: en Android 17 el modo escritorio admite por defecto hasta 4 aplicaciones abiertas a la vez; se ha quitado el límite.

### Batería

- `heraldctl battery`: consulta los ciclos y el estado de salud de las dos celdas, protección de carga prolongada, límite de carga, y carga de bypass (con el cargador conectado, el equipo funciona con alimentación directa del cargador, sin cargar ni descargar la batería).

### Red y Bluetooth

- Banda Wi-Fi de 6GHz y corrección del código de país; Wi-Fi APEX reempaquetado.
- Corrección de la interfaz de punto de acceso: Pixel Tablet oculta por defecto la entrada del punto de acceso WLAN; se ha restaurado la entrada en Ajustes y el mosaico de ajustes rápidos.
- Corrección del audio Bluetooth: se desactivan el offload A2DP por hardware y LE Audio, y se usa la codificación por software del vendor; se corrigen los auriculares sin sonido y las llamadas que no se oyen.

### Compatibilidad de aplicaciones

- Portado de Tango, con soporte para APK de 32 bits; se pueden usar aplicaciones de 32 bits, tomando como referencia el OnePlus 12 con Android 15 ColorOS.
- Incluye el Android System WebView completo (156.0.8078.47), disponible desde el primer arranque. La combinación original de la imagen Pixel ("paquete de marcador + descompresión en el primer arranque") no se descomprime en este dispositivo, y el inicio de sesión de la cuenta de Google y pantallas similares se cerraban.

### Reducción del sistema

- Elimina las aplicaciones y datos exclusivos de Pixel, elimina los componentes de hardware de Pixel que este equipo no tiene y limpia los restos de GSI (Mystic).
- Aplicaciones de Google: solo se eliminan 8 (Calculadora, Calendario, Contactos, Consejos de Pixel, Mensajes, Kids Space, Google Play Libros, YouTube Kids). Se conservan las demás aplicaciones de Google, incluido Bienestar digital.

### Limpieza y cierre

- Desactiva la depuración (adb root y la depuración vienen activados por defecto en la versión original).
- Oculta las huellas de GSI, para evitar la detección de GSI.
- Elimina la ventana emergente "Se ha producido un problema interno en el dispositivo" al arrancar.
- Unifica la cadena de autoría del kernel, limpia los precompilados caducados, reempaqueta boot y lo refirma para la AVB de este equipo.
- En Ajustes, el "Número de versión" muestra la versión de este paquete.

### Interfaz

- Permite ocultar la barra de navegación (en modo tablet y en modo teléfono). Al ocultarla quedan unos 3 píxeles de espacio en la parte inferior.

### Correcciones de estabilidad

- Corrección de reglas de SELinux: arranca en modo Enforcing desde el inicio.
- Corrección del dominio del servicio de conexión de dispositivos (DeviceConnectionService), que ya no falla cada 10 segundos.

Los parches y el sistema de compilación están en [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods). Cada versión base tiene su propio repositorio; este solo corresponde a Android 17 `DP11.261016.003`. Para Android 15 véanse [tb321fu_pixelos_15_tangorpro_bp1a.250505.005](https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_bp1a.250505.005) y [tb321fu_pixelos_15_tangorpro_ap3a.241005.015](https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_ap3a.241005.015).

## Requisitos

- Lenovo Legion Y700 de tercera generación TB321FU, capaz de entrar en el modo 9008 (EDL).
- PC con Windows, cable USB y 7-Zip.
- El flasheo conservando datos solo se aplica si el dispositivo ya tenía este sistema del proyecto con Android 17; en los demás casos, use el flasheo con borrado de datos.

## Instalación

### Instalación rápida

Descargue todos los volúmenes (`.7z.001`, `.7z.002`, `.7z.003`) desde [Releases](https://github.com/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003/releases/latest) y colóquelos en el mismo directorio. Descomprima `.7z.001` con 7-Zip. Con el dispositivo en modo 9008, ejecute `flash.bat` en el directorio descomprimido (borra los datos). El paquete incluye el firmware 9008 completo de Lenovo; no hace falta flashear antes un firmware oficial.

### Instalación normal

```bash
sha256sum -c SHA256SUMS.txt
7z x pixelos_tangorpro_dp11.261016.003_user-v1.2_optional.7z.001
```

Para flashear conservando datos, ejecute `flash_keep_data.bat`. Ambos scripts están en la raíz del directorio descomprimido; al ejecutarlos, introduzca el número de puerto 9008 (por ejemplo `COM18`). Si tras el flasheo el equipo no se enciende solo, mantenga pulsados el botón de encendido y el de bajar volumen durante más de 8 segundos.

## Guía

| Archivo | Contenido |
| --- | --- |
| `…user-v1.2_optional.7z.001`, `.002`, `.003` | Volúmenes 7z del paquete 9008 completo, de como máximo 2000 MiB cada uno |
| `SHA256SUMS.txt` | Sumas de verificación de los volúmenes; el sha256 del paquete completo tras concatenar los volúmenes figura en las notas del Release correspondiente |
| `module-list.md` | Lista de módulos de esta versión, igual que [modules-user-v1.2](../modules-user-v1.2.md) |

### Uso del desbloqueo facial

Ajustes → Seguridad y privacidad → Desbloqueo del dispositivo → Desbloqueo facial → introduzca el PIN → complete el registro; después, basta con mirar a la cámara frontal en la pantalla de bloqueo. Es biometría WEAK, **prohibido** usarla para proteger aplicaciones de pagos o bancarias.

### Gestión de la batería

Use el comando `heraldctl battery` (requiere una terminal o una aplicación de frontend emparejada con herald):

```bash
heraldctl battery info              # ciclos de las dos celdas, salud, fecha
heraldctl battery protect on 75 80  # protección de carga prolongada: detiene la carga al 80% y vuelve a cargar al bajar al 75%; protect off la desactiva
heraldctl battery limit 80          # límite de carga del 80%; limit off lo desactiva
heraldctl battery bypass on         # carga de bypass; solo se considera activa si la respuesta incluye verified=yes; bypass off la desactiva
```

### Identificación de versión

La versión pública es `user-vMAJOR.MINOR`; el número interno `V<数字>` está escrito en la imagen. user-v1.2 = V3. Cada línea de versión base se numera desde su propio v1.0. Con la suplantación global desactivada, en Ajustes → Acerca de la tableta → "Número de versión" aparece `pixelos_tangorpro_dp11.261016.003_user-v1.2_optional`. Cada versión añade una marca propia al último campo de `ro.system_ext.build.fingerprint` (`.7c5abca8` en user-v1.2), para que al flashear conservando datos el sistema vuelva a analizar las aplicaciones del sistema como en una actualización.

## Actualización

Esta es la primera versión pública de este repositorio (la Release de user-v1.0 no se terminó de subir y se retiró por no tener un WebView funcional; user-v1.1 no se publicó), por lo que no hay una versión pública anterior desde la que actualizar. Si viene del sistema original de fábrica, de Android 15 o de cualquier otro sistema, ejecute `flash.bat` (borra los datos) con el paquete completo. El flasheo conservando datos (`flash_keep_data.bat`) solo se aplica si el dispositivo ya tenía el sistema Android 17 de este proyecto. Verificación: tras arrancar, el número de versión en Ajustes debe contener `user-v1.2`.

## Desinstalación

No hay un método separado para la desinstalación rápida. Desinstalación completa: vuelva a flashear el paquete 9008 del ZUI 17.0.12 original de Lenovo con todo el sistema; los datos se borrarán.

## Agradecimientos

- La imagen oficial de Google Android 17 QPR3 Beta 3 (`DP11.261016.003`) para Pixel Tablet, convertida a GSI con la herramienta MysticGSI, como paquete base.
- El desbloqueo facial está portado de ParanoidSense de PixelOS (SDK de Megvii).
- El motor de certificación es una bifurcación de [ITxiao6666/OhMyKeymint](https://github.com/ITxiao6666/OhMyKeymint) (autor original: James Clef / qwq233).
- El firmware original de ZUI de Lenovo, la cámara de Lenovo y las herramientas de flasheo de Lenovo.

Los datos de autoría y de cumplimiento están en [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md).

## Licencia

Este repositorio todavía no ha elegido una licencia y no concede ninguna. El estado de licencia de cada componente de terceros está en [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md).
