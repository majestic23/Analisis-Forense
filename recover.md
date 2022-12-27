## RECUPERACIÓN DE ARCHIVOS

### Pre-requisitos 

Disponer de la imagen win7.img - Descargar archivos necesarios desde[ https://bit.ly/2KE4VEX ](https://bit.ly/2KE4VEX)

### METODO 1: pothorec 

Es un software diseñado para recuperar archivos perdidos incluyendo videos, documentos y archivos de los discos duros y CDRoms así como imágenes perdidas (por eso el nombre  PhotoRecovery)  de  las  memorias  de  las  cámaras  fotográficas,  MP3  players,  PenDrives,  etc. PhotoRec ignora el sistema de archivos y hace una búsqueda profunda de los datos, funcionando incluso si su sistema de archivos está muy dañado o ha sido re-formateado 

### METODO 2: testdisk 

Es un software libre para realizar la recuperación de datos. Diseñado principalmente para ayudar a recuperar particiones perdidas y / o hacer un disco nuevamente “booteable” o iniciable, cuando estos síntomas son causados por fallas de software, 

ciertos tipos de virus o errores humanos; como accidentalmente borrar una tabla de particiones. 

### METODO 3: FTK Imager 


-----------------

### Procedimiento Photorec  

Proceder a ejecutar un Terminal utilizando el ícono ubicado en el menú del lado izquierdo.  

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.003.png)

En la Máquina Virtual de SIFT proceder a abrir un Terminal. Luego escribir los siguientes comandos. 
```
$ cd Desktop 

$ mkdir Recuperados 
```
Proceda a copiar y descomprimir la imagen forense (win7.img) capturada del disco duro  del  Sistema Windows 7. 

Para realizar la recuperación de archivo se utilizará Photorec con el siguiente comando. 
```
$ sudo photorec /log win7.img 
```
Al ejecutar el comando se abrirá una interfaz en modo consola. Para moverse entre las opciones 

se debe utilizar las teclas de dirección o “flechas”. En la primera ventana seleccionar la opción “Proceed” para luego presionar la tecla “enter”. Se presentará una nueva ventana donde se solicita información sobre la partición desde la cual se requiere realizar la recuperación de archivos. 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.010.jpeg)

En la siguiente ventana seleccionar el tipo de sistema de archivos de la partición seleccionada en 

la ventana anterior. Por defecto PhotoRec tratará de reconocer el tipo de sistema de archivos. 

La siguiente ventana requiere definir si se extraerán los archivos desde toda la partición seleccionada, o únicamente se escanearán los archivos desde el espacio sin asignar. 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.011.jpeg)

Selecccione “File Opt” para seleccionar solamente algunos tipos de archivos 

Ahora seleccione “s” para deshabilitar el check y elija solo los tipos de archivo siguientes: doc y pdf 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.012.jpeg)

Ahora seleccione “b” para salvar los settings y luego “Ok” 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.013.jpeg)

Ahora seleccione “search” para que inicie la búsqueda de archivos borrados, considere que se deberá indicar el tipo de partición de la unidad, este caso como la herramienta reconoce el tipo de sistema de archivo (NTFS) pulsar “other” 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.014.jpeg)

Luego, seleccionar esta última opción “Free”.  

En la siguiente ventana se solicita definir la ubicación o carpeta donde serán almacenados los archivos recuperados.  

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.015.jpeg)

Cuando se haya definido la ubicación, escribir la letra “C”. 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.016.jpeg)

Para visualizar los archivos recuperados por Photorec, ingresar a la carpeta “ “/Recuperados/” donde se encontrarán carpetas de nombres “recup\_dir.1”, “recup\_dir.2” y “recup\_dir.3”, etc. Ingresar a cada una de estas para luego analizar sus contenidos. 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.017.jpeg)

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.018.png)

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.019.jpeg)

Para salir de la herramienta Photorec utilizar la opción “Quit” varias veces y presionar la tecla “Enter”.  

### Procedimiento testdisk 

En la Máquina Virtual SIFT abrir un Terminal. 

De ser requerido listar las particiones utilizando TestDisk. 

```
$ sudo testdisk /list 
```

Ejecutar TestDisk con la imagen forense capturada desde el disco duro del Sistema Windows 7. 

```
$ sudo testdisk /log win7.img 
```
Se presentará la ventana siguiente donde se debe indicar el dispositivo o medio de almacenamiento sobre el cual se realizará el análisis. Por defecto será seleccionado el indicado al momento de ejecutar testDisk.  

Seleccionar “Proceed” y presionar la tecla “Enter”. 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.020.jpeg)

En la siguiente ventana se requiere definir el tipo de la tabla de particiones, en este caso seleccionar la partición “Intel/PC partition”, presionar “Enter”. 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.021.jpeg)

En la nueva ventana seleccionar la opción “Analyse” o Analizar, la cual permitirá analizar la estructura actual de las particiones y buscar particiones perdidas.  

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.022.jpeg)Después de presionar la tecla “enter”, seleccionar la opción “Quick Search” presionando nuevamente la tecla “enter”. 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.023.jpeg)

Se presentarán las particiones identificadas. Para listar la estructura de los directorios presionar la letra “P”. 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.024.jpeg)En caso existieran directorios o archivos borrados que requieran ser recuperados, proceder manualmente a seleccionar un archivo, archivos o todos los archivos presionando la respectiva 

letra.  

Tip: Se cuenta con información en la carpeta siguiente: /Users/issa/Downloads 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.025.jpeg)

Luego definir una carpeta de destino donde serán copiados los archivos a recuperar, tal y como se detalló en la recuperación de archivos utilizando la herramienta Photorec. 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.026.jpeg)

Para este caso use el directorio Recuperados 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.027.png)

Cuando el procedimiento de recuperación finalice, se muestra la pantalla similar a la siguiente 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.028.jpeg)

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.029.jpeg)

### Procedimiento Access Data FTK Imager 

Añadir la imagen forense capturada desde el disco duro de un Sistema Windows 7 utilizando la opción “File -> Add Evidence Item”.  

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.030.jpeg)

Luego seleccionar la opción “Image File” y presionar el botón “Siguiente”.  

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.031.png)Se  requiere  localizar  el  lugar  donde  reside  la  imagen  de  la  evidencia  utilizando  el  botón  “Browse”. Seleccionada el ítem de evidencia y presionar el botón “Finish” para finalizar este proceso. 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.032.png)

El ítem de evidencia añadida se ubicará en el panel “Evidence Tree”, ubicado en la parte superior izquierda. Para navegar por la estructura de directorios hacer clic en el ícono [+] para expandir las carpetas. Ingresar por ejemplo a algunas de las carpetas para tratar de identificar cuáles son los archivos borrados. 

Para el ejemplo considera la ruta siguiente: 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.033.png)

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.034.jpeg)

Para recuperar manualmente alguno de los archivos borrados se deberá primero seleccionarlo, para luego hacer clic derecho. Se presentará un menú con tres opciones, donde se seleccionará la opción “Export Files...”.  

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.035.png)

En la nueva ventana se debe definir la carpeta en la cual se copiará el 

archivo recuperado, para luego presionar el botón “Aceptar”. Finalizado este proceso se presentará una ventana donde se detallarán los resultados. 

![](aspose/Aspose.Words.4ee15991-6e50-4da7-beb3-c824fd993a89.036.png)
