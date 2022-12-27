# Análisis de Timelines usando Plaso/Log2Timeline y TimelineExplorer
El trabajo se realizará en una máquina virtual **SIFT Workstation**, la cual tiene por defecto el **Plaso** con todas sus herramientas. Entre sus principales herramientas se tienen: log2timeline, pinfo, psort y psteal. El análisis se realizará mediante el software **Timeline Explorer**, perteneciente a las herramientas de Eric Zimmerman.

## Plaso (log2timeline)

Se utilizará el **log2timeline** para empezar y poder observar lo que entrega como archivo final.

### Log2timeline
Esta herramienta es utilizada para extraer **timestamps** y **artefactos** que se pueden encontrar en una imagen forense. Lo que entregará esta herramienta es un **archivo plaso**, la cual posteriormente se debe convertir a un parámetro reconocible utilizando otras de las herramientas mencionadas anteriormente. 

La estructura más simple del **log2timeline** es el siguiente:
```
log2timeline.py --storage-file [nombre_archivo] [imagen_forense]
```
Donde **nombre_archivo** hace referencia al nombre que tendrá el archivo plaso que nos entregará el comando e **imagen_forese** es para ingresar aquella imagen que deseamos analizar. Para este trabajo, se utilizará una imagen que se tiene guardada en la ubicación *Downloads/SCHARDT* y el nombre del archivo final será **timeline.plaso**.

Entonces tenemos el siguiente comando:
```
log2timeline.py --storage-file timeline.plaso SCHARDT.dd
```
Una vez concluída el proceso, se obtendrá la siguiente pantalla:

![l2tl1](https://user-images.githubusercontent.com/36284620/207025248-cc2b225d-52f0-49f4-a3b0-770a9600ee04.PNG)

No obstante, éste no es el único modo de utilizar **log2timeline**, pues esta herramienta cuenta con otros dos parámetros que pueden ser utilizados para mejorar la búsqueda de información: **parsers** y **filtros**. Por un lado, los **parsers** son utilizados para enfocarse en ciertos artefactos que contiene la imagen (p.ej: prefetch, webhist, winreg, etc.)

Si se desea conocer todos los **parsers** que utiliza el **log2timeline**, se puede utilizar el siguiente comando:
```
log2timeline.py --parsers list | more
```
Teniendo como resultado lo siguiente:

![l2tl2](https://user-images.githubusercontent.com/36284620/207026425-d08084e5-6493-41db-be57-c244004dbe86.PNG)

En este caso, dado que la máquina que analizaremos es Windows, utilizaremos el parser preset win7 de la siguiente manera:
```
log2timeline.py --parsers win7 --storage-file timeline2.plaso SCHARDT.dd
```
Por otro lado se tienen los **filtros de archivos** que, tal cual dice su nombre, son utilizados para especificar cierto tipo de archivos dentro de la imagen. Para este caso se puede utilizar el **github** de Mark Hallman, quien desarrollo una lista de los filtros que se pueden utilizar en el sistema operativo Windows. Primero se debe descargar la lista, por lo que se utilizará el comando *wget* para ello:

Link de Mark Hallman: https://raw.githubusercontent.com/mark-hallman/plaso_filters/master/filter_windows.txt
```
wget https://raw.githubusercontent.com/mark-hallman/plaso_filters/master/filter_windows.txt
```
Finalmente, para incluir esta lista de filtros en nuestro comando, se escribe lo siguiente:
```
log2timeline.py --parsers win7 -f filter_windows.txt --storage-file timeline2.plaso SCHARDT.dd
```

### Pinfo

**Pinfo** es utilizado únicamente para obtener información del archivo plaso que se consiguió mediante **log2timeline**. En ese sentido, se puede tener información como: fecha en la que se realizó el análisis, los parsers utilizados, eventos generados por cada uno, total de eventos, etc.

Para ello, se ingresa:
```
pinfo.py timeline.plaso | more
```
Y el resulado es el siguiente:

![pinfo](https://user-images.githubusercontent.com/36284620/207028827-4bc9be90-f22d-4aee-a225-2c2c425c6746.PNG)

### Psort

Por último, se tiene la herramienta **psort** para realizar dos tareas importantes: convertir el archivo plaso a otro formato y filtrar fechas. Para realizar el cambio de formato, simplemente se intruduce:
```
psort.py -o [formato_final] -w [nombre_archivo_final] [archivo_plaso]
```
Entre los formatos que se pueden obtener se encuentran: json, kml, l2tcsv (CSV), xlsx, etc. Si se desea mayor información de éstos, se puede ingresar un *-o list* en lugar de lo último; para este caso se utilizará el **l2tcsv**.
```
psort.py -o l2tcsv -w lista.csv timeline.plaso
```
Como se mencionó anteriormente, también se puede definir una fecha en específico para realizar el análisis. Para ello, se utiliza *--slice* e ingresar la fecha bajo el formato *YYYY-MM-DDThh:mm:ss*. Cabe destacar que bajo este método se analiza la fecha indicada con un rango de +/- 5 minutos por defecto.
```
psort.py --output_time_zone utc -o l2tcsv -w lista3.csv timeline.plaso --slice “2004-08-25T11:20:55”
```
Si se desea modificar estos 5 minutos, se utiliza *--slice_size* para agregar los minutos que uno desee. Si no se tiene identificado una fecha en específico, se puede trabajar con rangos de tiempo. Para ello, se utiliza la siguiente estructura:
```
psort.py --output_time_zone utc -o l2tcsv -w lista5.csv timeline.plaso “date < ‘2004-08-19T00:00:00’ and date > ‘2004-08-25T23:59:59’”
```

### Psteal

La herramienta **psteal** permite realizar el análisis de la imagen forense y convertirlo al formato que se desea en un único comando, siendo éste:
```
psteal.py --source SCHARDT.dd -o l2tcsv -w lista7.csv
```
La principal desventaja de este método es que no permite realizar un análisis más específico mediante el uso de parsers ni filtros, por lo que el archivo final poseerá mayor información y teniendo data que no sea necesaria para la investigación.

## Timeline Explorer

Para realizar un análisis de los archivos *.csv* obtenidos, se utilizará el software **Timeline Explorer**. Este software pertenece al grupo de herramientas que ofrece Eric Zimmerman y, precisamente, fue desarrollado para poder visualizar los CSV generados por Plaso sin la necesidad de utilizar Excel, además de tener una interfaz de fácil entendimiento y con diversas opciones.

Página de descarga: https://ericzimmerman.github.io/#!index.md

Para abrir nuestros archivos, únicamente se debe ingresar a *File -> Open* y seleccionar el *.csv* que deseamos analizar. La pantalla que se mostrará es la que se aprecia a continuación:

![tle](https://user-images.githubusercontent.com/36284620/207034841-e9f3f2a8-c1da-461f-ad38-fe215f07e6a1.PNG)













