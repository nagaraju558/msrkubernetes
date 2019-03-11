Hi Madam/Sir,
             This is regarding the test on devops deploying an application using kubernetes cluster
             
             
I) Creating a EKS cluster 
      1) Create a role for eks and atttach policies for that
      2) Create a stack for vpc and security group 
      3) Install Iam-authenticator
      4) Install and configure kubectl for EKS 
      5) Create a EKS-cluster using the details 
                 -------> kubernetes version
                 -------> role
                 -------> vpc id which was created by a stack
                 -------> security group
      6) Update the kube-config file( for this aws-cli should be above 1.16.73 and python 2.7.9 above versions)
      7) Now create a worker-nodes using cloudformation
      8) After creating open cloud formation in that output tab,nodeinstancerole should be copied.
      9) Edit  AWS IAM Authenticator configuration map and paste the nodeinstancerole in that
      10) Apply the changes
II) Deploying packages using helm 
           ---> Installing Helm Package Manager with its libraries
           ---> Create Service account tiller and RBAC rule for tiller service account in yaml code
           ---> Apply that changes 
           ---> initialize the helm
           ---> verify the tiller pod is running in kube-system NameSpace
    deploying tomcat using Helm
           ----> helm install stable/tomcat
           ----> it will sucessfully download the tomcat with loadbalancer and it is ready 
           ----> Get the application URL by running these commands:
                       ---> it will take some time for loadbalancer ip available
                       ---> kubectl get svc -w(you eill be able to show name,type,cluster ip,external port(loadbalancer dns),ports
                       ---> http://$SERVICE_IP:portnum we will be able to access via web
    deploying mysql using Helm
           -----> helm install stable/mysql(version 5.7)
           -----> it will download and ask to get your root password run
                            ---> MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default plinking-puma-mysql -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)
                            ----> echo $MYSQL_ROOT_PASSWORD(password will be displayed)
           -----> To connect to your database:
                  ---> 1. Run an Ubuntu pod that you can use as a client:
                             ---> kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il
                  ---> 2. Install the mysql client:
                             ----> apt-get update && apt-get install mysql-client -y
                  ---> 3. Connect using the mysql cli, then provide your password:
                             ----> mysql -h plinking-puma-mysql -p
           -----> To connect to your database directly from outside the K8s cluster:
                  ---> MYSQL_HOST=127.0.0.1
                  ---> MYSQL_PORT=3306
                  Execute the following command to route the connection:
                     ---> kubectl port-forward svc/plinking-puma-mysql 3306
                     ---> mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}
 
