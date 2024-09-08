# k3s-invidious

This project is a personal adaptation of [invidious-helm-chart
](https://github.com/iv-org/invidious-helm-chart/tree/master/invidious).

## Choose the image for your CPU

Modify the values.yaml file according to your CPU.

For ARM:

````yaml
tag: latest-arm64
````

Other:

````yaml
tag: latest
````

## Generation of a random key

````bash
openssl rand -hex 32
````

Add your key to the values.yaml file:

````yaml
hmac_key: "<Your key>"
````

## Add your domain IP

Add IP of your k3s cluster or the name of your domain.

This is an important step without it if you want to use invidious API with Yattee for example you will not have thumbnails.

````yaml
domain: <IP of your k3s cluster>
````

## Deploy

````bash
helm install invidious invidious/invidious \
  --namespace invidious \
  --create-namespace \
  -f values.yaml
````

## Check

````bash
kubectl get svc invidious -n invidious -o yaml
````

Check if the nodePort in the values.yaml is correct, if not you will have problems with the thumbnails in the api.

If this is incorrect add the one given by k3s in values.yaml by modifying:

````yaml
nodePort: 30277 
````

````yaml
external_port: 30277
````

Now update the new information.

````bash
helm upgrade invidious invidious/invidious \
  --namespace invidious \
  --create-namespace \
  -f values.yaml
````

And kill the invidious pod it creates itself with the new values.

## URL invidious

http://<domain>:<external_port>

## Delete all

````bash
kubectl delete namespace invidious
````
