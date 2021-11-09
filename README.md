[![N|Solid](.images/BU-LockupH-WatsonCollege-342.png)](https://www.binghamton.edu/computer-science/index.html)

# CS552 - Cloud Computing

## _Miniproject-2_

Microservices and Orchestration via building a simple two-tier live chat microservices and deploy it in Minikube.

### Minikube pod usage

_Docker permission -_

```sh
$ sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
$ sudo chmod g+rwx "$HOME/.docker" -R
````

- Step 1: Start minikube cluster

```sh
$ minikube start
```

- Step 2 : Change docker environment for minikube 

```sh
$ kubectl get po -A
$ alias kubectl="minikube kubectl --"
$ eval $(minikube docker-env)
```

- Step 3 : Download app files

```sh
$ git clone https://github.com/hb0313/livechat-container.git
$ make build --directory ~/miniproject2-container/mongodb
```

- Step 4 : Deploy mongodb minikube image

```sh
$ git clone https://github.com/hb0313/livechat-minikube.git
$ cd livechat-minikube
$ kubectl apply -f mongodb-deploy.yaml
```

- Step 5 : Deploy mongodb service

```sh
$ kubectl apply -f serviceDB.yaml
$ kubectl get service
```

copy the external IP

- Step 6 : Update the livechat-container/webserver/app/config.py and update the host: 'external_ip'

```sh
$ vi ~/miniproject2-container/webserver/app/config.py
```

- Step 7 : Deploy webserver minikube image and service

```sh
$ make build --directory ~/miniproject2-container/webserver
$ kubectl apply -f webserver-deploy.yaml
$ kubectl apply -f serviceWEB.yaml
```

- Step 8 : Forward port to host (VM/GCP)

```sh
$ kubectl port-forward --address 0.0.0.0 service/webserver-service 8080:8080
```
