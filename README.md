# FUNDAMENTALS WINDOWS

Bienvenidos a la guía básica, para principiantes, del sistema operativo Windows realizada por Ramón Peinado, se van a explicar tanto conceptos básicos como conceptos mas técnicos. Cabe destacar que **las herramientas** usadas para los ejemplos y algunas imágenes se han descargado desde https://learn.microsoft.com/es-es/sysinternals/downloads/sysinternals-suite . Este archivo contiene las herramientas de solución de problemas individuales y los archivos de ayuda.

## CONCEPTOS BASICOS



### Arquitectura y funcionamiento de Windows

------



#### 1. Capas de abstracción de hardware (HAL or Hardware Abstraction Layers).

Definimos como **capa de abstracción de hardware** (**HAL**) al **software** que maneja toda la **comunicación** entre el **hardware y el núcleo/kernel** . El núcleo es el corazón del sistema operativo y tiene control sobre todo el equipo. Se encarga de todas las solicitudes de entrada y salida, la memoria y todos los periféricos conectados al equipo.

Los equipos con sistema operativo Windows utilizan diferentes tipos de hardware. El sistema operativo puede instalarse en un ordenador comprado o en uno ensamblado por el usuario. Al instalar el sistema operativo, este ha de aislarse de las diferencias de hardware. 

En esta imagen podemos observar la arquitectura básica de Windows.

<img src="/img/1ºimagenn.jpg" style="zoom:150%;" />





#### 2. User mode y Kernel mode.

Las **aplicaciones instaladas** se ejecutan en **modo usuario**, mientras que **el código del sistema** operativo se ejecuta en **modo kernel**. El código en modo kernel tiene acceso ilimitado al hardware y puede ejecutar cualquier instrucción de la CPU, además de referenciar cualquier dirección de memoria directamente. Los **fallos en el código del kernel** pueden provocar el **bloqueo/freeze de todo el equipo**, los programas en modo usuario, como las aplicaciones, no tienen acceso directo al hardware ni a las ubicaciones de memoria. **Los fallos en modo usuario** se restringen a la aplicación y **son recuperables**.





#### 3. Sistema de archivos de Windows.

**exFAT:**

- *Se trata de un sistema de archivos **sencillo compatible con muchos sistemas operativos** diferentes.*

- *FAT tiene **limitaciones en cuanto al número de particiones, el tamaño de las particiones y el tamaño de los archivos** que puede manejar, por lo que ya no se suele utilizar en discos duros (HD) ni en unidades de estado sólido (SSD).*

- *Tanto FAT16 como FAT32 están disponibles para su uso, siendo **FAT32 el más común** al disponer de menos restricciones que FAT16.*

**HFS+:**

- *Este sistema de archivos **se utiliza en ordenadores MAC OS X** y permite **nombres de archivo, tamaños de archivo y tamaños de partición** mucho más largos que los sistemas de archivos anteriores.*

- ***Windows puede leer** **datos de** particiones **HFS+**, a pesar de que no es compatible con Windows sin un software especial.*

**EXT:**

- *Este sistema de archivos **se** **utiliza** con ordenadores basados en **Linux**.*
- *Aunque **Windows** no lo admite, **puede leer** datos de **particiones EXT** **con un software** especial.*

**NTFS:**

- *Es el sistema de archivos más utilizado al instalar Windows. **Todas** **las versiones de Windows y Linux** son **compatibles con NTFS**.*

- *Los ordenadores **Mac-OS X sólo pueden leer una partición NTFS**. Pueden escribir en una partición NTFS tras instalar controladores especiales.*



**NTFS** es el sistema de archivos más utilizado en Windows por que es compatible con archivos y particiones de gran tamaño, y es compatible con otros sistemas operativos. Además, NTFS es **confiable** y cuenta con **características de recuperación**, también admite muchas **funciones de seguridad** (Admite cifrado de archivos). El control de acceso a los datos se logra mediante descriptores de seguridad que contienen la propiedad del archivo y los permisos. NTFS también registra múltiples marcas de tiempo para rastrear la actividad de los archivos. 

El formateo de NTFS crea estructuras importantes en el disco:

- ***Sector de arranque de la partición**: es el primer bloque de 16 sectores del disco y contiene la ubicación de la Tabla Maestra de Archivos (MFT or Master File Table). Los últimos 16 sectores contienen una copia del sector de arranque.*
- ***Tabla Maestra de Archivos (MFT)**: contiene las ubicaciones de todos los archivos y directorios en la partición, incluyendo información de seguridad y marcas de tiempo.*
- ***Archivos del sistema**: son archivos ocultos que almacenan información sobre otros volúmenes y atributos de archivos.*
- ***Área de archivos**: es el área principal de la partición donde se almacenan los archivos y directorios.*





#### 4. Proceso de arranque de Windows.

Definimos como **proceso de arranque o boot** a las **acciones que se producen** **entre** el momento en que se **pulsa el botón de encendido del ordenador** **hasta que Windows se carga** completamente.  

<img src="/img/2ºimagenn.jpg" style="zoom:150%;" />

Existen dos tipos de firmware informático para este boot:

- **BIOS**: El firmware fue creado en 1980 y funciona de la misma manera que cuando se creó. El proceso comienza con la fase de inicialización de la BIOS. Aquí es cuando **se inicializan los dispositivos** **de hardware** y se realiza una **auto prueba de encendido** (POST) para asegurarse de que todos estos dispositivos se están comunicando. Cuando **se descubre el disco del sistema, finaliza el POST**. **La última instrucción del POST** es buscar el registro de arranque maestro (MBR or Main Boot Record). **El MBR** **contiene** un pequeño **programa** que se **encarga de localizar y cargar el sistema operativo**. La BIOS ejecuta este código y el sistema operativo comienza a cargarse.

- **UEFI**: UEFI **diseñado para sustituir a BIOS** y adaptarse a las nuevas funciones las cuales BIOS no cumplía, **la UEFI luce un diseño mucho más amigable** y actual que la BIOS. La UEFi también mejora en funciones, pues **puede actualizarse conectándose a Internet**, además el **arranque** es **mas rápido y seguro** (gracias al Secure Boot).





#### 5. Inicio de Windows.

Hay dos elementos importantes del **registro**(explicado mas adelante) que se utilizan para **iniciar automáticamente aplicaciones y servicios**:

- **HKEY_LOCAL_MACHINE**: Se almacenan varios aspectos de la configuración de Windows, incluida la información sobre los servicios que se inician con cada arranque.

- **HKEY_CURRENT_USER**:  Se almacenan varios aspectos relacionados con el usuario que ha iniciado sesión, incluyendo información sobre los servicios que se inician sólo cuando este usuario inicia sesión.

**Diferentes entradas** en estas ubicaciones **del registro definen qué servicios y aplicaciones se iniciarán**, tal y como indica su tipo de entrada. Estos tipos incluyen *Run, RunOnce, RunServices, RunServicesOnce y Userinit*. Estas entradas se pueden introducir manualmente en el registro, pero es mucho más seguro utilizar **la herramienta Msconfig.exe.**

La herramienta Msconfig abre la ventana de Configuración del Sistema: 

<img src="/img/3ºimagenn.PNG" style="zoom:150%;" />

- **General**: Se pueden elegir tres tipos de arranque diferentes. Normal carga todos los controladores y servicios. Diagnóstico carga sólo los controladores y servicios básicos. Selectivo permite al usuario elegir qué cargar en el arranque.


- **Boot**: Se puede elegir cualquier sistema operativo instalado para arrancar. También hay opciones para el arranque seguro, que se utiliza para solucionar problemas de inicio.

- **Services**: los servicios instalados se enumeran aquí para que puedan ser elegidos para iniciarse en el arranque.

- **Startup**: Todas las aplicaciones y servicios configurados para iniciarse automáticamente al arrancar pueden activarse o desactivarse abriendo el administrador de tareas desde esta pestaña

- **Tools**: Muchas herramientas habituales del sistema operativo pueden iniciarse directamente desde esta pestaña.





#### 6. Apagado de Windows.

Si se apaga la alimentación sin previo aviso al sistema operativo, los archivos abiertos, los servicios cerrados en el orden incorrecto y las aplicaciones bloqueadas pueden causar daño al ordenador. El ordenador necesita tiempo para cerrar cada aplicación, apagar cada servicio y registrar cualquier cambio de configuración antes de que se pierda la alimentación.

**Mientras apagamos el pc**, el **ordenador** **cerrará primero** las **aplicaciones en modo usuario** y **luego** los **procesos** en **modo kerne**l. Si un proceso en modo usuario no responde en cierto tiempo, el sistema operativo mostrará una notificación y permitirá al usuario esperar a que la aplicación responda o finalizar el proceso de forma forzada.

Hay tres opciones diferentes para apagar el ordenador:

- **Apagar** - Apaga el ordenador (power off).
- **Reiniciar** - Reinicia el ordenador (apagado y encendido).
- **Hibernar** - Registra el estado actual del ordenador y del entorno de usuario y lo almacena en un archivo. La hibernación permite a los usuarios reanudar la sesión donde la dejaron muy rápidamente.





#### 7. Procesos, hilos y servicios.

Se define como **proceso** a **cualquier programa que se esté ejecutando en el momento**. Cada proceso que se ejecuta se compone de al menos un hilo o "*thread*". 

Podemos definir un **hilo** de procesamiento como el **flujo de control de datos de un programa**. Es un medio que **permite administrar las tareas de un procesador y de sus diferentes núcleos de una forma más eficiente**. 

Dicho de otra forma, **cada hilo de procesamiento contiene un trozo de la tarea a realizar**, esto es más simple de realizar que si introducimos la tarea completa en el núcleo físico. De esta forma **la CPU es capaz de procesar varias tareas al mismo tiempo** y de forma simultánea, de hecho, podrá hacer tantas tareas como hilos tenga, y **normalmente son una o dos por cada núcleo**.

Los hilos dedicados a un proceso están contenidos en el mismo espacio de direcciones. Esto significa que estos hilos no pueden acceder al espacio de direcciones de ningún otro proceso. Esto esta hecho asi para evitar la corrupción de otros procesos.

Los servicios proporcionan funciones de larga duración, como la conexión inalámbrica o el acceso a un servidor FTP. Algunos programas dependen de uno o más servicios para funcionar correctamente. El cierre de un servicio puede afectar negativamente a las aplicaciones o a otros servicios.





#### 8. Asignación de Memoria.

Un **ordenador** funciona **almacena**ndo **instrucciones** **en** la memoria **RAM** **hasta que la CPU las procesa**. El espacio de direcciones virtuales de un proceso es el conjunto de direcciones virtuales que el proceso puede utilizar.

**Cada proceso** en un **ordenador Windows de 32 bits** **admite** un **espacio de direcciones virtuales** que **permite abordar hasta 4 gigabytes**. Cada proceso en un o**rdenador Windows de 64 bits** admite un **espacio de direcciones virtuales** de **8 terabytes**

Cada proceso en el espacio de usuario se ejecuta en un espacio de direcciones privado, separado de otros procesos en el espacio de usuario. Cuando el proceso en el espacio de usuario necesita acceder a recursos del kernel, debe utilizar un identificador de proceso.

Una **herramienta** poderosa **para** **ver** la **asignación** de **memoria** es **RAMMap**, es parte de la suite de herramientas Windows Sysinternals. 

<img src="/img/4ºimagenn.PNG" style="zoom:150%;" />





#### 9. Registro de Windows.

La información sobre hardware, aplicaciones, usuarios y configuraciones del sistema en Windows se almacena en una base de datos llamada registro. Esta base de datos registra también las interacciones entre estos elementos, como los archivos que una aplicación abre y los detalles de propiedades de carpetas y aplicaciones. El registro es una base de datos jerárquica en la que el nivel más alto se conoce como "colmena", seguido de claves y subclaves. Los valores almacenan datos y se encuentran en las claves y subclaves. 

- **HKEY_CURRENT_USER (HKCU)**: Contiene información sobre el usuario conectado en ese momento.
- **HKEY_USER (HKU)**: Contiene información sobre todas las cuentas de usuario del host.
- **HKEY_CLASSES_ROOT (HKCR)**: Contiene información sobre los registros de vinculación e incrustación de objetos (OLE). OLE permite a los usuarios incrustar objetos de otras aplicaciones (como una hoja de cálculo) en un único documento (como un documento de Word).
- **HKEY_LOCAL_MACHINE (HKLM)**: Contiene información relacionada con el sistema.
- **HKEY_CURRENT_CONFIG (HKCC)**: Contiene información sobre el perfil de hardware actual.

No se pueden crear nuevas colmenas. Una cuenta con privilegios administrativos puede crear, modificar o eliminar las claves y valores del registro. La herramienta regedit.exe se utiliza para modificar el registro.

Las claves de registro pueden contener una subclave o un valor. Estos son los distintos valores que pueden contener.

- **REG_BINARY** - Números o valores booleanos
- **REG_DWORD** - Números mayores de 32 bits o datos sin procesar
- **REG_SZ** - Valores de cadena

**Las aplicaciones** potencialmente **maliciosas** pueden **agregar claves** al **registro** para iniciarse automáticamente al encender el ordenador. Durante un arranque normal, estas aplicaciones no mostrarán ninguna ventana o indicación visible al usuario, ya que su ejecución está oculta en el registro. Esto **puede ser especialmente peligroso si se trata de un keylogger** u otro tipo de software malicioso que comprometa la seguridad del equipo sin el conocimiento ni el consentimiento del usuario. Al realizar auditorías de seguridad o reparar sistemas infectados, es fundamental revisar las ubicaciones de inicio de las aplicaciones en el registro para asegurarse de que cada elemento sea reconocido y seguro antes de ejecutarlo.





#### 10. Uso de herramientas para la exploración de procesos, hilos y registro.

Vamos a usar algunas de las herramientas proporcionadas en SysInternals Suite

**Explorar Procesos:**

Vamos a realizar la **exploración de un proceso activo** mediante la herramienta **procexp.exe de SysInternals Suite**

<img src="/img/5ºimagenn.PNG" style="zoom:150%;" />

La manera mas simple de encontrar un proceso activo es **la mira** de la barra superior <img src="/img/6ºimagenn.jpg" style="zoom: 80%;" />esta herramienta nos deja apuntar hacia una ventana para encontrar el proceso correspondiente a esta. Una vez ubicada **podemos matar el proceso con el boton derecho/boton DEL**, esto nos cerrara la ventana.

<img src="/img/7ºimagenn.PNG" style="zoom:150%;" />

**Si por ejemplo ejecutamos un ping** dentro de nuestra consola de comandos y analizamos los procesos, **podemos observar el proceso principal y un proceso secundario** que se encuentra activo hasta que finaliza nuestro comando.

<img src="/img/9ºimagenn.PNG" style="zoom:150%;" />

Se puede observar que tenemos otro **proceso secundario llamado conhost.exe**, este puede ser sospechoso, **nuestra herramienta** dispone de **una opción para comprobar si hay contenido malicioso**. Para comprobarlo seleccionamos el proceso secundario, botón derecho y clicamos en Check VirusTotal, se nos mostrara entonces otra columna correspondiente a VirusTotal. Si clicamos directamente nos hará una análisis dentro de la pagina virustotal.com. 

<img src="/img/10ºimagenn.PNG" style="zoom:150%;" />

Si clicamos directamente sobre el 0/75 nos hará una **análisis dentro de la pagina virustotal.com y su base de datos.**  

<img src="/img/11ºimagenn.PNG" style="zoom:150%;" />





**Exploración de Hilos e Identificadores**

Para acceder a la información de hilos dentro de nuestra herramienta **procexp.exe** clicamos en el proceso secundario y **accedemos a propiedades**.



<img src="/img/12ºimagenn.PNG"  />

Podemos observar también los identificadores, estos apuntan a archivos claves de registro e hilos

<img src="/img/13ºimagenn.PNG"  />





**Explorando el Registro de Windows**

Anteriormente hemos aceptado el acuerdo de licencia o EULA del software ProcessExplorer, vamos a ubicarlo en el registro y a modificarlo de manera que nos vuelva a salir.

**HKEY_CURRENT_USER** > **Software** > **Sysinternals** > **Process Explorer**<img src="/img/14ºimagenn.PNG"  />

Y buscamos EulaAccepted, actualmente el valor en el registro ha de ser 1, 0x00000001

<img src="/img/15ºimagenn.PNG"  />

Lo modificamos a 0 

<img src="/img/16ºimagenn.PNG"  />

Y el programa de ProccesExplorer nos volvera a pedir que aceptemos el acuerdo de licencia al inciar el .exe

<img src="/img/17ºimagenn.PNG"  />







### Configuración y Monitorización

------





#### 1. Ejecutar como administrador.

Como medida de seguridad recomendada, no es aconsejable iniciar sesión en Windows utilizando la cuenta de Administrador o una cuenta con privilegios administrativos. Si un malware se hace con privilegios administrativos, tendrá acceso total a todos los archivos y carpetas del equipo.

A veces, es necesario ejecutar o instalar software que requiere privilegios de administrador. Para hacerlo, existen dos formas diferentes de realizarlo.

- **Administrador**: Clic con el botón derecho en el comando en el Explorador de archivos de Windows y elija Ejecutar como administrador.
- **Símbolo del sistema como Admin**: El comando que se ejecute desde esta línea de comandos se realizará con privilegios de Administrador.





#### 2. **Usuarios locales y dominios**.

Cuando se inicia un nuevo equipo o se instala Windows, se solicita que cree una cuenta. Esta cuenta, conocida como **usuario local**, contiene todos los **ajustes personalizados, permisos de acceso, ubicaciones de archivos** y otros **datos específicos del usuario**. Además, hay otras dos cuentas preexistentes: **invitado y administrador**. Estas cuentas están desactivadas de forma predeterminada, no se recomienda habilitar la cuenta de administrador ni dar privilegios administrativos a los usuarios estándar. En caso de que un usuario necesite realizar una función que requiera privilegios administrativos, el sistema solicitará la contraseña de administrador y solo permitirá llevar a cabo esa tarea.

Para facilitar la administración de usuarios, Windows utiliza grupos. Un grupo tiene un nombre y un conjunto específico de permisos asociados. Cuando un usuario se agrega a un grupo, hereda los permisos otorgados a dicho grupo. Un usuario puede formar parte de varios grupos para obtener diferentes conjuntos de permisos. **Windows incluye varios grupos de usuarios integrados** que se utilizan para tareas específicas. **La gestión de usuarios y grupos locales se realiza a través de la herramienta "lusrmgr.msc"** del Panel de control.

<img src="/img/18ºimagenn.PNG"  />

Windows también utiliza dominios para establecer permisos. Un **dominio es un servicio de red que almacena y administra** todas las **cuentas de usuario, grupos, computadoras, periféricos y configuraciones de seguridad** en una base de datos. Esta base de datos se encuentra en computadoras especiales llamadas controladores de dominio (Domain Controller).Cada usuario y equipo en el dominio debe autenticarse con el DC para iniciar sesión y acceder a los recursos de la red. Las configuraciones de seguridad para cada usuario y equipo son establecidas por el DC.





#### 3. CLI y PowerShell

La **interfaz de línea de comandos (CLI)** de Windows se usa para **ejecutar programas**, **navegar** por el sistema de archivos **y gestionar archivos y carpetas**. Se pueden crear archivos por lotes (batch files) para ejecutar múltiples comandos en sucesión, de manera parecida a un script.

Para abrir la CLI de Windows, busca cmd.exe y haz clic en el programa. Se ofrece la opción de Ejecutar como administrador, lo que otorga mucho más poder a los comandos que se utilizarán.

Características de la CLI:

- Los nombres de archivos y rutas no distinguen entre mayúsculas y minúsculas de forma predeterminada. 
- Los dispositivos de almacenamiento se les asigna una letra como referencia. 
- La letra de la unidad va seguida de dos puntos y una barra invertida (∖). Esto indica la raíz, o nivel más alto, del dispositivo. 
- La jerarquía de carpetas y archivos en el dispositivo se indica separándolos con una barra invertida.
- Los comandos que tienen opciones opcionales utilizan la barra diagonal (/) para delimitar entre el comando y la opción de la opción. 
- La tecla Tab sirve para autocompletar comandos.
- Windows guarda un historial de los comandos.
- Accede a los comandos ingresados previamente utilizando las teclas.

**COMANDOS CLI MAS USADOS Y UTILES**:

| COMANDO                       | DESCRIPCIÓN                                                  |
| :---------------------------- | :----------------------------------------------------------- |
| `cd `                         | Uno de los comandos más esenciales de la consola de Windows. Sirve para cambiar de directorio, utilizando la fórmula *cd < RutaDirectorio >* para ir al directorio o carpeta concreta que le digas |
| `dir`                         | El comando lista el contenido del directorio o carpeta donde te encuentras, mostrando todas las subcarpetas o archivos que tiene. |
| `tree nombrecarpeta`          | Te muestra el árbol de directorios de una carpeta concreta que le digas |
| `cls`                         | Limpia la ventana de la consola de Windows, borrando todo lo que se ha escrito en ella, tanto tus comandos como las respuestas de la propia consola. |
| `exit`                        | Cierra la ventana de la consola de Windows.                  |
| `help`                        | Muestra todos los comandos que hay disponibles, poniendo en cada uno una breve descripción en inglés. |
| `copy "archivo" "destino"`    | Copia uno o más archivos en la dirección que tu elijas.      |
| `robocopy`                    | Una función mejorada más rápida y eficiente, y que permite hacer acciones como cancelar o retomar la copia. Muestra también un indicador de progreso. |
| `move `                       | Mueve el archivo concreto que quieras del lugar o carpeta en el que está a otra dirección que le digas. Es como copiar, pero sin dejar el archivo en su ubicación original. |
| `del "archivo" "destino"`     | Borra el archivo o carpeta que le indiques.                  |
| `rename "archivo" `           | Te permite cambiarle el nombre al archivo que consideres oportuno, e incluso incluyendo su extensión también puedes cambiarla. Aunque será un cambio como el que haces en la interfaz principal de Windows, sin conversión y sin que implique que va a funcionar bien con la nueva extensión. |
| `md "nombredecarpeta"`        | Crea una carpeta con el nombre que le asignes en la dirección en la que te encuentres en ese momento. |
| `type "archivo"."extension" ` | Te permite crear un archivo desde la propia ventana de comandos. Esto quiere decir que no sólo vas a crear un archivo, sino que también podrás escribir el texto que quieras en su interior. |
| `format`                      | Sirve para formatear la unidad                               |

**COMANDOS CLI DIAGNOSTICO**

| COMANDO                  | DESCRIPCIÓN                                                  |
| :----------------------- | :----------------------------------------------------------- |
| `systeminfo`             | Sirve para obtener la información sobre el ordenador o sistema en el que estás trabajando. Da datos como el nombre del sistema, el procesador, la memoria RAM, la placa base o el almacenamiento interno que tienes. |
| `chkdsk`                 | Realiza un análisis de la superficie del disco duro para detectar fallos como posibles sectores defectuosos, y también hace comprobaciones en la estructura lógica del sistema de archivos y repara cualquier error (archivos perdidos, nombres sin sentido, carpetas a las que no se puede acceder, etc.). |
| `ipconfig`               | Muestra la información de tu conexión, incluyendo tu dirección IP, la máscara de subred o la puerta de enlace por defecto. |
| `netstat`                | Analiza y muestra las estadísticas del protocolo y las conexiones TCP/IP en uso por tus dispositivos. Con ello, puedes solucionar posibles problemas de conexión mirando el estado de los puertos y conexiones de tu equipo |
| `nslookup`               | Permite hacer consultas al sistema de nombres de dominio (DNS). |
| `tracert "direcionhost"` | Te ayuda a saber el camino que sigue tu conexión hasta llegar al host que le indiques. |
| `getmac`                 | Muestra la dirección MAC de tu ordenador.                    |
| `ver`                    | Devuelve la versión numérica exacta de tu sistema operativo. |
| `time`                   | Muestra la hora exacta que tiene tu ordenador.               |
| `driverquery`            | Te muestra la lista completa de todos los drivers que tienes instalados en el ordenador, con su nombre de módulo, nombre completo y el tipo de controlador del que se trata. |
| `tasklist`               | Te muestra la lista completa de todos los procesos que tienes en ejecución en tu sistema, así como la cantidad de memoria que está utilizando cada uno de ellos. . |
| `sfc`                    | Examina la integridad de todos los archivos de tu sistema y reemplaza los que detecte que estén dañados utilizando las copias en caché del sistema. Necesita permisos de administrador |
| `cleanmgr`               | Cuando lo escribas te aparecerá una ventana emergente pidiéndote que selecciones una unidad de disco. Lo que habrás hecho es lanzar la aplicación de Windows para liberar espacio. |
| `winsat formal`          | La consola de Windows ejecuta un benchmark completo que analiza el rendimiento del equipo y todos sus componentes. Este comando, el WINSAT, también puede ser acompañado de otros apellidos más allá de FORMAL, como por ejemplo CPUFORMAL para medir sólo el rendimiento de la CPU, MEMFORMAL para el de la RAM, GRAPHICSFORMAL para la tarjeta gráfica o DISKFORMAL para las unidades de almacenamiento. |
| `defrag`                 | Inicia la desfragmentación del disco duro que le indiques. Igual que la aplicación nativa que Windows tiene para ello. |
| `diskpart`               | Escribe este comando añadiéndole los atributos LIST DISK o LIST VOLUME para obtener un listado de los discos o volúmenes del equipo con esta función para gestionar las particiones y discos duros. |



**Windows PowerShell**, se utiliza para **crear scripts y automatizar tareas** que la CLI regular no puede realizar. PowerShell también proporciona una CLI para iniciar comandos. PowerShell es un programa integrado en Windows y se puede abrir buscando "powershell" y haciendo clic en el programa. PowerShell también se puede ejecutar con privilegios de administrador.

Estos son los tipos de comandos que PowerShell puede ejecutar:

- Cmdlets: estos comandos realizan una acción y devuelven una salida u objeto al siguiente comando que se ejecutará. 
- Scripts de PowerShell: son archivos con extensión .ps1 que contienen comandos de PowerShell que se ejecutan. 
- Funciones de PowerShell: son fragmentos de código que se pueden hacer referencia en un script.

Existen cuatro niveles de ayuda en Windows PowerShell:

- get-help comando PS: muestra ayuda básica para un comando. 
- get-help comando PS [-examples]: muestra ayuda básica para un comando con ejemplos. 
- get-help comando PS [-detailed]: muestra ayuda detallada para un comando con ejemplos. 
- get-help comando PS [-full]: muestra toda la información de ayuda para un comando con ejemplos en mayor profundidad.



**COMANDOS ESENCIALES DE POWERSHELL**

| COMANDO                | DESCRIPCIÓN                                                  |
| :--------------------- | :----------------------------------------------------------- |
| `Get-Command`          | Escribiendo en la consola «Get-Command -Name <nombre>» obtenemos un conjunto de comandos que incluyen ese nombre en específico. |
| `Get-Host`             | Se obtiene la versión de Windows PowerShell que está usando el sistema. |
| `Get-History`          | Con este comando se obtiene un historial de todos los comandos que se ejecutaron bajo una sesión de PowerShell y que actualmente se encuentran ejecutándose. |
| `Get-Random`           | Se obtiene un número aleatorio entre 0 y 2.147.483.646.      |
| `Get-Service`          | Brindará información acerca de los servicios que se están ejecutando y los que ya fueron detenidos. |
| `Get-Help`             | Una ayuda básica para conocer más acerca de los cmdlets y sus funciones. |
| `Get-Date`             | Para saber de una forma rápida qué día fue en una determinada fecha del pasado, usando este comando se obtendrá el día exacto. |
| `Copy-Item`            | Con este comando se pueden copiar carpetas o archivos.       |
| `Invoke-Command`       | En el momento en que quieras ejecutar un script o un comando PowerShell (de forma local o remota, en uno o varios ordenadores), «Invoke-Command» va a ser tu mejor opción. Es simple de utilizar y te ayudará a gestionar ordenadores por lotes.Es necesario tipear Invoke-Command junto al script o comando con su localización exacta. |
| `Invoke-Expression`    | Con Invoke-Expression se ejecuta otra expresión o comando. Si te encuentras ingresando una cadena de entrada o una expresión, en primer lugar este comando la va a analizar y a continuación la ejecutará. |
| `Invoke-WebRequest`    | Similar a cURL en Linux, se puede hacer un inicio de sesión, un scraping y la descarga de información relacionada a servicios y páginas web. |
| `Set-ExecutionPolicy ` | Si bien podemos crear e iniciar scripts (.ps1) desde PowerShell, vamos a encontrarnos limitados debido a cuestiones de seguridad. Sin embargo, esto puede ser modificado a través de la categoría de seguridad empleando el cmdlet Set-ExecutionPolicy. Solo es necesario tipear Set-ExecutionPolicy junto a una de las cuatro opciones de seguridad para hacer los cambios que se requieren:  Restricted, All Signed, Remote Signed, Unrestricted. |
| `Get-Item`             | En caso de que estés buscando información acerca de un elemento con una ubicación concreta, como podría ser un directorio en el disco duro. |
| `Remove-Item`          | En caso de que desees borrar elementos como carpetas, archivos, funciones y variables y claves del registro. |
| `Get-Content`          | Cuando necesites todo lo que incluye en cuanto a contenido un archivo de texto en una ruta concreta, ábrelo y léelo utilizando un editor de textos como el Bloc de Notas. |
| `Set-Content`          | Es posible almacenar texto en un archivo, algo parecido a lo que se puede hacer con «echo» en el Bash. |
| `Get-Variable`         | Si estás en PowerShell tratando de utilizar variables, esto podrá ser hecho con el cmdlet Get-Variable, con el que vas a poder visualizar dichos valores. Este comando muestra los valores en una tabla, desde donde se pueden utilizar, incluir y excluir comodines. |
| `Set-Variable`         | El valor de una variable puede ser establecido, modificado o reinicializado con este cmdlet. |
| `Get-Process`          | Obtendrá la lista de procesos activos en ese momento.        |
| `Start-Process`        | Con este cmdlet, Windows PowerShell hace que sea mucho más fácil ejecutar procesos en el equipo. |
| `Stop-Process`         | Con este cmdlet puedes detener un proceso, ya sea que haya sido iniciado por ti o por otro usuario. |
| `Start-Service`        | Si necesitas comenzar un servicio en el PC.                  |





#### 4. Instrumentación de Administración Windows

**Windows Management Instrumentation (WMI)** se utiliza para **administrar equipos remotos**. **Permite obtener información** sobre los componentes del equipo, **estadísticas de hardware y software**, y **monitorear** el estado de **salud** de los **equipos** remotas. 

Para abrir el control de WMI desde el Panel de control, debes hacer doble clic en *Herramientas administrativas > Administración de equipos para abrir la ventana de Administración de equipos*, luego *expandir el árbol de Servicios* y aplicaciones y hacer *clic derecho en el icono de Control de WMI > Propiedades*.

<img src="/img/19ºimagenn.PNG"  />

- General: Proporciona información resumida sobre la computadora local y WMI. 
- Copia de seguridad/ restauración: Permite realizar copias de seguridad manuales de las estadísticas recopiladas por WMI. 
- Seguridad: Configuración para determinar quién tiene acceso a diferentes estadísticas de WMI. 
- Avanzado: Configuración para establecer el espacio de nombres predeterminado para WMI.

**El acceso a WMI debe estar estrictamente limitado** para mantener seguro a nuestro equipo.





#### 5. El comando Net

El comando "**net**" es una **herramienta fundamental para la administración y el mantenimiento del sistema operativo**. Este comando ofrece una variedad de subcomandos que se pueden combinar con interruptores para obtener resultados específicos y enfocados en diferentes aspectos del sistema.

- `net accounts`: Establece los requisitos de inicio de sesión y contraseña para los usuarios
- `net session`: Enumera o desconecta sesiones entre una computadora y otras computadoras en la red
- `net share`: Crea, elimina o administra recursos compartidos.
- `net start`: Inicia un servicio de red o enumera los servicios de red en ejecución.
- `net stop`: Detiene un servicio de red.
- `net use`: Conecta, desconecta y muestra información sobre recursos de red compartidos.
- `net view`: Muestra una lista de computadoras y dispositivos de red en la red.





#### 6. Administrador de tareas y Monitor de recursos

 Existen dos herramientas muy importantes y útiles para ayudar a un administrador a comprender las distintas aplicaciones, servicios y procesos que se están ejecutando en una computadora con Windows. Estas herramientas también brindan información sobre el rendimiento de la computadora, como el uso de la CPU, la memoria y la red. 

**Administrador de tareas**

El Administrador de tareas, proporciona mucha información sobre el software que se está ejecutando y el rendimiento general de la computadora.

<img src="/img/20ºimagenn.PNG"  />

- **Procesos**: Enumera todos los programas y procesos que están en ejecución actualmente. Muestra la utilización de la CPU, memoria, disco y red de cada proceso. Se pueden examinar las propiedades de un proceso o finalizarlo si no se comporta correctamente o se ha detenido.
- **Rendimiento**:  Proporciona una visión general útil del rendimiento de la CPU, memoria, disco y red. Al hacer clic en cada elemento en el panel izquierdo se mostrarán estadísticas detalladas de ese elemento en el panel derecho.
- **Historial de aplicaciones**: Proporciona información sobre las aplicaciones que están consumiendo más recursos de los que deberían. Haz clic en "Opciones" y selecciona "Mostrar historial de todos los procesos" para ver el historial de todos los procesos que se han ejecutado desde que se inició el equipo.
- **Inicio**: se muestran todas las aplicaciones y servicios que se inician cuando se enciende el equipo. Para deshabilitar un programa para que no se inicie en el arranque, haz clic derecho sobre el elemento y elige "Deshabilitar".
- **Usuarios**: Se muestran todos los usuarios que están conectados al equipo. También se muestran todos los recursos que están utilizando las aplicaciones y procesos de cada usuario. Desde esta pestaña, un administrador puede desconectar a un usuario del equipo
- **Detalles**: Proporciona opciones adicionales de gestión para los procesos, como establecer una prioridad para que el procesador dedique más o menos tiempo a un proceso. También se puede establecer la afinidad de la CPU, lo que determina qué núcleo o CPU utilizará un programa. Además, una función útil llamada "Analizar cadena de espera" muestra cualquier proceso para el cual otro proceso está esperando, esta función ayuda a determinar si un proceso simplemente está esperando o está bloqueado.
- **Servicios**: Se muestran todos los servicios que se han cargado. También se muestra el ID del proceso (PID) y una breve descripción, junto con el estado, ya sea en ejecución (Running) o detenido (Stopped). En la parte inferior, hay un botón para abrir la consola de Servicios, que proporciona una gestión adicional de los servicios. 

**Monitor de recursos**

Cuando se requiere obtener información más detallada sobre el uso de recursos, se puede recurrir al Monitor de Recursos

<img src="/img/21ºimagenn.PNG"  />

- **Info general**: Muestra el uso general de cada recurso. Si seleccionas un único proceso, se filtrará en todas las pestañas para mostrar únicamente las estadísticas de ese proceso.
- **CPU**: Muestra el PID, el número de hilos, el CPU que está utilizando el proceso y el uso promedio de CPU de cada proceso. Es posible obtener información adicional sobre los servicios en los que el proceso se basa, así como los identificadores y módulos asociados, al expandir las filas inferiores. 
- **Disco:** Muestra todos los procesos que están utilizando un disco, junto con estadísticas de lectura/escritura y una descripción general de cada dispositivo de almacenamiento.
- **Red**: Muestra todos los procesos que están utilizando la red, junto con estadísticas de lectura/escritura. Lo más importante es que se muestran las conexiones TCP actuales, junto con todos los puertos que están en escucha. Esta pestaña es muy útil para determinar qué aplicaciones y procesos están comunicándose a través de la red. Permite identificar si hay un proceso no autorizado accediendo a la red, escuchando una comunicación y la dirección con la que se está comunicando
- **Memoria**: Muestra toda la información estadística sobre cómo cada proceso utiliza la memoria. Además, se muestra una descripción general del uso de toda la memoria RAM debajo de la fila de Procesos. 





#### 7. Networking

Para configurar las propiedades de red de Windows y probar la configuración de red, se utiliza el Centro de redes y recursos compartidos. La forma más sencilla de ejecutar esta herramienta es buscarla y hacer clic en ella. Utilice el **Centro de redes y recursos compartidos** para **verificar o crear conexiones de red**, **configurar el uso compartido de red** y **cambiar** la **config**uración **del adaptador** de red.

**Centro de redes y recursos compartidos**

<img src="/img/22ºimagenn.PNG"  />

 La vista inicial muestra una descripción general de la red activa. Esta vista **muestra** si hay **acceso** a **Internet** y si la **red** es **privada, pública o de** **invitados**. También se **muestra el tipo de red**, ya sea por **cable o inalámbrica**. Desde esta ventana, puede ver el grupo doméstico al que pertenece la computadora o crear uno si aún no forma parte de un grupo doméstico. Esta herramienta también se puede utilizar para cambiar la configuración del adaptador, cambiar la configuración de uso compartido avanzada, configurar una nueva conexión o solucionar problemas.

**Cambiar la configuración del adaptador**

Para configurar un adaptador de red, elija "Cambiar la configuración del adaptador" en el Centro de redes y recursos compartidos para mostrar todas las conexiones de red disponibles. Seleccione el adaptador que desea configurar. En este caso, cambiamos la configuración de un adaptador Ethernet para obtener su dirección IPv4 automáticamente de la red.

- Acceder a las **propiedades del adaptador** Clic derecho > propiedades
- Acceder a las **propiedades de TCP/IPV4** Clic derech en TCP > propiedades
- Cambiar la configuración: Puedes elegir **Obtener una dirección automáticamente** si hay un servidor DHCP disponible en la red. Si deseas configurar la **dirección manualmente, **puedes ingresar la **dirección**, la **máscara de subred**, la **puerta de enlace** predeterminada y los **servidores DNS** para configurar el adaptador. 





#### 8. Acceso a recursos de red

Además de acceder a carpetas compartidas en hosts remotos, es posible iniciar sesión en un equipo remoto y controlar esa computadora como si estuvieras trabajando de forma local, lo que te permite realizar cambios de configuración, instalar software o solucionar problemas. En el entorno de Windows, esta función se logra mediante el uso del Protocolo de Escritorio Remoto (RDP). Cuando se investigan incidentes de seguridad, los analistas de seguridad suelen utilizar el RDP con frecuencia para acceder a equipos remotos. Para iniciar el RDP y establecer una conexión con un equipo remoto, simplemente busca "escritorio remoto" y haz clic en la aplicación.

<img src="/img/23ºimagenn.jpg"  />

El formato de Convención de Nombres Universal (UNC) se utiliza para conectarse a los recursos, por ejemplo:

**∖∖nombredeservidor∖nombredelaunidad∖archivo**



Se puede aplicar **control de acceso** a las carpetas y archivos para r**estringir a usuarios y grupos** a funciones específicas, como **lectura, escritura o denegación**. También existen carpetas compartidas especiales que se crean automáticamente en Windows. Estas carpetas compartidas se llaman carpetas compartidas administrativas. Una carpeta compartida administrativa se identifica por el signo de dólar ($) que viene después del nombre de la carpeta compartida.

La forma más sencilla de conectarse a una carpeta compartida es escribir la UNC de la carpeta en el Explorador de archivos de Windows, en el cuadro en la parte superior de la pantalla que muestra la lista de navegación de la ubicación actual del sistema de archivos. Cuando Windows intenta conectarse a la carpeta compartida, se nos pedirá que proporcionemos las credenciales.

Al **activar RDP**, se debe tener **precaución** debido a que puede ser un **objetivo para amenazas**.





#### 10. Windows Server

Windows Server es una edición especial de Windows utilizada principalmente en entornos de centros de datos. Windows Server está diseñado para alojar una amplia gama de servicios y desempeñar diversos roles en una empresa.

Servicios que ofrece Windows Server:

- **Servicios de red**: DNS, DHCP, servicios de terminal, controlador de red y virtualización de red Hyper-V. 
- **Servicios de archivos**: SMB, NFS y DFS. Servicios web: FTP, HTTP y HTTPS. 
- **Administración**: directivas de grupo y control de servicios de Active Directory.

Mas adelante se realizara otra guía especializada en Windows Server.



### Seguridad Windows

------





#### 1. Comando Netstat

El comando netstat **permite** **buscar conexiones entrantes o salientes no autorizadas**. Al ejecutar el comando netstat por sí solo, se mostrarán todas las conexiones TCP activas.

Cuando un equipo está infectado con malware, suele abrir puertos de comunicación no autorizados para enviar y recibir datos. Al examinar estas conexiones, es posible identificar los programas que están escuchando conexiones no autorizadas. Si se sospecha que un programa es malware, se puede realizar una investigación para verificar su legitimidad y eliminarlo. 

Para facilitar este proceso, es posible vincular las conexiones a los procesos en ejecución que las crearon utilizando el Administrador de tareas. Para hacerlo, abre una ventana de símbolo del sistema con privilegios de administrador y ejecuta el comando netstat -abno, tal como se muestra en la salida del comando.

<img src="/img/24ºimagenn.PNG"  />

Es posible que haya más de un proceso listado con el mismo nombre. Para **mostrar los PID de los procesos** en el **Administrador de tareas,** abre el Administrador de tareas, haz clic derecho en el encabezado de la tabla y selecciona "PID".

<img src="/img/25ºimagenn.PNG"  />





#### 2. Visor de eventos

El **Visor de Eventos** de Windows **registra el historial de eventos de aplicaciones, seguridad y sistema**. Estos archivos de registro son una herramienta para solucionar problemas. 

Windows incluye dos categorías de registros de eventos: **Registros de Windows** y **Registros de Aplicaciones y Servicios**. Cada una de estas categorías tiene varios tipos de registros. Los eventos que se muestran en estos registros tienen un nivel: **información, advertencia, error o crítico**. También se **registran la fecha y hora** en que ocurrió el evento, junto con la **fuente y un ID** que se relaciona con ese tipo de evento.

Además, es posible crear vistas personalizadas, lo cual es útil para buscar tipos específicos de eventos, encontrar eventos que ocurrieron durante un período de tiempo determinado y mostrar eventos de un nivel en particular, entre otros criterios. Existe una vista personalizada incorporada llamada "**Eventos Administrativos**" que **muestra** todos los **eventos críticos, errores y advertencias** de todos los registros administrativos. 

Los **registros de eventos de seguridad** **se encuentran** en la categoría de **Registros de Windows** y utilizan identificadores de eventos para identificar el tipo de evento.

<img src="/img/26ºimagenn.PNG"  />





#### 3. Gestión de Actualizaciones

Windows verifica regularmente el sitio web de **Windows Update** en busca de actualizaciones prioritarias que pueden ayudar a proteger una computadora de las últimas amenazas de seguridad. Estas actualizaciones incluyen **actualizaciones de seguridad, actualizaciones críticas y paquetes de servicio**. 

Los atacantes siempre buscan formas de comprometer computadoras y aprovechar códigos defectuosos. Es importante mantener Windows actualizado con los últimos parches de seguridad **para protegerse de ataques**, especialmente los "**Zero Day**" exploits.

Dependiendo de la configuración que elijas, Windows descargará e instalará automáticamente las actualizaciones prioritarias que tu computadora necesita o te notificará cuando estas actualizaciones estén disponibles. 

Todo esto se puede configurar buscando "Windows update" en nuestro buscador del sistema.

También hay configuraciones para las horas en las que la computadora no se reiniciará automáticamente,  puedes elegir cuándo reiniciar la computadora después de una actualización, si es necesario, con las opciones de reinicio, además hay opciones avanzadas disponibles para elegir cómo se instalan las actualizaciones y cómo se actualizan otros productos de Microsoft.





#### 4. Directiva de seguridad local

Es un conjunto de objetivos que garantiza la seguridad de una red, los datos y los sistemas informáticos en una organización. 

En redes con computadoras Windows, se configura Active Directory con Dominios en un servidor de Windows. Las computadoras se unen al dominio y se aplica una Directiva de Seguridad del Dominio. Sin embargo, para computadoras independientes sin un dominio, se puede utilizar la Directiva de Seguridad Local de Windows. Basta con buscar Directiva de seguridad local en el buscador del sistema para encontrarlo.

<img src="/img/27ºimagenn.PNG"  />

- La **Directiva de Contraseña** en la Política de Seguridad Local establece los **criterios para las contraseñas** de los usuarios locales.
- La **Directiva de Bloqueo de Cuenta** **previene intentos de inicio de sesión por fuerza bruta**. Al establecer un límite de intentos fallidos, la cuenta se bloquea temporalmente para protegerla contra ataques.
- La D**irectiva de Seguridad Local** también ofrece configuraciones adicionales, como **derechos de usuario, reglas de firewall y restricciones de ejecución** de archivos con AppLocker.

Es importante establecer reglas de seguridad, como bloquear automáticamente la computadora cuando el protector de pantalla se activa, para protegerla en ausencia del usuario.

**Si la Directiva de Seguridad Local es la misma** en varias computadoras independientes, **se puede exportar y copiar el archivo de Directiva para aplicarlo en otras máquinas**. Esto simplifica la configuración y asegura consistencia en las Directivas de seguridad.





#### 5. Windows Defender

Windows defender es un **programa de seguridad antimalware** el cual tiene el propósito de **buscar y solucionar amenazas** que afecten al sistema.

<img src="/img/28ºimagenn.PNG"  />

El **malware** engloba **virus, gusanos, troyanos, keyloggers, spyware y adware**. Estos tienen como objetivo invadir la privacidad, robar información, dañar la computadora o corromper datos. **Es crucial proteger las computadoras y dispositivos móviles utilizando software antimalware confiable**.

Aunque Windows Defender trabaja en segundo plano, puedes realizar escaneos manuales de la computadora y los dispositivos de almacenamiento. También puedes actualizar manualmente las definiciones de virus y spyware en la pestaña de actualización.

Es posible que el defender de Windows no sea capaz de eliminar algunas amenazas por completo, podemos usar otros software de renombre como **malwarebytes, karpersky, bitdefender...**





#### 6. Cortafuegos/Firewall de Windows Defender

Un firewall **deniega selectivamente el tráfico a una computadora o segmento de red**. Los firewalls generalmente funcionan abriendo y cerrando los puertos utilizados por diversas aplicaciones. Al **abrir solo** los **puertos necesarios en un firewall**, estás implementando una política de **seguridad restrictiva**. Una **política de seguridad permisiv**a permite el **acceso a través de todos los puertos**, **excepto** aquellos que **se deniegan explícitamente**.

<img src="/img/29ºimagenn.PNG"  />

Se pueden encontrar muchas configuraciones adicionales en Configuración avanzada. Aquí puedes crear reglas de tráfico entrante o saliente basadas en diferentes criterios. También puedes importar y exportar políticas o supervisar diferentes aspectos del firewall.

Si deseas utilizar un firewall de software diferente, deberás desactivar el Firewall de Windows. Para desactivar el Firewall de Windows, haz clic en Activar o desactivar el Firewall de Windows.

<img src="/img/30ºimagenn.PNG"  />





## BIBLIOGRAFIA y AGRADECIMIENTOS

Esta guía básica no podía haber sido realizada sin la ayuda de este curso:

Operating Systems Basics 																					Cisco Networking Academy.

Mencionar también la gran ayuda recibida por parte de toda la comunidad de Windows y sus foros .

