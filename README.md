# Kubernetes local com Kind

## Passo a passo para subir um cluster kubernetes

### Pré requisitos

Docker Desktop instalado e rodando (Download)
kubectl instalado (Guia oficial)
Kind instalado (Guia oficial)

Instalação do Kind no Windows
Baixe o executável do Kind:
kind-windows-amd64.exe (última versão)
Renomeie para kind.exe e coloque em uma pasta do seu PATH (ex: C:\Program Files\kind\).
Adicione essa pasta ao PATH do sistema, se necessário.

### Passo a passo

1. Criando o cluster

´´´shell
kind create cluster --name meu-cluster --config kind-config.yaml
kubectl cluster-info --context kind-meu-cluster
kubectl get nodes
´´´

2. Exemplo de deploy de um pod nginx:

´´´shell
kubectl create deployment nginx --image=nginx
kubectl get pods
´´´

2.1. Expondo o serviço nginx

´´´shell
kubectl port-forward deployment/nginx 8080:80
´´´

3. Instalando o ArgoCD

´´´shell
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
´´´

3.1 Por padrão, o Argo CD só está acessível dentro do cluster. Para acessar via browser, faça um port-forward:

´´´shell
kubectl port-forward svc/argocd-server -n argocd 8080:443
´´´

3.2 Obtendo a senha admin

´´´shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

--1J-rhtKmZBreiXhU
´´´