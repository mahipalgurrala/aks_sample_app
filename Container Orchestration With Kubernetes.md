# **WORKING WITH CONTAINER ORCHESTRATION USING KUBERNETES**

---

## **Pre-requisite Docker and Kubernetes**

---
To check the status of the Kubernetes cluster use -

```bash
kubectl cluster-info
```

12. To list all the available nodes that is part of this cluster use -

```bash
kubectl get nodes
```

13. If you do not have an account at `hub.docker.com`, then sign up for a new one or use the existing one. You also have to validate yours email id that would activate the docker hub account.

<a href="https://hub.docker.com/signup" target="_blank">Sign Up Docker Hub!</a>

---

### **Activity 1 - Creating a docker image**

---

1. To install git in master server use -

```bash
sudo yum install -y git
```

2. Once git is installed, then clone the repository.

```bash
git clone https://gitlab.com/jaswanth1824/container-orchestration-with-kubernetes.git
```

3. Move to the **`lab-files/python-flask-app`** directory inside the **`container-orchestration-with-kubernetes`** directory. 

```bash
cd ~/container-orchestration-with-kubernetes/lab-files/python-flask-app
```

4. Refer the files in **`python-flask-app`** directory.

```bash
ls -al
```

5. To open the **`Dockerfile`** and modify its configuration use -

```bash
vim Dockerfile
```

6. To build a new docker image use -

```bash
# Don't forget to use the . at the end
docker image build -t python-flask-app:1.0 .
```

7. To see the docker image created just now use -

```cmd
docker image ls
```

8. To run a new container from the docker image created use -

```cmd
docker container run --detach --rm --name py-web-app-1 --publish 8888:80 python-flask-app:1.0
```

9. Open a new browser tab window and use the URL given below to access the application.

```txt
http://<HOST-IP-ADDRESS>:8888
```

10. To stop the running container use -

```cmd
docker container stop py-web-app-1
```

11.  To log in to your account at the docker hub registry use - 

```cmd
docker login
```

**NOTE**: Use your docker hub id and password.

12.  To push the image first `tag` it with repository name (i.e. docker hub id).

```cmd
docker image tag python-flask-app:1.0 <docker-hub-id>/python-flask-app:1.0
docker image tag python-flask-app:1.0 <docker-hub-id>/python-flask-app:latest
```

**NOTE**: The image that is pushed or pulled has the format **`domain/repository/image:tag`**. If not specified, the domain defaults to **`docker.io`**, the repository defaults to **`library`**, and the tag defaults to **`latest`**. 

13. To view all the repository images and its tags available locally use -

```cmd
docker image ls | grep -i python
```

14.  To push the image to the docker hub registry use -

```cmd
docker push <docker-hub-id>/python-flask-app:1.0
docker push <docker-hub-id>/python-flask-app:latest
```

15. Once the image is pushed, you can view the image in your docker hub registered account.

16. To remove the python-flask-app images from the host machine use -

```cmd
docker image rm python-flask-app:1.0 <docker-hub-id>/python-flask-app:latest <docker-hub-id>/python-flask-app:1.0
```

17.  To use the pushed image from docker hub registry use -

```cmd
docker container run --detach --rm --name py-web-app-1 --publish 8888:80 <docker-hub-id>/python-flask-app
```

18. To view the application open a new browser tab window, and use -

```txt
http://<HOST-IP-ADDRESS>:8888
```

19. To stop the running container use -

```cmd
docker container stop py-web-app-1
```

20. To move back to the k8s/lab-files directory use -

```bash
cd ~/container-orchestration-with-kubernetes/lab-files
```

---

### **Activity 2 - Deploying an app to the Kubernetes Cluster using the kubectl cli, and exploring its different basic commands**

---

1. To deploy an application using `kubectl` in the Kubernetes cluster use -

```bash
kubectl create deployment k8s-bootcamp-deploy --image=gcr.io/google-samples/kubernetes-bootcamp:v1
```

**NOTE**: The name of deployment is **`k8s-bootcamp-deploy`**, which uses the image specified through the **`--image`** option (include full URL for images hosted outside docker hub).


2. To list all the deployments (in default namespace) use -

```bash
kubectl get deployments
# OR
kubectl get deploy
```

3. To list all the replicasets (in default namespace) use -

```bash
kubectl get replicasets
OR
kubectl get rs
```

4. To list all the pods (in default namespace) use -

```bash
kubectl get pods
OR
kubectl get po
```

5. The option `-o` with value `wide` provides additional information about the pods.

```bash
kubectl get pods -o wide
OR
kubectl get po -o wide
```

6. To get full details of a `deployment` use -

```bash
kubectl describe deployment k8s-bootcamp-deploy
# OR
kubectl describe deploy k8s-bootcamp-deploy
```

7.  To get full details of a `replicaset` use -

```bash
kubectl describe replicaset <repliacset-name>
# OR
kubectl describe rs <replicaset-name>
```

8. To get full details of a `pod` use -

```bash
kubectl describe pods <pod-name>
# OR
kubectl describe po <pod-name>
```

9. The command **`kubectl proxy`** creates a proxy server between the localhost and the Kubernetes API Server. When the proxy server is running, it does not show any output, and its termination is possible by pressing the key combination of **`Ctrl + C`**.

```bash
kubectl proxy
```

[Reference Link - kubectl proxy](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#proxy)

10.  When the proxy server is running, you can explore the APIs using curl, wget, or a browser. To get the details use -

```bash
curl http://localhost:8001/
curl http://localhost:8001/version
curl http://localhost:8001/api/v1
curl http://localhost:8001/apis/apps/v1
```

11.  To delete the deployment created earlier execute the below command -

```bash
kubectl delete deployment k8s-bootcamp-deploy
# OR
kubectl delete deploy k8s-bootcamp-deploy
```

**NOTE:** Deletion of deployment also deletes the associated replicaset and pods.

---

### **Activity 3 - Launch a pod in the Kubernetes Cluster using YAML file and troubleshooting it**

---

1. To create a pod from **`k8s-bootcamp-pod.yml`** file use -

```bash
kubectl apply -f k8s-bootcamp-pod.yml
```

2. To print the logs for a container in a pod or specified resource, use the `logs` command. If the pod has only one container, the container name is optional else specify its name using the `-c` option.

```bash
kubectl logs k8s-bootcamp-pod
# OR
kubectl logs k8s-bootcamp-pod -c k8s-bootcamp-container
```

3. To execute a command directly inside the container use -

```bash
# kubectl exec -it <POD-NAME> -c <CONTAINER-NAME> -- <COMMAND>
kubectl exec -it k8s-bootcamp-pod -c k8s-bootcamp-container -- bash
```

4. Type **`exit`** to come out of the container.

5. To quickly access an application use the **`port-forward`** command.  

```bash
kubectl port-forward --address <YOUR_VM_IP_ADDRESS> pod/k8s-bootcamp-pod 8000:8080
```

**NOTE**: This command requires the node to have **`socat`** installed. However, there is a much better way to access the application using **`services`**, which will be explored later.

6. To delete the resources created with the file use -

```bash
kubectl delete -f k8s-bootcamp-pod.yml
```

---

### **Activity 4 - Deploying app and exposing it using services**

---

1. Refer the **`k8s-bootcamp-deployment.yml`** file in the **`lab-files`** directory.

2. To create a deployment use -

```bash
kubectl apply -f k8s-bootcamp-deployment.yml 
```

3. Refer the `k8s-bootcamp-service.yml` file in the `lab-files` directory.

4. To create the service from YAML file use -

```bash
kubectl apply -f k8s-bootcamp-service.yml
```

5. To view list of available service in default namespace use -

```bash
kubectl get services 
# OR
kubectl get svc
```

**NOTE**: The NodePort service assigns a port number between the range of 30000-32767. The docker containers port number range is from 32768 - 65535. 

6. Try the below different options to access the App UI

```bash
curl http://<HOST_IP>:<NodePort>
```
---

7. To delete the deployment and service created with file use -

```bash
kubectl delete -f k8s-bootcamp-deployment.yml -f k8s-bootcamp-service.yml
```

Additionally,

8. To expose the deployment using a service, we can also use Kubectl expose command as shown below.

  For this, first create deployment using following command and expose the deployment using kubectl expose command

 ```bash
kubectl apply -f k8s-bootcamp-deployment.yml
```

```bash
kubectl expose deployment/k8s-bootcamp-deploy --name=k8s-bootcamp-svc --type=NodePort --port=8080
```

**NOTE**: Using expose command with type as **`NodePort`**, a node port is always assigned randomly.

[Reference Link - kubectl expose](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#expose)

9. Each pod in the Kubernetes cluster can access other pods using their exposed service name or the pod DNS name.

Try the below command -

```bash
kubectl run busybox-pod --image=busybox:1.28.1 -- sleep 3600
kubectl exec busybox-pod -it -- nslookup k8s-bootcamp-svc
kubectl exec busybox-pod -it -- nslookup k8s-bootcamp-svc.default.svc.cluster.local
# NOTE THE IP ADDRESS IN WHICH . IS REPLACE WITH -
# XXX.XXX.XXX.XXX ---> XXX-XXX-XXX-XXX
kubectl exec busybox-pod -it -- nslookup <XXX-XXX-XXX-XXX>.default.pod.cluster.local
```

**NOTE**: The DNS name is added automatically to the pods, which is taken care by the kube DNS component. nslookup is the utility that helps to resolve the DNS name.

---

### **Activity 5 - Working with Labels**

---

1. To deploy the `k8s-bootcamp-pod` use -

```bash
kubectl apply -f k8s-bootcamp-pod.yml
```

2. To display the resources with their respective labels, use the **`--show-labels`** option -

```bash
kubectl get all --show-labels
kubectl get pods --show-labels
```

3.  To apply a new label to various resource types, use the **`label`** command followed by resource type, object name, and the new label to apply.

```bash
kubectl label <RESOURCE-TYPE> <RESOURCE-NAME> KEY_1=VAL_1 ... KEY_N=VAL_N
```

```bash
kubectl label pods <pod-name> role=frontend type=webapp
# Check the labels on the pod.
kubectl get pods --show-labels
```

4.  To remove the label previously added use -

```bash
# append the hypen (or minus) symbol to the key
kubectl label <RESOURCE-TYPE> <RESOURCE-NAME> key-
```

```bash
kubectl label pods <pod-name> role- type-
# Check the labels on the pod.
kubectl get pods --show-labels
```

---

### **Activity 6 - Scaling a Deployed App**

---

1. To deploy the app use -

```bash
kubectl apply -f k8s-bootcamp-deployment.yml --record
```

2. If there is an existing deployment, you can use the **`edit`** command to scale the app by modifying the number of replicas.

```bash
kubectl edit deployment/k8s-bootcamp-deploy
```

3. The **`scale`** command with the **`--replicas`** option modifies the existing replicas of the deployment.

```bash
# scale to ten replicas
kubectl scale deployment/k8s-bootcamp-deploy --replicas=10
```

4. Deploy the service by using the **`k8s-bootcamp-service.yml`** file.

```bash
kubectl apply -f k8s-bootcamp-service.yml
```

5. Use the curl utility to access the application deployed.

```bash
curl http://<YOUR_VM_IP_ADDRESS>:<NodePort>
```

**NOTE**: The service internal load balancer balances the traffic across the replicas.

---

### **Activity 7 - Updating a Deployed App**

---

1. To initiate a rolling update by changing the old image of the container with a new one use -

```bash
kubectl set image deployment/k8s-bootcamp-deploy k8s-bootcamp-container=jocatalin/kubernetes-bootcamp:v2 --record
```

2. The app UI with the new image is accessible using service.

```bash
curl http://<YOUR_VM_IP_ADDRESS>:<NodePort>
```

3. To check the `rollout status` of deployment use -

```bash
kubectl rollout status deployment/k8s-bootcamp-deploy
```

4. To check the `rollout history` of deployment, use -

```bash
kubectl rollout history deployment/k8s-bootcamp-deploy
```

5. To rollout a new update of the app, use -

```bash
kubectl set image deployment/k8s-bootcamp-deploy k8s-bootcamp-container=jocatalin/kubernetes-bootcamp:v10 --record
```

**NOTE**: *Image tagged with v10 is not available*.

6. To check the `rollout history` again execute the below command -

```bash
kubectl rollout history deployment/k8s-bootcamp-deploy
```

7. To revert back the deployment to previous known state use -

```bash
kubectl rollout undo deployment/k8s-bootcamp-deploy
```

8. Verify the deployment reverted back to the last state by executing the command -

```bash
kubectl get deploy, pod
```

9. To revert back to a specific revision use the option **`--to-revision=<revision-number>`**, where revision number can be found through **`rollout history`** command.

```bash
kubectl rollout history deploy/k8s-bootcamp-deploy
kubectl rollout undo deployment/k8s-bootcamp-deploy --to-revision=1
```

10. To delete the resources created use -

```bash
kubectl delete -f k8s-bootcamp-deployment.yml -f k8s-bootcamp-service.yml
```

---

### **DO IT YOURSELF**

---

* Create a single YAML file to write both the deployment and service configuration. 
* The service must expose the deployment on the host machine. 
* Use the **`python-flask-app`** image created earlier in the pod spec.
* Keep everything inside **`my-app`** directory

---

### **Activity 8 - Deploying webserver in Kubernetes cluster using namespaces**

---

1. To create a new namespace you can use -

```bash
kubectl create namespace development
# OR
kubectl create ns development
```

2. To create and run a pod inside specific namespace, use the **`--namespace`** or **`-n`** option.

```bash
kubectl run nginx-pod --image=nginx:alpine --namespace=development
```

3. To list available pods in a specific namespace use -

```bash
kubectl get pods --namespace development
# OR
kubectl get pods -n development
```

4. To delete the namespace use -

```bash
kubectl delete namespace development
# OR
kubectl delete ns development
```

**NOTE**: Deleting a user-defined namespace will clear the resources defined in that namespace.  

5. Move to the `my-webserver` directory in the `lab-files` directory, and refer the file `my-webserver-pod.yml`.

```bash
cd ~/container-orchestration-with-kubernetes/lab-files/my-webserver
vim my-webserver-pod.yml
```

6. To create the pod use -

```bash
kubectl apply -f my-webserver-pod.yml
```

7. To view the pod in the `development` namespace use -

```bash
kubectl get pods -n development
# to view pods from all namespaces use the command
kubectl get pods --all-namespaces
```

---

### **Activity 9 - Understanding the difference between ReplicationController and ReplicaSet**

---

1. Refer the file **`my-webserver-rc.yml`**. The `ReplicationController`, which is part of the core API **`v1`**, does not mandatorily require **`selector`**.  

2. To add replication controller object use -

```bash
kubectl apply -f my-webserver-rc.yml
```

3. The desired state is three web server pods, as mentioned in the **`replicas`**. To check the resources created in the development namespace use -

```bash
kubectl get all -n development
```

**NOTE**: Two webserver pods were added, as there was already an existing pod with label `app=webserver`.

4.  The ReplicationController is responsible for managing the desired state of the pods. So, it always ensures that the desired state is maintained. Try deleting any of the pod and observe.

```bash
kubectl delete pod <pod-name> -n development
kubectl get pods -n development
```

5.  To scale the number of replicas use -

```bash
kubectl scale rc nginx-rc --replicas=5 -n development
kubectl get pods -n development
```

6. The ReplicaSet, which is part of the latest **`apps/v1`** API, makes use of the **`selector`** property mandatory. It gives more flexibility in terms of selecting the pods based on matching expressions. It is used by the Deployment object internally to maintain the desired state.   

```yml
  selector:
    matchExpressions:
    - {key: app, operator: In, values: [nginx, webserver]}
    - {key: environment, operator: NotIn, values: [production]}
```

7. Refer the file **`my-webserver-rs.yml`**, and to create the resource use -

```bash
kubectl apply -f my-webserver-rs.yml
```

8. To create the service and expose the `replicaset` resource use -

```bash
kubectl apply -f my-webserver-svc.yml
```

9.  To view the service endpoints use -

```bash
# to display the IP address of pods
kubectl get pods -n development -o wide
# to view the service
kubectl get svc -n development
# to check the service endpoints
kubectl get ep -n development
```

10. On a new browser window tab, use the host IP address with nodePort as a URL address to access the nginx web server application.

```txt
http://<HOST-IP-ADDRESS>:<NODE-PORT>
```

11. To clean up all the resources created in the `development` namespace use -

```bash
kubectl delete all --all -n development
kubectl get all -n development
```

---

### **Activity 10 - Deploying MySQL in Kubernetes Cluster with Secret and Persistent Volumes**

---

1. Refer the link, to access the MySQL docker image description on the Docker public registry. As described, it's mandatory to provide the **`MYSQL_ROOT_PASSWORD`** environment variable when creating a MySQL container.

[Reference Link - MySQL Image](https://hub.docker.com/_/mysql)

2. To pass value to the **`MYSQL_ROOT_PASSWORD`** environment variable, we will use a secret object. There are two ways to achieve this:

```bash
# first convert the text into base64 encoding using the command
echo -n "YOUR-PASSWORD" | base64; echo
```

i. The imperative way -

```bash
kubectl create secret generic mysql-secret --from-literal=password=<YOUR-BASE64-ENCODED-VALUE> -n development
```

ii. The declarative way -

Move to the **`my-db`** directory in the **`lab-files`** directory, and refer the file **`mysql-secret.yml`**.

```bash
cd ~/container-orchestration-with-kubernetes/lab-files/my-db
vim mysql-secret.yml
```

**NOTE**: The Secrets store data in the **`base64`** format, whereas the ConfigMaps store data in a plain text. For sensitive data like keys, passwords, database connection string etc. always use the `Secrets`.

```yml
apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
  namespace: development
  labels:
    app: mysql
data:
  password: <YOUR-BASE64-ENCODED-VALUE> # REPLACE HERE
```

3. To check the secret object created use -

```bash
# gets all the secret object in development namespace
kubectl get secret -n development
# describe a specific secret object, in this case mysql-secret
kubectl describe secret mysql-secret -n development
```

4. A PersistentVolume (PV) is a piece of storage in the cluster that has been provisioned by an administrator. It is a cluster resource. PVs are volume plugins like Volumes, but have a lifecycle independent of any individual pod that uses the PV. To create a new `PersistentVolume` with the name of `mysql-pv` of type `hostPath` apply the configuration given in `mysql-pv.yml` file -

```bash
kubectl apply -f mysql-pv.yml
```

5. To check the persistent volume use -

```bash
kubectl get persistentvolumes
# OR
kubectl get pv
```

[Reference Link - Volumes](https://kubernetes.io/docs/concepts/storage/volumes/)

[Reference Link - Plugins](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#types-of-persistent-volumes)

[Reference Link - Access Modes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes)

[Reference Link - hostPath](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath)

6. A PersistentVolumeClaim (PVC) is a request for storage by a user. It is similar to a Pod. Pods consume node resources and PVCs consume PV resources. To create a new `PersistentVolumeClaim` with the name of `mysql-pvc` apply the configuration given in `mysql-pvc.yml` file -

```bash
kubectl apply -f mysql-pvc.yml
```

7. To check the persistent volume claim created use -

```bash
kubectl get persistentvolumeclaims -n development
# OR
kubectl get pvc -n development
```

8. To launch a new mysql pod refer file **`mysql-pod.yml`**. To apply the configuration use -

```bash
kubectl apply -f mysql-pod.yml
```

9.  To apply a watch, use the -w option that will display the state change based on different events.

```bash
kubectl get pods -n development -w
```

**NOTE**: Press `Ctrl + C` / `Control + C` to come out of the watch.  

10.  To exec into the mysql-pod use -

```bash
kubectl exec -it mysql-pod -n development -- bash
```

11. Now, to access the mysql server use -

```bash
mysql -u root -p
```

12. Enter the password that was encoded in the base64 format.

13. To add some data to mysql database server use the queries given below -

```sql
CREATE DATABASE accenture;
USE accenture;
CREATE TABLE employees (id INT AUTO_INCREMENT PRIMARY KEY, first_name VARCHAR(50) NOT NULL, last_name VARCHAR(50) NOT NULL);
INSERT INTO employees (first_name, last_name) VALUES ('Khozema', 'Nullwala');
INSERT INTO employees (first_name, last_name) VALUES ('Soma', 'Tiwari');
INSERT INTO employees (first_name, last_name) VALUES ('Daniel', 'Joshua');
SELECT * FROM employees;
```

14.  To exit from mysql database server type `exit` command, and to come out of the container bash shell type `exit` again.

15. To delete the `mysql-pod` use -

```bash
kubectl delete -f mysql-pod.yml
```

**NOTE**: The above command deletes only the mysql-pod, but other resources is persisted that can be attach again to a new mysql database server pod.

16. To create the pod once again use -

```bash
kubectl apply -f mysql-pod.yml && kubectl get pods -n development -w
```

17. Use the given below set of commands to fetch the data through a SQL query -

```bash
kubectl exec -it mysql-pod bash -n development

mysql -u root -p
SHOW databases;
USE accenture;
SELECT * FROM employees;
exit
exit
```

18. To delete mysql-pod created from the file use -

```bash
kubectl delete -f mysql-pod.yml
```

19. To delete the secrets, claim and volume created from the file use -

```bash
kubectl delete -f mysql-secret.yml
kubectl delete -f mysql-pvc.yml
kubectl delete -f mysql-pv.yml
```

---

### **Activity 11 - Deploying multi-tier app on to Kubernetes cluster**

---

Before you continue with this activity, please refer the [Docker Voting App Sample](https://github.com/dockersamples/example-voting-app).

The different components of the Voting App is given below -

* **Vote** — front end that enables a user to choose between a cat and a dog.
* **Redis** — database where votes are stored.
* **Worker** — service that get votes from Redis and store the results in a Postgres database.
* **Db** — the Postgres database in which vote results are stored.
* **Result** — front end, displaying the results of the vote.

1. To move at the `my-multi-tier-app` directory use -

```bash
cd ~/k8s/lab-files/my-multi-tier-app
```

2. Refer the content of `redis-deploy.yml`, `postgres-deploy.yml`, `result-deploy.yml`, `vote-deploy.yml`, and `worker-deploy.yml` files for this activity.

3. To deploy different resources use -

```bash
kubectl apply -f redis-deploy.yml
kubectl apply -f postgres-deploy.yml
kubectl apply -f vote-deploy.yml
kubectl apply -f result-deploy.yml
kubectl apply -f worker-deploy.yml
```

4. To list all the services in the development namespace use -

```bash
kubectl get svc -n development
```

**NOTE**: The NodePort service for voting and result expose the node ports, take a note of it to view the both the front-end application.

5. On a new browser tab window access the voting front-end app, and then cast your vote.

6. Open another browser tab window access the result front-end app to check the voting result.

7. To delete all the resources created in the `development` namespace use -

```bash
kubectl delete all --all -n development
kubectl delete pv postgres-pv
```

---
