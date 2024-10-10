# Kubernetes-Course

## KUBERNETES TAKES CARE OF THE ...
  - Automatic deployment of the containerized applications across different servers
  - Distribution of the load across multiple servers
  - Auto-scaling of the deployed applications
  - Monitoring and health check of the containers
  - Replacement of the failed containers

## POD
  - POD is the smallest unit in the k8s world
    
![image](https://github.com/user-attachments/assets/4875bb1b-f49f-4b0e-92e3-75d003ff1f43)

## K8s services
![image](https://github.com/user-attachments/assets/323adb88-1c03-46c3-9b7c-795dd05e63d6)

 ### Master Node
  - API Server:
    Là điểm truy cập chính cho tất cả các yêu cầu đến Kubernetes. Nó xử lý các yêu cầu từ người dùng, các thành phần khác trong hệ thống và quản lý trạng thái của cluster.
  - Scheduler:
    Chịu trách nhiệm phân bổ các Pod vào các Worker Node dựa trên tài nguyên có sẵn và các quy tắc đã định.
  - Kube Controller Manager:
    Chạy các controller để quản lý trạng thái của các đối tượng trong cluster, đảm bảo rằng số lượng và trạng thái của các Pod, ReplicaSets, và các đối tượng khác là chính xác.
  - Cloud Controller Manager:
  Quản lý các tương tác với nhà cung cấp dịch vụ đám mây (nếu có). Nó cho phép Kubernetes hoạt động với các dịch vụ đám mây như tạo và quản lý tài nguyên. (example: loadbalancer)
  - etcd:
  Là cơ sở dữ liệu phân tán, nơi lưu trữ tất cả các thông tin cấu hình và trạng thái của cluster.
### Worker Node
  - kubelet:
    Là agent chạy trên mỗi Worker Node, chịu trách nhiệm quản lý các Pod và container. Nó đảm bảo rằng các container đang chạy đúng theo định nghĩa.
  - kube-proxy:
    Chịu trách nhiệm về mạng và định tuyến lưu lượng đến các Pod. Nó quản lý các quy tắc mạng và đảm bảo rằng các dịch vụ có thể được truy cập từ bên ngoài.
  - Container Runtime:
    Là phần mềm chạy container, ví dụ như Docker hoặc containerd. Nó chịu trách nhiệm tạo và quản lý các container trên Worker Node.
    
![image](https://github.com/user-attachments/assets/d4468135-ed09-424a-b08c-4be791bd3353)

## Settings

Minikube for linux:
    `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
     sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64`
Vertual Box:

Start your cluster
   ` minikube start --driver=docker `
Get IP
   ` minikube ip `
SSH mode:
  `NOTE`
  SSH is a standard protocol for management of any servers (including remote servers).
  
  But Minikube also provides command to SSH into the local minikube node
    ` minikube ssh `
    
  If you set `--driver=docker` you should use `minikube ssh` because `ssh docker@<minikube IP>` will not work.

  Minikube node user credentials: `Username: docker Password: tcuser`

- Get PODS by namespaces:   ` kubectl get pods --namespace=kube-system `
- To create Pods using  :   ` kubectl run test-cluster --image=nginx   `
- Connect with container:   ` docker exec -it 29d9038fe23c sh `
- Get hostname: `test-cluster`
- Get IP using hostname -i: `10.244.0.3 `
- Inspect page : `curl 10.244.0.3`
- Set alias: `alias k="kubectl"`

## Deployments 
A Deployment is the most common way to manage and scale the number of pods in Kubernetes. With a Deployment, you can:

  - Create and manage pods: A Deployment ensures that the specified number of identical pods are running at any given time.
  - Scale pods: You can easily increase or decrease the number of replicas (pods) managed by the Deployment.
  - Update configurations: You can modify the configuration of the pods, including container images, environment variables, and more. Kubernetes will automatically handle rolling updates to ensure minimal downtime.
  - Ensure consistency: All the pods in a Deployment are identical, meaning they are created using the same configuration and run the same container image.

- Create deployment by using ` k create deployment nginx-deplyment --image=nginx `
- Scale pods by using `k scale deployment nginx-deplyment --replicas=5 `
- If u want to access pod , we will need to access NODE by using `minikube ssh`
- Creating deployment based on the custom Docker image ` k create deployment k8-web-hello --image=huuquanganhdinh573/k8s-web-hello `

## Services
- If you want to allow external access , you will use service to manange external IP . By using `  k expose deployment nginx-deplyment --port=8080 --target-port=80 `
- Cluster IP is only available inside of the K8s clusters

----------------------------------------------------------------------------------------
![image](https://github.com/user-attachments/assets/39ae0ead-1693-4938-960d-8d1e74da11c9)

# All of step to deploy docker image
1.   `k expose deployment <deployment-name> --port=<port-pod>`
2.   Scaling custom image deployment `k scale deployment <deployment-name> --replicas=<number>`
3.   Creating NodePort Service ` k expose deployment <deployment-name>  --type=NodePort --port=3000`
4.   Accessing NodePort service ` minikube service <deployment-name> `
5.   ` k expose deployment k8s-web-hello --type=LoadBalancer --port=3000 `
6.   If you want to roll out the new version withour any interuption of service -> use strategy type is `RollingUpdate`. The new pod will be replaced one by one and the old pod will be still running.
7.   `docker build . -t huuquanganhdinh573/k8s-web-hello:2.0.0` -> `docker push huuquanganhdinh573/k8s-web-hello:2.0.0`
8.   Updating imgae in pod `k set image deployment k8s-web-hello k8s-web-hello=huuquanganhdinh573/k8s-web-hello:2.0.0`
9.   CKing status by using  ` k rollout status deployment k8s-web-hello`

![image](https://github.com/user-attachments/assets/7a69bf3d-cc4f-4d53-a4ad-a60d73fc8b57)
A DNS lookup (Domain Name System lookup) is the process of translating a domain name (like example.com or nginx) into its corresponding IP address (e.g., 192.168.1.1
- ` k exec k8s-web-to-nginx-6d4c878bd9-96hhk -- nslookup nginx ` : we tried to reso lve nginx name from inside of the container which belongs to this pod 
