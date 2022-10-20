# Pré-requis
- Avoir installé le client oc sur votre environnement de dévelopment
- Avoir installer le plugin kubernetes neat
- Avoir un environnement de dévelopement avec client git (vscode, notepad etc..)

# Clone du repo d'example
```
git clone https://github.com:amelie1979/example-php-app.git
```
# Configurer OpenShift pour inclure un nouveau template dans le Catalog Developer (doit être fait avec un compte administrateur OpenShift)

```
oc create -f https://raw.githubusercontent.com/amelie1979/example-php-app/main/Template/Template-PHP.yaml
```

# Activité en jour 1
## Création d'un project

```
oc new-project demo-abc
```

## Exporter le nom de l'application (sans caractère spécial, point ou trait d'union)

```
export app_name=abcapp1
```

## Création des objets sécure (confgimap & secrets & credential pour le registre d'image externe) (dans mon repo les config sont sous le répertoire config)

```
cd config
oc create configmap ${app_name}-config --from-file=config.json
```

## Example de déploiement en command line avec paramètres
```
oc new-app --template=abc-php-example --param=NAME=${app_name} --param=IMAGE_ARTIFACTORY_LOCATION=docker.io/appuio/example-php-docker-helloworld
```

# Activité en jour 2
## Prendre les yaml généré en copie et les garder pour la gestion en jour2 des configurations kubernetes

```
kubectl neat get dc ${app_name} -o yaml > ${app_name}-dc.yaml

kubectl neat get route ${app_name} -o yaml > ${app_name}-route.yaml

kubectl neat get service ${app_name} -o yaml > ${app_name}-svc.yaml

kubectl neat get is ${app_name} -o yaml > ${app_name}-is.yaml

kubectl neat get cm ${app_name}-config -o yaml > ${app_name}-cm-config.yaml
```