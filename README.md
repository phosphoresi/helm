# Pour Openshift Origin

## Prérequis
Il faut avoir helm cli installé et bien configuré sur le master. Vous pouvez le voir en faisant :
```
helm version
Client: &version.Version{SemVer:"v2.9.0", GitCommit:"f6025bb9ee7daf9fee0026541c90a6f557a3e0bc", GitTreeState:"clean"}
Server: &version.Version{SemVer:"v2.9.0", GitCommit:"f6025bb9ee7daf9fee0026541c90a6f557a3e0bc", GitTreeState:"clean"}
``` 

## Ajouter un dépot
- Executer depuis le master les commandes suivantes :
### On l'ajoute
```
# helm repo add <nom_du_depot> https://raw.githubusercontent.com/phosphoresi/helm/master/<nom_du_dossier>
```
- Exemple : 
```
# helm repo add wordpress https://raw.githubusercontent.com/phosphoresi/helm/master/wordpress-openshift
```
### On vérifie qu'il a bien été ajouté :
```
# helm repo list
```

## Déployer l'application

### On peut chercher les applications disponibles :
```
# helm search <nom_du_dépot>/
```
- Exemple :
```
# helm search wordpress/

NAME                         	CHART VERSION	APP VERSION	DESCRIPTION                
wordpress/wordpress-openshift	0.1.0        	1.0        	A Helm chart for Kubernetes
```
### On déploie l'application :
```
# helm install --namespace=<namespace> <nom_du_depot>/<nom_du_dossier>
```
- Exemple :
```
# helm install --namespace=wordpress wordpress/wordpress-openshift
```
