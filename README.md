## Decisiones Técnicas y Razonamiento
### Estructura
He creado una aplicación muy sencilla que te da la temperatura prevista para los próximos días.  
He dividido este proyecto en 2 servicios, un backend y un frontend para poder manejarlos independientemente.  
La carpeta k8s contiene todos los manifiestos para ambos servicios (Tanto deployment como service), he decidido también incluir el kustomization.yaml para poder mantener mejor los manifiestos ante futuros cambios.

### 1. Contenerización
He creado un Dockerfile para cada servicio.
Utilizando el entorno Docker de Minikube he incluido las imagenes en Minikube con el nombre de `weather-frontend` y `weather-backend`.

<img width="886" height="400" alt="1" src="https://github.com/user-attachments/assets/9256df1b-cd51-45f1-8d68-8dea51440fe4" />

### 2. Salud (Probes)
He configurado los Health Checks específicos para garantizar la estabilidad del sistema en ambos servicios (Liveness Probe y Readiness Probe).
Utilice el tipo ClusterIP en ambos Pods para poder acceder a los servicios.

### 3. Despliegue con Argo CD (GitOps)
Una vez accedido a Argo CD, cree una aplicación apuntando a la carpeta k8s/. Para que cualquier cambio de los manifiestos en GitHub se refleje en el clúster, así el estado de Git será el estado real.

<img width="1919" height="963" alt="2" src="https://github.com/user-attachments/assets/3c9ff39b-d171-4f0e-a03c-8244dc3d7d5f" />

### 4. Resultado
Una vez se ha sincronizado y se han pasado todos los healthcheck, GitHub es ahora la única fuente de verdad.  
  
Como adición, en el desarrollo me apareció el error 143 (SIGTERM) en el pod de backend. Comprobé que se debia a que a Spring no le daba tiempo a inicializar antes de realizar los healthchecks.
Por lo que ajusté los tiempos para que Spring cargara completamente antes de recibir healthchecks.

<img width="1919" height="965" alt="3" src="https://github.com/user-attachments/assets/fdb6ecca-8027-4bf4-9b7c-dfd76f00d638" />
