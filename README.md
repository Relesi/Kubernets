## kubernetes - Infrastructure and Cloud Architecture

This is a simple implementation to run two pods on Kubernets

### Create mysql
Creating a pod for mysql
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: bd-mysql
  labels:
    app: bd-mysql
spec:
  containers:
    - name: bd-mysql
      image: mysql:5.6
      env:
        - name: MYSQL_ROOT_PASSWORD
          value: root
      ports:
        - containerPort: 3306
```
*Note:* kubectl apply -f mysql.yaml   
*Note:* cd pods --> kubectl apply -f pods


### Create nginx
Creating a pod for nginx
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-nginx
  labels:
    app: web-nginx
spec:
  containers:
    - name: web-nginx
      image: nginx:stable
      ports:
        - containerPort: 80
```
*Note:* kubectl apply -f nginx.yaml  
*Note:* cd pods --> kubectl apply -f nginx
 
### Create serives
Creating a pod for nginx
```yaml
apiVersion: v1
kind: Service
metadata:
  name: pod-services-nginx-pod
spec:
  selector:
    app: nginx-pod
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: 80
```
*Note:* kubectl apply -f serives.yaml 
*Note:* cd serives --> kubectl apply -f services


For this execution you will need the following tools installed.

## Requeriments

- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Docker](https://www.docker.com/get-started)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)


### Pod creation

```sh
kubectl apply -f pods
```  

The expected result should be:
```
pod/bd-mysql created
pod/web-nginx created

```

### Pod services

```sh
kubectl apply -f pod-serives
```  

The expected result should be:
```
service/pod-services-nginx-pod created

```

### Verify all Service got created

```sh
kubectl get all
```  

The expected result should be:
```
NAME            READY   STATUS             RESTARTS   AGE
pod/app-nginx   1/1     Running            10         64m
pod/db          0/1     ImagePullBackOff   0          64m
pod/mysql       0/1     ErrImagePull       0          14h
pod/nginx       1/1     Running            8          14h

NAME                         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/db-service           ClusterIP      None            <none>        3306/TCP       64m
service/kubernetes           ClusterIP      10.96.0.1       <none>        443/TCP        6d15h
service/lb-service           LoadBalancer   10.101.148.84   <pending>     80:31976/TCP   64m
service/nginx-svc            LoadBalancer   10.103.53.116   <pending>     80:32036/TCP   27m
service/services-nginx-pod   LoadBalancer   10.105.69.43    <pending>     80:31047/TCP   14h

```

### Verify all pods created

```sh
kubectl get pods
```  

The expected result should be:
```
NAME        READY   STATUS             RESTARTS       AGE
app-nginx   1/1     Running            13 (16m ago)   99m
bd-mysql    0/1     ImagePullBackOff   0              14m
db          0/1     ImagePullBackOff   0              99m
mysql       0/1     ImagePullBackOff   0              15h
nginx       1/1     Running            12 (16m ago)   15h
web-nginx   1/1     Running            0              14m

```


