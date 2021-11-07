# See README in task folder
#install minikube + dashboard + metrics-server
minikube start --driver=virtualbox
minikube addons enable metrics-server
minikube dashboard

#minikube commands
minikube kubectl
minikube ssh -n m02
minikube addons list
minikube stop
minikube delete
minikube node add


#kubectl commands
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
label pods nginx-f77774fc5-cl5c5 app-    <- удаление лейбла app

#alias for minikube kubectl in Powershell $PROFILE
echo 'Function minikubectl {minikube.exe kubectl -- $args}' >> $PROFILE
echo 'Set-Alias k minikubectl' >> $PROFILE


#install dashboard using kubectl 
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
kubectl get pod -n kubernetes-dashboard

kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl edit -n kube-system deployment metrics-server
add spec:      containers:      - args:        - --kubelet-insecure-tls

##login web UI
$TOKEN=[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("$(kubectl get secrets -n kube-system $(kubectl get sa -n kube-system default -o jsonpath='{.secrets[0].name}') -o jsonpath='{.data.token}')"))
echo $TOKEN
echo http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
kubectl proxy

## Inspiration
