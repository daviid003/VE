# Práctica Final
En esta práctica se ha llevado a cabo una explicación de los objetos y controladores que no se han visto en clase, además de incluir códigos de ejemplo de cada uno de ellos y un ejemplo que engloba la mayoría de ellos. 

## Controladores
En este apartado se van a definir los distintos controladores que no se han visto en clase de kubernetes. 

### StatefulSet
Un Statefulset es un controlador de Kubernetes que se usa para administrar y mantener uno o más pods. Está diseñado para usarse con aplicaciones con estado y sistemas distribuidos. Para saber cuándo usar StatefulSets, a diferencia de otros controladores, veamos dos ejemplos:
  * **1**: Tenemos una base de datos MySQL y la amplio a tres réplicas, y una aplicación quiere acceder al clúster de MySQL para leer y escribir datos. La solicitud se reenviará a tres pods. Sin embargo, la solicitud de escritura solo se reenviará al primer pod y los datos se sincronizarán con los otros pods. Esto se puede lograr usando Statefulset.
  * **2**: Cuando eliminamos MySQL pod o lo reiniciamos, se puede tener acceso a los datos en el mismo volumen, gracias a que eliminar o reducir un StatefulSet no eliminará los volumenes asociados con la aplicación con estado. 

Las aplicaciones con estado siempre necesitan una identidad fija. Mientras que el objeto de implementación de Kubernetes ofrece ID aleatorios para cada pod, el controlador StatefulSets de Kubernetes ofrece un número ordinal para cada pod a partir de cero, como mysql-0, mysql-1, mysql-2, etc.

Para aplicaciones con estado con un controlador StatefulSet, es posible configurar el primer Pod como principal y otros Pods como réplicas: el primer Pod manejará las solicitudes de lectura y escritura del usuario, y otros Pods siempre se sincronizarán con el primer Pod para la replicación de datos. Si el Pod muere, se crea un nuevo Pod con el mismo nombre.

<p align="center">
 <img src="https://loft.sh/blog/images/content/stateful-set-bp-5.png" width="" height="300" />
</p>

En resumen, StatefulSets ofrece las siguientes ventajas en comparación con los objetos de implementación:
  * **1**: Números ordenados para cada Pod.
  * **2**: El primer Pod puede ser primario, lo que lo convierte en una buena opción al crear una configuración de base de datos replicada, que maneja tanto la lectura como la escritura.
  * **3**: Otros Pods actúan como réplicas
  * **4**: Solo se crearán nuevos Pods si el Pod anterior está en estado de ejecución y clonará los datos del Pod anterior.
  * **5**: La eliminación de Pods ocurre en orden inverso.

### DaemonSet
Un Daemonset funciona de manera que se garantiza que todos (o algunos) de los nodos ejecuten una copia de un Pod. Conforme se añaden más nodos al clúster, nuevos Pods son añadidos a los mismos. Conforme se eliminan nodos del clúster, dichos Pods se destruyen. Al eliminar un DaemonSet se limpian todos los Pods que han sido creados.

<p align="center">
 <img src="https://bucket-hg.oss-cn-shanghai.aliyuncs.com/img/daemonset.png" width="" height="300" />
</p>

Algunos de los casos en los que se usa este tipo de controlador son:
 * Ejecutar un proceso de almacenamiento en el clúster.
 * Ejecutar un proceso de recolección de logs en cada nodo.
 * Ejecutar un proceso de monitorización de nodos en cada nodo.
 
De forma básica, se debería de usar un DaemonSet, cubriendo todos los nodos, por cada tipo de proceso.

### Job
Un Job funciona creando uno o más pods y continuará reintentando la ejecución de los pods hasta que un número específico de ellos finalice con éxito. A medida que los pods se completan con éxito, el trabajo realiza un seguimiento de las finalizaciones exitosas. Cuando se alcanza un número específico de finalizaciones correctas, la tarea (es decir, por el Job) se completa. Eliminar un Job limpiará los Pods que creó. La suspensión de un Job eliminará sus Pods activos hasta que el Job se reanude nuevamente.

<p align="center">
 <img src="https://miro.medium.com/max/607/1*OSKkb761FJ2gShR6sYdnjQ.png" width="" height="300" />
</p>

Un caso simple es crear un objeto Job para ejecutar de manera fiable un pod hasta su finalización. El objeto Job iniciará un nuevo Pod si el primer Pod falla o se elimina.
También se puede usar un Job para ejecutar varios Pods en paralelo.
Si se deseda ejecutar un Job en un horario concreto se utiliza CronJob, que ejecuta el Job periódicamente en el horario indicado.


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
  
<p align="center">
 <img src="https://assets.openshift.com/hubfs/Imported_Blog_Media/pv_arch.png" width="450" height="390" />
</p>
  
  
### Namespace
Namespace es un objeto que permite crear una clasificación adicional en un recurso, es decir, dentro de un recurso se pueden crear diferentes agrupaciones en función del objetivo del mismo. Esta herramienta es muy útil cuando disponemos de múltiples agrupaciones con nombres parecidos en el mismo clúster. Gracias a este objeto, la comunicación pod-to-pod se ve facilitada si se emplea el mismo namespace. Al ser clústeres virutales, se pueden ubicar encima del mismo clúster físico. 
<p align="center">
 <img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1624180587417/PJTk9hiNv.png?auto=compress,format&format=webp" width="500" height="300" />
</p>


### Secrets
Secrets son objetos que permiten almacenar datos confidenciales, como lo son nombres de usuario y contraseñas o tokens. Estos objetos pueden crearse a partir de archivos TXT o desde archivos yaml. Una vez son creados, estos objetos se pueden estructurar en un pod o en el controlador de réplicas como variables de entorno o volúmenes. Debido a que los secretos se pueden crear independientemente de los pods que los usan, existe menos riesgo de que el secret (y sus datos) queden expuestos durante el flujo de trabajo de creación, visualización y edición de pods. Kubernetes y las aplicaciones que se ejecutan en su clúster también pueden tomar precauciones adicionales con los secrets, como evitar escribir datos secretos en el almacenamiento no volátil.
<p align="center">
 <img src="https://drek4537l1klr.cloudfront.net/yuen/v-6/Figures/07_img_0001.png" width="450" height="350" />
</p>


### ConfigMaps
ConfigMaps son objetos que permiten almacenar datos no confidenciales con formato clave-valor. Los pods pueden emplear estos objetos como variables de entorno, argumentos en la línea de comando o como ficheros de configuración en un volumen. A diferencia de Secrets, este objeto no proporciona encriptación alguna. Los ConfigMaps están diseñados específicamente para encapsular pequeñas cantidades de datos de configuración insensibles. Se usan comúnmente para almacenar la dirección IP del servidor de la base de datos, la dirección de correo electrónico saliente para su aplicación y otras configuraciones específicas de la aplicación que necesita configurar fuera de sus Pods.

<p align="center">
 <img src="https://drek4537l1klr.cloudfront.net/luksa/Figures/07fig02.jpg" width="450" height="350" />
</p>
