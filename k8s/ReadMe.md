
https://www.docker.elastic.co/#

https://kauri.io/(25)-install-elasticsearch-and-kibana-to-store-and-visualize-monitoring-data/e5b86351f38940b8a071267062f052cb/a


## STEPS
$ kubectl delete ns monitoring
$ kubectl apply  -f k8s/0.monitoring.namespace.yml

$ kubectl apply -f k8s/1.elasticsearch-master.yml
$ kubectl apply -f k8s/2.elasticsearch-data.yml
$ kubectl apply -f k8s/3.elasticsearch-client.yml

$ kubectl get all -n monitoring
$ kubectl get pods -n monitoring

$ kubectl logs -f -n monitoring \
  $(kubectl get pods -n monitoring | grep elasticsearch-master | sed -n 1p | awk '{print $1}') \
  | grep "Cluster health status changed from \[YELLOW\] to \[GREEN\]"

$ kubectl exec $(kubectl get pods -n monitoring | grep elasticsearch-client | sed -n 1p | awk '{print $1}') \
    -n monitoring \
    -- bin/elasticsearch-setup-passwords auto -b

Changed password for user apm_system
PASSWORD apm_system = MXZid2aht1wCcnMzsj64

Changed password for user kibana
PASSWORD kibana = Fq3yZilRsYZfUPvaUPAv

Changed password for user logstash_system
PASSWORD logstash_system = AsIJNpPT1kLqmb526KBZ

Changed password for user beats_system
PASSWORD beats_system = sbiKRGEEWlqhcXJo2a4v

Changed password for user remote_monitoring_user
PASSWORD remote_monitoring_user = lSRIChplAFS56SBvb6Zf

Changed password for user elastic
PASSWORD elastic = MK3oUqZr5hxiZcdBsB2k

$ kubectl create secret generic elasticsearch-pw-elastic \
    -n monitoring \
    --from-literal password=MK3oUqZr5hxiZcdBsB2k

$ kubectl apply  -f k8s/4.kibana.yml
$ kubectl logs -f -n monitoring $(kubectl get pods -n monitoring | grep kibana | sed -n 1p | awk '{print $1}') \
     | grep "Status changed from yellow to green"

$ kubectl get all -n monitoring
$ kubectl get service kibana -n monitoring

$ kubectl apply -f k8s/5.kube-state-metrics.yml

$ kubectl apply -f k8s/6.metricbeat.yml
$ kubectl get all -n monitoring -l app=metricbeat

$ kubectl apply -f k8s/7.filebeat.yml
$ kubectl get all -n monitoring -l app=filebeat