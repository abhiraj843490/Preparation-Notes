Kubernetes is a orchestration tool, providing facility to Automatic deployment, scaling, load-balancing.

## Server LOG check
ls -ltr
ssh -i 'dev-product-dyp2_key.pem' ec2-user@ec2-3-16-217-152.us-east-2.compute.amazonaws.com
kubectl get ns
kubectl get po -n devdyp2


kubectl logs -f -l app=providermgmt -n dyp2-dev5
kubectl logs -f htgauthserver-d9768b4cc-57wcp -n dyp2-dev02
kubectl describe po providermgmt -n  dyp2-dev02
kubectl delete po applications-64f78d4748-ml6fz -n dypdev2


## Deployment

kubectl scale --replicas=1 deploy microservice-name -n dyp2-dev02

kubectl scale --replicas=0 deploy <service_name> -n devdyp2
kubectl scale --replicas=2 deploy applications -n devdyp2

## Check mongo
kubectl get ns
kubectl get po -n devdyp2
kubectl exec -it mongo-mongodb-7d9fb69b9d-9jj5c sh -n devdyp2
grep -i mongo

------------FIND PASS OF MONGO-------------
kubectl exec -it mongo-mongodb-7d9fb69b9d-mntpw sh -n devdyp2
env | grep PASS


Build

1 . Dashboard -> South_Dakota_EKS -> Build -> Backend Build -> <select-branch>
2. click on Build now


https://www.baeldung.com/java-visitor-pattern


-------------------cherry-pick-------------------
git cherry-pick <commit-id>5242464cd25b7044524fe3ef2bf6993f1d288da8
git add .
git cherry-pick --continue
--------------------------------------------------------------

Port forwarding for the checking the details:
1. kubectl port-forward pod/providermgmt-5cd8766659-5xjgc 13080:13080 -n devdyp2
2. curl 'http://localhost:13080/api/providermgmt/externaMsgTemplates'









