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
  SSH is a standard protocol for management of any servers (including remote servers)
  But Minikube also provides command to SSH into the local minikube node
    ` minikube ssh `
  If you set `--driver=docker` you should use `minikube ssh` because `ssh docker@<minikube IP>` will not work

  Minikube node user credentials:
      `Username: docker
      Password: tcuser`
