# Práctica Final
## Objetos
En este apartado se van a definir los distintos objetos que no se han visto en clase de Kubernetes.
### Volume
Un volumen es un directorio al que pueden acceder los contenedores de un pod. Estos volúmenes no se limitan a ningún contenedor, sino que adminte todos los contenedores implementados dentro del pod. Existen numerosos tipos de volúmenes con diferentes configuraciones. Los más destacados son los siguientes:
  * **emptyDir**: Tipo de volumen que se crea cuando un pod se asigna por primera vez a un nodo. Permanece activo mientas el pod se ejecuta en ese nodo. Inicialmente se encuentra vacío y los contenedores en el pod pueden leer y escribir sobre él. Una vez se elimina el pod del nodo, se borran todos los datos.
  * **hostPath**: Tipo de volumen que monta un archivo o directorio desde el sistema de archivos del nodo host en su pod.
  * **gcePersistentDisk**: Tipo de volumen que monta un disco persistente de GCE (Google Compute Engine) en su pod. Los datos almacenados en el GCE permanecen intactos cuando el pod se elimina del nodo.
  * **awsElasticBlockStore**: Tipo de volumen con la misma función que el gcePersistentDisk, pero en vez de montarlo en un disco persistente de GCE, lo hace sobre un Elastic Block Store de Amazon Web Service (AWS). Una vez se elimine el pod del nodo, sus datos también permanecen intactos. 
  * **nfs**: Tipo de volumen que permite montar un NFS (sistema de archivos en red) existente en su módulo. Los datos almacenados en este tipo de volúmenes no se borran cuando el pod se elimina del nodo.
  * **iscsi**: Tipo de volumen que permite montar un volumen iSCSI (SCSI sobre IP) existente en su pod. 

En los tipos de volúmenes que se acaban de mencionar, se ha nombrado la peresistencia de volúmenes, es decir, no existe pérdida de datos tras la eliminación del pod. Dentro de los volúmenes persistentes existen dos tipos:
  * ***PV*** (Volumen Persistente): Se trata de un espacio de almacenamiento en la red que ha sido aprovisionada por el administrador. Es un recurso en el clúster que es independiente de cualquier pod individual que use el PV. 
  * ***PVC*** (Reclamación de Volumen Persistente): Se trata de un almacenamiento solicitado por Kubernetes para sus pods. El usuario no necesita conocer el aprovisionamiento subyacente, es decir, el usuario desconoce como y donde se le ha asignado el espacio solicitado. 
  
### Namespace
Namespace es un objeto que permite crear una clasificación adicional en un recurso, es decir, dentro de un recurso se pueden crear diferentes agrupaciones en función del objetivo del mismo. Esta herramienta es muy útil cuando disponemos de múltiples agrupaciones con nombres parecidos en el mismo clúster. Gracias a este objeto, la comunicación pod-to-pod se ve facilitada si se emplea el mismo namespace. Al ser clústeres virutales, se pueden ubicar encima del mismo clúster físico. 
<p align="center">
 <img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1624180587417/PJTk9hiNv.png?auto=compress,format&format=webp" width="350" height="200" />
</p>

### Secrets
Secrets son objetos que permiten almacenar datos confidenciales, como lo son nombres de usuario y contraseñas (con cifrado). Estos objetos pueden crearse a partir de archivos TXT o desde archivos yaml. Una vez son creados, estos objetos se pueden estructurar en un pod o en el controlador de réplicas como variables de entorno o volúmenes. 
<p align="center">
 <img src="https://drek4537l1klr.cloudfront.net/yuen/v-6/Figures/07_img_0001.png" width="300" height="200" />
</p>

### ConfigMaps
ConfigMaps son objetos que permiten almacenar datos no confidenciales con formato clave-valor. Los pods pueden emplear estos objetos como variables de entorno, argumentos en la línea de comando o como ficheros de configuración en un volumen. A diferencia de Secrets, este objeto no proporciona encriptación alguna. 

<p align="center">
 <img src="https://cloud.google.com/kubernetes-engine/images/configmap.png" width="150" height="230" />
</p>
