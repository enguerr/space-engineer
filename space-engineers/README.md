# Space Engineer Helm charts

* [space-engineers](space-engineers/)

# To expose ports 8080/tcp and 27016/udp on kubernetes using nginx ingress
```bash
kubectl apply  -n ingress-nginx -f - << EOF
apiVersion: v1
kind: ConfigMap
metadata:
  name: udp
  namespace: ingress-nginx
data:
  27016: "space-engineer/se-space-engineers:27016"
EOF
kubectl apply  -n ingress-nginx -f - << EOF
apiVersion: v1
kind: ConfigMap
metadata:
  name: tcp
  namespace: ingress-nginx
data:
  8080: "space-engineer/se-space-engineers:8080"
EOF
```


# To import a new instance
Launch the "Space Engineer dedicated server" on your windows box.
Configure un new instance and launch it.
Stop it and exit software.
Now go the "C:\ProgramData\Space Engineer Dedicated Server" Directory
And zip the folder.

To upload it on the persistent volum, you have to edit the deployment like this:
```bash
     initContainers:
        - name: space-engineers-init
          image: ubuntu:xenial
          command:
            - bash
            - '-c'
          args:
            - >-
              echo "Installing required packages"; apt-get update && apt-get
              install wget unzip -y; echo "Ensuring Directory Structure is
              Correct"; mkdir -p
              /appdata/space-engineers/SpaceEngineersDedicated; mkdir -p
              /appdata/space-engineers/steamcmd; mkdir -p
              /appdata/space-engineers/World; mkdir -p
              /appdata/space-engineers/Plugins; echo "Directory Check Complete";
              echo "Checking if World exists";  echo "Complete"; **while true; do sleep 5; done**
          resources: {}
          volumeMounts:
            - name: appdata
              mountPath: /appdata
          terminationMessagePath: /dev/termination-log
          terminationMessagePolicy: File
          imagePullPolicy: IfNotPresent

```

Now restart your pod and you can use shell as long as you want.
When you have finished, you can remove the  **while true; do sleep 5; done** 

