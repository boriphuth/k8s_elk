# k8s_elk
deploy elk stack in kubernetes

## Usage
- `manifests` Can run in production environment
- `experimental` In the experimental stage

使用之前需要修改各个yaml文件的Storage，Service相关的参数，根据实际情况选择合适的介质，修改完之后执行`kubectl -f manifests` 即可。


## Component
- es
- logstash
- kibana
- kafka
- zookeeper

## More
- [https://mp.weixin.qq.com/s/93_Jf8P69Q0nkw1Ip7MsFQ](https://mp.weixin.qq.com/s/93_Jf8P69Q0nkw1Ip7MsFQ)

更多干货请关注作者哦 
![](image/qrcode.jpg)

# Elastic stack on Kubernetes
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.17.5/bin/darwin/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client

$ minikube delete
$ minikube start --cpus 4 --memory 8192 --kubernetes-version v1.17.5
$ kubectl apply -f  manifests/
$ kubectl delete -f manifests/

$ kubectl apply -f kibana-ingress.yaml
$ kubectl delete -f kibana-ingress.yaml

$ kubectl get secret monitor-es-elastic-user -n beats -o=jsonpath='{.data.elastic}' | base64 --decode; echo


$ kubectl create secret generic elasticsearch-pw-elastic \
    -n monitoring \
    --from-literal password=3JW4tPdspoUHzQsfQyAI


$ kubectl get all -n elkstack
    
```
$ kubectl delete ns elkstack

$ kubectl apply -f manifests/elasticsearch.yaml
$ kubectl delete -f manifests/elasticsearch.yaml

$ kubectl get all -n elkstack
$ kubectl get pods -n elkstack | grep enode-0 | sed -n 1p | awk '{print $1}'

$ kubectl apply -f manifests/kibana.yaml 
$ kubectl delete -f manifests/kibana.yaml

$ kubectl apply -f manifests/logstash.yaml
$ kubectl delete -f manifests/logstash.yaml

$ kubectl apply -f manifests/zookeeper.yaml
$ kubectl delete -f manifests/zookeeper.yaml

$ kubectl apply -f manifests/kafka.yaml
$ kubectl delete -f manifests/kafka.yaml

$ kubectl get all -n elkstack
$ kubectl describe po logstash-644859c47f-47p58 -n elkstack