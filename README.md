[under revision]

# Node.JS + MySQL Web App.<br><br>Setting up a service mesh with Istio.<br><br>Observability with Kiali, Jaeger, Prometheus, Grafana.

<p align="center">
  <a href="https://skillicons.dev">
    <img height="50" width="50" src="https://cdn.simpleicons.org/Istio/#4863A0" /><img height="50" width="50" src="https://cdn.simpleicons.org/helm/blue" /><img src="https://skillicons.dev/icons?i=kubernetes,aws,docker,nodejs,mysql,prometheus,grafana"/><img height="50" width="40" src="https://github.com/otam-mato/istio/assets/113034133/1f62a830-6e8a-46bd-9e1b-ca60da77a662" /><img height="50" width="150" src="https://github.com/otam-mato/istio/assets/113034133/a1ebe489-2c75-41c2-9026-addaabc273d7" /><img height="50" width="50" src="https://cdn.simpleicons.org/terraform/blue" />
  </a>
</p>




<br>

> [!NOTE]
> This is a part of a series of demo projects in which I manipulate a Node.js application applying various technologies.<br>
>
> The app built using Node.js and Express and integrates with MySQL database, originally presented at this [GitHub Repository](https://github.com/otam-mato/nodejs_mysql_web_app_terraform.git). The previous [project](https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes.git) involved deployment of the app on **Azure AKS**.
>
> Transition to microservices often brings complexities related to traffic management, security, and observability. This is where **Istio** steps in, offering a comprehensive service mesh solution that streamlines these challenges. Istio is an open source service mesh platform designed to enhance the management, security, policy enforcement, and observability of microservices-based applications.
> 
> In the current installment, we're setting up the service mesh with **Istio** and demonstrate observability features with **Kiali**, **Jaeger**, **Prometheus** and **Grafana**.
<br>

## Deployment Strategy

1. We will be using these official [Terraform-based EKS Blueprints]() for Istio to deploy:
   - An EKS cluster and other relevant resources.
   - A service mesh based on Istio.
     
2. We will use V1 and V2 versions of the app which were previously uploaded into this demo [DockerHub repository](https://hub.docker.com/repository/docker/montcarotte/fullstack_nodejs_mysql_demo/general)
   - V1 deployment which will host the version 1 of the app.
   - V2 deployment which will host the version 2 of the app.
  
3. We will deploy both app versions simultaneously using **HELM**

<br>

## Technologies used
- **Istio**
- **Amazon Web Services**
- **AWS EKS**
- **Kiali**
- **Jaeger**
- **Prometheus**
- **Grafana**
- **Node.JS**
- **Express**
- **JavaScript**
- **MySQL**
- **Docker**
- **Kubernetes**
- **HELM**
<br>

## Functionality

This two-tier web application interfaces with a MySQL database, facilitating CRUD (Create, Read, Update, Delete) operations on the database records.

**<details markdown=1><summary markdown="span">Detailed app description</summary>**

## Summary

The app sets up a web server for a supplier management system. It allows viewing, adding, updating, and deleting suppliers. 

#### **Dependencies and Modules**:
   - **express**: The framework that allows us to set up and run a web server.
   - **body-parser**: A tool that lets the server read and understand data sent in requests.
   - **cors**: Ensures the server can communicate with different web addresses or domains.
   - **mustache-express**: A template engine, letting the server display dynamic web pages using the Mustache format.
   - **serve-favicon**: Provides the small icon seen on browser tabs for the website.
   - **Custom Modules**: 
     - `supplier.controller`: Handles the logic for managing suppliers like fetching, adding, or updating their details.
     - `config.js`: Keeps the server's settings for connectind to the MySQL database.

#### **Configuration**:
   - The server starts on a port taken from a setting (like an environment variable) or uses `3000` as a default.

#### **Middleware**:
   - It's equipped to understand data in JSON format or when it's URL-encoded.
   - It can chat with web pages hosted elsewhere, thanks to CORS.
   - Mustache is the chosen format for web pages, with templates stored in a folder named `views`.
   - There's a public storage (`public`) for things like images or stylesheets, accessible by anyone visiting the site.
   - The site's tiny browser tab icon is fetched using `serve-favicon`.

#### **Routes (Webpage Endpoints)**:
   - **Home**: `GET /`: Serves the home page.
   - **Supplier Operations**: 
     - `GET /suppliers/`: Fetches and displays all suppliers.
     - `GET /supplier-add`: Serves a page to add a new supplier.
     - `POST /supplier-add`: Receives data to add a new supplier.
     - `GET /supplier-update/:id`: Serves a page to update details of a supplier using its ID.
     - `POST /supplier-update`: Receives updated data of a supplier.
     - `POST /supplier-remove/:id`: Removes a supplier using its ID.

#### **Starting Up**:
   - The server comes to life, starts listening for visits, and announces its awakening with a log message.

</details>

<br>

## Architecture

<br>

![image](https://github.com/otam-mato/istio/assets/113034133/241749c0-e9a1-452b-adde-1b891891d3f8)

<br>

## ScreenShots

<br>

## Prerequisites

- A work station or an **EC2 instance**.
- I will be using **AWS EKS**, and this official blueprint to deploy AWS resources, EKS cluster and Istio: <br>
- **HELM** [installed](https://helm.sh/docs/intro/install/)
- **ISTIO** [installed](https://istio.io/latest/docs/setup/getting-started/)
- **Kubectl** [installed](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)
- **AWS CLI** [installed](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html#getting-started-install-instructions)
- **AWS configure**

  https://istio.io/latest/docs/setup/platform-setup/amazon-eks/ <br>
  https://aws-ia.github.io/terraform-aws-eks-blueprints/patterns/istio/

  <img width="800" alt="Screenshot 2024-01-14 at 21 09 50" src="https://github.com/otam-mato/istio/assets/113034133/b79cb294-f06e-40db-b517-ef66a106d43e">

- **Kubernetes configured (update kubeconfig)**

  ```
  aws eks --region us-west-2 update-kubeconfig --name istio-on-eks
  ```

<br>

## Steps

### Setting up Istio and deploying observability tools

1. Enable Istio in a Namespace

   ```bash
   kubectl label namespace default istio-injection=enabled
   ```
   <img width="800" alt="Screenshot 2024-01-15 at 21 28 18" src="https://github.com/otam-mato/istio/assets/113034133/df2bc9a6-d317-4c75-a77f-2fc6c2413d6f">

2. Use the following code snippet to add the Istio Observability Add-ons on the EKS cluster with deployed Istio.

   ```
   for ADDON in kiali jaeger prometheus grafana
   do
       ADDON_URL="https://raw.githubusercontent.com/istio/istio/release-1.20/samples/addons/$ADDON.yaml"
       kubectl apply -f $ADDON_URL
   done
   ```
   <img width="800" alt="Screenshot 2024-01-15 at 21 40 31" src="https://github.com/otam-mato/istio/assets/113034133/ea46157e-0859-4d1f-8c56-e933fd8e7916">

3. Validate the setup of the observability add-ons by running the following commands and accessing each of the service endpoints using this URL of the form http://localhost:<port> where <port> is one of the port number for the corresponding service.

   ```bash
   # Visualize Istio Mesh console using Kiali
   kubectl port-forward svc/kiali 20001:20001 -n istio-system
    
   # Get to the Prometheus UI
   kubectl port-forward svc/prometheus 9090:9090 -n istio-system
    
   # Visualize metrics in using Grafana
   kubectl port-forward svc/grafana 3000:3000 -n istio-system
    
   # Visualize application traces via Jaeger
   kubectl port-forward svc/jaeger 16686:16686 -n istio-system
   ```

### Deploying the app V1 with HELM

1. Deploying the app

2. Deploying the istio components
   
4. Monitoring with **KIALI**

### Deploying the app V2 with HELM

1. Deploying the app

2. Deploying the istio components
   
4. Monitoring with **KIALI**

### Demo of the observability tools

1. Demo of Prometheus
<img width="800" alt="Screenshot 2024-01-15 at 22 35 39" src="https://github.com/otam-mato/istio/assets/113034133/6507b117-a5de-43ca-aca0-9d4332e3412a">
<img width="800" alt="Screenshot 2024-01-15 at 22 36 52" src="https://github.com/otam-mato/istio/assets/113034133/1b92b73f-9427-4d09-990e-0e7ed403e0a2">
<img width="800" alt="Screenshot 2024-01-15 at 22 37 34" src="https://github.com/otam-mato/istio/assets/113034133/ced9c917-851d-4fcf-bda2-d4c4641fbe65">

2. Demo of Grafana
<img width="800" alt="Screenshot 2024-01-15 at 22 40 47" src="https://github.com/otam-mato/istio/assets/113034133/e90e5446-c9dc-40bc-8d9e-8f34c29013a9">
<img width="800" alt="Screenshot 2024-01-15 at 22 42 12" src="https://github.com/otam-mato/istio/assets/113034133/c66a8655-576d-4b74-9cf6-58bb20a49fd2">
<img width="800" alt="Screenshot 2024-01-15 at 22 43 39" src="https://github.com/otam-mato/istio/assets/113034133/f920c10c-57ee-42a4-94f2-aa228a014c82">
<img width="800" alt="Screenshot 2024-01-15 at 22 46 16" src="https://github.com/otam-mato/istio/assets/113034133/e7124474-28a1-4181-bada-756651ddba69">

4. Demo of Jaeger
<img width="800" alt="Screenshot 2024-01-15 at 22 27 24" src="https://github.com/otam-mato/istio/assets/113034133/2540476a-45fb-44f8-a1a7-3e08260cb52a">
<img width="800" alt="Screenshot 2024-01-15 at 22 28 53" src="https://github.com/otam-mato/istio/assets/113034133/45c1a84d-cfaf-4ca5-a7e9-8bca74cb99a0">
<img width="800" alt="Screenshot 2024-01-15 at 22 29 46" src="https://github.com/otam-mato/istio/assets/113034133/981549be-43d3-479b-a63f-069c2bbc069e">
