# Kubernetes Certmanager for Let's encrypt SSL certificates

## Create a namespace to run cert-manager 
```shell
kubectl create namespace cert-manager
```
## Elevate privileges to that of a cluster-admin
```shell
kubectl create clusterrolebinding cluster-admin-binding \
  --clusterrole=cluster-admin \
  --user=$(gcloud config get-value core/account)
```

## Install the CustomResourceDefinitions and cert-manager itself
```shell
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v0.11.0/cert-manager.yaml --validate=false
```

## Configuring Issuer
Kubernetes configuration for **Issuer** resource
```yaml
apiVersion: cert-manager.io/v1alpha2
kind: Issuer
metadata:
  name: letsencrypt
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: xyz@gmail.com

    # Name of a secret used to store the ACME account private key
    privateKeySecretRef:
      name: letsencrypt

    # ACME DNS-01 provider configurations
    solvers:
    # An empty 'selector' means that this solver matches all domains
    - selector: {}
      dns01:
        cloudflare:
          email: uvw@gmail.com
          # !! Remember to create a k8s secret before
          # kubectl create secret generic cloudflare-api-key-secret
          apiKeySecretRef:
            name: cloudflare-api-key-secret
            key: api-key 
```

Where 
**kind** is **Issuer**,
**server** under **acme** section is acme server URL,
**email** under **acme** section is the email id for registering in Lets encrypt ACME server,
**dnso1** for using dns chalange and cloudflare for using cloudflare dns for validation,
**email** under   **cloudflare** is the dns account email,
**apiKeySecretRef** is the cloudflare api-key, make sure that you create a secret for the same as the name mentioned in the configuration

## Certificate

Kubernetes configuration for **Certificate** resource
```yaml
apiVersion: cert-manager.io/v1alpha2
kind: Certificate
metadata:
  name: xyz-com
spec:
  secretName: xyz-tk-tls
  issuerRef:
    name: letsencrypt
  commonName: '*.xyz.tk'
  dnsNames:
  - '*.xyz.tk'
```
Where
**kind** is **certifcate**,
**secretName** is the secret which has the certificate and private key generated,
**issuerRef** is letsencrypt and it is the name of **issuer** we created earlier,
**commonName** is given as `*.xyz.com`  for wildcard certificate

## For retrieving certificate and private key from secret

**certificate**
```shell
kubectl get secrets xyz-tk-tls -o yaml | grep -e tls.crt | awk '{print $2}' | base64 -d
```
**private key**
```shell
kubectl get secrets xyz-tk-tls -o yaml | grep -e tls.key | awk '{print $2}' | base64 -d
```

## For reference

https://docs.cert-manager.io/en/latest/getting-started/install/kubernetes.html