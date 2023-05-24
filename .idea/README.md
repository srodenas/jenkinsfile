Fichero jenkins para automatizacion del build y despliegue:
1.- Creacion del build, descargar desde https://github.com/srodenas/hola-maven.git el codigo y crear el build con maven. El .war me lo guardo en Jenkins
2.- Hacer pruebas, que no tenemos.
3.- Hacer un push a https://github.com/srodenas/pruebaDespliegue.git del .war
4.- Desplegar el .war de jenkins por ssh al servidor de produccion.
(Necesitamos las credenciales bien configuradas)