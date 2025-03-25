# Kubernetes: Como Funciona e Por Que Utilizá-lo?
## O Que é Kubernetes?
Kubernetes (K8s) é uma plataforma de **orquestração de contêineres** que automatiza o gerenciamento, a escalabilidade e a implantação de aplicações. Ele foi desenvolvido pelo Google e, posteriormente, doado à **Cloud Native Computing Foundation (CNCF)**, tornando-se o padrão da indústria para a execução de aplicações em contêineres.

## Por Que Utilizar o Kubernetes?
Antes de entender como ele funciona, é importante saber por que ele é amplamente adotado:

**✅ Escalabilidade Automática** – Kubernetes pode escalar aplicações automaticamente conforme a demanda.
**✅ Alta Disponibilidade** – Se um contêiner falhar, o Kubernetes o reinicia automaticamente.
**✅ Orquestração de Múltiplos Contêineres** – Permite a distribuição eficiente dos serviços.
**✅ Facilidade de Deploy** – Suporta Rolling Updates, Canary Releases e Blue-Green Deployments.
**✅ Multi-cloud e Portabilidade** – Roda em AWS, Azure, GCP, On-premise, permitindo evitar o vendor lock-in.
**✅ Gestão de Recursos** – Utiliza CPU e memória de forma otimizada para evitar desperdícios.
**✅ Resiliência e Autocorreção** – Se um nó cair, os pods são redistribuídos automaticamente.
**✅ Integração com DevOps e CI/CD** – Facilita pipelines de entrega contínua e GitOps.
**✅ Networking e Service Discovery** – Facilita a comunicação entre aplicações dentro do cluster.
**✅ Segurança e Governança** – Implementa RBAC (Role-Based Access Control), Network Policies e Secret Management.

# Como Funciona o Kubernetes?
O Kubernetes funciona organizando aplicações dentro de contêineres e gerenciando sua execução em um cluster de máquinas. Ele divide sua infraestrutura em dois componentes principais:

**📌 1. Control Plane** (Plano de Controle) – Gerencia o cluster.
**📌 2. Worker Nodes** (Nodos de Trabalho) – Executam as aplicações.

## 1. Control Plane (Plano de Controle)
O Control Plane é responsável por orquestrar os recursos do cluster e garantir que os contêineres estejam rodando corretamente. Ele inclui:

### ✅ API Server
- Exposição da API Kubernetes via REST.
- Recebe comandos do kubectl, CI/CD, dashboards ou APIs externas.

### ✅ etcd
- Banco de dados chave-valor que armazena a configuração e estado do cluster.

### ✅ Scheduler
- Distribui os Pods entre os Nodes conforme a capacidade disponível.

### ✅ Controller Manager
- Mantém o estado desejado do cluster, garantindo que os pods rodem corretamente.

### ✅ Cloud Controller Manager
- Facilita a integração do Kubernetes com provedores de nuvem como AWS, GCP e Azure.

## 2. Worker Nodes (Nós de Trabalho)
Os Worker Nodes são responsáveis por executar os contêineres das aplicações. Cada Node contém:

### ✅ Kubelet
- Agente responsável por garantir que os contêineres especificados no Pod estejam rodando corretamente.

### ✅ Kube Proxy
- Gerencia a comunicação de rede entre os Pods e o Cluster.

### ✅ Container Runtime
- Responsável por rodar os contêineres. Pode ser:
   - Docker
   - containerd
   - CRI-O

# 🏗 Componentes Principais do Kubernetes
Agora que entendemos a arquitetura geral, vamos detalhar alguns objetos principais do Kubernetes.

## 1. Pods
O Pod é a menor unidade do Kubernetes. Ele contém um ou mais contêineres e compartilha os mesmos volumes e rede.

### 📌 Exemplo de um Pod no Kubernetes:

yaml
```
Editar
apiVersion: v1
kind: Pod
metadata:
  name: meu-pod
spec:
  containers:
  - name: app
    image: nginx
    ports:
    - containerPort: 80

```
## 2. Deployments
O Deployment gerencia atualizações de aplicações de maneira controlada.

### 📌 Exemplo de Deployment:
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
### 📌 O que ele faz?
- Cria 3 réplicas do Pod.
- Se um Pod falhar, o Kubernetes o reinicia automaticamente.

## 3. Services
Os Services são usados para expor Pods dentro e fora do cluster.

### 📌 Tipos de Services:
- **ClusterIP** – Comunicação interna dentro do cluster.
- **NodePort** – Exposição do serviço em uma porta fixa do Node.
- **LoadBalancer** – Usa um balanceador de carga externo.

### 📌 Exemplo de Service:

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
## 4. ConfigMaps & Secrets
**ConfigMaps** armazenam configurações, enquanto **Secrets** armazenam informações sensíveis, como credenciais.

### 📌 Exemplo de Secret:

yaml
```
apiVersion: v1
kind: Secret
metadata:
  name: meu-secret
type: Opaque
data:
  senha: bXlzZWNyZXRwYXNzd29yZA==  # (Senha codificada em base64)
```

## 5. Auto Scaling (Escalabilidade)
O Kubernetes oferece diferentes tipos de escalabilidade automática:

### 📌 Horizontal Pod Autoscaler (HPA)
- Aumenta o número de Pods com base no consumo de CPU/memória.

### 📌 Cluster Autoscaler
- Adiciona ou remove Nodes no cluster.

### 📌 Vertical Pod Autoscaler (VPA)
- Ajusta a quantidade de CPU/memória de cada Pod automaticamente.

### 📌 Exemplo de HPA:

yaml
```
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: meu-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: meu-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```
## 📈 Casos de Uso do Kubernetes
Kubernetes é amplamente utilizado por grandes empresas para várias finalidades:

✅ **Microserviços** – Orquestração de aplicações distribuídas.
✅ **Big Data & Machine Learning** – Execução de pipelines de dados.
✅ **Edge Computing & IoT** – Gerenciamento de workloads distribuídos.
✅ **Servidores de Jogos** – Automação e escalabilidade para games online.
✅ **Plataformas de Streaming** – Infraestrutura para Netflix, YouTube, etc.

## 🎯 Conclusão
O **Kubernetes** revolucionou a forma como aplicações são executadas, tornando-se essencial para quem deseja trabalhar com **Cloud, DevOps e Microserviços**.

### 📌 Por que usar Kubernetes?
✔ Automação completa de deploys
✔ Escalabilidade sob demanda
✔ Alta disponibilidade e resiliência
✔ Portabilidade entre provedores de nuvem