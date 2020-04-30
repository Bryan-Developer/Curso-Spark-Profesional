# Laboratorio 01: Curso Spark Profesional  
## Preaparaciones
1. Tener cuenta [Google Cloud Plaform](https://cloud.google.com/?&utm_source=google&utm_medium=cpc&utm_campaign=latam-PE-all-es-dr-bkws-all-all-trial-e-dr-1008075-LUAC0010196&utm_content=text-ad-none-none-DEV_c-CRE_382274978090-ADGP_BKWS+%7C+Multi+~+GCP-KWID_43700047166266623-kwd-155951229-userloc_9073192&utm_term=KW_gcp-ST_GCP&gclid=Cj0KCQjw7qn1BRDqARIsAKMbHDbWZr4pzBc3YJ9tji0hQIX7tp8d28BjO7oN3CR7x6mbHk76tVhZrAgaAtcwEALw_wcB&gclsrc=aw.ds).  
2. Crear instacia de DataProc puede ser de manera UI o utilizar el siguiente comando en la consola.
```bash
gcloud beta dataproc clusters create cluster-laboratorio --enable-component-gateway --region us-central1 --subnet default --zone us-central1-b --master-machine-type n1-standard-2 --master-boot-disk-size 100 --num-workers 2 --worker-machine-type n1-standard-2 --worker-boot-disk-size 300 --image-version 1.3-deb9 --optional-components ANACONDA,JUPYTER --scopes 'https://www.googleapis.com/auth/cloud-platform' --project [nombre de tu projecto aqui]
```
3. Conectarse por cualquier cliente ssh.
## Ejercicios
- [ ] [Guiado](#Guiado)  
- [ ] [Tarea](#Tarea)  
## Guiado  

1. Ejecuta la siguente sentencia en tu Consola ``` hdfs dfs -ls / ``` lista todo el contenido de tu carpeta datanode 
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -ls /
Found 3 items
drwx------   - mapred hadoop          0 2020-04-30 12:23 /hadoop
drwxrwxrwt   - hdfs   hadoop          0 2020-04-30 12:24 /tmp
drwxrwxrwt   - hdfs   hadoop          0 2020-04-30 12:23 /user
```
2. Ejecuta la siguente sentencia en tu Consola ``` hdfs dfs -ls /user ``` lista todo el contenido de la carpeta user en de tu datanode  
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -ls /user
Found 8 items
drwxrwxrwt   - hdfs hadoop          0 2020-04-30 12:23 /user/hbase
drwxrwxrwt   - hdfs hadoop          0 2020-04-30 12:23 /user/hdfs
drwxrwxrwt   - hdfs hadoop          0 2020-04-30 12:23 /user/hive
drwxrwxrwt   - hdfs hadoop          0 2020-04-30 12:23 /user/mapred
drwxrwxrwt   - hdfs hadoop          0 2020-04-30 12:23 /user/pig
drwxrwxrwt   - hdfs hadoop          0 2020-04-30 12:23 /user/spark
drwxrwxrwt   - hdfs hadoop          0 2020-04-30 12:23 /user/yarn
drwxrwxrwt   - hdfs hadoop          0 2020-04-30 12:23 /user/zookeeper
```
3. Crea la carpeta "user" y dentro de ella tu usuario con el siguiente comando ``` hdfs dfs -mkdir /user/[nombre de usuario o cualquiera] ```
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -mkdir /user
mkdir: user: File exists
bryan@cluster-laboratorio-m:~$ hdfs dfs -mkdir /user/bryan-developer
```  
4. Lista el contenido de tu creada de la siguiente manera ``` hdfs dfs -ls /user/bryan-developer ``` no va retornar nada ya que la carpeta esta vacia
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -ls /user/bryan-developer
```
5. Vamos a generar un csv en [Mockaroo](https://www.mockaroo.com/) y mandarlo por consola a nuestra instancia VM con el siguiente comando ``` gcloud compute scp persona-data.csv cluster-laboratorio-m:~/ ```
6. Ahora se va guardar los datos en el cluster con el siguiente comando ``` hdfs dfs -put [ruta al archivo o archivo] [ruta en hadoop datanode] ```
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -mkdir /user/bryan-developer/test1
bryan@cluster-laboratorio-m:~$ hdfs dfs -put persona-data.csv /user/bryan-developer/test1
bryan@cluster-laboratorio-m:~$ hdfs dfs -ls /user/bryan-developer/test1
Found 1 items
-rw-r--r--   2 bryan hadoop      47883 2020-04-30 15:22 /user/bryan-developer/test1/persona-data.csv
```
7. para vizualizar el contenido del archivo usaremos el siguiente comando ``` hdfs dfs -cat [ruta al archivo en datanode] ```
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -cat /user/bryan-developer/test1/persona-data.csv
id,first_name,last_name,email,gender
1,Jonell,Lawranson,jlawranson0@blogger.com,Female
2,Conroy,Mockett,cmockett1@360.cn,Male
3,Adela,Skeats,askeats2@samsung.com,Female
4,Saraann,Purshouse,spurshouse3@liveinternet.ru,Female
5,Frederic,Poynter,fpoynter4@squarespace.com,Male
6,Windy,ODoherty,wodoherty5@miibeian.gov.cn,Female
7,Elysia,Brice,ebrice6@slashdot.org,Female
8,Allayne,Kropach,akropach7@mozilla.org,Male
9,Katey,Seppey,kseppey8@studiopress.com,Female
10,Revkah,Gosnell,rgosnell9@t.co,Female
```
8. Ejecutamos cksum del archivo en el sistema de archivos normal ``` cksum [ruta de tu archivo] ```  
```bash
bryan@cluster-laboratorio-m:~$ cksum persona-data.csv 
3973743809 47883 persona-data.csv
```
9. Ejecutamos cksum en hdfs ``` hdfs dfs -cat [ruta a tu archivo en hdfs] | cksum  ``` como no hay diferencia en los resultado no se ha perdido informacion
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -cat /user/bryan-developer/test1/persona-data.csv | cksum 
3973743809 47883
```
10. Crear un archivo vacio ``` hdfs dfs -touchz [ruta de creacion del archivo] ```
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -touchz /user/bryan-developer/voidfile.txt
bryan@cluster-laboratorio-m:~$ hdfs dfs -ls /user/bryan-developer
Found 2 items
drwxr-xr-x   - bryan hadoop          0 2020-04-30 15:22 /user/bryan-developer/test1
-rw-r--r--   2 bryan hadoop          0 2020-04-30 17:12 /user/bryan-developer/voidfile.txt
```
11. Crear de manera recursiva una estructura de carpetas ``` hdfs dfs -mkdir -p [ruta de creacion de las carpetas] ```
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -mkdir -p /user/bryan-developer/test2/una/nueva/ruta
bryan@cluster-laboratorio-m:~$ hdfs dfs -ls /user/bryan-developer/test2/una/nueva
Found 1 items
drwxr-xr-x   - bryan hadoop          0 2020-04-30 17:15 /user/bryan-developer/test2/una/nueva/ruta
```
12. Ver el espacio de un directorio ``` hdfs dfs -du -s -h [ruta del archivo o directorio] ```
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -du -s -h /user/bryan-developer
46.8 K  /user/bryan-developer
```
13. Ver el espacio del contenido de un directorio ``` hdfs dfs -du -s -h [ruta del archivo o directorio]/* ```
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -du -s -h /user/bryan-developer/*
46.8 K  /user/bryan-developer/test1
0  /user/bryan-developer/test2
0  /user/bryan-developer/voidfile.txt
```
14. Comando para eliminar un archivo ``` hdfs dfs -rm -f [ruta hacia el archivo] ```
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -ls /user/bryan-developer
Found 3 items
drwxr-xr-x   - bryan hadoop          0 2020-04-30 15:22 /user/bryan-developer/test1
drwxr-xr-x   - bryan hadoop          0 2020-04-30 17:15 /user/bryan-developer/test2
-rw-r--r--   2 bryan hadoop          0 2020-04-30 17:12 /user/bryan-developer/voidfile.txt
bryan@cluster-laboratorio-m:~$ hdfs dfs -rm -f /user/bryan-developer/voidfile.txt
Deleted /user/bryan-developer/voidfile.txt
```
15. Comando para eliminar una carpeta y sus archivos ``` hdfs dfs -rm -r -f [ruta hacia el directorio] ```
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -ls /user/bryan-developer/test2
Found 1 items
drwxr-xr-x   - bryan hadoop          0 2020-04-30 17:15 /user/bryan-developer/test2/una
bryan@cluster-laboratorio-m:~$ hdfs dfs -rm -r -f /user/bryan-developer/test2
Deleted /user/bryan-developer/test2
```
16. comando para crear en la raiz del directorio  ``` hdfs dfs -mkdir /[nombre del directorio] ```
```bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -mkdir /nueva_carpeta
bryan@cluster-laboratorio-m:~$ hdfs dfs -ls /
Found 4 items
drwx------   - mapred hadoop          0 2020-04-30 12:23 /hadoop
drwxr-xr-x   - bryan  hadoop          0 2020-04-30 17:27 /nueva_carpeta
drwxrwxrwt   - hdfs   hadoop          0 2020-04-30 12:24 /tmp
drwxrwxrwt   - hdfs   hadoop          0 2020-04-30 14:18 /user
```
17. Ingresar como super usuario a hadoop primero ejecutamos este comando ``` sudo su  ``` y luego ``` su hdfs ```
```bash
bryan@cluster-laboratorio-m:~$ sudo su
root@cluster-laboratorio-m:/home/bryan# su hdfs
hdfs@cluster-laboratorio-m:/home/bryan$
```
18. Comando para cambiar el grupo y el dueño del directorio o archivo ``` hdfs dfs -chown -R [usuario]:[grupo] [ruta del directorio] ```
```bash
hdfs@cluster-laboratorio-m:/home/bryan$ hdfs dfs -ls /nueva_carpeta
Found 1 items
drwxr-xr-x   - hdfs hadoop          0 2020-04-30 17:34 /nueva_carpeta/hijos
hdfs@cluster-laboratorio-m:/home/bryan$ hdfs dfs -chown -R bryan:bryan /nueva_carpeta/hijos
hdfs@cluster-laboratorio-m:/home/bryan$ hdfs dfs -ls /nueva_carpeta
Found 1 items
drwxr-xr-x   - bryan bryan          0 2020-04-30 17:34 /nueva_carpeta/hijos
```  
19. Comando para cambiar los permisos del archivo o directorio ``` hdfs dfs -chmod -R [Codigo de permisos] [ruta del directorio o archivo] ```
```bash
hdfs@cluster-laboratorio-m:/home/bryan$ hdfs dfs -ls /nueva_carpeta
Found 1 items
drwxr-xr-x   - bryan bryan          0 2020-04-30 17:34 /nueva_carpeta/hijos
hdfs@cluster-laboratorio-m:/home/bryan$ hdfs dfs -chmod 755 /nueva_carpeta/hijos
hdfs@cluster-laboratorio-m:/home/bryan$ hdfs dfs -ls /nueva_carpeta
Found 1 items
drwxr-xr-x   - bryan bryan          0 2020-04-30 17:34 /nueva_carpeta/hijos
hdfs@cluster-laboratorio-m:/home/bryan$ hdfs dfs -chmod 777 /nueva_carpeta/hijos
hdfs@cluster-laboratorio-m:/home/bryan$ hdfs dfs -ls /nueva_carpeta
Found 1 items
drwxrwxrwx   - bryan bryan          0 2020-04-30 17:34 /nueva_carpeta/hijos
```
## Ejercicios
- [x] [Guiado](#Guiado)  
- [ ] [Tarea](#Tarea) 
## Tarea
## Ejercicios
- [x] [Guiado](#Guiado)  
- [x] [Tarea](#Tarea) 
[GitHub](https://github.com/Bryan-Developer/Curso-Spark-Profesional/blob/development/Laboratorio01.md)  
[GitLab](https://gitlab.com/Bryan-Developer/curso-spark-profesional/-/blob/development/Laboratorio01.md)