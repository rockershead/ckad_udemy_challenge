1.Kubectl command fails so must check the /etc/kubernetes/manifests/kube-apiserver.yaml. Filename was wrong and port in kubconfig also wrong.

2.
 Node01 
k get nodes
NAME           STATUS                     ROLES           AGE   VERSION
controlplane   Ready                      control-plane   63m   v1.30.0
node01         Ready,SchedulingDisabled   <none>          61m   v1.30.0


So there is taint scheduling disabled but that does not mean u remove the taint. U have to use command: k uncordon node01…..tis is to specifically cordon off node01…so to cordon it, k cordon node01.  Hence my pod.yaml no need toleration!!!
