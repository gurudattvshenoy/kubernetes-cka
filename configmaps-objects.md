
**Config maps:**
Applications needs configuration data and common source of getting the data can be from .ini or XML or JSON or database etc. In k8s world, its configmaps. This config information is read from configmaps at the time of creating pods.

Data Source for configmaps
1. Directories
2. files
3. command line 

Steps:
1. Create a file in linux.
echo -n 'Non-sensitive data inside file-1' > file-1.txt
echo -n 'Non-sensitive data inside file-2' > file-2.txt

2. Create configmap 
kubectl create configmap nginx-configmap-volume --from-file=file-1.txt --from-file=file-2.txt

3. View config maps 
kubectl get configmap
kubectl describe configmap nginx-configmap-volume 

4. Create a pod using the below yaml
#Filename: nginx-pod-configmap-volume.yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-configmap-volume
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
Command:
kubectl create -f  nginx-pod-configmap-volume.yaml

