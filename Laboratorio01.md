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
- [ ] Tarea  
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
``` bash
bryan@cluster-laboratorio-m:~$ hdfs dfs -mkdir /user/bryan-developer/test1
bryan@cluster-laboratorio-m:~$ hdfs dfs -put persona-data.csv /user/bryan-developer/test1
bryan@cluster-laboratorio-m:~$ hdfs dfs -ls /user/bryan-developer/test1
Found 1 items
-rw-r--r--   2 bryan hadoop      47883 2020-04-30 15:22 /user/bryan-developer/test1/persona-data.csv
```
7. para viz
[GitHub](https://github.com/Bryan-Developer/Curso-Spark-Profesional/blob/development/Laboratorio01.md)  
[GitLab](https://gitlab.com/Bryan-Developer/curso-spark-profesional/-/blob/development/Laboratorio01.md)