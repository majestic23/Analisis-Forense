# COMANDOS EN LINUX PARA ADQUISICIÓN DE EVIDENCIA

## Mapear una unidad 

El primer paso es Conectar un USB Stick (Memoria USB), en una PC Virtual con VMWARE el montaje se realiza en automático o bien Ingresando a VM>Removable Devices>[USB Device], donde [USB Device] hace referencia a tu dispositivo. Si no fuera una PC virtual y es el sistema operativo propiamente dicho es necesario montar el USB, Discos Duros Externos o unidades en general, para el montaje y desmontaje de unidades referirse al Anexo 1 de este documento. En mi caso, como tengo una VM solo poniendo el USB y habilitando esta opción puedo conectar el USB al equipo

![image](https://user-images.githubusercontent.com/50930193/135363013-b1e684f9-bcdb-4a55-80ca-3fd308fe6391.png)

En seguida, se tiene que listar las unidades del sistema operativo. Para ello, ejecutar el siguiente comando para identificar cual es el nombre asignado al dispositivo USB, por ejemplo (/dev/sdc). Con esta información se procede a realizar la captura de la imagen forense.

```
$sudo fdisk -l
```

![image](https://user-images.githubusercontent.com/50930193/135363228-1b3bb03b-bcee-46f9-a7ad-6d9b90a705b2.png)

## AdquisicIón

### COMANDO: dd
dd es un comando en Unix e incluido por defecto en diversos sistemas GNU/Linux cuyo principal propósito es convertir y copiar un archivo. Es una de las herramientas más antiguas utilizadas para crear réplicas o imágenes forenses.

Pero antes hay que crear el hash a la unidad del usb
```
$sudo md5sum /dev/sdb1 > /tmp/img_sdb1.md5
```

Generará el archivo siguiente:

![image](https://user-images.githubusercontent.com/50930193/135363425-56ea3ebd-962a-4380-abc2-8a92e33bedca.png)

 

Ahora generamos el archivo de evidencia digital
```
$sudo dd if=/dev/sdb1 bs=512 conv=noerror,sync of=/tmp/img_sdb1.dd
 ```

![image](https://user-images.githubusercontent.com/50930193/135363716-369915c1-b6be-477b-95f4-b3a1477570e6.png)

Donde:
| parametro | descripción |
|-----------|-------------|
| if        | ruta origen |
| bs        | block size o tamaño de los bytes a copiar, 512 bytes a la vez           |
| conv      | conversiones sobre los datos de entrada          |
| noerror   | si hubiera error de lectura va a continuar el proceso de copia           |
| sync      | rellenar esos sectores dañados con bytes nulos           |
| of        | ruta destino de la imagen           |

Finalmente sacamos el hash de la imagen
 ```
$sudo md5sum /tmp/img_sdb1.dd > /tmp/img_sdb1.dd.md5
 ```



Si todo se ha realizado correctamente se habrá creado un archivo de nombre “img_sdbc1.dd”, el cual corresponde a la imagen forense obtenida desde la unidad USB. Es decir, es una copia exacta de todo el contenido del origen, que reside ahora en un archivo. 

Al comparar los hash MD5 contenidos en los archivos “img_sdc1.md5” y “img_sdc1.dd.md5, estos serán iguales, lo cual verifica la integridad y contenido exacto de la imagen forense capturada. 

Se sugiere además utilizar dos hashs como mínimo, por ejemplo MD5 o SHA1

 ```
$sudo md5sum /dev/sdb1 img_sdb1.dd > img_sdb1.dd.md5
$sudo sha1sum /dev/sdb1 img_sdb1.dd > img_sdb1.dd.sha1
 ```

NOTA: Si bien ejemplarizamos el uso del algoritmo md5 el que debería predominar es el algoritmo del sha debido a que se ha comprobado que el md5 ha sido vulnerado.

Por otro lado, si se quisiera dividir la imagen, se puede combinar el comando dd con el comando Split. Esto se utiliza por ejemplo para hacerlos caber en varios CDs o DVDs
 ```
$sudo dd if=/dev/sdb1 bs=512 conv=sync,noerror | split -d -b 650m - /tmp/sdb_image
 ```
 
Se puede cambiar el valor ligado al –b pero se recomienda que esto no sobrepase los 2000m por los límites de FAT32, Asimismo el parámetro –d es usado para dar un orden numérico (image.01, image.02, image.03)  a las imágenes generadas, si no se usa se ordenarán alfabéticamente (image.ab, image.ac) 
Finalmente se puede crear el hash mientras se ejecuta el comando para ello, se deberá usar el comando siguiente:
 ```
$sudo dd if=/dev/sdb1 bs=512 conv=sync,noerror | tee /tmp/sdb_image.img | md5sum > /tmp/sdb_image.md5
 ```
 
### COMANDO: dcfldd

dcfldd es la versión mejorada de dd desarrollada por el laboratorio de cómputo forense del Departamento de Defensa de los Estados Unidos, la cual cuenta con características muy útiles para los investigadores forenses.

La unidad USB ha sido detectada en “/dev/sdc1”. La opción “conv” convierte el archivo de acuerdo a la lista de palabras clave separadas por comas. “noerror” permite continuar después de errores de lectura, “sync” rellena cada bloque de entrada con “NULs”. La opción “hash” define el algoritmo a utilizar (MD5 y SHA1). La opción “hashlog” envía la salida de los hash a un archivo definido.

 ```
$ sudo dcfldd if=/dev/sdb1 of=/tmp/imagen_sdb1.dd conv=noerror,sync hash=md5,sha1 hashlog=/tmp/hashlog_img_sdb1_.txt
 ```
 
![image](https://user-images.githubusercontent.com/50930193/135364146-e17778ed-2d44-486f-b6d2-fdfec415ba32.png)

 

Con el siguiente comando se procede a visualizar el archivo de nombre “/tmp/hashlog_img_sdc1_.txt”.
```
$ sudo cat /tmp/hashlog_img_sdb1_.txt
```
![image](https://user-images.githubusercontent.com/50930193/135364222-a55a4d3d-d503-4e3e-8bcf-2ac30bc241f5.png)
 

Además del método utilizado para verificar los hashs obtenidos con los comandos md5sum y sha1sum, se puede utilizar también la misma herramienta dcfldd. La opción “vf” verifica si el archivo de nombre “/tmp/img_sdc1.dd” coincide con la entrada definida.
```
$ sudo dcfldd if=/dev/sdc1 vf=/tmp/img_sdc1.dd 
```
El mensaje expuesto por este procedimiento, indicará si los dos hashs obtenidos coinciden o no.

Si coinciden:

![image](https://user-images.githubusercontent.com/50930193/135364245-8aa3f555-0454-4ebe-ad4a-ef111ca276b3.png)


No coinciden:

![image](https://user-images.githubusercontent.com/50930193/135364263-4fed83ce-5a80-4af9-a118-cadc969791ae.png)

 
#### COMANDO: dc3dd

Dc3dd fue desarrollado por el DoD Cyber Crime Center y ha sido parchado por el proyecto GNU del dd command..
```
$ sudo dc3dd if=/dev/sdb1 of=/tmp/sdb1_image_dc3.img hash=sha1 log=/tmp/dc3dd.log
```
![image](https://user-images.githubusercontent.com/50930193/135364189-e2fe0bae-270c-4359-9257-b6f34de00a5e.png)

 



# ADQUISICIÓN DE IMÁGENES DESDE UN FOLDER O ARCHIVOS ESPECIFICO DE WINDOWS

Conectar un USB Stick (Memoria USB). 

Ejecutar el programa AccessData FTK Imager. 

Añadir un ítem de evidencia  (File > Add Evidence Item”)

 
![image](https://user-images.githubusercontent.com/50930193/135372779-9a7dffb6-a544-4606-b9de-05bd83c9d262.png)


luego seleccionar “Physical Drive”. 

 ![image](https://user-images.githubusercontent.com/50930193/135372792-dc4599b0-5c84-47a5-b5b6-e314c5ac9b0a.png)


En la siguiente ventana seleccionar la Unidad USB, y para finalizar se debe presionar el botón “Finish”, ubicado en la parte inferior.

 ![image](https://user-images.githubusercontent.com/50930193/135372822-5cef04d1-7e60-4807-9164-bc4c6599391b.png)

 
En la sección definida como “Evidence Tree” se visualizará el contenido de la Unidad USB. 

 ![image](https://user-images.githubusercontent.com/50930193/135372834-4a310f3d-0751-4732-8b3b-e2fdcaaa2be4.png)


Hacer clic derecho sobre alguna de las carpetas expuestas, ante lo cual se presentará 
un menú conteniendo cuatro opciones. 

Hacer clic en la opción “Add to Custom Content Image (AD1)”, para luego repetir el mismo procedimiento con otro archivo. Se visualizará el resultado de estas acciones en la ventana de nombre “Custom Contect Sources”.

 ![image](https://user-images.githubusercontent.com/50930193/135372848-7a9b9966-6c32-4371-8a38-74b8cc62b2e0.png)


Hacer clic en el botón “Create Image” ubicado en la parte inferior de esta ventana. Luego de esta acción, se presentará una nueva ventana donde se solicitará definir el destino en el cual se copiará la evidencia forense. 

![image](https://user-images.githubusercontent.com/50930193/135372864-a5ffe6c2-255d-4873-80d7-4fc24358064f.png)

 
Hacer clic en el botón “Add”, completar la información solicitada sobre la evidencia, hacer clic en el botón “Siguiente”, 

Seleccionar una carpeta donde se copiará la evidencia, utilizando el botón “Browse” y definir un nombre de archivo) en el campo (Image FileName).  Y luego dar click en “Finish”

![image](https://user-images.githubusercontent.com/50930193/135372873-57e66b3b-7a85-4989-b8f1-467df2083e06.png)

 


Para iniciar el proceso de captura, hacer clic en el botón “Start”. Con este procedimiento se generará una imagen forense, la cual únicamente contiene como evidencia, la carpeta y el archivo seleccionado desde la memoria USB.

  ![image](https://user-images.githubusercontent.com/50930193/135372887-791ce602-1c8d-4c7c-9f1f-ce5da789e2ba.png)
  
![image](https://user-images.githubusercontent.com/50930193/135372923-c090c2a8-7f76-4649-ba55-037b61f7e6e7.png)

 
# ADQUISICIÓN DE IMÁGENES DE ARCHIVOS PROTEGIDOS DE UN SISTEMA EN FUNCIONAMIENTO EN WINDOWS


Hacer clic en “File -> Obtain Protected Files”

 ![image](https://user-images.githubusercontent.com/50930193/135372932-1e753308-883f-4ae3-8552-c2bf81bad09c.png)

Luego definir el directorio en el cual se copiarán los archivos capturados,utilizando el botón “Browse”. En la parte inferior de la ventana se mostrarán dos opciones, seleccionar la opción “Password recovery and all Registry Files”. 


 ![image](https://user-images.githubusercontent.com/50930193/135372943-4c796e1f-576f-4639-b9bc-1d197868bb92.png)



Luego hacer clic en el botón “OK”, para iniciar el proceso de copia.
 
Este procedimiento capturará en una carpeta definida los archivos “Users”, “System”, “SAM”,
“NTUSER.DA”T, “Default”, “Security”, “Software”, entre otros. En clases posteriores, para visualizar (ANALIZAR) algunos de estos archivos se puede utilizar AccessData Registry Viewer, o RegRipper

 
![image](https://user-images.githubusercontent.com/50930193/135372958-a0958a95-3f5f-410a-8d37-8e51978ee9f3.png)

![image](https://user-images.githubusercontent.com/50930193/135372966-6336c10a-bb23-48cc-9d7d-c6b8dca26b2c.png)


 
