# Revisión de Metadata

Para este taller Descargar archivos necesarios desde https://bit.ly/2KE4VEX 

Admeás de emplear la distribución del software SANS Investigate Forensic Toolkit (SIFT)  el cual incluye ExifTool se empleará GHiro Workstation es un appliance 
Vmware en Ubuntu preparado para realizar análisis de imágenes forenses

También se puede emplear GeoSetter (para sistemas operativos Windows)

Sitios Opcionales

GPS COORDINATES
https://www.gps-coordinates.net/ PIC2MAP https://www.pic2map.com
metapicz
http://metapicz.com/#location-box

# Análisis de Imagenes

Añada al VMWARE la siguiente imagen: Ghiro Appliance 0.2.1-1.ova 

![image](https://user-images.githubusercontent.com/50930193/204651993-3553c4b8-e185-4f3b-b80e-3a87cd038d27.png)

Cree un caso (botón mas) Acceda a esta opción y luego emplee el boton "+"

![image](https://user-images.githubusercontent.com/50930193/204652078-621f8032-76f0-420d-b014-65a70e0876d1.png)

![image](https://user-images.githubusercontent.com/50930193/204652184-4bec08c7-bda3-4a41-86e2-d8d91e1a3319.png)

Ya en el caso, añada alguna imagen, para ello, ubique imágenes tomadas desde su teléfono móvil, si no los tiene considere tomar unas fotos con el siguiente criterio: 

1) Activando ubicación 2) Sin activar ubicación

![image](https://user-images.githubusercontent.com/50930193/204652342-b6b5e714-ee58-43e3-8bd8-53b8b1b81810.png)

Luego ya puede analizar la imagen

![image](https://user-images.githubusercontent.com/50930193/204652417-ee28fb1b-50cd-4083-883a-d996441c8872.png)

Considere los datos EXIF de la imagen para saber dónde se tomó la foto, Con esta información puede ir al enlace siguientes: https://www.gps-coordinates.net/ e ingrese los datos resultados


![image](https://user-images.githubusercontent.com/50930193/204652496-6986d93e-5f47-461a-97ed-78de71c008e7.png)

Ahora intente ingresando a este sitio: https://www.pic2map.com

Finalmente intente con el software GeoSetter

# Análisis de Archivos varios

Descargar archivos doc que resulten de buscar el siguiente texto

![image](https://user-images.githubusercontent.com/50930193/204652864-0abcd2f5-26a6-4f28-8339-c4be4398e6c8.png)

guarde el documento

![image](https://user-images.githubusercontent.com/50930193/204652933-2b16ea6b-0d3b-4979-ac33-9f52db4808b0.png)

Repita esta operación en 5 archivos de diferentes sitios pero de archivos de diferente formato

Ingrese a dicha carpeta desde el terminal y on cada archivo descargado podrás ubicar la metadata de cada archivo digitando el comando siguiente

```
$exiftool [Archivo_Descargado.doc] 
```
![image](https://user-images.githubusercontent.com/50930193/204653672-ee149d28-467d-43e5-9fd8-3b900b94513d.png)

Revisa las propiedades, repita este mismo paso en PDF, XLSX, DOCx, entre otros, el listado total de formatos soportados aparecen aqui https://exiftool.org/
