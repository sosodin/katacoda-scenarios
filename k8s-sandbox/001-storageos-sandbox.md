Verify that you have a master and a worker node (`node01`) running:

`kubectl get nodes`{{execute}}

Create daemonset:

`kubectl apply -f storageos-daemonset.yaml`{{execute}}

Check that Kubernetes has started the StorageOS pod:

`kubectl get pods`{{execute}}

Encode the api address and create secret:

`APIADDRESS=$(echo -n "tcp://[[HOST2_IP]]:5705" | base64); sed -i "s/REPLACE/$APIADDRESS/g" storageos-secret.yaml`{{execute}}
`kubectl create -f storageos-secret.yaml`{{execute}}

Create `fast` StorageClass:

`kubectl create -f storageos-storageclass.yaml`{{execute}}
`kubectl get storageclass`{{execute}}

Create PersistentVolumeClaim which references the `fast` StorageClass:

`kubectl create -f storageos-pvc.yaml`{{execute}}
`kubectl get pvc`{{execute}}

Create a Redis pod which references `fast0001`:

`kubectl create -f storageos-pod.yaml`{{execute}}
`kubectl get pv`{{execute}}

============================================================

Alternatively, you can apply all files at once (you will still need to encode the API address above):

`kubectl apply -f storageos-pvc.yaml -f storageos-pod.yaml -f storageos-secret.yaml -f storageos-storageclass.yaml`{{execute}}
