Use Kops to Create Cluster  
(https://kubernetes.io/docs/setup/custom-cloud/kops/)


* `brew upgrade kops`
* `aws s3 mb s3://k8s.btcc.shop`
* `export KOPS_STATE_STORE=s3://k8s.btcc.shop`
* `kops create cluster --zones=us-east-1c useast1.k8s.btcc.shop`
* `kops update cluster useast1.k8s.btcc.shop --yes`

更改然后更新
* `kops edit cluster useast1.k8s.btcc.shop`
* `kops edit ig nodes useast1.k8s.btcc.shop`
* `kops update cluster --yes`

删除K8s集群
* `kops delete cluster useast1.k8s.btcc.shop --yes`

导出配置到kubectl
* `kops export kubecfg useast1.k8s.btcc.shop`

得到Token用于访问UI
* `kops get secrets admin -oplaintext`

绑定一个外部IP
* `kops create cluster --zones=us-east-1c useast1.k8s.btcc.shop --associate-public-ip=true`
* `kops update cluster --yes`
* `kops rolling-update cluster`