# Pour Openshift Origin

- Executer depuis le master les commandes suivantes :

## Installation Helm et Tiller

- On se connecte
```
# oc login -u <user>
```

- Créer un projet tiller
```
# oc new-project tiller
```
- Exportez le namespace de tiller
```
# export TILLER_NAMESPACE=tiller
```
- Téléchargez helm client 
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
- Installez tiller 
```
# oc process -f https://github.com/openshift/origin/raw/master/examples/helm/tiller-template.yaml -p TILLER_NAMESPACE="${TILLER_NAMESPACE}" -p HELM_VERSION=v2.9.0 | oc create -f -
```
- Donnez les droits à tiller 
```
oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:tiller:tiller
```

## Déployer odoo-openshift

### Ajouter le dépot nécessaire :
```
# helm repo add registry https://gitlab.sudokeys.com/tsaunier/registry-openshift/raw/master/?private_token=EZ6t6ZwszsBBtmkU2tU_
# helm repo add odoo https://gitlab.sudokeys.com/tsaunier/odoo-openshift/raw/master/?private_token=EZ6t6ZwszsBBtmkU2tU_
# helm repo update
```
### On vérifie qu'il a bien été ajouté :
```
# helm repo list
```

## Déployer l'application

### On peut chercher les applications disponibles :

```
# helm search odoo/

NAME                         	CHART VERSION	APP VERSION	DESCRIPTION                
odoo/odoo-openshift	0.1.0        	1.0        	A Helm chart for Kubernetes
```
### On créer le projet :
```
# oc new-project odoo
```
### On déploie l'application :

On déploie d'abord le registry

```
# helm install registry/registry-openshift
```
Puis l'application 
```
# helm install --namespace=odoo --set passworduser=<"YWxZdEVSdEhBTWVnSUFsRHJhc0g="> --set passwordroot=<"ZU5hQ3RVTUJsZXlFclBBcnlkZVINCg=="> --set host=<odoo.example.com> --set storagepostgres=<5Gi> --set storageodoo=<20Gi> odoo/odoo-openshift
```
Si vous déployer un deuxième wordpress dans le même namespace, vous devez spécifier les variables nginxname, odooname et postgresname.

```
# helm install --namespace=odoo --set passworduser=<"YWxZdEVSdEhBTWVnSUFsRHJhc0g="> --set passwordroot=<"ZU5hQ3RVTUJsZXlFclBBcnlkZVINCg=="> --set host=<odoo.example.com> --set storagepostgres=<5Gi> --set storageodoo=<20Gi> --set nginxname=<nginx2> --set odooname=<odoo2> --set postgresname=<postgres2> wordpress/wordpress-openshift
```
Vous pouvez remplacer toutes les valeurs spécifié dans le fichier values.yaml et utilisant l'option :
```
--set <variable>=<valeur>
```
Example
```
--set storagepostgres=50Gi
```

