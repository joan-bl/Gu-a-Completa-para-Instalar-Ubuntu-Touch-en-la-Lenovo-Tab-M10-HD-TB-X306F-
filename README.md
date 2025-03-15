# Guía Completa: Instalación de Ubuntu Touch en Lenovo Tab M10 HD (TB-X306F)

## Tabla de Contenidos
1. [Introducción y Requerimientos](#introducción-y-requerimientos)
2. [Preparación del Dispositivo](#preparación-del-dispositivo)
3. [Instalación de Drivers y Herramientas](#instalación-de-drivers-y-herramientas)
4. [Desbloqueo del Bootloader](#desbloqueo-del-bootloader)
5. [Flasheo de Firmware Android 11 Stock](#flasheo-de-firmware-android-11-stock)
6. [Instalación de Ubuntu Touch](#instalación-de-ubuntu-touch)
7. [Resolución de Problemas Comunes](#resolución-de-problemas-comunes)
8. [Alternativas si el Proceso Falla](#alternativas-si-el-proceso-falla)
9. [Referencias y Recursos Adicionales](#referencias-y-recursos-adicionales)

## Introducción y Requerimientos

Este documento proporciona una guía completa para instalar Ubuntu Touch en una tablet Lenovo Tab M10 HD (modelo TB-X306F). El dispositivo utiliza un chipset MediaTek, lo que añade complejidad al proceso en comparación con otros dispositivos.

### Requerimientos:

- **Dispositivo**: Lenovo Tab M10 HD (TB-X306F) con batería cargada al menos al 50%
- **Computadora**: Windows 10/11 (preferiblemente)
- **Conexión USB**: Cable USB de buena calidad que soporte transferencia de datos
- **Respaldo**: Realiza una copia de seguridad de tus datos importantes antes de comenzar
- **Tiempo**: El proceso completo puede llevar entre 1-3 horas
- **Riesgos**: Este proceso puede "brickear" (inutilizar) tu dispositivo si algo sale mal

**NOTA IMPORTANTE**: Este proceso eliminará todos los datos de tu dispositivo. Asegúrate de hacer una copia de seguridad de cualquier información importante.

## Preparación del Dispositivo

### 1. Activar Opciones de Desarrollador:

1. Ve a **Ajustes > Acerca del dispositivo**
2. Toca 7 veces seguidas en **Número de compilación** hasta que aparezca el mensaje "Ya eres un desarrollador"
3. Regresa a **Ajustes** y ahora verás una nueva opción llamada **Opciones de desarrollador**

### 2. Configurar las Opciones de Desarrollador:

1. Ve a **Ajustes > Opciones de desarrollador**
2. Activa **Desbloqueo OEM** (también llamado "OEM Unlocking")
3. Activa **Depuración USB**
4. Desactiva **Verificar aplicaciones por USB**
5. En algunos dispositivos, es necesario también activar **Modo Fastboot**

### 3. Verificar Compatibilidad:

1. Asegúrate de que tu dispositivo es realmente el modelo TB-X306F
2. En **Ajustes > Acerca del dispositivo**, verifica que tienes un procesador MediaTek

## Instalación de Drivers y Herramientas

### 1. Drivers USB MediaTek y Lenovo:

Los drivers MediaTek y Lenovo son esenciales para que tu PC pueda comunicarse correctamente con la tablet en diferentes modos (normal, fastboot, recovery).

1. Descarga los drivers USB MediaTek desde [este enlace](https://www.mediatek.com/products/smartphones/usb-drivers) o [Driver Easy](https://www.drivereasy.com/knowledge/download-and-install-mediatek-usb-drivers/)
2. Descarga los drivers específicos de Lenovo para el modelo TB-X306F desde [el sitio oficial de Lenovo](https://pcsupport.lenovo.com)
3. Instala ambos drivers en tu PC
4. Reinicia el PC después de la instalación

### 2. Plataforma de Herramientas Android (ADB y Fastboot):

1. Descarga la versión oficial de Android Platform Tools desde [aquí](https://developer.android.com/tools/releases/platform-tools)
2. Extrae el contenido a una ubicación fácil de recordar (ej. C:\android-sdk)
3. Añade la ruta a las variables de entorno:
   - Busca "variables de entorno" en Windows
   - Edita la variable PATH
   - Añade la ruta donde extrajiste las herramientas (ej: C:\android-sdk\platform-tools)
4. Abre una nueva ventana de Command Prompt (cmd) como administrador
5. Verifica la instalación con:
   ```
   adb version
   fastboot --version
   ```

### 3. SP Flash Tool y MTK Auth Bypass Tool (Herramientas Específicas para MediaTek):

1. Descarga SP Flash Tool desde [spflashtool.org](https://spflashtool.org/) o [aquí](https://www.spflashtool.com/download)
2. Extrae el contenido a una carpeta dedicada
3. Busca la versión compatible con Windows (generalmente v5.1916 o posterior)
4. Descarga MTK Auth Bypass Tool - esta herramienta puede ser necesaria si tienes problemas con el Secure Boot de tu dispositivo MediaTek
5. Ten en cuenta que algunas versiones de SP Flash Tool funcionan mejor con ciertos dispositivos - si encuentras problemas, prueba con una versión diferente (como v5.1628 o v5.2128)

### 4. UBports Installer:

1. Descarga UBports Installer desde [devices.ubuntu-touch.io/installer](https://devices.ubuntu-touch.io/installer/)
2. Instala la aplicación siguiendo las instrucciones del instalador

## Desbloqueo del Bootloader

El bootloader es lo primero que se ejecuta cuando enciendes el dispositivo. Debe desbloquearse para poder instalar sistemas operativos alternativos.

### 1. Preparar el PC y el dispositivo:

1. Conecta la tablet al PC mediante cable USB
2. Asegúrate de que la depuración USB esté activada
3. Cuando aparezca el mensaje "¿Permitir depuración USB?", marca "Siempre permitir desde este equipo" y toca "Permitir"
4. Abre una ventana de Command Prompt como administrador

### 2. Verificar la conexión ADB:

1. Ejecuta:
   ```
   adb devices
   ```
2. Deberías ver tu dispositivo listado con la palabra "device" al lado (no "unauthorized")

### 3. Reiniciar en modo Bootloader:

1. Ejecuta:
   ```
   adb reboot bootloader
   ```
2. La tablet se reiniciará y mostrará una pantalla con "FASTBOOT mode..." o similar

### 4. Verificar conexión en modo Fastboot:

1. Ejecuta:
   ```
   fastboot devices
   ```
2. Deberías ver tu dispositivo listado

**NOTA**: Si el dispositivo no aparece en fastboot devices, pero aparece en el Administrador de dispositivos de Windows (posiblemente como "Android" con una advertencia amarilla), debes instalar el controlador correcto:
   
1. Abre el Administrador de dispositivos
2. Localiza el dispositivo (probablemente aparezca como "Android" con una advertencia)
3. Haz clic derecho y selecciona "Actualizar controlador"
4. Selecciona "Buscar controladores en mi equipo"
5. Selecciona "Elegir de una lista de controladores disponibles en mi equipo"
6. Selecciona "Android Bootloader Interface" o un controlador similar

### 5. Desbloquear el Bootloader:

1. Ejecuta uno de estos comandos (diferentes dispositivos usan diferentes comandos):
   ```
   fastboot oem unlock
   ```
   O:
   ```
   fastboot flashing unlock
   ```

2. En la pantalla de la tablet, usa los botones de volumen para navegar y el botón de encendido para confirmar el desbloqueo
3. El dispositivo se reiniciará y borrará todos los datos durante este proceso

**NOTA**: En algunos dispositivos Lenovo, es necesario obtener un código de desbloqueo del sitio oficial de Lenovo. Si los comandos anteriores no funcionan, visita el sitio de soporte de Lenovo para información específica sobre tu modelo.

**IMPORTANTE**: Tu modelo TB-X306F tiene Secure Boot activado, lo que puede bloquear el proceso de desbloqueo estándar. Si los comandos normales de fastboot no funcionan, es posible que necesites usar MTK Auth Bypass Tool para superar esta protección. En casos extremos, también puedes intentar entrar manualmente en modo bootloader manteniendo presionados los botones de encendido + volumen hacia abajo mientras la tablet está apagada.

## Flasheo de Firmware Android 11 Stock

Según la documentación de UBports, se requiere tener Android 11 stock antes de instalar Ubuntu Touch.

### 1. Obtener el firmware:

1. Descarga el firmware Stock Android 11 para TB-X306F. Puedes encontrarlo en:
   - [Repositorio de la comunidad UBports](https://gitlab.com/ubports/porting/community-ports/android11/lenovo-tab-m10-hd/lenovo-m10-hd)
   - [Sitio oficial de Lenovo](https://pcsupport.lenovo.com/downloads/DS548918)

2. Extrae el contenido a una carpeta dedicada

### 2. Preparar SP Flash Tool:

1. Abre SP Flash Tool (ejecuta Flash_tool.exe como administrador)
2. En la interfaz de SP Flash Tool:
   - Haz clic en la pestaña "Download"
   - Haz clic en "Scatter-loading" y selecciona el archivo scatter.txt o MT6762_Android_scatter.txt del firmware que descargaste
   - Asegúrate de que todas las particiones están seleccionadas
   - Selecciona "Download Only" en el menú desplegable

### 3. Flashear el firmware:

1. Apaga completamente la tablet
2. Haz clic en el botón "Download" en SP Flash Tool
3. Conecta la tablet al PC mientras está apagada
4. SP Flash Tool debería detectar el dispositivo automáticamente y comenzar a flashear
5. Espera a que el proceso termine (aparecerá un círculo verde con "Download OK" cuando se complete)
6. La tablet se reiniciará automáticamente

**NOTA**: Si SP Flash Tool no detecta el dispositivo, prueba con diferentes cables USB, puertos USB, o reinstala los drivers MediaTek.

## Instalación de Ubuntu Touch

Una vez que el dispositivo tiene Android 11 stock y el bootloader desbloqueado, podemos instalar Ubuntu Touch.

### 1. Preparar el dispositivo:

1. Asegúrate de que el dispositivo tiene al menos 50% de batería
2. Activa nuevamente las Opciones de desarrollador y la Depuración USB en Android 11
3. Conecta el dispositivo al PC

### 2. Usar UBports Installer:

1. Abre UBports Installer
2. El instalador debería detectar automáticamente tu dispositivo como "Lenovo Tab M10 HD (TB-X306F)"
3. Sigue las instrucciones en pantalla para instalar Ubuntu Touch
4. El instalador te guiará a través de los diferentes pasos, que pueden incluir:
   - Reinicio en modo fastboot
   - Instalación de una recovery personalizada
   - Instalación del sistema Ubuntu Touch
   - Configuración inicial

5. No desconectes el dispositivo durante todo el proceso

### 3. Configuración inicial de Ubuntu Touch:

1. Una vez completada la instalación, la tablet se reiniciará en Ubuntu Touch
2. Sigue el asistente de configuración inicial para configurar Wi-Fi, idioma, etc.

## Resolución de Problemas Comunes

### Problema 1: ADB no detecta el dispositivo

**Solución**:
- Verifica que la depuración USB esté activada
- Acepta el mensaje de permitir depuración USB en la tablet
- Prueba con diferentes cables USB
- Reinstala los drivers
- Reinicia tanto la tablet como el PC

### Problema 2: Fastboot no detecta el dispositivo

**Solución**:
- Verifica en el Administrador de dispositivos si aparece con una advertencia amarilla
- Instala los controladores correctos manualmente como se explicó anteriormente
- Prueba con el comando `adb reboot bootloader` desde el modo normal
- Intenta mantener presionados los botones de volumen y encendido para entrar en fastboot manualmente

### Problema 3: SP Flash Tool no detecta el dispositivo

**Solución**:
- Asegúrate de que la tablet está completamente apagada antes de conectarla
- Usa un puerto USB diferente (preferiblemente USB 2.0)
- Reinstala los drivers MediaTek
- Prueba una versión diferente de SP Flash Tool
- Consulta la guía específica en [este enlace](https://gist.github.com/thekosmix/1a7fc2308e9c9ea78bb4ad24bc0cda1a)

### Problema 4: Error durante el flasheo

**Solución**:
- Asegúrate de usar el firmware correcto para tu modelo específico
- Verifica que todas las particiones están seleccionadas correctamente
- Intenta flashear solo las particiones esenciales
- Consulta los foros de XDA para soluciones específicas a tu error

### Problema 5: UBports Installer se queda atascado

**Solución**:
- Verifica la conexión a internet
- Intenta ejecutar el instalador como administrador
- Verifica que el bootloader está realmente desbloqueado
- Considera instalar un recovery personalizado manualmente antes de usar UBports Installer
- Consulta la [comunidad de UBports](https://forums.ubports.com/) para asistencia específica

### Problema 6: Bootloop después de instalar Ubuntu Touch

**Solución**:
- Entra en recovery mode (normalmente manteniendo presionado Power + Volumen arriba)
- Realiza un factory reset desde el recovery
- Si eso no funciona, reinstala Android 11 con SP Flash Tool
- Asegúrate de que la versión de Ubuntu Touch es compatible con tu modelo específico
- Intenta flashear un recovery personalizado antes de reinstalar Ubuntu Touch

## Alternativas si el Proceso Falla

Si después de varios intentos no puedes instalar Ubuntu Touch, considera estas alternativas:

### 1. Optimizar Android existente:

1. Realiza un restablecimiento de fábrica
2. Desinstala aplicaciones no esenciales (bloatware)
3. Usa un launcher ligero como Nova Launcher
4. Desactiva animaciones y efectos visuales

### 2. Entorno de programación Python en Android:

1. Instala Termux desde la Play Store
2. Dentro de Termux, ejecuta:
   ```
   pkg update
   pkg upgrade
   pkg install python
   ```
3. Complementa con Pydroid 3 para un IDE más completo

### 3. LineageOS u otras ROM alternativas:

Busca si hay una ROM de LineageOS compatible con tu dispositivo, que suele ser más ligera que la ROM stock.

## Referencias y Recursos Adicionales

- [Repositorio oficial del port para Lenovo Tab M10 HD](https://gitlab.com/ubports/porting/community-ports/android11/lenovo-tab-m10-hd/lenovo-m10-hd)
- [Guía de SP Flash Tool](https://spflashtool.org/)
- [Foros XDA para TB-X306F](https://xdaforums.com/t/lenovo-tb-x306f-how-to-root-and-flash-recovery-help.4236557/)
- [Solución a problemas con SP Flash Tool](https://gist.github.com/thekosmix/1a7fc2308e9c9ea78bb4ad24bc0cda1a)
- [Guía para desbloquear bootloader de Lenovo](https://unlocktechy.com/lenovo-tab-m10-hd-gen-2-unlock-bootloader)
- [Drivers MediaTek USB](https://www.drivereasy.com/knowledge/download-and-install-mediatek-usb-drivers/)
- [Comunidad de UBports](https://forums.ubports.com/)
- [Guía oficial de Ubuntu Touch](https://ubuntu-touch.io/)
- [XDA Developers - Foro General](https://xdaforums.com/)

### Canales de Soporte en Tiempo Real

Para obtener ayuda en tiempo real durante el proceso, considera unirte a estos grupos:
- [Grupo de Telegram de UBports](https://t.me/ubports)
- [Discord de UBports](https://discord.gg/3PFsCmZ)

---

**NOTA FINAL**: Este proceso implica riesgos y tu dispositivo podría quedar inutilizable si algo sale mal. Realiza una copia de seguridad de todos tus datos importantes antes de comenzar. Si no tienes experiencia con estos procedimientos, considera buscar ayuda en foros especializados o comunidades como XDA Developers o UBports.
