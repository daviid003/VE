# Práctica Final
## Controladores
En este apartado se van a definir los distintos controladores que no se han visto en clase de kubernetes. 
### StatefulSet
Un Statefulset es un controlador de Kubernetes que se usa para administrar y mantener uno o más pods. Está diseñado para usarse con aplicaciones con estado y sistemas distribuidos. Para saber cuándo usar StatefulSets, a diferencia de otros controladores, veamos dos ejemplos:
  * **1**: Tenemos una base de datos MySQL y la amplio a tres réplicas, y una aplicación quiere acceder al clúster de MySQL para leer y escribir datos. La solicitud se reenviará a tres pods. Sin embargo, la solicitud de escritura solo se reenviará al primer pod y los datos se sincronizarán con los otros pods. Esto se puede lograr usando Statefulset.
  * **2**: Cuando eliminamos MySQL pod o lo reiniciamos, se puede tener acceso a los datos en el mismo volumen, gracias a que eliminar o reducir un StatefulSet no eliminará los volumenes asociados con la aplicación con estado. 

Las aplicaciones con estado siempre necesitan una identidad fija. Mientras que el objeto de implementación de Kubernetes ofrece ID aleatorios para cada pod, el controlador StatefulSets de Kubernetes ofrece un número ordinal para cada pod a partir de cero, como mysql-0, mysql-1, mysql-2, etc.

Para aplicaciones con estado con un controlador StatefulSet, es posible configurar el primer Pod como principal y otros Pods como réplicas: el primer Pod manejará las solicitudes de lectura y escritura del usuario, y otros Pods siempre se sincronizarán con el primer Pod para la replicación de datos. Si el Pod muere, se crea un nuevo Pod con el mismo nombre.

En resumen, StatefulSets ofrece las siguientes ventajas en comparación con los objetos de implementación:
  * **1**: Números ordenados para cada Pod.
  * **2**: El primer Pod puede ser primario, lo que lo convierte en una buena opción al crear una configuración de base de datos replicada, que maneja tanto la lectura como la escritura.
  * **3**: Otros Pods actúan como réplicas
  * **4**: Solo se crearán nuevos Pods si el Pod anterior está en estado de ejecución y clonará los datos del Pod anterior.
  * **5**: La eliminación de Pods ocurre en orden inverso.


  
### DaemonSet

### Job
