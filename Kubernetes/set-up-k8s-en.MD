1. Lanuch an Ubuntu EC2 instance
  
2. Install AWS CLI
   ```sh
    curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip
    apt install unzip python
    unzip awscli-bundle.zip
    #sudo apt-get install unzip - if you dont have unzip in your system
    ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
   ```
3. Install kubectl on ubuntu instance
   ```sh
   curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
    chmod +x ./kubectl
    sudo mv ./kubectl /usr/local/bin/kubectl
   ```

4. Install kops on ubuntu instance
   ```sh
    curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
    chmod +x kops-linux-amd64
    sudo mv kops-linux-amd64 /usr/local/bin/kops
   ```
5. Create EC2 Role and Attach the Role to EC2 Kubernetes-manager
   Policies: 
     AmazonEC2FullAccess 
     AmazonS3FullAccess 
     AmazonRoute53FullAccess 
     IAMFullAccess
 
6. AWS CONFIG
  ```sh
  root@ip-172-31-40-142:/home/ubuntu# aws configure
  AWS Access Key ID [None]:
  AWS Secret Access Key [None]:
  Default region name [None]: ap-southeast-2
  Default output format [None]:
  ```
  
7. Create EC2 Role and Attach the Role to EC2 Kubernetes-manager
   Policies: 
   AmazonEC2FullAccess 
   AmazonS3FullAccess 
   AmazonRoute53FullAccess 
   IAMFullAccess
   
8. Create Rout53: zhaoxin.com, Create S3: k8s-demo.zhaoxin.com

9. Expose environment variable on EC2 Kubernetes-manager
   ```sh
   export KOPS_STATE_STORE=s3://k8s-demo.zhaoxin.com
   ```
10. Create sshkeys before creating cluster
   ```sh
   ssh-keygen
   ```
   
11. Create kubernetes cluster definitions on S3 bucket and create the cluster
   ```sh
   kops create cluster --cloud=aws --zones=ap-southeast-2a --name=k8s-demo.zhaoxin.com --dns-zone=zhaoxin.com --dns private 
   kops update cluster --name k8s-demo.zhaoxin.com --yes
   ```


