# Kubernetes: Guia Completo para Orquestração de Contêineres
Kubernetes (K8s) é uma plataforma de código aberto usada para orquestração de contêineres, permitindo automação no gerenciamento, escalabilidade e implantação de aplicações em contêineres. Criado pelo Google e mantido pela Cloud Native Computing Foundation (CNCF), o Kubernetes se tornou o padrão para a execução de aplicações modernas na nuvem.

## Índice
### Introdução ao Kubernetes

1. O que é Kubernetes?
- História e evolução
- Arquitetura geral do Kubernetes

2. Componentes do Kubernetes
- Control Plane
  - API Server
  - etcd
  - Scheduler
  - Controller Manager
- Componentes do Node
   - Kubelet
   - Kube Proxy
   - Container Runtime (Docker, containerd, CRI-O)

3. Objetos Fundamentais no Kubernetes
- Pods
- Deployments
- Services
- ConfigMaps & Secrets
- Namespaces
- Volumes e Persistent Volumes (PV e PVC)
- StatefulSets

4. Gerenciamento de Configuração e Storage
- ConfigMaps e Secrets
- Volumes e Tipos de Storage no Kubernetes

5. Rede no Kubernetes
- Modelo de rede do Kubernetes
- CNI (Container Network Interface)
- Service Mesh e Istio

6. Escalabilidade e Auto Scaling
- Horizontal Pod Autoscaler (HPA)
- Cluster Autoscaler
- Vertical Pod Autoscaler (VPA)

7. Monitoramento e Logging
- Prometheus & Grafana
- EFK (Elasticsearch, Fluentd, Kibana)
- OpenTelemetry

8. Segurança no Kubernetes
- RBAC (Role-Based Access Control)
- Network Policies
- Pod Security Standards (PSS)
- Secrets Management com HashiCorp Vault

9. CI/CD no Kubernetes
- GitOps com ArgoCD e FluxCD
- Jenkins X
- Integração com GitHub Actions

10. Desafios e Melhores Práticas
- Estratégias de Deploy (Rolling, Blue-Green, Canary)
- Gestão de custos em clusters Kubernetes
- Performance e otimização

11. Kubernetes na Nuvem
- Amazon EKS
- Google GKE
- Azure AKS
- Minikube e Kind para desenvolvimento local

12. Kubernetes e Serverless
- Knative
- OpenFaaS

13. Kubernetes e Service Mesh
- Istio, Linkerd, Consul

14. Exemplos Práticos
- Implantação de uma aplicação em Kubernetes
- Exemplo de CI/CD com GitHub Actions
- Monitoramento com Prometheus e Grafana

15. Conclusão
- O futuro do Kubernetes
- Recursos para aprofundamento

## 1. Introdução ao Kubernetes
### O que é Kubernetes?
Kubernetes é um sistema de código aberto para automatizar a implantação, escalabilidade e gerenciamento de aplicações em contêineres. Ele simplifica a administração de infraestrutura baseada em contêineres, permitindo que aplicações sejam distribuídas e escaladas de forma eficiente.

### História e Evolução
- Criado pelo Google em 2014 com base no projeto interno Borg.
- Doado à CNCF (Cloud Native Computing Foundation).
- Cresceu para se tornar o padrão de orquestração de contêineres.

### Arquitetura Geral do Kubernetes
O Kubernetes segue um modelo de **arquitetura mestre-escravo**, onde o **Control Plane** gerencia os Nodes dentro do cluster.

## 2. Componentes do Kubernetes
O Kubernetes possui dois conjuntos principais de componentes:

### Control Plane
- **API Server**: Interface RESTful que gerencia requisições.
- **etcd**: Armazena os estados do cluster.
**Scheduler**: Responsável por alocar Pods nos Nodes.
**Controller Manager**: Garante que o estado do cluster corresponda ao desejado.

### Componentes do Node
- **Kubelet**: Agente que roda nos Nodes e garante que os containers estejam em execução.
- **Kube Proxy**: Gerencia a comunicação de rede no cluster.
- **Container Runtime**: O motor responsável por rodar os containers (Docker, containerd, CRI-O).

## 3. Objetos Fundamentais no Kubernetes
### Pods
A menor unidade de execução no Kubernetes. Um Pod pode conter um ou mais containers.

yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: meu-primeiro-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```
### Deployments
Permitem o gerenciamento de atualizações de aplicações.

yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: meu-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: minha-app
  template:
    metadata:
      labels:
        app: minha-app
    spec:
      containers:
      - name: app-container
        image: nginx:latest
```
### Services
Define como os Pods podem ser acessados dentro ou fora do cluster.

yaml
```
apiVersion: v1
kind: Service
metadata:
  name: meu-service
spec:
  selector:
    app: minha-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```
## 4. Rede no Kubernetes
Kubernetes implementa uma solução de rede baseada em ***CNI (Container Network Interface)***.

Principais opções:
- Flannel
- Calico
- Cilium

### Service Mesh
- Istio
- Linkerd
- Consul

## 5. Segurança no Kubernetes
### RBAC (Role-Based Access Control)
Controla permissões de usuários e aplicações no cluster.

yaml
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

### Network Policies
Definem restrições de tráfego entre Pods.

yaml
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

## 6. Monitoramento e Logging
### Prometheus & Grafana
- Coleta métricas e gera alertas.
- Grafana exibe dashboards visuais.

### EFK Stack
- Elasticsearch: Armazena logs.
- Fluentd: Agente coletor.
- Kibana: Interface para análise.

## 7. Kubernetes na Nuvem
### Amazon EKS
- Gerenciado pela AWS.

### Google GKE
- Integrado ao Google Cloud.

### Azure AKS
- Plataforma da Microsoft.

## 8. Exemplos Práticos
### CI/CD com GitHub Actions
yaml
```
name: Deploy para Kubernetes
on: push
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout do código
        uses: actions/checkout@v2
      - name: Login no DockerHub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin
      - name: Construção da Imagem
        run: docker build -t minha-app:latest .
      - name: Aplicação no Kubernetes
        run: kubectl apply -f deployment.yaml
```
## Conclusão
Kubernetes revolucionou a forma como implantamos aplicações na nuvem. Sua flexibilidade, escalabilidade e resiliência o tornaram a principal escolha para aplicações modernas.

### Perguntas Comuns
- O que é Kubernetes?
- Qual a diferença entre Docker e Kubernetes?
- O que são Pods?
- Como escalar aplicações no Kubernetes?
- Como funciona a segurança no Kubernetes?
