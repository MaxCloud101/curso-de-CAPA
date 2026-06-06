# Diffing Customization

El Diffing Customization en Argo CD sirve para indicarle a Argo:

"Ignora estas diferencias porque sé que Kubernetes o algún controlador las va a modificar automáticamente."

Sin esta funcionalidad, muchas aplicaciones aparecen constantemente como OutOfSync aunque realmente no haya ningún problema.

## ¿Por qué ocurre?

Argo CD compara:
```
Git (Desired State)
        VS
Cluster (Live State)
```
Si encuentra diferencias:
```
Git != Live
```
marca la aplicación como:
```
OutOfSync
```
El problema es que Kubernetes y muchos operadores modifican recursos después de aplicarlos.

Por ejemplo:
```
spec:
  replicas: 3
```
Podría terminar en el cluster como:
```
spec:
  replicas: 5
```
porque un HPA lo cambió.

Argo verá:
```
3 != 5
```
y marcará OutOfSync.
