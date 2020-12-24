
**Config maps:**
Applications needs configuration data and common source of getting the data can be from .ini or XML or JSON or database etc. In k8s world, its configmaps. This config information is read from configmaps at the time of creating pods.

```
A ConfigMap is an API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as 
environment variables, command-line arguments, or as configuration files in a volume.
```


Data Source for configmaps
1. Directories
2. files
3. command line 

Steps:
1. Create a file in linux.<br/>
```
echo -n 'Non-sensitive data inside file-1' > file-1.txt
echo -n 'Non-sensitive data inside file-2' > file-2.txt
```
<br/>
2. Create configmap 
<br/>

```
kubectl create configmap nginx-configmap-volume --from-file=file-1.txt --from-file=file-2.txt
```
<br/>

3. View all config maps 
<br/>

```
   kubectl get configmap<br/>
```
4. describe specific configmap
<br/> 

```
   kubectl describe configmap nginx-configmap-volume
```
<br/> 

5. Create a pod using the below yaml
<br/>
# Filename: nginx-pod-configmap-volume.yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod-configmap-vol
spec:
  containers:
  - name: nginx-container
    image: nginx
    volumeMounts:
    - name: test-vol
      mountPath: "/etc/non-sensitive-data"
      readOnly: true
  volumes:
    - name: test-vol
      configMap:
        name: nginx-configmap-vol
        items:
        - key: file-1.txt
          path: file-a.txt
        - key: file-2.txt
          path: file-b.txt
```
<br/>
# Command:
```
kubectl create -f  nginx-pod-configmap-volume.yaml
```

6. Test the pod and see the files file-a.txt with content "Non-sensitive data inside file-1" and file-b.txt with content "Non-sensitive data inside file-2" are created in /etc/non-sensitive-data

7. delete the configmap and delete it
kubectl delete pod  nginx-pod-configmap-vol
kubectle delete cm nginx-configmap-volume
