# Laboratorio 01: Curso Spark Profesional  
## Preaparaciones
1. Tener cuenta [Google Cloud Plaform](https://cloud.google.com/?&utm_source=google&utm_medium=cpc&utm_campaign=latam-PE-all-es-dr-bkws-all-all-trial-e-dr-1008075-LUAC0010196&utm_content=text-ad-none-none-DEV_c-CRE_382274978090-ADGP_BKWS+%7C+Multi+~+GCP-KWID_43700047166266623-kwd-155951229-userloc_9073192&utm_term=KW_gcp-ST_GCP&gclid=Cj0KCQjw7qn1BRDqARIsAKMbHDbWZr4pzBc3YJ9tji0hQIX7tp8d28BjO7oN3CR7x6mbHk76tVhZrAgaAtcwEALw_wcB&gclsrc=aw.ds).  
2. Crear instacia de DataProc puede ser de manera UI o utilizar el siguiente comando en la consola.
```bash
gcloud beta dataproc clusters create cluster-laboratorio --enable-component-gateway --region us-central1 --subnet default --zone us-central1-b --master-machine-type n1-standard-2 --master-boot-disk-size 100 --num-workers 2 --worker-machine-type n1-standard-2 --worker-boot-disk-size 300 --image-version 1.3-deb9 --optional-components ANACONDA,JUPYTER --scopes 'https://www.googleapis.com/auth/cloud-platform' --project bryan-data-engineer
```
## Ejercicios
- [ ] [Guiado](#Guiado)  
- [ ] Tarea  
### Guiado
[GitHub](https://github.com/Bryan-Developer/Curso-Spark-Profesional/blob/development/Laboratorio01.md)  
[GitLab](https://gitlab.com/Bryan-Developer/curso-spark-profesional/-/blob/development/Laboratorio01.md)