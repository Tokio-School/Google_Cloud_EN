# **Google Kubernetes Engine**

Unit M2U2 - Exercise 6

**What are we going to do?**

1. Create a Kubernetes cluster with Google Kubernetes Engine.
2. Deploy an application over the GKE cluster.
3. Check how the GKE cluster scales with the load.

**Before getting started**

1. Access the web console: [Google Cloud Console](https://console.cloud.google.com/).
2. Log in with your user account if necessary.
3. Once inside the console, check that you are working on your project. You can check this in the project information card in the control panel on the home page or in the project selection drop-down menu to the left of the blue top bar (next to the 3 blank hexagons).
4. You can divide your desktop into 2 horizontal windows, one with the console and one with the instructions.
5. If during the exercise you need to access Cloud Shell, you can activate it in the ">_" icon on the right in the blue top bar. You can use the terminal in the same window of the console or maximise it and open it in a new window. You can also access the terminal directly at [shell.cloud.google.com](https://shell.cloud.google.com/) and the IDE at [ide.cloud.google.com](https://ide.cloud.google.com/).

_Note: There are no questions for this exercise._

**Task 1: Create a Kubernetes cluster and deploy a web application**

Let&#39;s start by creating a Kubernetes cluster, deploying some applications and checking their auto-scale.

Before you start, enable the Gooogle Kubernetes Engine API:

1. In the console, navigate to  **APIs and Services > Library**.
2. Look for the Google Kubernetes Engine API and enable it or make sure it&#39;s already enabled.

**Create a GKE Cluster**

We will deploy a Kubernetes cluster with GKE using the console:

1. In the console, navigate to  **Kubernetes Engine > Clusters**  and select  **Create**.
2. Select  **GKE Standard**  as the deployment mode.
3. On the cluster creation page, explore all the options to find out what features are available.
4. Create a cluster with the following features:
  - Location type: zonal.
  - Zone: europe-west1-b.
  - Node groups: 1 node group of 3-5 e2-medium nodes with auto-scale.

When we create a cluster from the Cloud SDK, the credentials of the kubectl tool are automatically configured. However, if we do this from the console, we must configure them:

1. In the console, in  **Kubernetes Engine > Clusters** , find the  **Actions**  column for your cluster, and on the 3-point vertical menu, select  **Connect**.
2. It will show you a command that you can run in Cloud Shell by pressing the  **Run in Cloud Shell** button.
3. Once kubectlis is configured, we can check it with kubectl cluster-info or kubectl get all --all-namespaces.

We have thus created a Kubernetes cluster and configured kubectl with the credentials to access it.

_DELIVERABLES:_

1. M2U2-6-task_1-file_1-screenshot_1.jpg: Screenshot of the cluster detail page, showing all its information.
2. M2U2-6-task_1-file_2-screenshot_2.jpg: Screenshot of the response of the kubectl cluster-info command.

**Deploying a web application**

Now let's deploy a stateless web application to the cluster:

1. Create a file called my-deployment-50001.yaml with the following Kubernetes manifest:

apiVersion: apps/v1

kind: Deployment

metadata:

name: my-deployment-50001

spec:

selector:

matchLabels:

app: products

department: sales

replicas: 3

template:

metadata:

labels:

app: products

department: sales

spec:

containers:

- name: hello

image: "us-docker.pkg.dev/google-samples/containers/gke/hello-app:2.0"

env:

- name: "PORT"

value: "50001"

1. Now create the Kubernetes Deployment with the application in the cluster with the command kubectl apply -f my-deployment-50001.yaml.
2. To verify that the application has been deployed correctly, check the Pods and Deployment with the commands kubectl get pods, kubectl get deployments, and kubectl describe deployment my-deployment-50001.

Once the application is deployed, we will create a Load Balancer Service to access the application:

1. Create a file called my-lb-service.yaml with the following Kubernetes manifest:

apiVersion: v1

kind: Service

metadata:

name: my-lb-service

spec:

type: LoadBalancer

selector:

app: products

department: sales

ports:

- protocol: TCP

port: 60000

targetPort: 50001

1. Now create the Kubernetes Service by deploying the object with the command kubectl apply -f my-lb-service.yaml.
2. Wait for the load balancer to be created and its external IP to be available with the kubectl get services --watch command, and when the external IP is available cancel it with CTRL + C.
3. Once the external IP is available, access it from your browser via HTTP to port 60000: http://EXTERNAL_IP_LB:60000.
4. You can verify that a true load balancer has been created in GCP by navigating to  **Network Services >Load Balancing in the console**.

_DELIVERABLES:_

1. M2U2-6-task_1-file_3-screenshot_3.jpg: Screenshot of the browser showing the application and the URL of the load balancer.
2. M2U2-6-task_1-file_4-screenshot_4.jpg: Screenshot of the load balancer detail page, showing all its information.

**Check auto-scaling**

Now let's enable horizontal auto-scaling of Pods and check how they auto-scaled the number of replicas of Pods and Nodes in the cluster.

First let's enable auto-scaling with a HorizontalPodAuto-scaler for Deployment:

1. Enable CPU-dependent deployment auto-scaling: kubectl autoscale deployment my-deployment-50001my-deployment-50001 --min 3 --max -1 --cpu-percent 50.
2. Check the HorizontalPodAutoescaler settings with the kubectl get hpa and kubectl describe hpa HPA_NAME commands (you can find the HPA_NAME in the kubectl get hpa response).
3. Check the current number of Pods and the auto-scaling settings of the Deployment with the commands kubectl get pods, kubectl get deployments, and kubectl describe deployment my-deployment-50001.

Once auto-scaling is enabled, we will overload our application and see how the number of Pods and Nodes increases:

1. Retrieve the external IP from the load balancer: kubectl get services.
2. Overload the application with the command ab -n 500000 -c 1000 http://EXTERNAL_IP:60000 and check the number of Pods and Nodes deployed:
  1. To increase the load you can change the value of the flags -n (total number of requests) and -c (number of concurrent requests) until you see that the number of Pods and Nodes increases.
  2. If you need more capacity, you can run the overload from a Linux VM instance created just for that purpose, with a high number of vCPUs in order to reach its maximum bandwidth.
  3. You can check the number of Pods with kubectl get pods and the number of Nodes with kubectl get nodes.

We have thus been able to enable the auto-scaling of an application and check it with a load test.

_DELIVERABLES:_ 
M2U2-6-task_1-file_5-screenshot_5.jpg: Screenshot of the result of the commands showing the number of Pods and Nodes, different from the minimum number.

**Deploying a stateful application**

Now let's deploy a stateful application that will use Compute Engine persistent disks:

1. Create a file called my-statefulset.yaml with the following Kubernetes manifest:

apiVersion: apps/v1

kind: StatefulSet

metadata:

name: web

spec:

serviceName: "nginx"

replicas: 2

selector:

matchLabels:

app: nginx

template:

metadata:

labels:

app: nginx

spec:

containers:

- name: nginx

image: k8s.gcr.io/nginx-slim:0.8

ports:

- containerPort: 80

name: web

volumeMounts:

- name: www

mountPath: /usr/share/nginx/html

volumeClaimTemplates:

- metadata:

name: www

spec:

accessModes: ["ReadWriteOnce"]

resources:

requests:

storage: 1Gi

1. Now create the StatefulSet with the command kubectl apply -f my-statefulset.yaml.
2. Create a file called my-statefulset-service.yaml with the following Kubernetes manifest:

apiVersion: v1

kind: Service

metadata:

name: nginx

labels:

app: nginx

spec:

ports:

- port: 80

name: web

clusterIP: None

selector:

app: nginx

1. Now create the Kubernetes Service by deploying the object with the command kubectl apply -f my-statefulset-service.yaml.
2. To verify that the stateful application has been deployed correctly, check the Pods, the StatefulSet, and the service with the commands kubectl get pods -l app=nginx, kubectl get statefulset, kubectl describe statefulset web, and kubectl get services.

Thus, we have deployed a stateful application using a Kubernetes StatefulSet.

_DELIVERABLES:_ 
M2U2-6-task_2-file_1-screenshot_1.jpg: Screenshot of the result of the commands showing the Pods of the StatefulSet and the description of the StatefulSet.

**Deliverables Summary**

1. M2U2-6-task_1-file_1-screenshot_1.jpg: Screenshot of the cluster detail page, showing all its information.
2. M2U2-6-task_1-file_2-screenshot_2.jpg: Screenshot of the response of the kubectl cluster-info command.
3. M2U2-6-task_1-file_3-screenshot_3.jpg: Screenshot of the browser showing the application and the URL of the load balancer.
4. M2U2-6-task_1-file_4-screenshot_4.jpg: Screenshot of the load balancer detail page, showing all its information.
5. M2U2-6-task_1-file_5-screenshot_5.jpg: Screenshot of the result of the commands showing the number of Pods and Nodes, different from the minimum number.
6. M2U2-6-task_2-file_1-screenshot_1.jpg: Screenshot of the result of the commands showing the Pods of the StatefulSet and the description of the StatefulSet.

**Clean up resources**

Follow the instructions below carefully to clean up the resources and settings used in your project. This will ensure that you avoid ongoing costs and problems in subsequent fiscal years in particular.

1. In the web console, remove the GKE cluster.
2. (Optional) Delete local manifest files.
