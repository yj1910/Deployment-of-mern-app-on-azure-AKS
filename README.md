## In this repository, we have done the deployment of three tier mern application on azure aks cluster and done the path based routing for the app by ingress azure load balancer service.

### prerequisite-
1. Access of azure portal
2. knowledage of Docker and docker hub
3. Basics of azure kubernetes architecture

#### step 1 - create a azure cluster(aks) by azure portal below are configuration -
Basics
Subscription: Free Trial
Resource Group: Creat New: aks-rg1
Kubernetes Cluster Name: aksdemo1
Region: (US) Central US
Kubernetes Version: Select what ever is latest stable version\

NODE TYPE-
The master node (system pool), you don’t need to create a separate master node pool. It’s automatically created when you set up the AKS cluster:-
Node Size: Standard DS2 v2 (Default one)
Node Count: 1

For the worker node (user pool):-
Specify the desired configuration:
Node count: Set it to 1 (since you want only one worker node).
Node size (SKU): Choose an appropriate VM size (e.g., Standard_DS2_v2).
all config set on default

Network Policy: Azure
Rest all leave to defaults

Create and review

#### step 2 - connect to the aks cluster-
1.Connect to the azure cli
```bash
az account set --subscription <subscription_ID>   #Set the cluster subscription
az aks get-credentials --resource-group three-tier-mern-app --name mern-app --overwrite-existing

kubectl get nodes           #List Kubernetes Worker Nodes
kubectl get nodes -o wide
```

2.Connect to the local ubuntu command line  (alternative)
```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash  #install azure cli in ubuntu v #install kubectl too
az login --tenant  <tenant_ID>
az account set --subscription a22f1649-e11a-4ece-b9d6-51acc1609b0a
az aks get-credentials --resource-group three-tier-mern-app --name mern-app --overwrite-existing

kubectl get nodes  #List AKS Nodes
kubectl get nodes -o wide
```

#### step 3 - 

1. Clone code from remote repository (GitHub) -
git clone -b devops https://github.com/DevMadhup/wanderlust.git

2. Verify nodes are in ready state or not -
kubectl get nodes

3. Create kubernetes namespace -
kubectl create namespace <namespace_name>

4. Update kubernetes config context -
kubectl config set-context --current --namespace <namespace_name>

5. Enable DNS resolution on kubernetes cluster :
Check coredns pod in kube-system namespace and you will find Both coredns pods are running on master(system) node
```bash kubectl get pods -n kube-system -o wide | grep -i core ```
   
We also need to run coredns pod in worker node on worker node as well for DNS resolution
```bash kubectl edit deploy coredns -n kube-system -o yaml ```
Make 'replicas' count from 2 to 4

6. Navigate to frontend directory :
```bash cd frontend ```
.env.docker file and change the public IP Address with your worker node public IP :
```bash vi .env.docker ```

![image](https://github.com/yj1910/Deployment-of-3-tier-mern-app/assets/83238190/69900c6f-43d8-4044-a184-744b6fc714c7)
