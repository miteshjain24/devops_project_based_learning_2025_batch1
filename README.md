# devops_project_based_learning_2025_batch1

https://aws.amazon.com/
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

# namespace: kubenates namespaces are like rooms in hotel.
# in kubernates, loadbalance are called service: kubectl get svc
# aws configure

https://github.com/eksctl-io/eksctl/releases
There are many way to create kube cluster like eksctl, terraform etcs.
# eksctl create cluster --name devopsproject

https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/


################################ manual way ##################################
kubectl apply -f https://raw.githubusercontent.com/vimallinuxworld13/eks_istio_bookinfo_app/refs/heads/master/bookinfo.yaml

kubectl port-forward   svc/productpage  80:9080

kubectl delete all --all

##########################################################################


####################### GitOps ##########################################
Jenkins and other tool also can apply on kubenates but they required cluster credential which we dont want to give. However, Argocd work inside the kubecluster only as a server, so it is better way.

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml



kubectl config set-context --current --namespace=argocd


https://github.com/argoproj/argo-cd/releases

argocd login --core
or
cd Downloads
./argocd-windows-amd64.exe  login --core



argocd admin initial-password -n argocd
or
./argocd-windows-amd64.exe admin initial-password -n argocd


kubectl port-forward svc/argocd-server -n argocd 8080:443



https://github.com/
https://git-scm.com/downloads

echo "# devops_project_based_learning_2025_batch1" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin https://github.com/vimallinuxworld13/devops_project_based_learning_2025_batch1.git
git push -u origin master


curl https://raw.githubusercontent.com/vimallinuxworld13/eks_istio_bookinfo_app/refs/heads/master/bookinfo.yaml  -o bookinfo.yaml



git add .
git commit -m first .
git push


kubectl get pods -n default

kubectl port-forward  svc/productpage  -n default  80:9080


http://127.0.0.1/productpage

Note: argocd enable the application resourse on kubernates: kubectl get application -n argocd


############################### ISTIO-SERVICE MASH ######################
Candary deployment stragey: when we want weighted deployment meaning we can send 5% to one pod, 45% other and remaining 50% other. this kind of unequal traffic distribution is called Candry deployment but kubernates dont have capability to perform candry deployment which can be resolved by Istio service mash.
Basically kubernates can perform loadbanalcing in Round roubin fasion. It distribute traffic equal to all pods. It can't perform weight distribution.

Another reason to use istio for observalibility like howmuch traffic came, which pod down or slow, metric information ects we can get using istio. For that istio use envoy proxy. basically in pod it creates one more container in front of our actual app container. every in and out traffic passthrough envoy proxy only. envoy is setting service mash.
Envoy work as side car container pattern here.






https://github.com/istio/istio/releases/tag/1.24.2


istioctl  install --set profile=demo


kubectl get pods -n istio-system

kubectl get svc  -n istio-system

kubectl label namespace default  istio-injection=enabled



kubectl delete pods  --all -n default
Note: when we delete all pods, then its kubernates nature to bring back all pods again.

kubectl describe details-v1-57cdc5b69d-z8bp9 -n default


kubectl get pods -n default

Note: if we see-kubectl get svc -n default- all the pods are having clusterId, which are not accessible by external source, if we want it then we need to conver clusterId to Loadbalancer type.
To make it public below are the ways
1. port forwarding
2. clusterid to LB
3. istioctl dashboard kialy(this command also internally used port forwardig by istio)

Note: Istio enable the resouces like vs and gateway in kubernates: kubectl get vs -n default
kubectl get gateway -n default.

Note: in below Loadbalance will connect to gateway and gateway will connect to Productpage service.

kubectl apply -f bookinfo-gateway.yaml

or

curl https://raw.githubusercontent.com/vimallinuxworld13/devops_project_based_learning_2025_batch1/refs/heads/master/bookinfo-gateway.yaml -o bookinfo-gateway.yaml

git add .
git commit -m first .
git push

############################### Applying Kiali, Jaeger, Prometheus, Grafana etcs ###############################
Note: Setting up Kailii, Jaeger, Prometheus on kubernates cluster is tough task, so much configuration is required. So to setup these kind of things helm is useful.
it is having small package called chart. we can download helm ctl and perform the activity.
However, to make it short for now in follwoing project- we are using other approache.already some setup manifests(like grafana,loki,prometheus,kiali etccs) are available so we are just applying these.
 
cd "C:\Users\Vimal Daga\Downloads\istio-1.24.2-win-amd64\istio-1.24.2\samples"

# kubectl.exe  apply -f addons/

istioctl  dashboard kiali

Note: There are many tracing tools available in market like Jaeger, jipkins etcs. After applying Jaeger, Our application traffic or tracing wil be enabled. 
Kiali is visualization tool for traffic and tracing backed by Jager.

Note: 1. Premetheus is time series data base. it stores metric information.
2.Grafana is also visualization tool for metrics information backed by Prometheus.
3.Query which can be help to do perform operation on prometheus is called PromoQL query langaure like sql.
4. On grafana we need to write the query specific to datasource like for Prometheus we need to write or it used PromoQL, for mongo db it use mongo db specific query.
5. Loki is used for logs.
6. basically envoy is forward the traffic to study and prometheus is scrapping data from study.



istioctl dashboard prometheus

# on prometheus UI also, we can write some promoQL queries and look on various matric but better visualization tool is grafana, we can write promoQL query and visualize in better way-
write promo query and  it will provide various facility on top of it.

istioctl dashboard grafana


########################### DevSecOps ##################

In kube cluster so many thing connected each other.
So identification of any vuneralibity or misconfiguration or infected docker image is very difficult.
To resolve these issue we use Trivy. Trivy can scan entiry kubnates cluster. it can be run by jenkins in CI part.
One more software is there i.e. Scout, that is specifically used to analyse image. this we should use at jenkins during build process like in build process build, run unit test, run trivy or scout, run synk etcs and if every thing fine the only upload the image to aws ecr or docker hub.


At very last delete whole cluster by : eksctl delete cluster --name devopsproject

Useful links:
https://github.com/vimallinuxworld13/devops_project_based_learning_2025_batch1
download zip for istio, so it contains both ctl and other folder: https://github.com/istio/istio/releases/tag/1.24.2
https://drive.google.com/drive/folders/1lGQyBwZHTa-dtNroFNm0BbuaRactQTPp

https://discord.com/invite/k37pHhUb

https://bit.ly/LW_discord

Note: For one time kube configuration we should do by kube apply only not by argocd. If any file configuration keep on changing then push these file to github where is argocd will take care.

Note: In kiali, we can set the weight traffic and download the ds.yaml and vs.yaml from kiali and push into github where in argocd will deploy it and we started seeing the traffic as per our yamls.















