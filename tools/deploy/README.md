# Deploy Lucida using Kubernetes

1. Run `./cluster_up.sh` to create a Kubernetes cluster on a single machine via Docker.
It assumes that Docker is already installed.
If you want to create a cluster with more than one machines,
please refer to [the official documentation](http://kubernetes.io/docs/).

2. Modify the `hostPath` fields of `mongo-controller.yaml` and `qa-controller.yaml`
to point to the directories where you want to store the data for MongoDB and OpenEphyra.
The default paths probably won't work on your machine.
Make sure you have write access to the directories you specify.
For example, modify the last section of `qa-controller.yaml` to be:

```
      volumes:
        - hostPath:
            path: /home/<your_username>/Documents/lucida_data_for_Kuebrnetes
          name: openephyra-persistent-storage
```

3. Run `./start_services.sh` to launch all Kubernetes services and pods.
It assumes that a local cluster is set up.
To debug, you can run `kubectl get service` to check the services,
`kubectl get pod` and `kubectl describe pod` to check the pods,
`docker ps | grep <controller_name>` followed by `docker exec -it <running_container_id> bash` to check the running containers.
Also, if MongoDB container is constantly being created without making progress, 
run `sudo netstat -tulpn | grep 27017` and kill the currently running MongoDB instance which also uses the port 27017.