# Objetivo
Automatizar o ciclo completo de desenvolvimento, build, deploy e execução de uma aplicação FastAPI simples, usando GitHub Actions para CI/CD, Docker Hub como registry, e ArgoCD para entrega contínua em Kubernetes local com Rancher Desktop.

# Pré-requisitos

- [ ] Conta no GitHub com repositório público
- [ ] Conta no Docker Hub com token de acesso
- [ ] Rancher Desktop com Kubernetes habilitado
- [ ] kubectl configurado corretamente
- [ ] ArgoCD instalado no cluster local
- [ ] Python 3, Docker e Git instalados

# Deployment

### Configurar o Argo CD

1. Criar um namespace
    ```sh
    kubectl create namespace argocd
    ```
    
2. Instalar Argo CD no namespace
    ```sh
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```

3. Instalar o Argo CD CLI

    3.1 Instalar em ambiente **Windows**
    ```sh
    choco install argocd-cli
    ```

    3.2 Instalar em ambiente **Linux**
    ```sh
    curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
    
    sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
    
    rm argocd-linux-amd64
    ```

### Logar no Argo CD

1. Pegar senha inicial do Argo CD

    1.1 Pegar senha em ambiente **Windows**
    
    ```sh
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}")))
    ```
  
    1.2 Pegar senha em ambiente **Linux**
    
    ```sh
    kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
    ```
    
2. Port-forward para acessar ArgoCD
  
    ```sh
    kubectl port-forward svc/argocd-server -n argocd 38080:443
    ```

3. Logar no Argo CD via linha de comando
  
    ```sh
    argocd login localhost:38080
    ```
    > Usuário: `admin` |
    > Senha: `sua_senha_inicial`

4. Acessar interface do ArgoCD 

    No seu browser: `localhost:38080`

### Configurar a aplicação no ArgoCD

1. Criar a application

    ```sh
    kubectl apply -f argocd/hello-app-application.yaml
    ```

2. Conferir pods criados
   ```sh
    kubectl get pods -n hello-python
    ```  
3. Acessar a aplicação
   No seu browser: `localhost:8080`
