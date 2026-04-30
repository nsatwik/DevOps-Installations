1. create a user in IAM with admin full access, and create a access key & secret access key
2. Create a role for ec2 instance with admin full access
3. launch ec2 instance and configure aws-cli
4. aws ecr get-login (this is show output like this docker login -u AWS -p “encryted-password”  -e “ecr pvt-ur”)
5. use the same command to login to ecr docker (output will be login succeded)
6. docker secret key will be created in /root/.docker/config.jason
7. Now create a registry key by using this command 
    kubectl create secret generic my-registry-key --from-file=.dockerconfigjson=.docker/config.json --type=kubernetes.io/dockerconfigjson
8. To check the secret key use this command 
     kubectl get secret or kubectl get secret -o yaml
9. create a deployment file 
```bash
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      imagePullSecrets:
      - name: my-registry-key
      containers:
      - name: nginx
        image: 992321249651.dkr.ecr.ap-south-1.amazonaws.com/my-repo:nginx_image
        imagePullPolicy: Always
        ports:
        - containerPort: 80
```

10. kubectl apply -f deployment.yaml
11. kubectl create service nodeport nginx --tcp=80:80
12. kubectl get svc
13. ping node ip with svc port

