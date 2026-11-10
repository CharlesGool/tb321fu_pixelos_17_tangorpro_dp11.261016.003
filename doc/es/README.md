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

Paquete 9008 del GSI de Android 17 de Pixel Tablet para la Lenovo Legion Y700 de tercera generación (TB321FU). El paquete base es la imagen oficial de Google Android 17 QPR3 Beta 3 (`DP11.261016.003`), un sistema en fase Beta. Versión oficial actual: **user-v2.0** (versión interna V23); los cambios están más abajo en [Novedades](#novedades-user-v20) y los problemas conocidos en [LOG](LOG.md#limitaciones).

> En Releases hay por ahora el paquete completo de user-v1.2 y parches incrementales escalonados para cada versión posterior. Para una instalación limpia, flashee primero el paquete completo de user-v1.2 y luego actualice paso a paso; véanse [Instalación](#instalación) y [Actualización](#actualización).

### Lo principal, antes de empezar

1. Este paquete 9008 se puede flashear sin desbloquear el BL. Está firmado y no requiere desactivar la verificación de vbmeta. Incluye el firmware 9008 completo de la ZUI 17.0.12.044 original de Lenovo, por lo que no hace falta flashear antes un firmware oficial.
2. Todas las funciones siguientes están implementadas sin root.
3. El paquete base es una versión Beta de Google, por lo que el propio sistema puede tener problemas de esa fase.
4. Este paquete se puede compilar automáticamente. Con un PC Linux, puede cambiar el GSI y la versión de la base y compilarlo usted mismo (la base y el firmware original deben aportarse); véase [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods).

### Novedades (user-v2.0)

Respecto a user-v1.9, los módulos pasan de 38 a 48. El registro punto por punto está en [CHANGELOG](CHANGELOG.md).

Añadido:

- **Niveles de la linterna**: el control de brillo que aparece al mantener pulsado el mosaico de la linterna ahora funciona de verdad, con 4 niveles distintos. El HAL de cámara original fija los dos LED a 75 mA cada uno e incluso apaga el segundo al cambiar de nivel con la linterna encendida; ahora la corriente del flash se reescribe según el nivel. Encender en nivel 1 o 2 desde apagado ya no da antes un destello fuerte, y funciona antes del primer desbloqueo tras reiniciar.
- **Vibración de doble motor**: un HAL de vibración del lado del sistema reconoce los dos motores Awinic, con primitivas, composiciones y vibración sincronizada; la respuesta háptica en apps de terceros funciona.
- **Tres apps integradas en Ajustes**, sin emparejamiento, y los privilegios avanzados quedan fuera del alcance del usuario normal:
  - Gestión de la batería: Ajustes → Batería → Gestión de la batería.
  - Suplantación de modelo y atestación de claves: Ajustes → Acerca de la tableta → debajo de Modelo.
  - Ocultar la barra de navegación: Ajustes → Sistema → Modo de navegación → parte inferior de la página del engranaje.
- **Retroceso predictivo**: las tres apps anteriores y la cámara Lenovo admiten la animación del gesto de retroceso de Android 13+.
- **Cerrar la funda apaga la pantalla y abrirla la enciende**; los bordes siguen la rotación al sostenerla en horizontal.
- **Mosaico "Brillo automático" en Ajustes rápidos**; una pulsación larga abre Pantalla y táctil.
- **Tratamiento como tableta solo Wi-Fi**: se quitan las funciones de doble SIM y telefonía.
- **Audio y vídeo en el uso de la batería**: el consumo de audio y vídeo ya no aparece como 0.

Corregido:

- **Fallo de keystore2**: tras reiniciarse herald (por fallo o al matarlo), keystore2 ya no sufre un segfault con él ni se bloquea el keystore, así que compartir Wi-Fi y otras operaciones que requieren autenticación ya no se cierran. La causa era que el script de arranque sobrescribía en el mismo sitio la carga inject que keystore2 tenía mapeada; ahora se sustituye de forma atómica.
- **Bucle de fallos de qesdk**: se añade `lseek` a la política seccomp del servicio qesdk de Qualcomm, que ya no falla cada 5 segundos.
- **Errores de init al arrancar**: se añaden los tipos SELinux de dos scripts de depuración QTI usados por el rc original de vendor, e init ya no recibe SIGABRT dos veces al arrancar.
- **dmesg saturado**: se silencian los registros de denegaciones SELinux de herald, que ya no desplazan otros registros del kernel.

### Módulos

Cada módulo está descrito en [modules-user-v2.0](../modules-user-v2.0.md). Esta versión tiene 48 módulos: 44 obligatorios y 4 opcionales. Abajo se enumeran por categorías todas las funciones de esta versión.

### Identidad y autenticación

- El modelo del dispositivo se restaura a TB321FU (`ro.product.*`); los campos de versión del sistema mantienen los valores originales de Android 17, sin declarar falsamente el Android 15 de fábrica.
- Motor de atestación de claves sin root: permite importar un keybox y emitir claves RKP localmente (basado en las funciones del módulo Oh My Keymint). La app "Suplantación de modelo y atestación de claves" está en Ajustes → Acerca de la tableta → debajo de Modelo.
- Suplantación global del modelo (módulo opcional `global-pif`): suplanta con un solo paso Pixel Tablet u otros modelos. Al activarla, el "Número de versión" en Ajustes también muestra el valor del modelo suplantado, y pueden detectarse montajes anómalos del sistema. Si no la quiere, no la active.
- `herald-core`: punto de entrada de comandos y plugins sin root (`heraldctl`); las aplicaciones de frontend lo usan para controlar funciones del sistema.
- `herald-apps`: canal dedicado para las apps integradas en la partición del sistema, sin emparejamiento; cada app solo puede ejecutar los comandos de su propia lista permitida, y funciona también antes del desbloqueo.

### Cámara

- Cámara original de Lenovo (ZuiCamera) integrada, tomada del firmware ZUI original: funcionan la cámara principal, la frontal y la macro. Se corrigen sus cierres, avisos de permisos y la falsa comprobación de "almacenamiento lleno" en GSI (en Android 17 la comprobación original siempre daba lleno y el obturador no respondía). Admite retroceso predictivo.
- Corrección de la cámara macro: la cámara 2 (macro) está disponible para apps de cámara de terceros.

### Desbloqueo facial

- Desbloqueo facial, portado de ParanoidSense de PixelOS (SDK de Megvii); usa la cámara frontal y sirve para desbloquear la pantalla y para escenarios de apps que no sean pagos. Es biometría WEAK (Clase 2) y no puede autorizar pagos ni abrir credenciales StrongBox. Los ganchos del framework se readaptaron al `services.jar` de Android 17.

### Pantalla y visualización

- Adaptación de la animación de arranque: la de Pixel Tablet está hecha para dimensiones horizontales y se ve deformada en el panel vertical de este dispositivo, así que se convierte a su tamaño. La imagen queda derecha con la tableta en horizontal.
- Corrección del brillo automático: restaura el interruptor de brillo adaptable en Ajustes, usa la curva y velocidad de respuesta originales de Lenovo y restaura el nivel manual mínimo original. Ajustes rápidos tiene un mosaico "Brillo automático"; una pulsación larga abre Pantalla y táctil.
- Corrección de la orientación: Pixel Tablet y este dispositivo tienen orientaciones predeterminadas distintas; las apps solo horizontales (como Bilibili a pantalla completa) ya no aparecen al revés.
- Cerrar la funda apaga la pantalla y abrirla la enciende; los bordes siguen la rotación al sostenerla en horizontal.
- Tocar dos veces para activar: Ajustes → Pantalla → Otros controles de pantalla → Tocar para activar; con la pantalla apagada, dos toques la encienden.
- Corrección de la tasa de refresco adaptable: restaura los temporizadores de inactividad/toque y la detección de contenido originales; baja la tasa en reposo, la sube al tocar y ajusta al vídeo según su fotograma.
- Niveles de visualización fluida: en Ajustes, "Smooth Display" activado = sin límite (máximo del panel 165Hz), desactivado = 120Hz.
- Rotación libre: el giro automático funciona a 90/180/270 grados, incluido el vertical invertido.
- Apps ilimitadas en modo escritorio: el modo escritorio de Android 17 permite por defecto 4 apps a la vez como máximo; se elimina ese límite.

### Vibración y linterna

- HAL de vibración de doble motor: se reconocen los dos motores Awinic, con primitivas, composiciones y vibración sincronizada; la respuesta háptica en apps de terceros funciona.
- Niveles de la linterna: mantenga pulsado el mosaico de la linterna para ajustar el brillo; los niveles 1 a 4 se aplican de verdad, y encender en nivel 1 o 2 desde apagado ya no da antes un destello fuerte.

### Batería

- App de gestión de la batería (Ajustes → Batería → Gestión de la batería) y el comando `heraldctl battery`: consulta de ciclos y salud de las dos celdas, protección de carga prolongada, límite de carga, carga de bypass (con el cargador conectado, este alimenta el equipo directamente y la batería ni carga ni descarga) y estado en tiempo real.
- Audio y vídeo en el uso de la batería: el consumo de audio y vídeo ya no aparece como 0.

### Red y Bluetooth

- Tratamiento como tableta solo Wi-Fi: se quitan las funciones de doble SIM y telefonía.
- Correcciones de la banda Wi-Fi de 6GHz y del código de país, con el APEX de Wi-Fi reempaquetado.
- Corrección de la interfaz del punto de acceso: Pixel Tablet oculta por defecto la entrada del punto de acceso Wi-Fi; se restauran la entrada en Ajustes y el mosaico de Ajustes rápidos.
- Audio Bluetooth: A2DP y LE Audio (LC3) usan la descarga por hardware del ADSP de Qualcomm, con la política de audio original de vendor; "Audio LE" se puede activar en los detalles del auricular; se corrige el retraso de unos 60 segundos en el audio de voz entrante con LE Audio. Se mantiene el parche de llamadas SCO.

### Compatibilidad de aplicaciones

- Tango portado, con soporte para APK de 32 bits, así que se pueden usar apps de 32 bits (basado en ColorOS Android 15 del OnePlus 12).
- Android System WebView completo integrado (156.0.8078.47), disponible desde el arranque; el "paquete stub + extracción en el primer arranque" original de la imagen Pixel no se podía extraer en este dispositivo, lo que hacía que pantallas como el inicio de sesión de Google se cerraran.
- Corrección de vídeo congelado: al desplazarse por vídeos en X y apps similares, la imagen ya no se queda congelada mientras sigue el sonido.

### Reducción del sistema

- Se eliminan las apps y datos exclusivos de Pixel, se recortan los componentes de hardware Pixel que este dispositivo no tiene y se limpian los restos del GSI (Mystic).
- Solo se eliminan 8 apps de Google: Calculadora, Calendario, Contactos, Mi Pixel, Mensajes, Kids Space, Play Libros y YouTube Kids. Las demás apps de Google (incluido Bienestar digital) se conservan.
- Se elimina Now Playing y sus modelos acústicos, que ya no ocupan el audio.

### Limpieza y cierre

- Depuración desactivada (la imagen original tenía adb root/depuración activados por defecto).
- Rastros de identidad del GSI ocultos (contra la detección de GSI).
- Se elimina el aviso de arranque "Se ha producido un problema interno con el dispositivo".
- El "Número de versión" en Ajustes muestra la versión de este paquete.
- Se unifica la cadena del compilador del kernel, se limpian precompilados obsoletos, se reempaqueta boot y se vuelve a firmar para el AVB de este dispositivo.

### Interfaz

- La barra de navegación se puede ocultar (en modo tableta y en modo teléfono): el interruptor al final de Ajustes → Sistema → Modo de navegación → página del engranaje, o `heraldctl navbar hide/show/status`. Solo se aplica con navegación por gestos y vuelve a mostrarse automáticamente al cambiar a navegación con tres botones. Al ocultarla queda un espacio de unos 3 píxeles en la parte inferior (para que el gesto de retroceso siga funcionando).

### Correcciones de estabilidad

- Correcciones de reglas SELinux: Enforcing desde el arranque.
- Corrección del dominio de DeviceConnectionService, que ya no falla cada 10 segundos.
- keystore2 ya no falla tras reiniciarse herald, así que compartir Wi-Fi y similares ya no se cierran.
- El servicio qesdk de Qualcomm ya no falla cada 5 segundos.
- init ya no recibe SIGABRT al arrancar; las denegaciones SELinux de herald ya no saturan dmesg.

Los parches y el sistema de compilación están en [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods). Cada versión base tiene su propio repositorio; este solo corresponde a Android 17 `DP11.261016.003`. Para Android 15 véanse [tb321fu_pixelos_15_tangorpro_bp1a.250505.005](https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_bp1a.250505.005) y [tb321fu_pixelos_15_tangorpro_ap3a.241005.015](https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_ap3a.241005.015).

## Requisitos

- Lenovo Legion Y700 de tercera generación TB321FU, capaz de entrar en el modo 9008 (EDL).
- PC con Windows, cable USB y 7-Zip.
- Para actualizar con parches incrementales hace falta `xdelta3.exe` para Windows (descárguelo usted; no viene en los parches).
- El flasheo conservando datos solo se aplica si el dispositivo ya tenía este sistema del proyecto con Android 17; en los demás casos, use el flasheo con borrado de datos.

## Instalación

### Instalación rápida

Por ahora el único paquete completo es user-v1.2. Descargue todos los volúmenes (`.7z.001`, `.7z.002`, `.7z.003`) desde la [Release de user-v1.2](https://github.com/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003/releases/tag/user-v1.2) y colóquelos en el mismo directorio. Descomprima `.7z.001` con 7-Zip. Con el dispositivo en modo 9008, ejecute `flash.bat` en el directorio descomprimido (borra los datos). El paquete incluye el firmware 9008 completo de Lenovo; no hace falta flashear antes un firmware oficial. Después, siga [Actualización](#actualización) paso a paso hasta user-v2.0.

### Instalación normal

```bash
sha256sum -c SHA256SUMS.txt
7z x pixelos_tangorpro_dp11.261016.003_user-v1.2_optional.7z.001
```

Para flashear conservando datos, ejecute `flash_keep_data.bat`. Ambos scripts están en la raíz del directorio descomprimido; al ejecutarlos, introduzca el número de puerto 9008 (por ejemplo `COM18`). Si tras el flasheo el equipo no se enciende solo, mantenga pulsados el botón de encendido y el de bajar volumen durante más de 8 segundos.

## Guía

| Archivo | Contenido |
| --- | --- |
| `…user-v1.2_optional.7z.001`, `.002`, `.003` | Volúmenes 7z del paquete 9008 completo, de como máximo 2000 MiB cada uno; solo en la Release de user-v1.2 |
| `…user-vX_patch_from_vY.zip` | Parche incremental (uno por Release desde user-v1.3) de la versión anterior a esta: xdelta de la imagen del sistema, `vbmeta_system.img`, textos y `升级说明.txt` (notas de actualización) |
| `SHA256SUMS.txt` | Sumas de verificación de los archivos de esta Release |
| `module-list.md` | Lista de módulos de esta versión, igual que [modules-user-v2.0](../modules-user-v2.0.md) |

### Uso del desbloqueo facial

Ajustes → Seguridad y privacidad → Desbloqueo del dispositivo → Desbloqueo facial → introduzca el PIN → complete el registro; después, basta con mirar a la cámara frontal en la pantalla de bloqueo. Es biometría WEAK, **prohibido** usarla para proteger aplicaciones de pagos o bancarias.

### Gestión de la batería

Abra Ajustes → Batería → Gestión de la batería para consultar y cambiar los ajustes en la app. También puede usar el comando `heraldctl battery` (requiere una terminal o una aplicación de frontend emparejada con herald):

```bash
heraldctl battery info              # ciclos de las dos celdas, salud, fecha
heraldctl battery protect on 75 80  # protección de carga prolongada: detiene la carga al 80% y vuelve a cargar al bajar al 75%; protect off la desactiva
heraldctl battery limit 80          # límite de carga del 80%; limit off lo desactiva
heraldctl battery bypass on         # carga de bypass; solo se considera activa si la respuesta incluye verified=yes; bypass off la desactiva
```

### Suplantación de modelo y atestación de claves

Se abre desde Ajustes → Acerca de la tableta → debajo de Modelo; gestione en la app la suplantación de modelo y la atestación de claves. El equivalente en línea de comandos es `heraldctl spoof tablet` / `heraldctl spoof on <modelo>`.

### Ocultar la barra de navegación

Active "Ocultar barra de navegación" al final de Ajustes → Sistema → Modo de navegación → página del engranaje. Requiere navegación por gestos; vuelve a mostrarse automáticamente al cambiar a tres botones y se oculta de nuevo al volver a gestos. El ajuste se mantiene tras reiniciar.

### Niveles de la linterna

Despliegue los Ajustes rápidos, mantenga pulsado el mosaico de la linterna para mostrar el control de brillo y llévelo a un nivel de 1 a 4.

### Identificación de versión

La versión pública es `user-vMAJOR.MINOR`; el número interno `V<número>` está escrito en la imagen. user-v2.0 = V23. Cada línea de versión base se numera desde su propio v1.0. Con la suplantación global desactivada, en Ajustes → Acerca de la tableta → "Número de versión" aparece `pixelos_tangorpro_dp11.261016.003_user-v2.0_optional`. Cada versión añade una marca propia al último campo de `ro.system_ext.build.fingerprint` (`.72bef0d4` en user-v2.0), para que al flashear conservando datos el sistema vuelva a analizar las aplicaciones del sistema como en una actualización.

## Actualización

El parche incremental de cada Release solo actualiza desde la versión anterior, así que hay que actualizar paso a paso:

| Versión actual | Parches que aplicar, en orden |
| --- | --- |
| user-v1.2 | v1.3 → v1.4 → v1.5 → v1.9 → v2.0 |
| user-v1.3 | v1.4 → v1.5 → v1.9 → v2.0 |
| user-v1.4 | v1.5 → v1.9 → v2.0 |
| user-v1.5 | v1.9 → v2.0 |
| user-v1.9 | v2.0 |

Cada paso (detalles en el `升级说明.txt` del parche):

1. Compruebe que el sha256 de `image\super_4.img` en su directorio del paquete actual coincide con el valor base de las notas.
2. Restaure el nuevo `super_4.img` con `xdelta3 -d` y compruebe el sha256 del resultado; si no coincide, bórrelo y no lo flashee.
3. Sobrescriba los archivos del mismo nombre en el directorio del paquete con el nuevo `super_4.img`, el `vbmeta_system.img` del parche y los textos.
4. Puede restaurar hasta user-v2.0 y flashear una sola vez; flashee con `flash_keep_data.bat` (conserva los datos). El primer arranque es más lento de lo normal.

Si viene del sistema original de fábrica, de la línea Android 15 o de otro sistema, ejecute primero `flash.bat` (borra los datos) con el paquete completo de user-v1.2 y luego actualice según la tabla. Verificación: tras arrancar, el número de versión en Ajustes debe contener `user-v2.0`.

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
