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

## Déployer wordpress-openshift

### Ajouter le dépot nécessaire :
```
# helm repo add wordpress https://raw.githubusercontent.com/phosphoresi/helm/master/wordpress-openshift
# helm repo update
```
### On vérifie qu'il a bien été ajouté :
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
### On créer le projet :
```
# oc new-project wordpress
```
### Donnez les droits pour wordpress
```
# oc login -u system:admin
# oc project wordpress
# oc adm policy add-scc-to-user anyuid -z default
```
### On déploie l'application :
 
```
# helm install --namespace=wordpress --set password=<"cGFzc3dvcmQ="> --set host=<wordpress.example.com> --set storagemariadb=<5Gi> wordpress/wordpress-openshift
```
Si vous déployer un deuxième wordpress dans le même namespace, vous devez spécifier les variables mariadbname et wordpressname.

```
# helm install --namespace=wordpress --set password=<"cGFzc3dvcmQ="> --set host=<wordpress.example.com> --set storagemariadb=<5Gi> --set mariadbname=<mariadb2> --set wordpressname=<wordpress2> wordpress/wordpress-openshift
```
Vous pouvez remplacer toutes les valeurs spécifié dans le fichier values.yaml et utilisant l'option :
```
--set <variable>=<valeur>
```
Example
```
--set storagemariadb=50Gi


# 1)-Installer Catalogue Helm sur Rancher

### Nous allons apprendre a installer et deployer un catalogue Helm.

```
1-) Acceder a l'interface Global du Cluster.
```
```
2-) Ensuite acceder a Catalogs.
```
```
3-) Puis sur "Ajouter un nouveau catalogue".
```
```
4-) Renseigner ensuite le nom de votre catalogue , ainsi que son lien vers Github !
```
```
5-) Enfin , Choissisez la branche sur laquelle deployer Le catalogue , puis cliquer sur créer.
```

# 2)-Deployer Catalogue Helm sur rancher

### Nous allons ensuite apprendre a deployer le Catalogue Helm que l'on viens d'installer

```
1-) Acceder a Un Namespaces que vous avez créer.
```
```
2-) Rendez-vous ensuite sur Catalogue-apps.
```
```
3-) Cliquer ensuite sur Launch.
```
```
4-) Maintenant , Filtrer la recherche pour trouver votre application dans votre catalogue Personnalisé.
```
```
5-) Puis il vous reste seulement a personalisé l'application en fonction de vos besoins.
```
# En plus !
Vous Pouvez egalement ajouter des Answer a l'application , ce sont des sortent de variables d'environnement.
