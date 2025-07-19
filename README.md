# 🚀 Two-Tier Flask + MySQL Application on Kubernetes using KIND (ClusterIP Service)

<img width="1918" height="920" alt="Screenshot 2025-07-19 114652" src="https://github.com/user-attachments/assets/60b8813b-c2c3-4a36-bc86-26c456dcbdf5" />


This project demonstrates the deployment of a simple **two-tier application** consisting of a **Flask frontend** and a **MySQL backend** using **Kubernetes** (via KIND) on an **AWS EC2 instance**.

---

## 📌 Project Overview

- 🐍 **Flask App**: Python-based REST API that connects to MySQL and serves web content.
- 🐬 **MySQL DB**: Stores application data.
- ☸️ **Kubernetes (KIND)**: Local K8s cluster using Docker.
- 🧠 **ClusterIP Service**: Used to expose services **internally** within the cluster.
- ☁️ **AWS EC2**: Host VM for KIND cluster (Ubuntu 24.04).

---

📁 File Structure
.
├── Dockerfile
├── flask-app/
│   └── app.py
├── 01-mysql-pv.yaml
├── 02-mysql-pvc.yaml
├── 03-mysql-deployment.yaml
├── 04-flask-deployment.yaml
├── kind-config.yaml
└── README.md



## 🧱 Architecture

```
Client (Your Local Machine via Port Forwarding)
    |
    | [localhost:5000]
    |
AWS EC2 Instance (via SSH Port Forwarding)
 └── KIND Cluster (3 nodes: 1 control-plane, 2 workers)
      ├── Flask Deployment + ClusterIP Service
      └── MySQL Deployment + ClusterIP Service
```

---

## 🔧Steps Performed

✅ EC2 Setup
Launched an Ubuntu EC2 instance.

Opened required security group ports (SSH, 5000).

Installed Docker, kubectl, kind.

---

## ✅ KIND Cluster Setup

kind create cluster --config kind-config.yaml

kubectl get nodes

---


## ✅ Docker Images

Created a Dockerfile for the Flask app.

Built and loaded the image into KIND:

docker build -t flask-app:latest .
kind load docker-image flask-app:latest

---

## ✅ Kubernetes Manifests
Deployment and Service YAMLs created for both:

Flask (frontend)

MySQL (backend)

Used ClusterIP type for both services.

## Applied using:
kubectl apply -f mysql-pv.yaml
kubectl apply -f mysql-pvc.yaml
kubectl apply -f mysql-deployment.yaml
kubectl apply -f two-tier-deployment.yaml
kubectl apply -f two-tier-svc.yaml
kubectl apply -f  mysql-svc.yaml

---

## ✅ Persistent Volume Setup for MySQL
Created PersistentVolume (PV) and PersistentVolumeClaim (PVC) to persist MySQL data.

Volume mounted to MySQL at /var/lib/mysql


volumeMounts:
<img width="1917" height="363" alt="Screenshot 2025-07-19 115056" src="https://github.com/user-attachments/assets/d3ac6ece-f29c-463b-a17a-c77edac51942" />

  - name: mysql-persistent-storage
    mountPath: /var/lib/mysql
    
<img width="1722" height="918" alt="Screenshot 2025-07-19 114827" src="https://github.com/user-attachments/assets/f2485878-1efa-4285-8936-30536583d469" />

<img width="1207" height="257" alt="Screenshot 2025-07-19 114834" src="https://github.com/user-attachments/assets/7383a9e3-40d6-41f0-9cca-ae98e1272a92" />


    

---

## MOST IMP PART FOR KIND CONFIG

✅ Port Forwarding (to Access Flask)
To access Flask app from your local machine:

ssh -i "<path to pem-key>" -L <portnumber>:localhost:<portnumber> ubuntu@<EC2_PUBLIC_IP>
kubectl port-forward svc/flask-service 5000:5000

## Then visit: http://localhost:5000


<img width="1305" height="400" alt="Screenshot 2025-07-19 114710" src="https://github.com/user-attachments/assets/a8a1e0d5-b682-4774-b69a-e7697c93be29" />

---

## ✅ Scaling
To scale the Flask deployment:

kubectl scale deployment flask-deployment --replicas=5

<img width="1538" height="411" alt="Screenshot 2025-07-19 115016" src="https://github.com/user-attachments/assets/10c42813-bea9-4bf8-ae7f-d08e6d2f2c71" />


---

## ✍️ Author
Suyash Dahitule
💼 #90DaysOfDevOps
🔗 LinkedIn
📌 AWS EC2 | KIND | K8s | Docker | Flask | MySQL
