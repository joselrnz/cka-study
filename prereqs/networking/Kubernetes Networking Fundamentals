# Kubernetes Networking Fundamentals

## 1. Pod Networking
Kubernetes assigns each **Pod** a unique IP address within the cluster. Every pod can communicate with any other pod without **NAT (Network Address Translation)**.

### **Example**
- **Pod A (IP: 10.244.1.2)** can communicate with **Pod B (IP: 10.244.1.3)**, even if they are on different nodes.

| **Pod A (IP)** | **Pod B (IP)** | **Communication** | **Action Taken by Kubernetes** |
|--------------|--------------|----------------|----------------------|
| 10.244.1.2  | 10.244.1.3  | HTTP Request | Routes directly using pod IP |

## 2. Service Abstraction
A **Service** provides a stable access point (IP and DNS) to a group of pods. Kubernetes updates service endpoints dynamically when pods change.

### **Service Types**

| **Service Type** | **External Access** | **Use Case** |
|-----------------|--------------------|-------------|
| ClusterIP | Internal cluster access only | Default type for internal communication |
| NodePort | Exposes a port on all nodes | Accessing services externally using a node's IP |
| LoadBalancer | Uses external cloud load balancer | Exposing services in cloud environments |
| ExternalName | Maps service to an external DNS | Redirecting requests outside the cluster |

### **Service Example (LoadBalancer)**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  selector:
    app: web-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```

## 3. Ingress and Egress
- **Ingress** manages incoming traffic to services inside the cluster.
- **Egress** controls outgoing traffic from the cluster to external resources.

### **Ingress Example** (Routing HTTP Traffic Based on Hostnames)
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-ingress
spec:
  rules:
  - host: www.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: frontend-service
            port:
              number: 80
```

### **Egress Example** (Allowing External HTTPS Traffic)
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-egress
spec:
  podSelector:
    matchLabels:
      role: client
  egress:
  - to:
    - ipBlock:
        cidr: 0.0.0.0/0
    ports:
    - protocol: TCP
      port: 443
```

## 4. Kube Proxy
`kube-proxy` enables network communication inside the cluster, implementing traffic routing using `iptables`, `ipvs`, or userspace forwarding.

### **Kube Proxy Example** (Using iptables for Load Balancing)
| **Service IP** | **Pod A IP** | **Pod B IP** | **Kube Proxy Rule** |
|--------------|--------------|--------------|---------------------|
| 10.96.0.1   | 10.244.1.2  | 10.244.1.3  | Routes traffic to one of these pod IPs |

## 5. Network Policies
**Network policies** control which pods can communicate with each other based on labels and namespaces.

### **Example: Restricting Access to Backend Pods**
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-backend
spec:
  podSelector:
    matchLabels:
      role: backend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: frontend
```

## 6. DNS in Kubernetes
Each service in Kubernetes gets an internal **DNS name** that resolves to its service IP.

### **Example (DNS Resolution in Cluster)**
| **Service** | **DNS Name** |
|------------|-------------|
| Frontend Service | frontend.prod.svc.cluster.local |
| Backend Service | backend.prod.svc.cluster.local |

### **DNS-Based Service Discovery Example**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  selector:
    app: backend
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
```

## 7. Load Balancing
Kubernetes automatically distributes traffic across pods backing a **Service**.

### **Example (Internal Load Balancing)**
| **Service Name** | **Pod IPs** | **Traffic Distribution** |
|-----------------|------------|--------------------------|
| web-app | 10.244.1.2, 10.244.1.3, 10.244.1.4 | Balanced across pods |

## 8. Service Mesh (Advanced)
A **Service Mesh** like Istio or Linkerd helps manage microservices communication, adding features like observability, security (TLS), and traffic management.

### **Example (Traffic Splitting with Istio)**
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: my-service
spec:
  hosts:
  - my-service
  http:
  - route:
    - destination:
        host: my-service
        subset: v1
      weight: 90
    - destination:
        host: my-service
        subset: v2
      weight: 10
```

## Summary of Kubernetes Networking Concepts
| **Concept** | **Description** | **Example** |
|------------|---------------|-----------|
| **Pod Networking** | Each pod gets a unique IP and can communicate directly with others. | Pod A (10.244.1.2) talks to Pod B (10.244.1.3) |
| **Service** | Provides stable access to a set of pods. | `frontend-service` exposing backend pods via LoadBalancer |
| **Ingress** | Manages external HTTP/S traffic into the cluster. | Ingress routing to `frontend-service` |
| **Egress** | Controls outbound traffic from the cluster. | Egress policy restricting external access |
| **Kube Proxy** | Routes traffic to pods via iptables or ipvs. | Balances traffic between backend pods |
| **Network Policies** | Defines rules for pod communication. | Allowing only frontend to talk to backend |
| **DNS** | Kubernetes DNS resolves service names. | `backend-service.prod.svc.cluster.local` resolving to a pod IP |
| **Load Balancing** | Distributes traffic across pods. | `web-app` service balancing requests across 3 pods |
| **Service Mesh** | Enhances microservice networking (security, routing). | Istio traffic splitting (90% v1, 10% v2) |

## Conclusion
Understanding Kubernetes networking is crucial for managing cluster communications effectively. These core concepts form the foundation for networking in Kubernetes, essential for the **Certified Kubernetes Administrator (CKA) exam**.

---
### 🚀 **Next Steps**
- Experiment with **Services, Ingress, and Network Policies** in a Kubernetes cluster.
- Deploy an **Istio Service Mesh** for advanced microservices management.
- Review **CKA networking objectives** and practice with real-world scenarios.

