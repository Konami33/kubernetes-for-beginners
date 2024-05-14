# Secrets

In Kubernetes, a Secret is an object used to store sensitive information such as passwords, OAuth tokens, and SSH keys. They are similar to configMaps, except that they are stored in an encoded or hashed format. Secrets are intended to be used to pass sensitive data to Pods in a secure manner, without exposing it to the Pod configuration or the container image.

# Creating Secret

To create a Secret named `my-db-secret` with the key-value pairs `DB_Host=mysql`, `DB_User=root` and  `DB_Password=paswrd`, we can use

1. Using Imperative Command:

To create a Secret using an imperative command, we can use the `kubectl create secret command` with the `generic` type. run the following command:

```bash
kubectl create secret generic my-db-secret \
  --from-literal=DB_Host=mysql \
  --from-literal=DB_User=root \
  --from-literal=DB_Password=paswrd
```
![alt text](./images/secret-1.png)

2. Using definition file

To create a Secret using a definition file, we need to create a YAML or JSON file with the Secret configuration.

```bash
vim secret.yaml
```

```YAML
apiVersion: v1
kind: Secret
metadata:
  name: my-db-secret
type: Opaque
data:
  DB_Host: bXlzcWw=   # base64 encoded value of 'mysql'
  DB_User: cm9vdA==   # base64 encoded value of 'root'
  DB_Password: cGFzd3Jk   # base64 encoded value of 'paswrd'
```

Then, run the following command to create the Secret from the definition file:

```bash
kubectl apply -f secret.yaml
```

![alt text](./images/secret-2.png)

# Encoding secret

However, one thing we discussed about secrets was that they are used to store
sensitive data and are stored in an encoded format. We can convert plain text to encoded format by linux `echo -n` command followed by the text.

```bash
echo –n 'mysql' | base64
```
```bash
echo –n 'root' | base64
```
```bash
echo –n 'paswrd' | base64
```
# View Secret

Now we have created secrets. To view the secret we can run

```bash
kubectl get secrets
```
![alt text](./images/get-secrets.png)


To view more information about the newly created secret, run

```bash
kubectl describe secrets
```

![alt text](./images/describe.png)

To view the information as well as the value of the secret, run

```bash
kubectl get secret my-db-secret -o yaml
```

![alt text](./images/value.png)

Now we can see the hashed values of the secret.

# Decode secret

To decode the hashed secret we can use the same command with an addition of --decode option.

```bash
echo –n 'bXlzcWw=' | base64 --decode
```
```bash
echo –n 'cm9vdA==' | base64 --decode
```
```bash
echo –n 'cGFzd3Jk' | base64 --decode
```

So, in this lab we have created a secret in both ways, encoded the values, display the information of the secret file and also decoded the hashed values.


