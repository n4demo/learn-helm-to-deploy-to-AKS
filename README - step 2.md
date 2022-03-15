- Author Nigel Wardle. Node4

1. Confirm your AZ CLI current working directory is set to 'helm-training' 
```
pwd
```

2. From the AZ CLI, from GitHub, clone the Helm training repository into your AZ CLI

```
git clone https://github.com/n4demo/learn-helm-to-deploy-to-AKS
```

3. You should see something like this:

```
nigel@Azure:~/helm-training$ git clone https://github.com/n4demo/learn-helm-to-deploy-to-AKS
Cloning into 'learn-helm-to-deploy-to-AKS'...
remote: Enumerating objects: 45, done.
remote: Counting objects: 100% (45/45), done.
remote: Compressing objects: 100% (40/40), done.
remote: Total 45 (delta 6), reused 40 (delta 3), pack-reused 0
Unpacking objects: 100% (45/45), done.
nigel@Azure:~/helm-training$ 
```

## If you have created in the wrong folder you can use: rm -r learn-helm-to-deploy-to-AKS -f

4. From AZ CLI change directory into the downloaded cloned repo, then list files.

```
cd ~/helm-training/learn-helm-to-deploy-to-AKS
```

```
ls test-app
```

5. From the AZ CLI Editor, review the files in the templates directory

6. To see and test how Helm merges these manifest files within the templates folder with values from the (empty) values file - run: helm template [NAME] [CHART] [flags] and review the trace output. Note this will not deploy anything.

```
helm template my-test-app test-app --namespace=test-app --create-namespace
```

7. Now deploy the application: helm install [NAME] [CHART] [flags]

```
helm install my-test-app test-app --namespace=test --create-namespace
```

```
NAME: my-test-app
LAST DEPLOYED: Tue Mar 15 12:29:03 2022
NAMESPACE: test
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

```
helm list --all --all-namespaces
```

8. Now we should replace hardcoded values in the manifest files with variable names that match values in the values file. Open the values.yaml file and add the code below:

```
deployment:
 image: "nginx"
 tag: "1.21.6"
```

9. Open the test-deploy.yaml and replace the image with:

```
image: {{ .Values.deployment.image }}:{{ .Values.deployment.tag }}
```

10. Upgrade the release 

```
helm upgrade my-test-app test-app --namespace=test --values ./test-app/values.yaml
```

### You should receive a response similar to the below

Release "my-test-app" has been upgraded. Happy Helming!
NAME: test-app
LAST DEPLOYED: Tue Mar 15 14:01:21 2022
NAMESPACE: test
STATUS: deployed
REVISION: 2
TEST SUITE: None

```
helm list --all --all-namespaces
```

11. Set to an older image from the command prompt
```
helm upgrade my-test-app test-app --namespace=test --set deployment.tag=1.8.1
```

12. To make our chart more generic, let's replace all hardoded refs to object names with a Helm value expression and value. Eg for test-cm.yaml we can edit as below:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Values.name }}
data:
  name: "test"
  ```

13.  Rename values.yaml file to dev-values.yaml and add the code below. Note :

```
name: dev
```

14. Now deploy a copy of the app into the Dev namespace with names that reflect that it is being used for dev
```
helm install dev-app test-app --namespace=dev --values ./test-app/dev-values.yaml
```

15. Finally, create a copy of dev-values.yaml as uat-values.yaml and update the values accordingly and deploy a UAT version
```
helm upgrade uat-app test-app --namespace=uat --values ./test-app/uat-values.yaml
```

```
helm list --all --all-namespaces
```

## Congratulations!! You now know how to deploy different versions, with multiple instances across different Kubernetes namespaces of a simple nginx application. 

## Finally, clear out you objects

```
helm uninstall my-test-app --namespace=test

helm uninstall dev-app --namespace=dev

helm uninstall uat-app --namespace=uat
```

```
k delete ns my-namespace

k delete ns dev

k delete ns uat
```

```
cd ~
rm -r ~/helm-training -f
```
