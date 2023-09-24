# Workshop Nerdearla: De local a Kubernetes 

## Paso 0: Instalar Herramientas/Entorno de trabajo

1. En este repositorio veran un directorio llamado Vagrant, el cual nos permitira levantar una maquina virtual con las herramientas necesarias para el workshop (docker, minikube, kubectl, argocd cli, etc)

Para poder levantar esta maquina, iremos al directorio Vagrant y corremos el siguiente comando: 

```bash
vagrant up
```

2. Ya con la maquina virtual levantada, nos podremos conectar con el siguiente comando 

```bash
vagrant ssh 
```

3. Es importante destacar que podemos hacer un tunel de vagrant a nuestra maquina host, esto nos permitira acceder a algun servicio corriendo en la VM desde el host. Para esto, usaremos el siguiente comando:

```bash
vagrant ssh -- -L 5000:localhost:5000
```

## Paso 1: Dockerfile

1. Para construir la imagen de docker podremos hacerlo de forma manual descargando el codigo y haciendo un docker build o utilizar la github action ya configurada para que en cada cambio que hagamos en algun archivo del directorio Docker, se ejecute y haga el proceso de construir la imagen y subirla a dockerhub, es importante haber activado Github Actions y configurado los secretos para subir la imagen a dockerhub. 

## Paso 2: Kubernetes

1. Para este paso es tan simple como verificar estar conectados a nuestro cluster de kubernetes (poder ejecutar un kubectl get pods -A sin errores) y asi crear nuestros recursos dentro del directorio Kubernetes con el siguiente comando:

```bash
kubectl apply -f Kubernetes/deployment.yaml
```

En caso de que querramos probar el escalado de nuestros recursos (deployments o replicasets) podremos utilizar un comando similar al siguiente:

```bash
kubectl scale --replicas=1 deployment/pokedex-flask -n nerdearla 
```


## Paso 3: ArgoCD 

1. Por ultimo, tendremos la parte de ArgoCD. Podremos crear aplicaciones ya sea utilizando la CLI o utilizando la interfaz grafica de Argo. En este caso utilizaremos el archivo deployment.yaml dentro del directorio pokedex-flask en el directorio de ArgoCD. Por ejemplo si quisieramos hacerlo con el ArgoCD CLI: 

```bash
argocd app create pokedex --repo https://github.com/zdenkotraste/workshop-nerdearla --path ArgoCD/pokedex-flask --dest-server https://kubernetes.default.svc --dest-namespace nerdearla
```

Es importante mencionar que en este caso el namespace "nerdearla" ya debe existir. 

Podremos crearlo con el siguiente comando:

```bash
kubectl create ns nerdearla
```