- Author Nigel Wardle. Node4

1. Confirm your AZ CLI working directory is set to helm-training 
```
pwd
```

2. From the AZ CLI clone the helm training repository into your AZ CLI

```
git clone https://github.com/n4demo/learn-helm-to-deploy-to-AKS
```
3. You should see this:

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

##If you have created in the wrong folder you can use: rm -r learn-helm-to-deploy-to-AKS -f

6. From AZ CLI change directory into downloaded cloned repo, then list files.

```
cd ~/helm-training/learn-helm-to-deploy-to-AKS 
```

7. From the AZ CLI Editor review the files in the templates directory?

8. To see and test how Helm merges these manifest files within the templates folder with values from the (empty) values file - run: Helm template command and review the trace output.
```
helm template test-app test-app --namespace=test-app --create-namespace
```
9. Now we should replace hardcoded values in the manifest files with variable names that match values in the values file.
