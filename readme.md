#Install Helm
sudo curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod +x get_helm.sh
sudo ./get_helm.sh
helm version

#Install AWX Operator
helm repo add awx-operator https://ansible.github.io/awx-operator/
"awx-operator" has been added to your repositories
helm repo update
helm install ansible-awx-operator awx-operator/awx-operator -n awx --create-namespace

#Lits pods 
sudo kubectl get pods -n awx

#Install LOCAL STORAGE, PV, PVC
sudo kubectl create -f local-storage-class.yaml
sudo kubectl get sc -n awx
sudo kubectl create -f pv.yaml
sudo kubectl create -f pvc.yaml
sudo kubectl get pv,pvc -n awx

#Deploy Ansible AWX
sudo kubectl create -f ansible-awx.yaml


#watch until all pods are cooked
sudo watch kubectl get pods -n awx

#final step
kubectl expose deployment ansible-awx-web --name ansible-awx-web-svc --type LoadBalancer -n awx

sudo kubectl get svc ansible-awx-web-svc  -n awx