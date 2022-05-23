# Práctica Final
## Objetos
En este apartado se van a definir los distintos objetos que no se han visto en clase de kubernetes.
### Volume
Un volumen es un directorio al que pueden acceder los contenedores de un pod. Estos volúmenes no se limitan a ningún contenedor, sino que adminte todos los contenedores implementados dentro del pod. Existen varios tipos: 
  * **emptyDir**: Tipo de volumen que se crea cuando un pod se asigna por primera vez a un nodo. Permanece activo mientas el pod se ejecuta en ese nodo. Inicialmente se encuentra vacío y los contenedores en el pod pueden leer y escribir sobre él. Una vez se elimina el pod del nodo, se borran todos los datos.
  
### Namespace

### Secrets

### ConfigMaps
