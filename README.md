# Pour Openshift Origin

- Executer depuis le master les commandes suivantes :

## Installer Helm et Tiller

- Créer un projet tiller
```
# oc new-project tiller
```
- Exporter le namespace de tiller
```
# export TILLER_NAMESPACE=tiller
```
- Télécharger helm client 
```
# curl -s https://storage.googleapis.com/kubernetes-helm/helm-v2.9.0-linux-amd64.tar.gz | tar xz
# cd linux-amd64
# mv helm /usr/bin
# helm init --client-only
...
$HELM_HOME has been configured at /.../.helm.
Not installing Tiller due to 'client-only' flag having been set
Happy Helming!
```
- Installer tiller 
```
# oc process -f https://github.com/openshift/origin/raw/master/examples/helm/tiller-template.yaml -p TILLER_NAMESPACE="${TILLER_NAMESPACE}" -p HELM_VERSION=v2.9.0 | oc create -f -
```
- Donnez les droits à tiller 
```
oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:tiller:tiller
```

## Déployer wordpress-openshift

### Ajouter les dépots nécessaires
```
# helm repo add wordpress https://raw.githubusercontent.com/phosphoresi/helm/master/wordpress-openshift
# helm repo add secret https://raw.githubusercontent.com/phosphoresi/helm/master/secret-wordpress
```
### On vérifie qu'ils ont bien été ajouté :
```
# helm repo list
```

## Déployer l'application

### On peut chercher les applications disponibles :

```
# helm search wordpress/

NAME                         	CHART VERSION	APP VERSION	DESCRIPTION                
wordpress/wordpress-openshift	0.1.0        	1.0        	A Helm chart for Kubernetes
```
### On déploie l'application :

```
# helm install --set password="cGFzc3dvcmQ=" --namespace=wordpress secret/secret-wordpress
```
