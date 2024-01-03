# Node.JS + MySQL Web App.<br><br>Setting up service mesh with Istio.

<p align="center">
  <a href="https://skillicons.dev">
    <img height="50" width="50" src="https://cdn.simpleicons.org/helm/blue" /><img height="50" width="50" src="https://cdn.simpleicons.org/istio/blue" /><img src="https://skillicons.dev/icons?i=kubernetes,azure,docker,nodejs,mysql"/><svg role="img" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><title>Istio</title><path d="M4 21 20 21 10 24zM4 20 10 19 10 8zM11 19 20 20 11 0z"/></svg>
  </a>
</p>



<br>

> [!NOTE]
> This is a part of a series of demo projects in which I manipulate a Node.js application applying various technologies.<br>
>
> The app built using Node.js and Express and integrates with MySQL database, originally presented at this [GitHub Repository](https://github.com/otam-mato/nodejs_mysql_web_app_terraform.git). The previous [project](https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes.git) involved the "Canary" deployment on **AWS EKS**.
>
> In the current installment, we're deploying the web application on **MS Azure AKS** using **HELM**. Subsequently, we'll introduce the second version of the app and then rollback to the first version.
<br>

## Deployment Strategy

The **HELM** is a package manager for Kubernetes applications. It simplifies the deployment and management of applications on Kubernetes clusters. Helm uses charts, which are packages of pre-configured Kubernetes resources, to define, install, and upgrade even the most complex Kubernetes applications and their dependencies. It helps streamline the deployment process, reduce errors, and enable versioning and rollbacks for Kubernetes applications.

1. In this demo, we will use **HELM** to deploy:
   - V1 deployment which will host the version 1 of the app.
   - V2 deployment which will host the version 2 of the app.
   - Finally we will rollback V2 to V1.

   V1 and V2 versions were previously uploaded into this demo [DockerHub repository](https://hub.docker.com/repository/docker/montcarotte/fullstack_nodejs_mysql_demo/general)

2. **The app and its database are placed in separate cantainers and separate Kubernetes pods to secure decoupling, resource isolation, scaling and resilience.**

<br>

## Technologies used
- **MS Azure**
- **AKS (Azure Kubernetes Service)**
- **Node.JS**
- **Express**
- **JavaScript**
- **MySQL**
- **Docker**
- **Kubernetes**
- **HELM**
<br>
