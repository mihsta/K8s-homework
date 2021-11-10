# See README in task folder

#install minikube + dashboard + metrics-server
```bash
minikube start --driver=virtualbox
minikube addons enable metrics-server
minikube dashboard
```
#minikube commands
```bash
minikube kubectl
minikube ssh -n m02
minikube addons list
minikube stop
minikube delete
minikube node add
```

#kubectl commands
```bash
kubectl proxy
kubectl get ns    <- списсок неймспейсов
kubectl create ns "name"    <- создание неймспейса
kubectl apply -f pod/pod.yaml     <- пытается изменить 
kubectl create -f pod/pod.yaml     <- пытается создать если есть уже, то выдаст ошибку
kubectl replace -f pod/pod.yaml --force    <- пересоздать контейнер
kubectl delete pods sidecar-container-demo
kubectl get pods - список подов
kubectl get deploy - список деплоев
kubectl get rs - список репликасетов    
kubectl exec -ti sidecar-container-demo -c main-container -- /bin/bash     <- подключение к контейнеру пода
kubectl port-forward sidecar-container-demo 8080:80    <- проброс порта до контейнера
kubectl get events
kubectl logs -f init-container-8ffb64c68-rr6gg -c init-container    <- логи пода инит-контейнера
kubectl run web --image=nginx:latest --dry-run=client -o yaml
kubectl explain pods.spec
kubectl rollout status deployment nginx
kubectl describe deployments.apps nginx
kubectl get pods -o wide --show-labels=true
kubectl label pods nginx-f77774fc5-cl5c5 app-    <- удаление лейбла app
kubectl get svc
kubectl autoscale deployment nginx --min=2 --max=5 --cpu-percent=80
kubectl api-versions
kubectl api-resources
kubectl replace --force -f .\source\K8s-homework\task_1\nginx-service.yaml
kubectl rollout history deployment nginx --revision=1
kubectl rollout undo deployment nginx --to-revision=1
kubectl set image deploy nginx nginx=nginx:1.13 --record=true
kubectl explain pods.spec.hostname
kubectl create configmap nginx-config --from-file=/path/to/dir
kubectl create configmap nginx-config --from-file=<pathtofile>
kubectl create configmap nginx-config --from-literal=special.how=very
kubectl get cm
kubectl rollout pause deploy nginx
kubectl rollout resume deploy nginx
kubectl get po
kubectl create secret generic db-user-pass --from-file=username=secretes/username.txt
kubectl get secret db-user-pass -o yaml
kubectl get secrets db-user-pass -o jsonpath='{.data.username}'|base64 -d
kubectl get cronjobs
kubectl get jobs
kubectl get clusterrole cluster-admin -o yaml
kubectl get sa
k get sa vasya -o yaml

#helm
helm create temp
helm search hub postgres
helm repo add nicholaswilde https://nicholaswilde.github.io/help-charts/
helm repo update
helm install postgres nicholaswilde/postgres
helm list
helm repo list
helm search repo postgres  <-поиск локальных репозиториев
helm pull nocholaswilde/postgres  
tar zxvf postgres-0.1.0.tgz
helm upgrade postgres .
kubectl logs postgres-66856cb96-48dxz
kubectl exec -ti postgres-66856cb96-48dxz -- bash
psql -h localhost postgres admin
helm delete postgres
helm ls

#Busybox-curl
kubectl -n default create deploy busybox --image=yauritux/busybox-curl -- /bin/sh -c "while true; do sleep 30; done;"
#Bash
kubectl -n default exec -it $(kubectl get pods -n default | grep busybox | awk '{print $1}') -- sh
#Powershell
kubectl -n default exec -it $(kubectl get po -n default | Select-String "busybox" | foreach { -split $_.line | select -index 0}) -- sh
```


#watch equivalent for powershell
while (1) {sleep 1; $(k get po; cls;)}

#alias for minikube kubectl in Powershell $PROFILE
```bash
echo 'Function minikubectl {minikube.exe kubectl -- $args}' >> $PROFILE
echo 'Set-Alias k minikubectl' >> $PROFILE
```

#alias for busybox-curl in Powershell $PROFILE
```bash
echo 'Function busyboxcurl {kubectl -n default exec -it $(kubectl get po -n default | Select-String "busybox" | foreach { -split $_.line | select -index 0}) -- sh}' >> $PROFILE
echo 'Set-Alias bbc busyboxcurl' >> $PROFILE
```

#install dashboard using kubectl 
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
kubectl get pod -n kubernetes-dashboard

kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl edit -n kube-system deployment metrics-server
add spec:      containers:      - args:        - --kubelet-insecure-tls
```
##login web UI
```bash
$TOKEN=[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("$(kubectl get secrets -n kube-system $(kubectl get sa -n kube-system default -o jsonpath='{.secrets[0].name}') -o jsonpath='{.data.token}')"))
echo $TOKEN
echo http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
kubectl proxy
```
## Inspiration
```bash
https://azure.microsoft.com/mediahandler/files/resourcefiles/kubernetes-learning-path/Kubernetes%20Learning%20Path_Version%202.0.pdf
https://devblogs.microsoft.com/powershell/native-commands-in-powershell-a-new-approach-part-2/
https://devblogs.microsoft.com/scripting/playing-with-json-and-powershell/
https://stackoverflow.com/questions/58711472/awk-from-bash-to-powershell
https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/
https://stackoverflow.com/questions/42564058/how-to-use-local-docker-images-with-minikube
https://kubernetes.io/docs/concepts/containers/images/#updating-images
https://newbedev.com/is-it-possible-to-rerun-kubernetes-job
https://kubernetes.io/blog/2021/04/12/introducing-suspended-jobs/
https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
https://docs.projectcalico.org/getting-started/kubernetes/hardway/overview
https://istio.io/latest/docs/setup/getting-started/
https://argo-cd.readthedocs.io/en/stable/
https://kustomize.io/
https://aws.amazon.com/ru/route53/
https://operatorhub.io/
https://ru.werf.io/
```


 