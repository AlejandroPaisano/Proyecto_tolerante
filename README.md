# Proyecto_tolerante

# Autores
Alejandro Paisano Flores

Luis Javier Cisneros Casillas

# Introduccion
Nuestra aplicacion creada con ayuda de node js, ejs y html genera una pequeña base de datos desde la cual podemos introducir articulos, buscar articulos dentro de la misma base, editarlos y eliminarlos de ser necesario.

![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/41bb0693-9958-4955-822f-5473801c7dc9) 
Imagen de la interfaz inicial

![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/6733e594-06a3-41bd-b052-982f2deec660)
Imagen de la interfaz para agregar objetos

![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/f562210c-7d3d-4a5f-8e79-bbc39c2b044e)
Imagen de la interfaz de los objetos

![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/3d34639c-544a-498b-ad04-d986a5f9122f)
Imagen de la interfaz de edicion

![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/b129eda2-1676-46dd-b60f-8faaad6bef35)
Imagen de la interfaz de objeto

Requisitos preevios:

Visual studio code
Instalar docker 
helm
minikube
kubernetes
istio
scoop
git

Primero pasaremos a los detalles de instalacion:

Para instalar helm, necesitaremos instalar scoop con anterioridad, para ello necesitaremos una version superior a la 5 de powershell (esto se puede saber escribiendo el comando Get-Host | Select-Object Version en un powershell). 
E instalar .net en su version mas reciente https://dotnet.microsoft.com/es-es/download (este es un ejecutable).

Hecho esto, correremos estos dos comandos en un powershell 

Set-ExecutionPolicy RemoteSigned -Scope CurrentUser # Optional: Needed to run a remote script the first time

irm get.scoop.sh | iex


ahora tendremos scoop instalado

Ahora descargaremos docker y crearemos una cuenta, esto para facilitarnos el uso de la imagen https://www.docker.com/products/docker-desktop/
Con nuestro docker listo e instalado, vamos a correr el siguiente comando: docker compose up --build
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/cd6fdef6-d519-482f-934d-3040e13612a1)

Con esto levantaremos nuestro servicio, el cual podremos ver en nuestra aplicacion de docker desktop:
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/2604346e-1ed7-4e85-98c7-b3205f1a2fb8)


una vez que nuestro servicio este funcional, podremos subirlo a nuestra cuenta de dockerhub, para ello usaremos el siguiente 
comando

docker push nombre-de-usuario/microser:latest

(este ultimo puede cambiar de acuerdo a la tagname que le agregemos al docker)

con esto hecho, podremos acceder a nuestro dockerhub y ver que nuestro archivo ha sido subido de forma exitosa a docker.

Como anotacion adicional, deberemos cambiar esta variale en el archivo compose, para poder permitirle a nuestro programa sacar la imagen desde nuestro propio dockerhub

![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/3d8866fc-33d6-49f2-b6b4-f6fbd0a48fae)

con esto hecho, pasaremos a la instalacion de kubernetes, minikubes y kompose para convertir nuestro archivo de compose a kubernetes.

Primero, instalaremos kubernetes para ello podemos usar el siguiente comando, solo si tenemos curl instalado

curl -LO https://dl.k8s.io/release/v1.28.4/bin/windows/amd64/kubectl.exe

En caso contrario, podemos instalar el binario desde la siguiente pagina https://kubernetes.io/es/docs/tasks/tools/included/install-kubectl-windows/
podemos comporobar que kubernetes se instalo correctamente con el siguiente comando:  

kubectl version --short

![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/7fd4962a-7cb5-4d46-9689-6ed8c49f61a7)

Despues instalaremos kompose, este se puede instalar de forma sencilla con el siguiente comando 

winget install Kubernetes.kompose

podemos comprobar que kompose se instalo con el siguiente comando 

kompose version

Y ahora instalaremos nuestro servicio de minikubes. Para ello primero descargaremos el instalador de la ultima version desde el siguiente enlace: https://minikube.sigs.k8s.io/docs/start/
con esto hecho, modificaremos la variable de entorno path con el siguiente comando en un powershell con permisos de administrador administrador:

$oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
if ($oldPath.Split(';') -inotcontains 'C:\minikube'){
[Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine)
}

En caso de que esto no se pueda, podemos sobre escribir la variable de path entrando en la aplicacion "editar las variables del entorno"
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/a054e2ff-a184-40cd-af36-63b7fd988759)

![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/0bbc8cba-ad46-4b00-b0ea-127910aa1895)

Aqui podemos modificar la variable path en ambos, la cuenta y el sistema para asegurarnos de que el cambio se refleje, para ello haremos doble click en la propia variable path, luego presionaremos el boton nuevo y escribiremos la direccion en la que se encuentra el exe de minikubes

![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/3ac282d7-13b6-4d5e-bc78-37ad405ca4d9)

Con esto hecho, cerraremos la consola y la volveremos a abrir antes de continuar.

El siguiente paso se puede saltar en caso de que ya se tengan los archivos yaml: usaremos el comando kompose convert -f docker-compose.yaml para convertir nuestro archivo docker compose en archivos de tipo yaml para kubernetes.
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/266ba4ac-7450-4ede-9634-339b7db54f7d)

con los yaml en nuestras manos, primero pasaremos a iniciar nuestro minikube con el comando 

minikube start
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/cbdf208b-12c3-4db8-96e3-e7031ba8d6f3)

(Para hacer uso de este comando, deberemos tener nuestra aplicacion de docker desktop activa)

esto creara una imagen de docker llamada "minikube" y cmabiara el kubectl para usar minikubes como su repositorio por defecto para kubernetes. Ahora pasaremos a crear nuestros deployments y servicios, primero iniciaremos nuestros servicios con el comando kubectl apply -f nombre-del-deployment

En este caso usaremos estos comandos:

 kubectl apply -f node-db-deployment.yaml
 kubectl apply -f node-db-service.yaml
 kubectl apply -f node-db-data-persistentvolumeclaim.yaml
 kubectl apply -f node-app-deployment.yaml
 kubectl apply -f node-app-service.yaml
 
Con esto tendremos nuestros servicios levantados y podremos verlos con los siguientes comandos:

kubectl get pods  
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/662eb8fa-0b15-4a73-a534-c82d0f6e0c61)

kubectl get svc
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/cf1f6d1d-3a8e-4632-9399-f7a7e71bb167)


(ignorar el kube monkey y el dos en ready, eso se debe al istio que se vera mas adelante)

Con esto hecho, podemos usar el comando de minikubes dashboard para poder lanzar un adshboard donde se mostrar el estado de los pods
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/039fc1d1-c977-48e7-9db6-8c3844a84e41)

y ahora usaremos dos comandos:

minikubes tunnel 

para poder hacer un tunel de conexion entre nuestro equipo y los kubernetes

![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/2b5673b8-cb57-4507-9942-c039282d539f)

Y ahora podremos entrar a nuestro servicio desde kubernetes escribiendo el comando

minikubes service nombre-de-la-aplicacion (en nuestro caso node-app)

Ahora vamos a pasar a instalar istio, primero entraremos en el siguiente link  https://github.com/istio/istio/releases?ref=enmilocalfunciona.io para descargar el paquete de istion que corresponda a nuestro sistema operativo, para winddows es la version 1.19.4

Ahora haremos lo mismo que con minikubes y agregaremos a las variables de path las direcciones del exe de istio, estas se suelen almacenar dentro de su carpetan bin.
Ahora ejecutaremos el siguiente comando para configurar istio

istio install --set profile=demo -y
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/f1b8c9bc-1401-41f3-9093-08e5bf8d3ddc)


Y usaremos el siguiente comando para configurar permitir la inyeccion de istio en nuestros pods

kubectl label namespace default istio-injection=enabled 
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/9f48d974-265f-4416-81d6-a33ed21e6441)

Usando el comando kubectl get deployment -n istio-system comprobaremos que los elementos de istio estan activos
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/ce580e90-d760-446f-935b-40594ca691f8)

Ahora borramos los deployments y lo services con los comandos:

 kubectl delete deployment node-db
 kubectl delete service node-db
 kubectl delete deployment node-app
 kubectl delete service node-app

 Y volveremosa a levantar los servicios con los comandos apply

 Con esto hecho, ahora los deployments tendran controladores de istio que nos permitiran seguir su telemetria usando el dashboard de kiali, este lo podemos levantar con el siguiente comando 

 istioctl dashboard kiali
 ![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/fd0d0789-e1df-4bca-aa54-f4567cc18d3f)
 
 ![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/68ca3583-10f2-4f51-ab22-6e59d206a6ee)

Aqui podremos ver los pods, mas importante, podremos ver el trafico entre ellos

![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/d779f75b-64c3-4147-a3b2-7af55fba109e)
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/9891ba86-de65-465f-a908-594bfbcf6d79)

Por ultimo, añadiremos kube-monkey, para ello primero instalaremos git. Entraremos a este link https://git-scm.com/download/win y usaremos el instalador, eligiendo las opciones por defecto

Luego, usaremos scoop para instalar helm con el comando:

scoop helm

Despues, usaremos este comando para clonar el repositorio de kobe-monkey

git clone https://github.com/asobti/kube-monkey

Con esto hecho, usaremos el comando 

helm install mmy-release kubemonkey/kube-monkey --version (numero de version dentro del repositorio)

Para instalar kube monkey en nuestra carpeta, usandoe el comando

kubectl get pods

podemos confirmar que kube-monkey esta activo
![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/129ab268-3b09-4815-b50d-b3c413c4824f)

Ahora, no notaremos que el servicio caiga, aunque este este configurado para ello, esto se debe a que su yaml esta configurado para reiniciarse tan pronto el mismo servicio caiga

![image](https://github.com/AlejandroPaisano/Proyecto_tolerante/assets/91223611/43490230-1c95-43a7-8ed4-8cf40424a65f)



