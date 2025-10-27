
Requirements:
StatefulSet - Name: redis-cluster
Replicas: 6
Pods status: Running (All 6 replicas)
Image: redis:5.0.1-alpine, Label = app: redis-cluster
container name: redis, command: ["/conf/update-node.sh", "redis-server", "/conf/redis.conf"]
Env: name: 'POD_IP', valueFrom: 'fieldRef', fieldPath: 'status.podIP' (apiVersion: v1)
Ports - name: 'client', containerPort: '6379'
Ports - name: 'gossip', containerPort: '16379'
Volume Mount - name: 'conf', mountPath: '/conf', readOnly:'false' (ConfigMap Mount)
Volume Mount - name: 'data', mountPath: '/data', readOnly:'false' (volumeClaim)
volumes - name: 'conf', Type: 'ConfigMap', ConfigMap Name: 'redis-cluster-configmap',
Volumes - name: 'conf', ConfigMap Name: 'redis-cluster-configmap', defaultMode = '0755'
volumeClaimTemplates - name: 'data'
volumeClaimTemplates - accessModes: 'ReadWriteOnce'
volumeClaimTemplates - Storage Request: '1Gi'

There were some new concepts involved here:

- The kind is not Deployment, because that will be stateless, we need Stateful
- In a stateful environmnent with multiple pods, we want each pod to have their own persistent volume dedicated for them and we dont want to create multiple pvcs which can be troublesome each time we increase the replicas, so we use volumeclaimtemplates.
- take note that configmap is mounted as a volume and we need to state defaultMode: 0755 because the default permissions for the files stated in configmap(/conf/update-node.sh) are 0644.we will get permission denied error when /conf/update-node.sh is being run
-  env:
          - name: POD_IP
            valueFrom:
              fieldRef:
                fieldPath: status.podIP
   This is a kubernetes downward api where I can feed the pods's ip address or namespace or metadata etc as a env variable